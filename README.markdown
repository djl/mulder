mulder
------

mulder snapshots, encrypts and uploads your files to S3.



USAGE
-----

List all backups with `mulder`:

    $ mulder
    documents
    pictures
    work


Run a backup with `mulder <backup>`:

    $ mulder documents



SETUP
-----

Mulder reads from an INI-style config file called `~/.mulder`. Each
section of this file corresponds to a single backup.

### AWS credentials

AWS access and secret keys are stored in an `auth` section. The
settings are not so cryptically called `access_key` and `secret_key`.


### Backup settings

* `bucket`

   The S3 bucket to upload to.

* `files`

   A list of comma-separated files and directories to be backed
   up. Each item in this set will be included in the final gzipped
   tarball.

* `name`

   The name of the archive to be created. Treated as an `strftime(3)` string.

* `exclude` (optional)

   A list of files to exclude from a backup.

* `gpg` (optional)

   Encrypt snapshots with your default key.



EXAMPLE
-------

    [auth]
    access_key = ACCESSKEY
    secret_key = SECRETKEY
    # alternative version with GNOME Keyring
    # access_key_eval = gnome-keyring-query get aws_access_key
    # secret_key_eval = gnome-keyring-query get aws_secret_key

    [documents]
    name = "documents-%Y-%m-%d"
    files = /home/bob/Documents
    bucket = bob-documents
    gpg = true

    [var]
    name = "var-%Y-%m-%d"
    files = /home/bob/var
    exclude = /home/bob/var/tmp
    bucket = bob-var

    [misc]
    name = "var-%Y-%m-%d"
    files = /home/bob/misc, /home/bob/this, /home/bob/that
    bucket = bob-misc


Each setting has a corresponding `_eval` setting which can be used to
call an external script.

If, for example, you kept your credentials in GNOME Keyring, you may
wish to do something like this:

    [auth]
    access_key_eval = gnome-keyring-query get aws_access_key
    secret_key_eval = gnome-keyring-query get aws_secret_key



REQUIREMENTS
------------

* [boto](http://boto.cloudhackers.com/)
* [progressbar](http://pypi.python.org/pypi/progressbar/) (optional, but it's pretty)



TODO
----

* Refactor. Damn this thing is ugly.
* Add support for backup-specific `access_key` and `secret_key` options
