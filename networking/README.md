# Networking

## Table of Content

### Remote Access to Home Lab using Tailscale

#### Server Side

Run the following command within the Proxmox shell of the Node (not in any LXC Containers). Why? Should any LXC containers go down, remote connections should still work provided that the actual node and Proxmox remains operational making remote debugging far easier.

```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

This will download the Tailscale Linux Client onto the server. But before Tailscale can be deployed, some configurations need to be made, the first being to enable IP Forwarding.

Run the following commands in order to enable IP Forwarding

```bash
echo 'net.ipv4.ip_forward = 1' >> /etc/sysctl.conf
sysctl -p
```

```bash
tailscale up --advertise-routes=[subnet range/subnet mask]
#Sample command: tailscale up --advertise-routes=192.168.1.0/24
```

#### Client Side

* **Windows**
* **Mac**
* **Linux**
  * **CLI** (Universal): `curl -fsSL https://tailscale.com/install.sh | sh`
  * **GUI** (Requires base CLI package)
    * [`systray`](https://tailscale.com/docs/features/client/linux-systray) - Official but in Beta
    * [Trayscale](https://github.com/DeedleFake/trayscale) - Unofficial - Standalone GUI 
    * [KTailctl](https://github.com/f-koehler/KTailctl) - Unofficial - Direct KDE Intergration
    * [TailScout](https://shreyam1008.github.io/tailScout/) - Unofficial but compliments Ubuntu/GTK Design Language well
* **iOS**: Install the official Tailscale app which can be found [here](https://apps.apple.com/us/app/tailscale/id1470499037) 
* **Android**: Install the official Tailscale app which can be found [here](https://play.google.com/store/apps/details?id=com.tailscale.ipn&hl=en_AU)

#### Linux debugging

If on Linux Client and Tailscale returns the following when running `tailscale status`
```bash
# Health check:
#     - Some peers are advertising routes but --accept-routes is false
```

run the following command: `sudo tailscale up --accept-routes --operator=[USERNAME]` - tailscale will output the correct command for your device if you just simply run `sudo tailscale up --accept-routes`

### Adguard - Ad and Tracker Blocking Network Wide
