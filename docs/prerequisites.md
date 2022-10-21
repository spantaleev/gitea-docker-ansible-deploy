# Prerequisites

- (Recommended) An **x86** or `arm64` (untested, but it may work) server running one of these operating systems:
  - **CentOS** (7+)
  - **Debian** (10/Buster or newer)
  - **Ubuntu** (18.04 or newer)
  - **Archlinux**

- the machine's SSH server needs to be moved to another port (e.g. `2222`), so that port `22` would be available for Gitea's own purposes

- `root` access to your server (or a user capable of elevating to `root` via `sudo`).

- [Python](https://www.python.org/) being installed on the server. Most distributions install Python by default, but some don't

- the [Ansible](http://ansible.com/) program being installed on your own computer. It's used to run this playbook and configures your server for you

- [`git`](https://git-scm.com/) is the recommended way to download the playbook to your computer

- Properly configured DNS records for `<your-domain>` (details in [Configuring DNS](configuring-dns.md)).

- Some TCP ports open. This playbook (actually [Docker itself](https://docs.docker.com/network/iptables/)) configures the server's internal firewall for you. In most cases, you don't need to do anything special. But **if your server is running behind another firewall**, you'd need to open these ports:

  - `22/tcp`: Gitea's SSH server
  - `80/tcp`: HTTP webserver
  - `443/tcp`: HTTPS webserver
  - `2222/tcp` (or some other): the system's SSH server that we've asked you to relocate away from port `22`, so that port `22` is free for Gitea to use

When ready to proceed, continue with [Configuring DNS](configuring-dns.md).
