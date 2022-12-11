# Woodpecker CI

This playbook can install and configure [Woodpecker CI](https://woodpecker-ci.org/) for you.

Woodpecker CI is a [Continuous Integration](https://en.wikipedia.org/wiki/Continuous_integration) engine which can build and deploy your code automatically after pushing to a Gitea repository.

A Woodpecker CI installation contains 2 components:

- one Woodpecker CI **server** (web interface, central management node)
- one or more Woodpecker CI **agent** instances (which run your CI jobs)

It's better to run the **agent** instances elsewhere (not on the Gitea server) - on a machine that doesn't contain sensitive data.
Small installations which only run trusted CI jobs can afford to run an agent instance on the Gitea server itself.

**Warning**: The example configuration below documents a setup in which both the **server** and a single **agent** instance run on the Gitea server itself. If you need to run CI jobs for untrusted repositories, you may wish to tweak the configuration in a way which deploys the **agent** nodes to a different server (other than Gitea).


## Preparation

### DNS configuration

The Woodpecker CI server is hosted on a domain name, as specified in the `devture_woodpecker_ci_server_server_fqn` variable (see **Installation** below).

You'll need to create a dedicated domain name for Woodpecker CI (e.g. `woodpecker.example.com`), pointing to the Gitea server - a DNS `CNAME` record is suitable.

### Gitea configuration

Below is a summary of the [Woodpecker CI documentation for Gitea](https://woodpecker-ci.org/docs/administration/vcs/gitea#registration):

1. Go to **User Settings** -> **Applications** (`/user/settings/applications`).
2. Create a new OAuth 2 application with **any** name and a **Redirect URI** of `https://YOUR_WOODPECKER_DOMAIN_NAME/authorize` (`YOUR_WOODPECKER_DOMAIN_NAME` needs to match what you've chosen for `devture_woodpecker_ci_server_server_fqn` - see the **DNS configuration** step above)
3. Copy the client ID and client secret for the newly created OAuth 2 application. You will need then below, during the **Installation** step


## Installation

Before adjusting your the `vars.yml` configuration as explained below, you will need to follow the **Preparation** steps.

**After** that, adjust your `vars.yml` configuration like this:

```yaml
#########################################
# Woodpecker CI server component        #
#########################################

devture_woodpecker_ci_server_enabled: true

# You will need to create a DNS record for this domain,
# as explained in "DNS configuration" above.
devture_woodpecker_ci_server_server_fqn: woodpecker.DOMAIN

# Generate this secret with `openssl rand -hex 32`
devture_woodpecker_ci_server_config_agent_secret: ''

# Add one or more Gitea usernames below.
# These users will have admin privileges upon signup.
devture_woodpecker_ci_server_config_admins:
  - YOUR_GITEA_USERNAME_HERE
  - ANOTHER_GITEA_USERNAME_HERE

# Populate these with the OAuth 2 application information
# (see the Gitea configuration section above)
devture_woodpecker_ci_server_config_gitea_client: GITEA_OAUTH_CLIENT_ID_HERE
devture_woodpecker_ci_server_config_gitea_secret: GITEA_OAUTH_CLIENT_SECRET_HERE

#########################################
# /Woodpecker CI server component       #
#########################################


#########################################
# Woodpecker CI agent component         #
#########################################

devture_woodpecker_ci_agent_enabled: true

#########################################
# /Woodpecker CI agent component        #
#########################################
```

Then proceed to install by re-running the playbook (`--tags=install-all,start`). See [installing](installing.md).


## Usage

After installation, you should be able to access the Woodpecker CI server instance at `https://woodpecker.DOMAIN` (matching the `devture_woodpecker_ci_server_server_fqn` value configured in `vars.yml`).

The **Log in** button should take you to Gitea, where you can authorize Woodpecker CI with the OAuth 2 application.

Follow the official Woodpecker CI [Getting started](https://woodpecker-ci.org/docs/usage/intro) documentation for additional usage details.
