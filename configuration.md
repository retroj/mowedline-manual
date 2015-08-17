
Configuration
=============

This document pertains to mowedline 0.2pre1.

Mowedline is configured by creating either a `~/.mowedline` or
`~/.config/mowedline/init.scm` file, or passing a file along with the
`-config` command. The documentation will sometimes refer to this file
as your ".mowedline", and when it does, it refers to either one. Some
configuration can also be done by passing commands to the `mowedline`
program.

Instead of using a static configuration format and being constrained by
the implicit limitations thereof, a mowedline configuration is written in
scheme (CHICKEN Scheme to be precise).  Your .mowedline is a scheme
program, allowing you to leverage the full power of the scheme programming
language to configure, enhance, and extend mowedline in novel and creative
ways.

The .mowedline file is loaded by the mowedline daemon at startup.  Its
primary responsibility is to create windows and populate them with
widgets.  If your .mowedline does not create a window, a default window
with one widget will be created.  Let's get right into an example.

    (window
     (widgets:text
      'name "default"
      'flex 1)))

Windows and widgets are instantiated with their specific procedure
such as `window` and `widgets:text` in the example. The arguments to
each function depend on what you're creating. In the case of `window`
there can first be pairs of property names and values, followed by any
widgets you would like to add.

Every widget must have a unique name.  The name is how widgets are
referred to in the command-line interface.

The `flex` property is explained below.

Mowedline provides a convenient alias `L` for `list`.

For more complex, real-world examples, see
[example-config](/mowedline/example-config).  For all the fascinating
details of the configuration API, read on...


Widget Types
------------

There is a small selection of built-in widget types that you can use.  We
expect to write more widget types over time, as we the users invent new
ideas.

### Common properties

These options can be used on any type of widget that appears in your
configuration.

 * background-color

    The background color of the widget.  See the explaination under
    'color' below for the allowed values of this property.  The
    default is #f, meaning it should use the window's background
    color.

 * flex

    The `flex` property makes a widget's width rubber.  When a window is
    drawn on screen, the available width is divvied up among the widgets.
    Normally, each widget is allocated exactly enough space to fit its
    contents.  A widget with flex, however, is allocated a portion of the
    _leftover_ space, after allocating space for all other widgets.  The
    flex values of all widgets are added up, and the remaining space is
    divided among them in proportion to each widget's flex value.  In our
    example, we have one widget with a flex value of 1.  The total flex is
    1, and our widget receives 1/1th of the space, or 100%.  If we add
    another widget and give it flex 2, that makes a total flex of 3, our
    first widget get's 1/3, and the new widget gets 2/3.

 * name

    Every widget must have a unique name.


### widgets:text

 * color

    The color in which to render the text.  This can be given as a string
    color name, a string hex specifier, a three element (R G B) list or a
    four element list (R G B A).  A string hex specifier is the familiar 6
    digit hexidecimal notation as used in HTML, \#000000 means black and
    \#ffffff means white.  The list forms are slightly more efficient,
    since they don't require the X server to be queried.  The numbers
    given in the list are on a scale of 0 to 1: (0 0 0 1) means black, and
    (1 1 1 1) means white.  The default is white.

 * font

    An Xft-style font name.  Use the program `fc-list` to see what fonts
    are installed on your computer.  Specify size by appending a hyphen
    followed by a number.  Specify other attributes by appending a colon
    and an attribute specifier.  See section 1.1
    [here](http://keithp.com/~keithp/render/Xft.tutorial) for more
    information on how to format font names.  Mowedline's default font is
    "mono-10:bold".

 * format

    A procedure of one argument that receives the raw text and which can
    return either a string or a list-structured markup definition.  Markup
    definitions can be used to colorize and buttonize text.  The markup
    language is very simple.  It is just a nested list that contains
    strings and markup elements.  Markup elements may take positional
    arguments.  The following elements are available:

     * (color \<color> ...)

        The argument is a color specification.  Text inside the color
        element will be colored with that color.

           '("a" (color "red" "b") "c")

     * (button \<thunk> ...)

        Thunk is a procedure of no arguments that will be called when the
        text inside the button is clicked.  In the following example,
        clicking on the "b" sends an update to the widget, "foo".

           '("a"
             (button (lambda () (update "foo" "bar"))
                     "b")
             "c")

     * (font \<font> ...)

        The argument is a font specification.  Text inside the font
        element will be rendered in that font.

           '("a" (font "Coffee Script-12" "b") "c")


 * text

    The text to display in the widget.  This is the value that gets set
    when you run `mowedline -update awidget 'some text'`, after possible
    processing by a format function.


### widgets:clock

All widgets:text properties can be used with a clock.

 * time-format

    A string giving the time->string format for the clock.


### widgets:flags

The flags widget is derived from the text widget, and serves the
purpose of displaying a set of flags.  The flags are pre-defined via
the 'flags slot and upon update, input is parsed and interpreted to
turn on, turn off, or clear and replace the set of currently displayed
flags.

Input is parsed as follows.  If the input begins with '+', then the
following flagnames are turned on.  If the input begins with '-', then the
following flagnames are turned off.  Otherwise, the given flagnames
replace the current flag set.  Flagnames are given in a simple sequence,
separated by spaces.

 * flags

    An alist giving the associations between flag names and display forms.
    For example:

       '(("foo" . (color "red" "f"))
         ("bar" . (color "blue" "b")))

Note: the `text` property of a flags widget will _always_ be a markup
structure, never a string, so format procedures should be adjusted
accordingly.



Window Properties
-----------------

 * height

    Window height in pixels.  If not given, defaults to the height of the
    tallest widget in the window.

 * position

    A symbol giving the position of the mowedline window on the screen.
    Supported values are `top` and `bottom`.

 * width

    Window width in pixels.  If not given, defaults to the width of the
    display.

 * margin-bottom, margin-left, margin-right, margin-top

    How far, in pixels, from the bottom, left, right or top edge of
    the screen the window should be placed. If not given, all default
    to 0.

 * background

    The background color of the window. Can be either of the values
    `inherit`, `transparent` or #f. The value `inherit` makes the
    background transparent for displays without a compositor by
    copying the root window's background. The value `transparent`
    makes the background transparent for displays _with_ a compositor.

    Note: If you use `inherit` with a compositor, repaint doesn't work
    right. If you use `transparent` without a compositor, you'll just
    get a black background.

Setting Defaults
----------------

The default values of many of the properties listed above can be changed
by a group of parameter procedures.  The following procedures are
available for configuring defaults.

 * text-widget-font
 * text-widget-color
 * text-widget-format
 * widget-background-color
 * widget-flex
 * window-position

Use these procedures in your .mowedline _before_ instantiating the
widgets which are to be affected.


### Examples

Change the default font for all text-widgets:

    (text-widget-font "DejaVu Sans Mono-10:bold")

Change the default text color of all text-widgets:

    (text-widget-color "#999999")
