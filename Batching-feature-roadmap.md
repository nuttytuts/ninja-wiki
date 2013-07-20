This page is created to discuss the batching possibilities for Ninja. See the [discussion that started the idea](https://groups.google.com/d/msg/ninja-build/kLq69BGRec8/Yb2MaiRMwEUJ).

## What is batching?

### Basic case

Let's imagine the following Ninja project, given an hypothetic compiler `cc` and a linker `ld`:

```ninja
rule cc: cc $in -o $out
rule link: ld $in -o $out
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

* the compiler can't use it hypothetic internal cache to precompile headers (both `.c` files include the giant and complicated `baz.h`);
* `cc` is loaded twice in memory, causing a process startup overhead.

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

Now, replace the two files by a complex, real-world project build process and you get the idea.

## So, what can we do?


