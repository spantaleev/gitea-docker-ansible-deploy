# Gitea server setup using Ansible and Docker

-------

**WARNING**: this playbook has been **made obsolete** by the [MASH playbook](https://github.com/mother-of-all-self-hosting/mash-playbook), which also supports installing the [Gitea](https://github.com/mother-of-all-self-hosting/mash-playbook/blob/main/docs/services/gitea.md) and [Woodpecker CI](https://github.com/mother-of-all-self-hosting/mash-playbook/blob/main/docs/services/woodpecker-ci.md) server and agent services.

-------

This [Ansible](https://www.ansible.com/) playbook can help you set up your own [Gitea](https://gitea.io/) server instance:

- on your own Debian/CentOS/RedHat server

- with all services ([Gitea](https://gitea.io/), [PostgreSQL](https://www.postgresql.org/), [Traefik](https://traefik.io), etc.) running in [Docker](https://www.docker.com/) containers

- powered by the official [gitea/gitea](https://hub.docker.com/r/gitea/gitea) container image

- [interoperates nicely](docs/configuring-playbook-interoperability.md) with [related](#related) Ansible playbooks or other services using Traefik for reverse-proxying

SSL certificates are automatically managed by a [Traefik](https://traefik.io) reverse-proxy.

Various components (Postgres, Traefik, etc.) can be disabled and replaced with your own other implementations (see [configuring the playbook](docs/configuring-playbook.md)).


## Features

Using this playbook, you can get the following services configured on your server:

- a [Gitea](https://gitea.io/) server - storing your Git data

- (optional) a [PostgreSQL](https://www.postgresql.org/) database for Gitea

- (optional) free [Let's Encrypt](https://letsencrypt.org/) SSL certificate, which secures the connection to the Gitea server

- (optional) [backups](docs/configuring-playbook-backups.md)

- (optional) [Woodpecker CI](https://woodpecker-ci.org/) server + agent setup integrated with the Gitea server - for running your [Continuous Integration](https://en.wikipedia.org/wiki/Continuous_integration) jobs

Basically, this playbook aims to get you up-and-running with all the basic necessities around Gitea.


## Installation

To configure and install Gitea on your own server, follow the [README in the docs/ directory](docs/README.md).


## Changes

This playbook evolves over time, sometimes with backward-incompatible changes.

When updating the playbook, refer to [the changelog](CHANGELOG.md) to catch up with what's new.


## Support

- Matrix room: [#gitea-docker-ansible-deploy:devture.com](https://matrix.to/#/#gitea-docker-ansible-deploy:devture.com)

- GitHub issues: [spantaleev/gitea-docker-ansible-deploy/issues](https://github.com/spantaleev/gitea-docker-ansible-deploy/issues)


## Related

You may also be interested in these other playbooks:

- [matrix-docker-ansible-deploy](https://github.com/spantaleev/matrix-docker-ansible-deploy) - for deploying a fully-featured [Matrix](https://matrix.org) homeserver

- [nextcloud-docker-ansible-deploy](https://github.com/spantaleev/nextcloud-docker-ansible-deploy) - for deploying a [Nextcloud](https://nextcloud.com/) server

- [peertube-docker-ansible-deploy](https://github.com/spantaleev/peertube-docker-ansible-deploy) - for deploying a [PeerTube](https://joinpeertube.org/) video-platform server

- [vaultwarden-docker-ansible-deploy](https://github.com/spantaleev/vaultwarden-docker-ansible-deploy) - for deploying a [Vaultwarden](https://github.com/dani-garcia/vaultwarden) password manager server (unofficial [Bitwarden](https://bitwarden.com/) compatible server)
