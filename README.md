# Proxmox Cluster Setup

## Hardware
- remember you will need an odd number of servers, so you have [quorum](https://pve.proxmox.com/wiki/Cluster_Manager#_quorum)
- you should have 1 drive for OS and at least another for mass storage, that will be combined in a pool
- networking must be wired, no wireless (but the option is now there, so maybe!)
- must have static IPs (in order usually) and 1 used for a common IP to get into the cluster
- ensure "Virtuallization" is turned on in the BIOS
  - ensure ALL the settings in every machine ar the same

## Load Proxmox
- get the latest [ISO](https://www.proxmox.com/en/downloads/proxmox-virtual-environment/iso)
- load it, setting your static IPs
- come up with naming convention for each server (VPS-00), starting at zero (since ceph storage starts at 0)
- confirm you can login to web interface (xxx.xxx.xxx.xxx:8006)

## Initial Fixes on each Machine
- run the [Proxmox Community: Post Install script](https://community-scripts.github.io/ProxmoxVE/scripts?id=post-pve-install) in the shell
  - DO NOT disable HA!
 
## Create the Cluster
  - on the master node (not with the common IP, but its IP), select:
    - DataCenter
    - Cluster
    - Create Cluster
- give it a Cluster Name (School-VPS)
- copy the "Join Information"
- goto the next node (with their IP address, not common) and join the cluster
  - enter in the password of the master node
- you will have to re-login to the servers

## Install Ceph
- ensure you are NOT using the shared IP address
- start on the master node (wit its IP address)
- select:
  - Ceph
  - change the "repository" to "No-Subscription"
  - ensure this one finishs completely before doing any others, this master node needs to be done first
 
## Install KeepAlived
- [YouTube Video](https://www.youtube.com/watch?v=82Q4SZMW-zg&list=PLwcxrRo-VwS2gNgY-GlPGDYZ6KCwSu3tD&index=3)
- [GitHub Repo](https://github.com/mrp-yt/Galaxy-Home-Lab/blob/main/Services/keepalived/keepalived-setup.md)
- REMEMBER: each machine gets a different priority (lower) and the "Master" node gets a special file, when setting up the config file
- also, ensure you set the IP to the one you are using as the single IP for cluster
- you can turn off the master and see if the others will pick up from the common IP address

## CloudFlare
- use community script
  - [Proxmox VE Cloudflared LXC](https://community-scripts.github.io/ProxmoxVE/scripts?id=cloudflared)
- be patient, it takes time to start on the 2nd step when you add in your key
- in CloudFlare Dashboard change the following:
  - noTLSVerify: true
  - disableChunkedEncoding: true
