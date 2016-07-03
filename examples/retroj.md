
retroj's Config
===============


2.0.0
-----

Here is a slightly simplified version of retroj's dot-mowedline, for
mowedline 2.0.0.  Its basic structure is as follows:

  - CHICKEN extensions can be imported with `use`.  We use coops and
    regex.

  - `buttonize-workspaces` and `colorize-clock` are two formatting
    procedures.  Buttonize-workspaces is an example of how to take a
    pre-formatted desktop list from a window manager and reparse it to add
    buttons and colors.  It is specific to my particular window manager
    configuration, so I give it only as an example.  Colorize-clock on the
    other hand can be used as-is.  It highlights the time in
    `widget:clock`'s output.

  - Set some defaults: `text-widget-font`, `text-widget-color`,
    `text-widget-format`.  Text-widget-format is set to
    `text-maybe-pad-left`, a procedure provided by mowedline that adds
    extra space to the left side of any non-empty text widget.

  - Declare the window.  The two widget:text and the widget:flags widgets
    receive their content via mowedline-client from my window manager and
    my irc client.  The other two, widget:active-window-title and
    widget:clock are automatically updated internally by mowedline.

Source:

    ;; -*- scheme -*-

    (use coops regex)

    (define (buttonize-workspaces text)
      (cons
       (if (string-prefix? "[" text) "" " ")
       (fold
        (lambda (token markup)
          (cons
           " "
           (cons
            (cond
             ((string-prefix? "[" token) ;; current workspace
              token)
             ((string-prefix? "!" token) ;; urgency notification
              (let ((ws (string-drop token 1)))
                `(button ,(lambda _ (switch-to-desktop ws))
                         (color (1 0 0 1) ,ws))))
             (else
              `(button ,(lambda _ (switch-to-desktop token))
                       ,token)))
            markup)))
        '()
        (reverse (string-tokenize text)))))

    (define (colorize-clock text)
      (list
       " "
       (let ((m (string-match
                 '(: (submatch (* any))
                     (submatch numeric numeric ":" numeric numeric)
                     (submatch (* any)))
                 text)))
         (cond
          (m (set! (third m)
                   `(color (.73 .73 .86 1)
                           (font "Coffee Script-11" " " ,(third m))))
             (cdr m))
          (else text)))
       " "))

    (window-background 'transparent)

    (text-widget-font "Inconsolata-11:bold")
    (text-widget-color '(.6 .6 .6 1))
    (text-widget-format text-maybe-pad-left)

    (widget-background-color #f)

    (window
     (widget:text
      name: "workspaces"
      format: buttonize-workspaces)
     (widget:flags
      name: "wmflags"
      color: '(.53 .53 .53 1)
      flags: '(("tabbed" . (color "paleturquoise3" "T"))
               ("float" . (color "firebrick3" "f"))
               ("inverse" . (color "cornflowerblue" "v"))
               ("magnifier" . (color "forestgreen" "m")))
      format: (lambda (markup) (append '(" (") markup '(")"))))
     (widget:spacer)
     (widget:active-window-title
      color: '(.4 .4 .4 1)
      flex: 1)
     (widget:map
      name: "email"
      format-pair: (lambda (k v)
                     (if v
                         (string-append (substring (->string k) 0 1)
                                        (->string v))
                         #f))
      format: (lambda (markup) (list "[" markup "]")))
     (widget:text
      name: "irc"
      format: (lambda (text) (text-maybe-pad-left (with-input-from-string text read))))
     (widget:clock
      format: colorize-clock))
