# Homelab

> Fully automated configuration of a homelab. From VPN, to LAN router, and K0s cluster

## How is it organized?

It's a set of ansible playbooks that you'll need to run in sequence to set your machines up. There's a separate [bootstrap folder](./bootstrap) responsible for standardizing the machines for the next playbooks. Since it involves secrets, but I'm too afraid to commit them, even though we have ansible-vault, each host has a folder with plain and secret values. The plain values are committed but the secrets are ignored. To better guide you, there's a `secrets.yaml.example` in each host to show what variables are expected.

## Setup

As it uses ansible-vault for the secrets, and I don't want to have a plain password file. You'll need to install Bitwarden and set the following environment variables:

- `BITWARDEN_ENTRY`: the name of the entry containing your vault password inside Bitwarden

The value in that entry will be used by the `./guarded` script that's responsible for unlocking the ansible-vault. Whenever you want to encrypt a value, you can use `./encrypt value` and use its output in a key of your inventory files.

## Installation

All machines must start with a simple Fedora installation (Server or Cloud), and then we need to bootstrap them before starting the process. For that, you'll want to run the playbook in the `bootstrap` folder. That playbook configures your machines with a hardened SSH setup and an operator user you'll be using instead of root.

After you've got your machines "standardized", a simple `./setup` will configure everything, but you can also do `./setup <host>` to configure a single machine.
