# Podman with Quadlet

> Run your containers as systemd services

Whenever you don't want to install a full blown virtualization platform (Incus) or a container orchestrator (K0s), Podman might be a good contender.

- Install the `podman` package in your system
- Create `.container` files in systemd locations
    - `/etc/containers/systemd/<system>/<name>.container` for containers that need elevated privileges
    - `~/.config/containers/systemd/<system>/<name>.container` for regular containers
- Reload the systemd daemon so Quadlet can generate the unit files with `systemctl daemon-reload` and `systemctl --user daemon-reload`
- Check if your services exist when listing services `systemctl list-units *.service` or `systemctl --user list-units *.service`
- Start/enable the containers you want with `systemctl start <name>.service` or `systemctl --user start <name>.service`
- You're ready to go! Enjoy your services!
