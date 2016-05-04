# Notes on using Emacs

Gained from years of experience!

## Package management

I'm trying *el-get* for emacs package management.

[https://github.com/dimitri/el-get](https://github.com/dimitri/el-get)

See this page for commands. The packages are installed in **~/emacs.d/**.

I also looked at
[MELPA](http://ergoemacs.org/emacs/emacs_package_system.html), but I
think I like *el-get* better.

I've installed:

* `auto-complete`
* `popup`
* `markdown-mode`
* `exec-path-from-shell` (execute `M-x exec-path-from-shell-copy-env PATH`)
* `python-environment`
* `jedi`
* `yaml-mode`
* `cmake-mode`
* `json-mode`
* `git-commit-mode`

## Meta key

Sometimes the meta key `M` isn't mapped properly.
Usually, you can fall back on using the `Esc` key instead.

## Commenting code

To comment a highlighted region, use:

	M-x comment-region

Uncomment with:

	M-x uncomment-region

Works in Python. 

Better -- this also works for comment and uncomment:

	M-;

## Moving between open buffers

Cycle through open buffers with:

    C-x [left or right arrow key]

Select an open buffer with:

    C-x b

## Using multiple windows

Make a pair of windows, side-by-side, with:

    C-x 3

Switch between these windows with:

    C-x o

## Goto location in file

Go to the beginning of a file:

    M-<

the end:

    M->

and a particular line number:

    M-g g <number>

## Autocomplete local variables

Autocomplete local Python variables with:

	M-\

## Refresh file in buffer

If a file in a buffer has changed on disk,
refresh it with

	M-x revert-buffer

(I've mapped this to a keybinding, `C-c r`)

Alternately, use

    C-x C-v RET

## Untabify a file

Use

    M-x untabify

Note that a tab can always be forced with `C-q TAB`.

## Apply .emacs after editing

Use

    M-x eval-buffer

which evaluates the code in a buffer.

## Access menubar in console mode

Starting emacs with `-nw` displays the menubar in a terminal.
Access this menu with:

	M-x menu-bar-open

On a Mac,
this sequence is bound to `Fn-F10`
(tested on laptop and desktop).

## Formatting JSON

Highlight region and apply:

	M-x json-reformat-region

## JavaScript mode

Start JavaScript mode with:

	M-x js-mode

This is also helpful for working with JSON (indenting, no validation).
However,
I also installed `json-mode`,
which should start automatically when a JSON file is loaded.

## Python IDE

I'd like to use emacs as my Python IDE. Use this page:

[http://www.jesshamrick.com/2012/09/18/emacs-as-a-python-ide/](http://www.jesshamrick.com/2012/09/18/emacs-as-a-python-ide/)

for ideas, but use *el-get* for package management.

## Colors

Emacs has its own color palette.
I like "AntiqueWhite1" in the foreground
and "Gray5" in the background.
