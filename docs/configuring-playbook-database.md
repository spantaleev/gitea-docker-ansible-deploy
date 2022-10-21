# Configuring the database

By default, this playbook installs Postgres in a container alongside Gitea.

To use your own Postgres server, use a `vars.yml` configuration like this:

```yaml
# Disable the integrated Postgres service
gitea_playbook_postgres_installation_enabled: false

# Uncomment and possibly change this, if you'd like to use another database engine.
# gitea_gitea_config_database_type: mysql

# Fill these out
gitea_gitea_config_database_hostname: ""
gitea_gitea_config_database_name: ""
gitea_gitea_config_database_username: ""
gitea_gitea_config_database_password: ""
```
