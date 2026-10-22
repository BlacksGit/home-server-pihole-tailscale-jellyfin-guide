# Home server starter guide: Pi-hole + Tailscale + Jellyfin on Ubuntu (free sections)

Copy-paste steps for a spare PC running Ubuntu Server 24.04. Read each command before you run it.

## Pi-hole: block ads for every device

```bash
curl -sSL https://install.pi-hole.net | sudo bash
sudo pihole setpassword
```

Set your router DHCP **DNS server** to the server IP so every device uses it. Test: `nslookup doubleclick.net SERVER_IP` should return `0.0.0.0`.

## Tailscale: reach home from anywhere

```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
```

To reach your whole LAN, enable forwarding and advertise the subnet:

```bash
echo 'net.ipv4.ip_forward=1' | sudo tee /etc/sysctl.d/99-tailscale.conf
sudo sysctl -p /etc/sysctl.d/99-tailscale.conf
sudo tailscale up --advertise-routes=192.168.1.0/24
```

Approve the route in the Tailscale admin console and **disable key expiry** for the server.

## Jellyfin

```bash
curl -s https://repo.jellyfin.org/install-debuntu.sh | sudo bash
```

Open `http://SERVER_IP:8096`. Prefer direct play (H.264/HEVC in MP4/MKV); enable VAAPI/QSV if your CPU transcodes slowly.

## The full runbook

The **Home Server Starter Runbook** PDF adds the Ubuntu install and hardening (SSH keys, ufw, automatic updates), fixed IP setup, restic backups, weekly health checks, checklists and an 8-row troubleshooting table: https://spectre5.gumroad.com/l/sovcom ($7).

More guides: https://patreon.com/123black
