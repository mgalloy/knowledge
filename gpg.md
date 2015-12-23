# GPG

A handy [cheatsheet](http://irtfweb.ifa.hawaii.edu/~lockhart/gpg/gpg-cs.html).

My old
[key](http://pgp.mit.edu:11371/pks/lookup?op=get&search=0xCCDFA6FF3AEFC393)
from RSI is still available at the
[MIT key server](http://pgp.mit.edu:11371/).

## Export a public key to a file

For my key at CSDMS:

	$ gpg --export -a "Mark Piper (CSDMS)" > mp-csdms-public.key

## Import a public key from a file

I used this to import my public key into my keyring on ***river***.

	$ gpg --import mp-csdms-public.key

## List keys

List all the keys on my keyring:

	$ gpg --list-keys

List only keys belonging to a user:

	$ gpg --list-keys "Mark Piper (CSDMS)"

