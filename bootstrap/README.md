# Bootstrap

> Make all servers equal!

The initial setup may vary depending on from where you started, but they must reach a standard configuration before you start running other playbooks. The assumption taken in this playbook is that your servers start with an unsecure SSH server, which accepts root logins with passwords, and that it's using a Fedora distribution.

Ansible will connect to your servers the first time through that insecure connection, using the root user and its password (`ansible_password`) instead of an SSH key. By the end of it, you should be unable to login with root on your server, but you should have a public key (`operator_public_ssh_key`) enabled for your operator user (`operator_user`). That user will have a password (`operator_password`), but you won't be able to use it for SSH.

As the first connection depends on a plain password, you'll need `sshpass` in the host running Ansible. Once a connection is established, you'll also need `python-passlib` (already managed by mise + uv) to create the password for your operator user.

## How to run?

It uses the same setup as other playbooks in this repository, but it's in a separate folder because it depends on a different connection model. You won't have access to the `setup` script, but you can run the playbook in here with: `../guarded ansible-playbook --vault-password-file=/bin/cat playbook.yaml`
