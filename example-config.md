Example Config
==============

0.2pre
------

Here is my current config for mowedline 0.2pre.  Six widgets in all,
'workspaces', 'wmflags', and 'title' are all updated by my xmonad, via
three `mowedline -read ...` pipes.  The next two, 'email' and 'irc', are
updated by using `mowedline update ...` in my email notification script
and a hook in my irc client.  Last is a clock, which is updated
internally.  The clock shows a good example of how to colorize text in
mowedline, and the workspaces widgets shows how to buttonize text.

    ;; -*- scheme -*-

    (define (string-maybe-pad-left text)
      (if (string-null? text)
          text
          (string-append " " text)))

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
                `(button (lambda () (switch-to-desktop ,ws))
                         (color (1 0 0 1) ,ws))))
             (else
              `(button (lambda () (switch-to-desktop ,token))
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
          (m (set! (third m) `(color (.73 .73 .86 1) ,(third m)))
             (cdr m))
          (else text)))
       " "))

    (text-widget-font "DejaVu Sans Mono-10:bold")
    (text-widget-color '(.6 .6 .6 1))
    (text-widget-format string-maybe-pad-left)

    (make <window>
      'widgets
      (L (make <text-widget>
           'name "workspaces"
           'format buttonize-workspaces)
         (make <flags>
           'name "wmflags"
           'color '(.53 .53 .53 1)
           'flags '(("tabbed" . (color "paleturquoise3" "T"))
                    ("float" . (color "firebrick3" "f"))
                    ("inverse" . (color "cornflowerblue" "v"))
                    ("magnifier" . (color "forestgreen" "m")))
           'format (lambda (markup) (append '(" (") markup '(")"))))
         (make <text-widget>
           'name "title"
           'color '(.4 .4 .4 1)
           'flex 1)
         (make <text-widget>
           'name "email")
         (make <text-widget>
           'name "irc"
           'format (lambda (text) (string-maybe-pad-left (string-trim-both text))))
         (make <clock>
           'format colorize-clock)))