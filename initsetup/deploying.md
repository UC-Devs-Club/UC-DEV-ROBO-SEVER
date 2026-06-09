# Deploying LXC

>[!IMPORTANT]
> This is part 2 of initial setup, and this page is covering deploying an LXC

## Table of Content 
* [Home](/README.md)
* Initial Setup
  * [Part 1](/initsetup/README.md)
  * Part 2 - You are here


### Setting up Cockpit

```bash
sudo apt install cockpit
sudo systemctl enable --now cockpit.socket
```

Now you should be able to log onto the Cockpit web console uses TCP port 9090. You can access your management dashboard by navigating to `https://<lxc-ip>:9090` in any standard web browser.

### Installing Docker

```bash
# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update

# Installing Docker 
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Enabling docker to launch upon boot
sudo systemctl enable docker

# Verify that Docker is running
sudo systemctl status docker
```
