Troubleshooting
===============

If you are having trouble with Mowedline and the solution isn't
described here, please feel free to either [report an issue] or join
us all in the #mowedline IRC channel on irc.freenode.net.

Following is a list of known problems and workarounds or fixes.

[report an issue]: https://github.com/retroj/mowedline/issues


Error: (assq) bad argument type: #f
-----------------------------------

If you are getting this error when running the mowedline program this
may be due to an incompatibility between the dbus egg (version 0.93)
and version 4.9.0.1 of chicken.  This has to do with the `assq`
function allowing a parameter of #f in previous versions of chicken.

The latest development version of dbus includes a fix for this issue.
To install it you need svn.  Once you have that installed you can
execute the following commands to get and install the latest
development version of the dbus egg:

    svn checkout --username anonymous --password '' \
      https://code.call-cc.org/svn/chicken-eggs/release/4/dbus
    cd dbus/trunk
    chicken-install -s

If all goes well mowedline should start now.


