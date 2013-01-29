Mowedline
=========

Mowedline is a status bar program for X, written in [Chicken
Scheme](http://www.call-cc.org/) with XLib, Xft, and DBus.  It was
inspired by [dzen2](http://sites.google.com/site/gotmor/dzen), and like
that program, aims at an unobtrusive, minimalist look.  It is based on a
client/daemon design, where the daemon maintains one or more windows, and
the client sends commands to the daemon over DBus to update the contents
of those windows.  A mowedline window is divided into widgets.  Each
widget has a unique name, by which the client can refer to it to update
its contents.

The runtime configuration is written in scheme.  It can either be
`~/.mowedline` or `~/.config/mowedline/init.scm`.  This file is loaded
when the daemon starts, and sets variables that affect global settings or
creates windows.  If the config script does not create any windows (or if
there is no config script) then the default window is created.


Obtaining Mowedline
-------------------

The source code can be obtained from [my git repository](/git/mowedline/).

You will also need to download and install one egg which is not yet
available via chicken-install:

 * [xft](/git/xft-egg/)

After cloning the xft-egg repository with git, enter the directory and do
a test build with `chicken-install -n`.  It may tell you that you need to
install other eggs first, like the xtypes egg.  For each, do for example:
`chicken-install xtypes`.  You will also need to have Xft development
headers installed on your system.  On Debian, the package for these is
called libxft-dev.  Once the egg builds successfully, install it with
`chicken-install -s`.

To build mowedline itself, enter its directory and do `csc mowedline.scm`,
then `csc mowedline-client.scm`.  If chicken reports a missing dependency,
install it with chicken-install.


Project Status
--------------

__January 29, 2013:__ Verion 0.2pre1 --- mowedline has been split into two
programs, mowedline and mowedline-client, to address the problem of
simultaneous multiple server starting,
[discussed here](/blog/2013/01/28/mowedline-three-bugs).

__August 30, 2011:__ Xft, unicode, color, and more, all supported in
mowedline 0.2pre.

__August 26, 2011:__ Progress on writing Xft bindings for Chicken Scheme.
They're not polished enough to release yet, but this will allow me to
start experimenting with using Xft in mowedline.

__March 24, 2011:__ _Mowedline 0.1_ The program now has the minimum
feature set to be considered useful, though much work remains to be done.


License
-------

Mowedline is licensed under the terms of GPL3.
