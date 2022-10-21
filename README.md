# Gitea server setup using Ansible and Docker

This [Ansible](https://www.ansible.com/) playbook can help you set up a [Gitea](https://gitea.io/) server instance:

- on your own Debian/CentOS/RedHat server
- with all services (Gitea, Postgres, Traefik, etc.) running in [Docker](https://www.docker.com/) containers
- powered by the official [gitea/gitea](https://github.com/gitea/gitea) Docker image

SSL certificates are automatically managed by a [Traefik](https://traefik.io) reverse-proxy.

Various components (Postgres, Traefik, etc.) can be disabled and replaced with your own other implementations.


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

- [matrix-docker-ansible-deploy](https://github.com/spantaleev/matrix-docker-ansible-deploy), for deploying a fully-featured [Matrix](https://matrix.org) homeserver

- [nextcloud-docker-ansible-deploy](https://github.com/spantaleev/nextcloud-docker-ansible-deploy) - for deploying a [Nextcloud](https://nextcloud.com/) server
