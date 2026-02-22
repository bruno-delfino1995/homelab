# Homelab

> Fully automated configuration of a homelab. From VPN, to LAN router, and K0s cluster

## How is it organized?

It's a set of ansible playbooks that you'll need to run in sequence to set your machines up. Depending on what capability you want to install in your machines, you'll run a specific set of playbooks, but they all expect a standard foundation.

I chose to have a set of separate folders instead of one highly configurable playbook because I'm not an Ansible expert. Also, I believe the barrier to entry becomes lower, and I can use scripts for quality of life improvements. Another driving force was the different connection modes necessary to get the machines started on the bootstrap playbook, and the normal way on the others.

## What about secrets?

Since each host is different, there'll be the usual `host_vars` folder in each playbook with a plain and secrets values. Even though we have ansible-vault, I think it's best to leave those out from the repository, so the plain values are committed but the secrets are ignored. Nevertheless, there's a `secrets.yaml.example` in each host to show what variables are expected so you don't stare at a blank.

To keep with the "no secrets directly available anywhere but my mind" idea, I don't want to have a plain password file. To prevent that, I'm using an external secrets manager to house the vault password used in this repository. For this purpose, I chose Bitwarden and you'll need to configure the path, inside Bitwarden, to your vault password in the `BITWARDEN_ENTRY` environment variable.

The value in that entry will be used by scripts through the Bitwarden CLI, which you'll need to install too. There's only two scripts related to encryption here: `guarded`, responsible for unlocking the ansible-vault; and `encrypt` to create vault encrypted values for secret files.

## How to set up?

For each of the [hosts I configure](./inventory.ini), you'll find a `secrets.yaml.example` file with the required facts about a host to run the playbook. Each of those fields needs to go through encryption to prevent leaking them by accident. However, to encrypt that, you need a vault password, that shouldn't be leaked at all cost. Therefore, to keep everything in a closed loop, `./scripts/guarded` sends the password to a command's stdin, and `scripts/encrypt` command encrypts strings through ansible-vaults mechanism.

Once your Bitwarden CLI setup is done, and you can query that value using `bw get password "$BITWARDEN_ENTRY"`, you can start encrypting whatever values you want with `./scripts/encrypt <value>`. The output of it can then be used to fill the keys in your personal secret files. Remember to also check group variables in the folders.

## How do I start?

All machines must start with a simple Fedora installation (Server or Cloud), and then we need to bootstrap them before starting the process. For that, you'll want to run the playbook in the [bootstrap folder](./bootstrap/). That playbook configures your machines with a hardened SSH setup and an operator user you'll be using instead of root. To run it, you can use a the `apply` script: `./scripts/apply bootstrap/playbook.yaml` for all hosts in the bootstrap folder; or `./scripts/apply <host> bootstrap/playbook.yaml` for a single host in there.

After you've got your machines "standardized", you can pick from the following list to set the desired capabilities of your hosts:

- [podman](./podman): to use Podman along with Caddy for a central reverse proxy for your containers
