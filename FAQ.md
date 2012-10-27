**Why does my program with colored output not have color under Ninja?**

Because Ninja runs commands in parallel, it buffers their output to prevent it from interleaving.  Commands will detect that their output is not directed to a terminal, assume they're going to a log file, and turn off their colored output.  To fix this, pass flags to the subcommand (like `-fcolor-diagnostics` to clang) to force color on.

(Ninja could use pseudo ttys to work around this further, but different systems have different limits on the number of ptys available, and I also worry that commands like `svn up` would assume they can interact with the user.)