
General Integration
===================

Mowedline's primary interface for updating the window contents is over the
DBus session bus.  The mowedline-client program is nothing more than a
small DBus client that provides a nice [command-line
syntax](/mowedline/command-line) for communicating with the mowedline
server.  You can in fact use any general DBus client to update mowedline
and different ones may be better suited than others for integration with
particular software.

You can for example update mowedline with the `dbus-send` utility that
comes with DBus:

    $ mowedline -q &
    $ dbus-send --session --type=method_call \
        --dest=mowedline.server / mowedline.interface.update \
        string:default \
        string:"hello, world!"
    $ dbus-send --session --type=method_call \
        --dest=mowedline.server / mowedline.interface.quit

Communicating with mowedline is done via DBus method-calls on the session
bus.  The other information that you have to provide is as follows:

 - **service:** mowedline.server
 - **interface:** mowedline.interface
 - **path:** /

The `update` method is the main method in the DBus interface for
interacting with mowedline.  It takes two arguments: a widget name and
text to write to the widget.  Some widget types take specially formatted
text to work their magic.

Integrating mowedline with other software in the general case, involves
nothing more complicated than arranging for that software to call
mowedline-client, dbus-send, or any other method of performing a DBus
method call that the software may provide.

Methods for specific programs will be covered in other articles in this
section.  Feel free to contact us with your own methods of integrating
mowedline with other programs.
