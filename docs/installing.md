# Installing

After [configuring DNS](configuring-dns.md) and [configuring the playbook](configuring-playbook.md), you're ready to install.

First, update the Ansible roles in this playbook by running `make roles`.

Then, run the playbook: `ansible-playbook -i inventory/hosts setup.yml --tags=setup-all`.

If your inventory file (`vars.yml`) contains encrypted variables, you may need to pass `--ask-vault-pass` to the `ansible-playbook` command.

After installing, you can start services: `ansible-playbook -i inventory/hosts setup.yml --tags=start`.


## Initial Gitea setup

After some time, you should be able to access your new Gitea instance at: `https://gitea.DOMAIN`.

Going there, you'll be taken to the initial setup wizard, which will let you assign some paswords and other configuration.
