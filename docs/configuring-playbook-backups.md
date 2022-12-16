# Backups

The backup and restore functionality is still not very advanced.


## Enabling backups

To enable Gitea backups you'll need this additional `vars.yml` configuration:

```yaml
gitea_backup_enabled: true

# Backups are gpg-encrypted.
#
# You can encrypt the password, so that it's not visible in your vars.yml file in clear text.
# Use the `ansible-vault encrypt_string` tool and the value provided by it.
# If you start using encrypted values in your vars.yml file (such as an encrypted password here),
# you'll need to pass `--ask-vault-pass` to all `ansible-playbook` commands that you run in the future.
gitea_backup_encryption_password: SOME_PASSWORD
```

**Warning**: a database dump will be included in the backup file **only** if you're using the integrated Postgres server provided with this playbook. If you're [using another database](configuring-playbook-database.md) it will **not** be backed up.

The backup system is activated every day at 06:30 (according to the `gitea_backup_schedule` variable).

To guarantee a consistent backup, Gitea will be temporarily stopped while the backup process is running.
This behavior can be changed by tweaking the `gitea_backup_stop_gitea_while_running` variable.

The backup system will generate a `/gitea/backup/gitea-latest-backup.tar.gpg` file.

You can pull it from another remote system or potentially push it elsewhere.


### Custom shell commands after backup creation

You can use the `gitea_backup_post_backup_custom_shell_commands` variable to set custom shell commands that will be executed after a backup is created.

Take a look at `roles/custom/gitea_backup/defaults/main.yml` for usage information.


### Pushing backups to backup providers

#### Backblaze B2

To push backups to a [Backblaze B2](https://www.backblaze.com/b2/cloud-storage.html) bucket, use the following additional `vars.yml` configuration:

```yaml
gitea_backup_b2_enabled: true
gitea_backup_b2_bucket: ''
gitea_backup_b2_key_id: ''
gitea_backup_b2_key_secret: ''
```

### Additional providers

For now, we don't support additional backup providers. Feel free to contribute support for others!


## Restoring backups

Restoring is a relatively manual process that goes like this:

1. If you have an existing Gitea service on the server, make sure it's stopped: `ansible-playbook -i inventory/hosts setup.yml --tags=stop`
2. Ensure there is no `/gitea` directory on the server
3. Get your `DATE-gitea-latest.tar.gpg` backup file onto the server
4. Decrypt it: `gpg --no-symkey-cache -d FILE.tar.gpg > FILE.tar`
5. Extract it: ` tar xf FILE.tar -C /` (this will create the `/gitea` directory and put all files under it)
6. Re-run the playbook **without** a `start` tag. See [Installing](installing.md)
8. Import the Postgres database dump (`/gitea/backup/data/latest-dump.sql.gz`) by running: `ansible-playbook -i inventory/hosts setup.yml --tags=import-postgres --extra-vars='{"server_path_postgres_dump": "/gitea/backup/data/latest-dump.sql.gz"}'`
9. Start services: `ansible-playbook -i inventory/hosts setup.yml --tags=start`
