# Updating

To update your Gitea server:

- update this playbook (`git pull` if you've cloned via git; downloading it a-new if you've downloaded it as an archive)
- run `make roles` to update the Ansible roles
- execute `ansible-playbook -i inventory/hosts setup.yml --tags=setup-all,start`
