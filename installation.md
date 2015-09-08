
Mowedline Installation
======================

Prerequisites
-------------

To install chicken you must at least have the development headers for
X11, XRender, and Xft.  On Debian, you can do this by installing the
packages libx11-dev, libxrender-dev, and libxft-dev. You must also
have the chicken package installed.

Installing mowedline
--------------------

The preferred and easiest way to install chicken is to use
`chicken-install`.

    chicken-install -s mowedline

The `-s` switch tells chicken-install to use sudo to get root
privileges. This is preferable to using `sudo chicken-install
mowedline`.

Installing mowedline from source
--------------------------------

If you want to build and/or install mowedline from source you should
first install all its dependencies.  Some of these might already be
installed.  You can check with `chicken-status`.

 * `chicken-install coops`
 * `chicken-install coops-utils`
 * `chicken-install dbus`
 * `chicken-install filepath`
 * `chicken-install imperative-command-line-a`
 * `chicken-install list-utils`
 * `chicken-install mailbox`
 * `chicken-install matchable`
 * `chicken-install miscmacros`
 * `chicken-install xft`
 * `chicken-install xlib`
 * `chicken-install xlib-utils`
 * `chicken-install xtypes`

The most up-to-date list of dependencies can always be found in the
`mowedline.meta` file in the source code repository.

### Install mowedline

 * change directory to where you keep source code, e.g. `cd ~/src`
 * `git clone git://github.com/retroj/mowedline.git`
 * `cd mowedline`
 * `chicken-install -s` if you want to build and install,
   `chicken-install -n` if you only want to build.
 * If you didn't install, you now have mowedline and mowedline-client
   binaries that you can symlink into a $PATH directory (like ~/bin/
   or /usr/local/bin).
