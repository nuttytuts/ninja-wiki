This page is created to discuss the batching possibilities for Ninja. See the [discussion that started the idea](https://groups.google.com/d/msg/ninja-build/kLq69BGRec8/Yb2MaiRMwEUJ).

## What is batching?

### Basic case

Let's imagine the following Ninja project, given an hypothetic compiler `cc` and a linker `ld`:

```ninja
rule cc
  command = cc $in -o $out
rule link
  command = ld $in -o $out
build foo.o: cc foo.c | baz.h
build bar.o: cc bar.c | baz.h
build a.out: foo.o bar.o
```

A full build (non-incremental) will execute three separate commands:

```sh
$ ninja
[1/3] cc foo.c -o foo.o
[2/3] cc bar.c -o bar.o
[3/3] ld foo.o bar.o -o a.out
```

This is not optimal because:

* the compiler can't use it *hypothetic* internal cache for headers (both `.c` files include the giant and complicated `baz.h`; note that it's not really header precompilation, it's more purely-internal header caching, and is dependent on compiler implementations);
* `cc` process is loaded twice in a row, causing some process startup overhead.

### Optimized case

Now, let's imagine our `cc` compiler accepts a special syntax for several input/output files: `cc --multiple input1.c output1.o input2.c output2.o`. The optimal build could be:

```sh
$ ninja
[1/2] cc --multiple foo.c foo.o bar.c bar.o
[2/2] ld foo.o bar.o -o a.out
```

### Performance comparaison

Let's arbitrary say:

* the header `baz.h` takes 2s to be parsed (it's a huge header);
* the `cc` compiler takes 1s to get started, before even starting input parsing;
* parsing+compilation of a single `.c` file takes 2s;
* the linker takes 2s.

Then:

* the non-optimal full build will take `2*(1+2+2)+2`: 12 seconds;
* the optimal full build will take `1+2+2*2+2`: 9 seconds.

Now, replace the two files by a complex, real-world project build process and you get the idea. **Important note:** 1 second may seems really a lot to get started, but in the Node.js world, for instance, it's actually the case because Google V8 recompiles the compiler's JS code itself at each call... Eg. with Stylus, CoffeeScript, etc. It's potentially the same for any scripted compilers: SASS in Ruby, Python, etc.

## So, what can we do?

Implement batching in Ninja! There are, obviously, a few blocking issues.

### How are output directories handled?

E.g. if two available commands are "build dir1/foo.o: cc foo.cc" and "build some/other/dir/bar.o: bar.cc", how can Ninja know how to assemble the command line such that the batch runner can run them correctly?

**Possibility:** if the compiler have a special syntax for it, it could just be `cc --multiple foo.cc dir1/foo.o  bar.cc some/other/dir/bar.o`. The biggest problem here is to have a compiler supporting multiple specific *output files* (in addition to multiple *inputs*). Then the syntax in the Ninja could be something like:

```ninja
rule cc
  command = cc --multiple $batch{$in $out}
```

More complete:

```ninja
rule cc
  command = cc $in -o $out
  batch_command = cc --multiple $batch{$in $out}
```

Another idea:

```ninja
rule cc: cc $ins --outputs $outs
```

### How many knobs to control batching does Ninja need to provide? 

E.g. do we need a way to limit how many files are batched together?  Maybe the answer to the output directory batching question is that we can only batch together files that share an output directory -- does this mean we then need an $output_dir variable usable in the rule's command?  (E.g. "command = mycompiler --output-dir=$output_dir $in", where $in can be multiple files.)

**Possibility:** if we let the compiler determine itself the name of outputs, then there is a problem with output directories indeed. If we have the possibility to provide both a list of inputs and corresponding outputs to the compiler, the problem is solved out-of-the-box. Then the batching can be done depending on the parallelism, for example with `-j4` we may want to divide the compilation of 100 files to 4 batches of 25 files each.

### How are failures handled?

If the batched compile exits with a failure status, does that mean all of the outputs passed to the command should be considered bad, or must we parse the output in some way to determine failure on a per-output basis?

**Possibility:** I believe parsing the output would be too fragile, it basically depends on the compiler stderr output format, and will break if this output changes.

But the thing is: batching is the *most useful* for full-builds. In a full build, it is not supposed to have compile errors, except in the case of environment mis-configuration. Plus, when getting batching errors, then we care a *bit* less about compile time.

So, the solution could be:

* considering all the batch outputs as dirty;
* divide the batch into 2 or more pieces;
* re-run each batches;
* re-divide in case of errors, etc.

Or, it could just explode the batch and run each file separately to ensure we directly find the errored files.

### How are dependencies extracted?

It's easy enough to say "just write out multiple .d files", but we hit the same output directory problem, and the whole reason for patching is for cl.exe which doesn't write .d files.  In cl's /showIncludes case, it lists each file it's compiling followed by that file's list of includes, so perhaps that format should just be required by Ninja's parser.

**Possibility:** yes that's a problem too. Things are simple in case the compiler supports it, of course:

```ninja
rule cc
  command = cc $in -o $out --dep-file $out.d
  batch_command = cc --multiple-with-deps $batch{$in $out $out.d}
  depfile = $out.d
```

Or even:

```ninja
rule cc
  command = cc $in -o $out --dep-file $out.d
  batch_command = cc --multiple $batch{$in $out} --dep-file $batch_name.d
  batch_depfile = $batch_name.d
```

## Existing compilers we'd like to support

### MSVC `cl.exe`

Controlling output path: "It turns out the /Fo option actually works [to specify an output directory], but the directory you specify must end with a backslash."
http://stackoverflow.com/questions/7706490/visual-c-command-line-compiler-cl-exe-redirect-obj-files

Dependencies: the `/showIncludes` flag writes output like:
```
input1.cc
Note: including path: ...bar.h
input2.cc
Note: including path: ...baz.h
```

### GCC

Output: there is apparently no way to specify output directory.
http://stackoverflow.com/questions/1814270/gcc-g-option-to-place-all-object-files-into-separate-directory

One solution is to change the current directory before building:
```sh
$ cd obj/
$ gcc -c ../src/foo.c ../src/bar.c
$ cd ../src
```

Ugly, but not painful if Ninja takes care of it behind the scenes.

### CoffeeScript (node.js)

Output: The `-o flag` can specify an output directory.

Dependencies: each file builds independently, there's no need.