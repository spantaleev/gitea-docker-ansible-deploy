# Configuring your DNS server

To set up Gitea on your domain, you'd need to do some DNS configuration.

## DNS settings for services enabled by default

| Type  | Host                         | Priority | Weight | Port | Target                 |
| ----- | ---------------------------- | -------- | ------ | ---- | ---------------------- |
| A     | `gitea`                      | -        | -      | -    | `gitea-server-IP`      |

Be mindful as to how long it will take for the DNS records to propagate.
