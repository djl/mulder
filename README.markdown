mulder
------

mulder snapshots, encrypts and uploads your files to S3.

Note: mulder is still in its awkward phase. There are a places where
it doesn't work quite right and there are places it blows up in a rather
spectacular fashion.



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

This section also supports two special settings: `access_key_eval` and
`secret_key_eval`. These settings can be used to specify a shell
command which returns the relevant values.

If, for example, you kept your credentials in GNOME Keyring, you may
wish to do something like this:

    [auth]
    access_key_eval = gnome-keyring-query get aws_access_key
    secret_key_eval = gnome-keyring-query get aws_secret_key


### Backup settings

* `bucket`
   The S3 bucket to upload to.

* `files`
   A list of comma-separated set of files and directories to be backed up.

* `name`
   The name of the archive to be created. Treated as an `strftime(3)` string.

* `exclude` (optional)
   A list of files to exclude from a backup.

* `gpg` (optional) 

   Encrypt snapshots with your default key



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



TODO
----

* Better error handling
* Validation of required arguments (name, files, bucket)
* Refactor. Damn this thing is ugly.
