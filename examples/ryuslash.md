ryuslash's Config
=================

0.2.9
-----

Here is ryuslash's dot-mowedline, for mowedline 0.2.9-dev.  Its basic
structure is as follows:

- The `not-text?` function is used by formatting functions to
  determine if we're dealing with a string or not.

- The `text-maybe-pad-both` function is a lot like mowedline's
  `text-maybe-pad-left`, except that it pads both sides if any content
  is present.

- The `add-fa-icon` function produces a formatting function that
  prepends the given [Font Awesome](http://fontawesome.io/) icon to
  the text if the text isn't empty.  The icon is just a character
  which will be shown as an icon by the font.

- The `tag-list-formatter` function is fed information from my window
  manager, [herbstluftwm](http://herbstluftwm.org/), which looks like
  this:

         #1	-2	:3	:4	:5	.6	.7	.8	.9

   Where each number is the name of a tag.  The meanings of the
   symbols is as following:

   - `#` means that I'm currently using that tag
   - `-` means that I can see the tag on another monitor
   - `:` means that I don't currently see the tag, but I have one or
     more windows visible on it.
   - `.` means that I don't currently see the tag and it's empty.
   - `!` means that the tag has an urgent window on it.

   The `split-tag-list` function splits the string up at each tab
   character and splits each resulting string into a list of
   characters.  The `tag-list-display` function just maps the
   `tag-display` function over the list.  The `tag-display` function
   uses `matchable` to determine the Font Awesome icon to use and
   color to show the icon in.

I prefer my mowedline to be completely transparent, so I set the
window's background to `transparent`.  I run the
[compton compositor](https://github.com/chjj/compton) to get real
transparency in both X windows and mowedline.  My IRC client updates
the irclist widget, which uses `identity` as formatter, because I
don't want to change anything about its formatting and my default
formatter is `text-maybe-pad-both`, it just happens that my IRC client
already pads its output with a single space on both sides.  My
taglist, email and keychain widgets are all updated by external
scripts and the active-window-title and clock widgets are filled by
mowedline automatically.

Source:

    (use srfi-1 matchable)

    (define (not-text? text)
      (or (null? text) (and (not (pair? text)) (string-null? text))))

    (define (text-maybe-pad-both text)
      (if (not-text? text)
          text
          (L " " text " ")))

    (define (add-fa-icon icon)
      (lambda (text)
        (if (not-text? text)
            text
            (list (list 'font "FontAwesome-10"
                        (string-append " " icon " "))
                  text))))

    (define (split-tag-list str)
      (map string->list (string-split str "\t")))

    (define tag-display
      (match-lambda
        ((#\# n) (L (L 'color "#ececec" 'font "FontAwesome-10" "") " "))
        ((#\- n) (L (L 'color "#bfbfbf" 'font "FontAwesome-10" "") " "))
        ((#\. n) (L (L 'color "#969696" 'font "FontAwesome-10" "") " "))
        ((#\: n) (L (L 'color "#969696" 'font "FontAwesome-10" "") " "))
        ((#\! n) (L (L 'color "#a85454" 'font "FontAwesome-10" "") " "))))

    (define (tag-list-display tag-list)
      (map tag-display tag-list))

    (define (tag-list-formatter text)
      (let ((tag-list (split-tag-list text)))
        (tag-list-display tag-list)))

    (text-widget-font "Fantasque Sans Mono-13:bold")
    (text-widget-color "#ededed")
    (text-widget-format text-maybe-pad-both)

    (window
     'position 'bottom
     'width 1890
     'margin-bottom 15
     'margin-left 15
     'margin-right 15
     'background 'transparent
     (widget:text 'name "taglist"
                  'format (compose string-maybe-pad-left tag-list-formatter))
     (widget:spacer 'width 5)
     (widget:active-window-title)
     (widget:spacer 'flex 1)
     (widget:text 'name "irclist" 'format identity)
     (widget:spacer 'width 5)
     (widget:text 'name "email"
                  'format (compose text-maybe-pad-both (add-fa-icon "")))
     (widget:spacer 'width 5)
     (widget:flags 'name "keychain"
                   'font "FontAwesome-10"
                   'flags '(("Unlocked" . "")
                            ("Locked" . "")))
     (widget:spacer 'width 5)
     (widget:clock))
