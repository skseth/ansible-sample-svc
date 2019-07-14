# Initialization

Initialize vms using vmscripts

ansible-playbook add-user-ssh.yml

While running, ansible_user has to be set to root - this could change in future.

sudo mount -t vboxsf -o uid=$UID,gid=$(id -g) work /home/devops/work

# Vault setup

See [Using vault to protect sensitive data](https://www.digitalocean.com/community/tutorials/how-to-use-vault-to-protect-sensitive-ansible-data-on-ubuntu-16-04)

## Nginx Setup Steps

https://gist.github.com/taufiqibrahim/d7f697de6bb8b93ca348a5b94d6adbfc

wget https://nginx.org/download/nginx-1.12.2.tar.gz