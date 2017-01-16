# Simple VM for storing backups from Dingo PostgreSQL

Currently Dingo PostgreSQL supports streaming continuous archives to Amazon S3. Some users are not yet allowed to use Amazon S3 and want a way to store backups within their current data center. This BOSH release is one option.

It deploys a single VM with a known user created, with a known private key, thus allowing `rsync` to copy/read files to it. Dingo PostgreSQL will subsequently be upgraded to support `rsync` as a method of pushing backups & WAL segments.

### Private key

To create a private key for the deployment manifest:

```
ssh-keygen -t rsa -N '' -f tmp/dingo-backup-storage
```

The output to the terminal may look similar to:

```
Generating public/private rsa key pair.
Your identification has been saved in tmp/dingo-backup-storage.
Your public key has been saved in tmp/dingo-backup-storage.pub.
The key fingerprint is:
SHA256:Whv1uvf2GnkxHbvsdKYgOk0ty5JYn9sOrbIOafU5RJg drnic@starkair.local
The key's randomart image is:
+---[RSA 2048]----+
|                 |
|         o       |
|        E o    . |
|         o .    +|
|        S ...  +.|
|       =.=o+. ..+|
|      =o.*B+o o++|
|     ...=.*B..+=.|
|       .+=++oo++.|
+----[SHA256]-----+
```

Two new files will be created within the `tmp` directory. The `tmp/dingo-backup-storage` file contains the private key.

This file will be picked up and inserted into the deployment manifest.

```
./templates/make_manifest warden
```
