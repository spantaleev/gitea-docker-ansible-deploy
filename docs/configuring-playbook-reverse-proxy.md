# Configuring the reverse-proxy

By default, this playbook installs a [Traefik](https://traefik.io/) reverse-proxy, but that can be disabled if you'd like to using your own Traefik instance or to reverse-proxy in another way.

To disable Traefik, use:

```yaml
# Disable Traefik
gitea_playbook_traefik_installation_enabled: false

# Expose the Gitea container's webserver to port 3000 on the loopback network interface only.
# You can reverse-proxy to it using a locally running webserver.
gitea_gitea_http_bind_port: '127.0.0.1:3000'

# Or:
# Expose the Gitea container's webserver to port 3000 on all network interfaces.
# You can reverse-proxy to it from another machine on the public or private network.
# gitea_gitea_http_bind_port: '3000'
```

