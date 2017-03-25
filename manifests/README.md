# Dingo Backup Storage deployment

```
bosh2 deploy manifests/dingo-backup-storage.yml --vars-store=tmp/creds.yml
```

## Indirect connection

Other deployments, such as Dingo PostgreSQL, can look up the SSH username (`dingo`) and private key via cross-deployment links.

```
instance_groups:
- name: router
  jobs:
  - name: broker
    consumes:
    - name: backup
      deployment: dingo-backup-storage
```

## Direct connection

The ssh user is `dingo`.

The generated private key can be found in the `--vars-store` file:

```
bosh2 int tmp/creds.yml --path /ssh/private_key
```
