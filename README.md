# Proxmox Cluster Setup

## Hardware
- remember you will need an odd number of servers, so you have [quorum](https://pve.proxmox.com/wiki/Cluster_Manager#_quorum)
- you should have 1 drive for OS and at least another for mass storage, that will be combined in a pool
- networking must be wired, no wireless (but the option is now there, so maybe!)
- must have static IPs (in order usually) and 1 used for a common IP to get into the cluster

## Load Proxmox
- get the latest [ISO](https://www.proxmox.com/en/downloads/proxmox-virtual-environment/iso)
- load it, setting your static IPs
- come up with naming convention for each server (VPS-00), starting at zero (since ceph storage starts at 0)
- confirm you can login to web interface (xxx.xxx.xxx.xxx:8006)

## Initial Fixes on each Machine
- run the [Proxmox Community: Post Install script](https://community-scripts.github.io/ProxmoxVE/scripts?id=post-pve-install) in the shell
  - DO NOT disable HA!
- install KEEPALIVED
  - [YouTube Video](https://www.youtube.com/watch?v=82Q4SZMW-zg&list=PLwcxrRo-VwS2gNgY-GlPGDYZ6KCwSu3tD&index=3)
  - [GitHub Repo](https://github.com/mrp-yt/Galaxy-Home-Lab/blob/main/Services/keepalived/keepalived-setup.md)
  - REMEMBER: each machine gets a different priority (lower) and the "Master" node gets a special file, when setting up the config file
  - also, ensure you set the IP to the one you are using as the single IP for cluster
