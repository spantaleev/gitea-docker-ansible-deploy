# 2023-03-16

## This playbook has been absobred into the MASH playbook

To avoid maintaining too many similarly-looking playbooks, I've decided to merge this playbook into the newly-created [MASH playbook](https://github.com/mother-of-all-self-hosting/mash-playbook). For more details about this, see the [Why create such a mega playbook?](https://github.com/mother-of-all-self-hosting/mash-playbook/tree/main#why-create-such-a-mega-playbook) section of the new playbook's README file.

**This `gitea-docker-ansible-deploy` playbook will not receive additional updates**.

Steps to migrate from `gitea-docker-ansible-deploy` to hosting Gitea using the MASH playbook:

1. Get started with the [MASH playbook](https://github.com/mother-of-all-self-hosting/mash-playbook). Do an initial installation which contains Postgres, Traefik, etc. Enabling the Gitea service is done below in step 2.

2. Configure the [Gitea service](https://github.com/mother-of-all-self-hosting/mash-playbook/blob/main/docs/services/gitea.md) with the MASH playbook. You can reuse your `vars.yml` file with these changes:

- **renaming** `gitea_gitea_` to `gitea_` in all variable names
- the **addition** of `gitea_enabled: true`
- the **removal** of various `gitea_playbook_` variables. You may need to replace them with `mosh_playbook_` variables
- the **removal** of `gitea_generic_secret_key`. This has been superseded by `mash_playbook_generic_secret_key`
- the **removal** of any other variables you had in your old `vars.yml` file (`devture_postgres_connection_password`, etc.). Your new `vars.yml` file likely already defines some of these variables, so there's no need to define them twice.

3. If you were using the [Woodpecker CI](https://github.com/mother-of-all-self-hosting/mash-playbook/blob/main/docs/services/woodpecker-ci.md) service (agent or server), configure it as well.

4. Do an initial installation by running the following command **in the MASH playbook's directory**: `just run-tags install-all`. NOTE: there's a difference between `just run-tags install-all` and `just install-all`; we use the former here, because we don't want to start the Gitea service just yet

5. SSH into the server and do the following:

   - Create a database dump by running: `/gitea/postgres/bin/dump-all /gitea`. This will create the `/gitea/latest-dump.sql.gz` file

    - Stop and disable all old Gitea services by running: `cd /etc/systemd/system && systemctl disable --now gitea*` (note the `*` at the end)

    - Sync the Gitea data by running: `rsync -avr /gitea/gitea/. /mash/gitea/.`

    - Fix permissions for the Gitea data: `chown -R mash:mash /mash/gitea`

    - (Optional) Sync the Woodpecker CI server data by running: `rsync -avr /gitea/woodpecker-ci/server/. /mash/woodpecker-ci/server/.`

    - (Optional) Fix permissions for the Woodpecker CI server data: `chown -R mash:mash /mash/woodpecker-ci/server/`

6. Import the Gitea database dump into the Postgres instance by running the following command **in the MASH playbook's directory**: `just run-tags import-postgres --extra-vars=server_path_postgres_dump=/gitea/latest-dump.sql.gz --extra-vars=postgres_default_import_database=gitea`

7.  Re-run the MASH playbook and start all services by running the following command **in the MASH playbook's directory**: `just run-tags install-all,start`

8. Verify that your new Gitea installation works

9. Clean up by SSH-ing into the server and doing the following:

    - `rm /etc/systemd/system/gitea*`
    - `rm -rf /gitea`
    - getting rid of this playbook


# 2023-01-13

## Support for running commands via just

We've previously used [make](https://www.gnu.org/software/make/) for easily running some playbook commands (e.g. `make roles` which triggers `ansible-galaxy`, see [Makefile](Makefile)).
Our `Makefile` is still around and you can still run these commands.

In addition, we've added support for running commands via [just](https://github.com/casey/just) - a more modern command-runner alternative to `make`. Instead of `make roles`, you can now run `just roles` to accomplish the same.

Our [justfile](justfile) already defines some additional helpful **shortcut** commands that weren't part of our `Makefile`. Here are some examples:

- `just install-all` to trigger the much longer `ansible-playbook -i inventory/hosts setup.yml --tags=install-all,start` command
- `just install-all --ask-vault-pass` - commands also support additional arguments (`--ask-vault-pass` will be appended to the above installation command)
- `just run-tags install-gitea-backup,start` - to run specific playbook tags
- `just start-all` - (re-)starts all services
- `just stop-group postgres` - to stop only the Postgres service

Additional helpful commands and shortcuts may be defined in the future.

This is all completely optional. If you find it difficult to [install `just`](https://github.com/casey/just#installation) or don't find any of this convenient, feel free to run all commands manually.


# 2022-12-14

# Container networks have flipped around

If you're using an externally-managed Traefik server or other reverse-proxy, you may need to adapt your `vars.yml` configuration.

To ensure connectivity of Gitea to Traefik, we used to put Gitea in Traefik's network (as a main network), and then also connect the Gitea container to "additional networks" (its own `gitea` network, etc.).

While this worked, it was a little backwards. We now have a better way to do things - putting Gitea in its own `gitea` network as main, and connecting the Gitea container to additional networks (e.g. `traefik`) after creating the container, but before starting it. This also seems to work well and is more straightforward.

The playbook will warn you if you're using any variables that have been renamed or dropped.


# 2022-11-25

# Traefik now runs in a separate container network from the rest of the Gitea services

Until now, Traefik ran in the same container network (`gitea`) as all Gitea services so it could reverse-proxy to them easily.

From now on:

- Traefik runs in its own new `traefik` container network

- Gitea services continue to run in the `gitea` container network

- Gitea services which Traefik needs to reverse-proxy to (e.g. the `gitea-gitea` container) are automatically connected to the `traefik` additional container network, thanks to a new `gitea_playbook_reverse_proxyable_services_additional_networks` variable controlling this behavior

To **upgrade your setup**, consider first stopping all services (running the playbook with `--tags=stop`) and then installing (`--tags=setup-all,start`).

If you're reverse-proxying via your own Traefik instance (not installed by this playbook), you may need to use this additional configuration: `nextcloud_playbook_reverse_proxyable_services_additional_networks: [traefik]` (for Traefik running in a container network named `traefik`).


# 2022-10-21

Initial release
