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
