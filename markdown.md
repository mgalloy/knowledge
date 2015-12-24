# Markdown

I'm using it here!

## Origins

John Gruber (Daring Fireball) came up with the
[original spec](http://daringfireball.net/projects/markdown/), but he
didn't do anything with it.  It's now becoming super-popular; it's
used in IPython Notebook and Github.

## Documentation

* [Markdown Basics](http://markdown-guide.readthedocs.org/en/latest/basics.html)
[readthedocs.org]
* A [cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet),
of sorts
[github.com]

## Viewing

On Chrome,
I've tried a series of extentions for viewing **.md** files:

* [Markdown Preview](https://chrome.google.com/webstore/detail/markdown-preview/jmchmkecamhbiokiopfpnfgbidieafmd)
* [Markdown Reader](https://chrome.google.com/webstore/detail/gpoigdifkoadgajcincpilkjmejcaanc)
* [Markdown Preview Plus](https://chrome.google.com/webstore/detail/febilkbfcbhebfnokafefeacimjdckgl) (yeah, it's different)

They all work fairly well.
Be sure to check the preference "Allow access to file URLs".

Alternately,
convert markdown to PDF or HTML with
[pandoc](http://johnmacfarlane.net/pandoc/),
which I've installed on ***solaria***.
For example,

	$ pandoc deb.md --from=markdown -t latex -o deb.pdf

## Text attributes

Because
<span style="font-style:oblique">I</span>
<span style="font-weight:bold">keep</span>
<span style="color:orange; font-variant:small-caps">forgetting</span>
how to do this.
See the
[CSS font property](https://developer.mozilla.org/en-US/docs/Web/CSS/font)
for values that can be set.

## Line breaks

Placing two empty spaces at the end of a line  
will force a line break (not a paragraph).

