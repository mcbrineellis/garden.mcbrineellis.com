Based on the instructions on the [Tailscale website](https://tailscale.com/download/linux/debian-bookworm), except without sudo as Proxmox doesn't have sudo installed by default.

```
curl -fsSL https://pkgs.tailscale.com/stable/debian/bookworm.noarmor.gpg | tee /usr/share/keyrings/tailscale-archive-keyring.gpg >/dev/null
curl -fsSL https://pkgs.tailscale.com/stable/debian/bookworm.tailscale-keyring.list | tee /etc/apt/sources.list.d/tailscale.list
apt-get update
apt-get install tailscale
tailscale up
tailscale ip -4
```