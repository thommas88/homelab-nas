# Homelab NAS – Proxmox + ZFS + Samba

## Overview
This project is part of my homelab, where I set up a NAS solution based on Proxmox and ZFS.  
The goal was to build a stable and flexible storage platform that provides redundancy, security, and a foundation for further experimentation.

## Goals
- Reliable storage with disk redundancy  
- Access from multiple devices (Windows and Linux)  
- Snapshots and simple restore capability  
- Image backup of VMs and containers  
- A stable foundation for future projects  

## Architecture
**Hardware:** i9-9900K, 32GB RAM, 2×8TB HDD (NAS), 2×2TB HDD (photo backup), 250GB SSD (OS/VM)  

**Software and services:**  
- Proxmox VE as hypervisor  
- TrueNAS SCALE as NAS VM (ZFS mirror pool)  
- Samba for network file sharing  
- Immich for automatic photo backup  
- Jellyfin for media streaming  
- Proxmox Backup Server for VM/container image backups  
- ZFS snapshots and scrubs for data integrity  

## What I Learned
- Setting up and managing ZFS mirrors in practice  
- Using datasets and snapshots to protect and organize data  
- Basic Samba configuration for network sharing  
- Experience running self-hosted services like Immich and Jellyfin  
- Integrating snapshots and image backups into a complete backup strategy  

## Next Steps
- Implement encrypted offsite backup with rclone  
- Add alerts in Grafana (Discord/Telegram/webhooks)  
- Evaluate SSD/NVMe caching for better NAS performance  

---

## Scope and Considerations
Several enterprise-level features were considered (for example full monitoring with Prometheus/Grafana and advanced network segmentation).  
For a homelab of this size, these were judged as unnecessary complexity, but they remain possible future extensions.  

---

A detailed project report can be found here:  
👉 [PROJECT_REPORT.md](./PROJECT_REPORT.md)
