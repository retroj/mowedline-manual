
Mowedline Installation
======================

The installation procedure for mowedline is as yet a bit complicated,
mainly because of two chicken egg dependencies that I have not yet
finished packaging for availability through chicken-install.

Install chicken.

Install the development headers for X11, XRender, and Xft.  On Debian, the
packages are libx11-dev, libxrender-dev, and libxft-dev.

Install chicken packages that are depended upon by mowedline.  Some of
these might already be installed.  You can check with `chicken-status`.

 * `chicken-install coops`
 * `chicken-install data-structures`
 * `chicken-install dbus`
 * `chicken-install environments`
 * `chicken-install filepath`
 * `chicken-install list-utils`
 * `chicken-install lolevel`
 * `chicken-install miscmacros`
 * `chicken-install posix`
 * `chicken-install xlib`

Install xtypes-egg.

 * change directory to where you keep source code, e.g. `cd ~/src`
 * `git clone http://retroj.net/git/xtypes-egg/.git/`
 * `cd xtypes-egg`
 * `chicken-install -n`
 * if there were no errors, proceed with install of xtypes-egg:
 * `chicken-install -s` (The -s flag means to use `sudo` to copy files to
   system locations.  If you cannot use sudo, refer to the documentation
   for chicken-install for other methods.)

Install xft-egg

 * change directory to where you keep source code, e.g. `cd ~/src`
 * `git clone http://retroj.net/git/xft-egg/.git/`
 * `cd xft-egg`
 * `chicken-install -n`
 * if there were no errors, proceed with install of xtypes-egg:
 * `chicken-install -s` (The -s flag means to use `sudo` to copy files to
   system locations.  If you cannot use sudo, refer to the documentation
   for chicken-install for other methods.)

Clone mowedline

 * change directory to where you keep source code, e.g. `cd ~/src`
 * `git clone http://retroj.net/git/mowedline/.git/`
 * `cd mowedline`
 * `csc mowedline.scm`
 * you now have a mowedline binary that you can symlink into a $PATH
   directory (like ~/bin/ or /usr/local/bin).
