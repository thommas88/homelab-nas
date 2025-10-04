# Homelab NAS – Proxmox + ZFS + Samba

## Overview
This project is part of my homelab, where I built a NAS solution on top of Proxmox VE using ZFS for redundancy and Samba for network file sharing.
The goal was to create a stable and flexible storage platform that provides reliability, accessibility, and a foundation for future self-hosted services.

## Goals
- Reliable storage with disk redundancy  
- Access from multiple devices (Windows and Linux)  
- Use snapshots for quick restore of files and datasets 
- Support image backups of VMs and containers
- Serve as a stable foundation for additional services (e.g. Immich, Jellyfin) 

## UML
![Homelab Diagram](images/UML.png)

## Architecture
**Hardware:** i9-9900K, 32GB RAM, 2×8TB HDD (NAS), 2×2TB HDD (photo backup), 250GB SSD (OS/VM)  

**Software and services:**  
- Proxmox VE as hypervisor  
- TrueNAS SCALE VM with ZFS mirror pool 
- Samba for network file sharing  
- Immich for automatic photo backup  
- Jellyfin for media streaming  
- Proxmox Backup Server for VM/container image backups  
- ZFS snapshots and scrubs for data integrity  

## Repository Structure

```
homelab-nas/
│
├─ README.md              ← Main project overview (this file)
├─ docs/
│   ├─ timeline.md       ← Chronological project log (dates, milestones)
│   ├─ config.md          ← Configuration examples (Proxmox VM, ZFS pool, SMB)
│   └─ errors-fixes.md    ← Problems encountered and solutions
└─ images/
    ├─ truenas-pool.png   ← Screenshot: ZFS pool status
    ├─ smb-shares.png     ← Screenshot: SMB share setup
    └─ network-diagram.png← Architecture diagram
```


## What I Learned
- Hands-on experience setting up and managing ZFS mirrors 
- Using datasets and snapshots for data organization and protection  
- Configuring Samba shares and managing ACLs for different users
- Running self-hosted services (Immich, Jellyfin) integrated with the NAS  
- Building a complete backup strategy with snapshots + VM image backups 

## Next Steps
- Implement encrypted offsite backup with rclone  
- Add alerts in Grafana (Discord/Telegram/webhooks)  
- Evaluate SSD/NVMe caching for better NAS performance  

---

## Scope and Considerations
Some enterprise-level features (e.g. advanced monitoring, network segmentation) were considered but excluded to keep the system manageable for a homelab of this size.
These remain possible extensions for future iterations.  

---

A detailed project report can be found here:  
👉 [PROJECT_REPORT.md](./PROJECT_REPORT.md)
