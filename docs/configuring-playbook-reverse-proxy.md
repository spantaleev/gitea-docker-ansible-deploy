# Configuring the reverse-proxy

By default, this playbook installs a [Traefik](https://traefik.io/) reverse-proxy, but that can be disabled if you'd like to using your own Traefik instance or to reverse-proxy in another way.


## Reverse-proxy type

To control the playbook's reverse-proxy integration use `gitea_playbook_reverse_proxy_type` variable, which controls the type of reverse-proxy that the playbook will use. Valid values:

  - `playbook-managed-traefik` (the default)
  - `other-traefik-container`, see [Using your own Traefik server (installed separately)](#using-your-own-traefik-server-installed-separately)
  - `none`, see [Disabling Traefik and reverse-proxying manually](#disabling-traefik-and-reverse-proxying-manually)

Learn more about these values and their behavior from [roles/custom/base/defaults/main.yml](../roles/custom/base/defaults/main.yml)


## Other variables of interest

The variables below are **automatically set** based on the reverse-proxy type (`gitea_playbook_reverse_proxy_type`). Nevertheless, you may find them useful if you need to do something more advanced.

- `devture_traefik_enabled` (same value as `gitea_playbook_traefik_role_enabled` by default - `true`) - controls whether the Traefik role's functionality is enabled or not. If disabled, the role will try to uninstall Traefik, etc. Flipping this to `false` disables Traefik, but also potentially uninstalls and deletes data in `/devture-traefik`.

- `gitea_playbook_traefik_role_enabled` (default `true`) - controls whether the Traefik role will execute or not. Setting this to `false` disables Traefik and doesn't touch `/devture-traefik` (which is potentially managed by another playbook)

- `gitea_playbook_traefik_labels_enabled` (default `true`) - controls whether Traefik container labels are attached to services. You may disable Traefik with the variables above, yet still keep attaching labels, so that a separately-installed Traefik instance can reverse-proxy to these services. If you're not using Traefik at all, flip this to `false`

- `gitea_playbook_reverse_proxyable_services_additional_network` (default `traefik`) - additional container network for reverse-proxyable services (like `gitea-gitea`). We default these to the `traefik` network for the default Traefik installation's benefit, but you can set this to another network


## Examples

### Using your own Traefik server (installed separately)

If you'd like to avoid the playbook installing its own Traefik server and instead use your own, use this configuration:

```yaml
gitea_playbook_reverse_proxy_type: other-traefik-container
# Specify the name of your Traefik network here
gitea_playbook_reverse_proxyable_services_additional_network: traefik
```

All services will have container labels attached, so that a Traefik instance can reverse-proxy to them.


### Using Traefik in local-only mode

Below is an example of **putting Traefik in local-only mode** (no SSL termination) and letting you use another SSL-terminating reverse-proxy in front:

```yaml
# We keep the default reverse-proxy type
# gitea_playbook_reverse_proxy_type: playbook-managed-traefik

# We disable Traefik's web-secure endpoint, which will disable SSL certificate retrieval and http-to-https redirection
devture_traefik_config_entrypoint_web_secure_enabled: false
```

You can now reverse-proxy to port `80` where Traefik handles domains for all services managed by the playbook (Gitea and potentially others, if enabled).


### Disabling Traefik and reverse-proxying manually

Below is an example of **disabling Traefik completely** and letting you reverse-proxy using other means:

```yaml
gitea_playbook_reverse_proxy_type: none

# Expose the Gitea container's webserver to port 3000 on the loopback network interface only.
# You can reverse-proxy to it using a locally running webserver.
gitea_gitea_http_bind_port: '127.0.0.1:3000'

# Or:
# Expose the Gitea container's webserver to port 3000 on all network interfaces.
# You can reverse-proxy to it from another machine on the public or private network.
# gitea_gitea_http_bind_port: '3000'
```

You can now reverse-proxy to port `3000` for Gitea. If you need to expose other services managed by this playbook, you'd need to expose them the same way (`_bind_port` variables) manually.
