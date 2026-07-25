# Networking

## Table of Content

### Remote Access to Home Lab using Tailscale

#### Server Side

Run the following command within the Proxmox shell of the Node (not in any LXC Containers). Why? Should any LXC containers go down, remote connections should still work provided that the actual node and Proxmox remains operational making remote debugging far easier.

#### Client Side

* **Windows**
* **Mac**
* **Linux**
  * **CLI** (Universal): ``
  * **GUI** (Requires base CLI package)
    * Arch based Distros - [Trayscale]() 
* **iOS**: Install the official Tailscale app which can be found [here](https://github.com/DeedleFake/trayscale) (Install via AUR)
* **Android**: Install the official Tailscale app which can be found [here]()

#### Linux debugging

If on Linux Client and Tailscale returns the following when running `tailscale status`
```bash
# Health check:
#     - Some peers are advertising routes but --accept-routes is false
```

run the following command: `sudo tailscale up --accept-routes --operator=[USERNAME]` - tailscale will output the correct command for your device if you just simply run `sudo tailscale up --accept-routes`

### Adguard - Ad and Tracker Blocking Network Wide
