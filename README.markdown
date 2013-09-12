mulder
------

mulder snapshots, encrypts and uploads your files somewhere.



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


### Backup settings

* `name`

   The name of the archive to be created. Treated as an `strftime(3)` string.

* `files`

   A list of comma-separated files and directories to be backed
   up. Each item in this set will be included in the final gzipped
   tarball.

* `destination`

   Where the new archive should be placed. This must be preceded by
   the relevant scheme - either `s3://` for Amazon S3 or `rsync://`
   for a remote server.

   Multiple destinations are delimited by commas.

* `exclude` (optional)

   A comma-separated list of files to exclude from a backup.

* `gpg` (optional)

   Encrypt snapshots with your default key.

* `access_key` and `secret_key` - (Required only for S3 uploads)

   The AWS access and secret keys.


Each setting has a corresponding `_eval` setting which can be used to
call an external script. See below for an example.

Global options can also be stored in a section called `DEFAULT`. Any
options stored here will be used if they are not present in an
individual backup.



EXAMPLE
-------

    [DEFAULT]
    access_key = ACCESSKEY
    secret_key = SECRETKEY


    [documents]
    name = documents-%Y-%m-%d
    files = /home/bob/Documents
    destination = s3://bob-documents
    gpg = true

    # override the global settings for this backup
    access_key_eval = gnome-keyring-query get aws_access_key_documents
    secret_key_eval = gnome-keyring-query get aws_secret_key_documents

    [var]
    name = var-%Y-%m-%d
    files = /home/bob/var
    exclude = /home/bob/var/tmp
    destination = s3://bob-var,
                  rsync://bob@host:/path/to/somewhere

    [misc]
    name = misc-%Y-%m-%d
    files = /home/bob/misc,
            /home/bob/this,
            /home/bob/that
    destination = rsync:///home/bob/backups # a local directory



REQUIREMENTS
------------

* S3 support requires [botocore](https://pypi.python.org/pypi/botocore)
