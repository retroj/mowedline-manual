
Repl
====

Mowedline itself does not provide a repl, and we're not too interested in
re-inventing the wheel on that score either because as a scheme program,
mowedline can be loaded into other CHICKEN Scheme programs that provide
them, like `csi`.

Mowedline wasn't originally developed with repl use in mind, so we are
still experimenting with the kind of api to provide for convenience within
one.

The basic formula to load mowedline in a repl environment is this:

    csi -e '(use mowedline)(mowedline-start)(repl)'

The mowedline repl can also be run in a terminal multiplexer like screen
or tmux, which allows you to have the repl running in the background
detached from any terminal, ready to be called up when needed.


Tmux
----

To start mowedline in a detached tmux session:

    tmux new-session -d -s mowedline-repl -n mowedline-repl \
        "csi -e '(use mowedline)(mowedline-start)(repl)'"

To call it up:

    tmux attach -t mowedline-repl

Then to detach again, use the key sequence `C-b d` or close the terminal.
