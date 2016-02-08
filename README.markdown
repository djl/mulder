mulder
------

A simple script to create a GPG-encrypted tarball.


USAGE
-----

List configs with `mulder`:

    $ mulder
    documents
    pictures
    work


Run a backup with `mulder <backup>`

    $ mulder documents


SETUP
-----

Configs are just directories kept in `~/.config/mulder`. Each directory
needs to contain at a file called `config`. This is a regular shell
script which will be sourced at run time. This file must contain the
following variables:

* `NAME`

  The name of GPG-encrypted tarball to be created. `mulder` will not
  append any extensions to this file (e.g `.tar.gz.gpg`) so you might
  want to do this yourself.

* `FILES`

  An array of files to be excluded in the archive.

* `EXCLUDE` (optional)

  An array of files or directories to be excluded from the archive.

`mulder` also supports pre- and post-tarball actions. Just add to
executables in your config directory called `pre` and `post`.

These are called immediately before and after the GPG-encrypted
archive is created. They will be given the path to the archive as
their first argument.

See `examples` for example configurations.
