# Background

## Overview
This project implements reliable single-node NAS environment inside a Proxmox homelab. The goal was to design a storage backbone that’s fault-tolerant, simple to maintain, and scalable for self-hosted services like Immich and Jellyfin. The setup emphasizes practical automation, data safety, and reproducible documentation.



## Summary

This project documents the migration from a 2×8TB ZFS mirror to a 4×8TB RAIDZ1 configuration inside a Proxmox-based homelab. The migration included HBA installation, disk passthrough reconfiguration, SMART validation, and ZFS pool redesign.

Focus areas:
- Raw disk passthrough via Proxmox
- ZFS pool architecture decisions
- Risk analysis and cost tradeoffs
- Reproducible infrastructure documentation



## Goals
- Initial storage configuration consisted of a 2×8TB ZFS mirror.  
- HBA install  
- Disk migration
- Goal: 4×8TB RAIDZ1



## Hardware Changes 
- Installed LSI SAS2308 HBA. The HBA is flashed in IT-mode to expose drives directly to the operating system without hardware RAID abstraction, ensuring full ZFS control over disk management.
- Why HBA instead of motherboard SATA? 
The motherboard provided six SATA ports. With additional drives installed, available ports were exhausted. The HBA provides expanded connectivity and improved long-term scalability.
- Moved all 8TB disks to HBA
- Verified SMART health before migration

ls -l /dev/disk/by-id/
smartctl -a /dev/sdX



## Config
Disks are passed through to the TrueNAS VM using raw block device passthrough via /dev/disk/by-id/ to ensure stable device mapping across reboots.



## ZFS Migration
- Old pool: 2×8TB mirror
- New pool: 4×8TB RAIDZ1
- Estimated usable: 21.83 TiB



## Design Decision
- Cost efficiency
- Storage density
- Acceptable risk profile
- Non-critical workload
- With 8TB disks, resilver duration is significant. RAIDZ1 was chosen due to cost-efficiency and non-critical workload classification. Important data remains backed up separately. The workload is classified as non-critical. Important data (personal photos and documents) is backed up separately on multiple devices.



## Final Architecture Diagram
```
Physical Layer:
  Fractal Define 7
  LSI SAS2308 HBA
  4×8TB HDD
  2×2TB HDD
  1×3TB HDD

Hypervisor Layer:
  Proxmox VE

VM Layer:
  TrueNAS VM (raw disk passthrough)

Storage Layer:
  tank (RAIDZ1 4×8TB)
  Immich (2×2TB mirror)
  pool_3tb (Workdrive for network-pc's)

```

![Zpool image](images/truenas-tank.png)
Final RAIDZ1 pool state in TrueNAS


![Zpool image](images/hba-card.jpeg)
LSI SAS2308 HBA


![Zpool image](images/back-side.jpg)
Physical disk layout and cabling


![Zpool image](images/Front-side.jpg)
LSI SAS2308 HBA installed in PCIe slot


