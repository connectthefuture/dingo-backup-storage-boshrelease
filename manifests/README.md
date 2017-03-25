# Dingo Backup Storage deployment

```
bosh2 deploy manifests/dingo-backup-storage.yml --vars-store=tmp/creds.yml
```

Next, run the `sanity-test` errand to confirm that the SSH user is successfully setup.

```
bosh2 run-errand sanity-test
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

## Store & retrieve creds from Vault

Instead of generating credentials into a plain text local `tmp/creds.yml`, you might want secrets hidden inside Hashicorp Vault.

**Note:** This section requires the `safe` and `spruce` CLI tools installed locally.

Generate the required credentials into Vault using `safe`:

```
export VAULT_PREFIX=secret/dingo-backup-storage/demo
safe ssh $VAULT_PREFIX/ssh_user
```

This will generate random secrets directly into Vault. To sanity check that they are there:

```
safe get $VAULT_PREFIX/ssh_user
```

You can fetch these into a useable YAML file via `spruce merge`:

```
spruce merge manifests/spruce-vault-creds.yml
```

This allows you to pass these secrets directly to the `bosh2` command with the `-l` flag:

```
bosh2 deploy manifests/dingo-backup-storage.yml \
  -l <(spruce merge manifests/spruce-vault-creds.yml)
```
