# Bootstrap

> Make all servers equal!

The initial setup may vary depending where you started, but they must reach a standard configuration before you start running other playbooks. The playbook works on the assumption that you have Fedora servers with an unsecure SSH configuration that accepts password logins for root.

## How it works?

Ansible will connect to your servers the first time through that insecure connection, using the root user and its password (`ansible_password`) instead of an SSH key. By the end of it, you should be unable to login with root on your server, but you should have a public key (`operator_public_ssh_key`) enabled for your operator user (`operator_user`). That user will have a password (`operator_password`), but you won't be able to use it for SSH.

## How to run?

As the first connection depends on a plain password, you'll need `sshpass` in the host running Ansible. Once a connection is established, you'll also need `python-passlib` (already managed by mise + uv) to create the password for your operator user.

You can run the playbook in here with: `../scripts/apply playbook.yaml`. In case you want to test on a single host, run it with the `../scripts/apply <host> playbook.yaml`. It'll will prompt you to unlock your Bitwarden vault and use that password to decrypt the Ansible vault to get the values for each host.
