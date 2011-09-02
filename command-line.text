
Command Line
============

The mowedline executable program itself serves as both a client and a
server.  If you run it and the mowedline daemon is not yet running, it
will fork and spawn the daemon.

The command line is parsed purely by position.  Command line options may
have any number of required arguments, but never optional arguments.  For
this reason, the leading hyphens on the option names are purely
stylistic.  Mowedline drops any leading hyphens on options, so you can
give as many as you want or zero.  In other words, `mowedline --help` does
the exact same thing as `mowedline -help` or `mowedline help`.


Special Options
---------------

 * -help

    Display command-line help.

 * -version

    Display the version of mowedline.


Client Options
--------------

These options comprise the main interface to controlling a running
mowedline daemon.

 * -quit

    Terminate the mowedline daemon.

 * -read \<widget>

    Read lines from stdin, and update the given widget with each line.

 * -update \<widget> \<value>

    Update the given widget with the given value.


Server Options
--------------

The command line can be used to do limited experimentation with mowedline,
via the _Server Options_.  However, after a little experimentation, you
will probably want to go on to writing a .mowedline file with your
permanent configuration.  Server options are evaluated at server startup,
just before loading the .mowedline.


 * -q

   Bypass startup file (.mowedline).

### Setting Defaults

These options set defaults.  They apply to all following widgets (or
windows) created on the command line, and in your .mowedline.

 * -bg \<color>

   Set the background color for succeeding widgets.

 * -fg \<color>

   Set the foreground color for succeeding widgets.

 * -flex \<value>

   Set the flex value for succeeding widgets.

 * -position \<value>

   Set the window position for succeeding windows.  Allowed values are
   `top` and `bottom`.


### Widgets and Windows

These options create widgets and windows.  If widget options are given,
but no window is explicitly created, one will be created automatically.
If no windows have been created by the time the .mowedline is finished
loading, a default window with one text-widget, named "default", will be
automatically created.

 * -text-widget \<name>

   Make a text-widget with the given name.

 * -clock

   Make a clock widget.

 * -window

   Create a window containing the foregoing widgets.

   This may seem counterintuitive at first.  The -window option comes
   _after_ any widget options on the command line.  This is because the
   window height is based on widget height, as well as other details of
   widget initialization.  It's like programming in Forth!

   Also note, if you're just making one window, you don't need to supply
   this option, as the default window will be created with either the
   widgets you specified or a single default widget.
