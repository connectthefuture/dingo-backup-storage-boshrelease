# Simple VM for storing backups from Dingo PostgreSQL

Currently Dingo PostgreSQL supports streaming continuous archives to Amazon S3. Some users are not yet allowed to use Amazon S3 and want a way to store backups within their current data center. This BOSH release is one option.

It deploys a single VM with a known user created, with a known private key, thus allowing `rsync` to copy/read files to it. Dingo PostgreSQL will subsequently be upgraded to support `rsync` as a method of pushing backups & WAL segments.


## Deployment

This BOSH release includes a very easy to use [`manifests/dingo-backup-storage.yml`](manifests/dingo-backup-storage.yml) manifest.

See [`manifests/README.md`](manifests/README.md) for extra instructions.
