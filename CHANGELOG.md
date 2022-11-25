# 2022-11-25

# Traefik now runs in a separate container network from the rest of the Gitea services

Until now, Traefik ran in the same container network (`gitea`) as all Gitea services so it could reverse-proxy to them easily.

From now on:

- Traefik runs in its own new `traefik` container network

- Gitea services continue to run in the `gitea` container network

- Gitea services which Traefik needs to reverse-proxy to (e.g. the `gitea-gitea` container) are automatically connected to the `traefik` additional container network, thanks to a new `gitea_playbook_reverse_proxyable_services_additional_networks` variable controlling this behavior

To **upgrade your setup**, consider first stopping all services (running the playbook with `--tags=stop`) and then installing (`--tags=setup-all,start`).


# 2022-10-21

Initial release
