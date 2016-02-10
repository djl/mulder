tarcrypt
--------

A simple script to create GPG-encrypted tarballs.


USAGE
-----

List configs with `tarcrypt`:

    $ tarcrypt
    documents
    pictures
    work


Run a backup with `tarcrypt <backup>`

    $ tarcrypt documents


SETUP
-----

Configs are just directories kept in `~/.config/tarcrypt`. Each
directory must contain a file called `config`. This is a regular shell
script which will be sourced at run time. This file must contain the
following variables:

* `NAME`

  The name of GPG-encrypted tarball to be created. `tarcrypt` will not
  append any extensions to this file (e.g `.tar.gz.gpg`) so you might
  want to do this yourself.

* `FILES`

  An array of files to be excluded in the archive.

* `EXCLUDE` (optional)

  An array of files or directories to be excluded from the archive.


HOOKS
-----

`tarcrypt` supports running custom actions at a few points. Just place
the following files in your config directories (i.e next to your
`config` file) and make them executable:


* `pre`

  Will be run just before the GPG-encrypted archive is created.

* `post`

  Will be run just after the GPG-encrypted is created.


All custom hooks will be passed a single argument - the path to the
newly created (or soon to be created) archive file.


EXAMPLES
--------

See `examples` for example configurations.
