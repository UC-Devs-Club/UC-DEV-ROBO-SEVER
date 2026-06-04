# System Setup

## Table of Contents
* [Home](https://github.com/RuhanShafi/HomeServer/blob/main/README.md)
* Initial Setup - You're Already Here :)
  - [Disable Enterprise Repositories](#disable-enterprise-repositories)
  - [Delete `local-lvm` and resize local](#delete-local-lvm-and-resize-local)
  - [Ensuring that `IOMMU` is enabled](#ensure-iommu-is-enabled)
* [Apps](https://github.com/RuhanShafi/HomeServer/blob/main/apps/README.md) - A brief rundown on all the different app and services that I run on my Home Lab
* [Media](https://github.com/RuhanShafi/HomeServer/blob/main/media/README.md) - Deeper Dive into my Jellyfin & *arr stack Setup

>[!info]
> This setup assumes that you have installed Proxmox onto the machine using standard settings were applicable

### Disable Enterprise Repositories

1. Navigate to Node > Repositories Disable the enterprise repositories.
2. Now click Add and enable the no subscription repository. Finally, go Updates > Refresh.
3. Upgrade your system by clicking Upgrade above the repository setting page.

### Delete `local-lvm` and Resize local

This one is a matter of personal perference, however I like using the boot disk as my flash storage, storing docker configs and ISO files with the ZFS tank pool, which will be discused later holding all the actual data. This is also because I can't afford another NVME just to hold docker configs.

1. Delete local-lvm manually from web interface under Datacenter > Storage.
2. Run the following commands within Node > Shell. 
```bash
lvremove /dev/pve/data
lvresize -l +100%FREE /dev/pve/root
resize2fs /dev/mapper/pve-root
```
3. Check to ensure your local storage partition is using all available space. Reassign storage for containers and VM if needed.

### Ensure `IOMMU`` is enabled

Before deploying any LXC and Services, first ensure that IOMMU is enabled, this allows Proxmox to securely assign physical PCIe devices (like GPUs, network cards, or storage HBAs) directly to a Virtual Machine (VM), particularly useful for Networking and Graphics work 

In order to Enable `IOMMU` on in grub configuration, on the Proxmox web GUI go to Node > Shell and enter the following command

```bash
nano /etc/default/grub
```

You will see the line with `GRUB_CMDLINE_LINUX_DEFAULT="quiet"`, all you need to do is add `intel_iommu=on` or `amd_iommu=on` depending on your system.

```bash
# Should look like this
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on"
```

Next run the following command in order to apply the changes persistently and reboot your system

```bash
update-grub
```

To verify that `IOMMU` is working, use the following commands

```bash
dmesg | grep -e DMAR -e IOMMU
dmesg | grep 'remapping'
```

### Creating the ZFS Pool

First, check out your disks and make sure that they're all there. Find this under Node > Disks. Make sure you wipe all the disks you plan on using and do note this will wipe any data on the disks, so make sure there is no important data on them and back up if needed.

Now, on the Proxmox sidebar for your datacenter, go to Disks > ZFS > Create: ZFS. This will pop up the screen to create a ZFS pool.

From this screen, it should show all of your drives, so select the ones you want in your pool, and select your RAID level (in my case RAIDZ for my tank pool) and compression, (in my case I use zfsd). Make sure you check the box that says Add to Storage. This will make the pools immediately available and will prevent using .raw files as opposed to my previous setup when I added directories.

>[!IMPORTANT]
> Now Proxmox is completely setup and good for use, if you want an overview of how I deploy the different LXC containers, feel free to look [here](./deploying.md).
