# Homelab NAS Project with Proxmox and TrueNAS SCALE

## System Specifications

**Host Server (Proxmox VE)**
- CPU: Intel i9-9900K
- RAM: 32 GB DDR4
- Boot Drive: 1 × 500 GB SSD (Proxmox OS)
- Storage Drives:
  - 2 × 8 TB HDD (ZFS mirror for NAS storage)
  - 2 × 2 TB HDD (reserved for testing or secondary pool)
- GPU: NVIDIA GTX 1070 Ti (not used for this project, possible for Jellyfin hardware transcoding later)
- Network: 1 Gbit/s LAN

**Virtual Machines**
- **TrueNAS SCALE VM**
  - CPU: 4 vCPUs (host-passthrough)
  - RAM: 8–12 GB
  - Storage: 2 × 8 TB raw disk passthrough (ZFS mirror pool `tank`)
- **Windows 11 VM (test client)**
  - CPU: 2 vCPUs
  - RAM: 4 GB
  - Purpose: verify SMB/NFS connectivity and NAS functionality

**Operating System Versions**
- Proxmox VE 8.x
- TrueNAS SCALE 24.x
- Windows 11 Pro (for test-VM)

> The two 2 TB HDDs are currently reserved for additional testing (e.g., creating a secondary pool or acting as a backup target).  
> Power/UPS has been considered, and might be added later.

---

## Planning Phase

### Requirements
- Storage of data and photos in a safe and robust way  
- Redundancy (able to survive a single disk failure without data loss)  
- Automatic photo backup from Android phone  
- Media server (Jellyfin) for movies and TV shows on the living room TV  
- Access to data from both physical PCs and VMs  
- **Security/Backup requirement:** Data integrity checks (bitrot protection) and offsite backup considered essential for photos  

### Alternatives Considered
- **ZFS directly in Proxmox** with a lightweight Linux container sharing via Samba/NFS  
- **TrueNAS SCALE as a VM** on Proxmox, with raw disk passthrough  
- **UnRAID** for flexibility with adding single disks, but without true bitrot protection  

### Chosen Solution
I decided to run **TrueNAS SCALE as a VM in Proxmox** with raw-disk passthrough.  

**Reasons:**
- TrueNAS provides a robust GUI and many built-in services (SMB, NFS, iSCSI, app catalog)  
- ZFS is managed in one place (within TrueNAS), simplifying operation  
- Strong for portfolio purposes → combination of hypervisor (Proxmox) and enterprise storage solution (ZFS)  

---

## Setup Phase

### Proxmox Installation
- Installed Proxmox on a dedicated system SSD  
- Left the two 8 TB HDDs untouched so they could be passed raw into TrueNAS  

### TrueNAS VM
- Created VM with 8 GB RAM, CPU type `host`, VirtIO NIC  
- Disks added via passthrough using `/dev/disk/by-id/...`  

### ZFS Pool
- Created ZFS pool `tank` in TrueNAS as a mirror (RAID1) of the two 8 TB disks  
- Result: 8 TB usable capacity with redundancy  

### Network Sharing
- Configured SMB shares for access from Windows  
- Tested connection from a Windows VM and a physical PC  

### Apps
- **Immich** for automatic Android photo backup → `tank/photos`  
- **Jellyfin** for media streaming → `tank/media/movies` and `tank/media/tv`  

---

## Testing and Operation

### Failover Test
- Physically disconnected one disk during operation  
- ZFS marked the pool as `DEGRADED`, but all data remained available  
- When the disk was reinserted, the rebuild started automatically  

### Snapshots
- Configured snapshots on `tank/photos`  
- Schedule:  
  - Daily snapshots kept for 30 days  
  - Weekly snapshots kept for 12 weeks  
  - Monthly snapshots kept for 12 months  
- Test: deleted a file and rolled back snapshot → file successfully restored  

### Scrubs
- Monthly scrubs scheduled on the `tank` pool to ensure data integrity  

### Performance
- `iperf3` between Windows PC and NAS → ~1 Gbit/s  
- File copy test (large file) → ~110 MB/s over SMB  

---

## Considerations and Scope

This homelab project was designed to be realistic and not overly complex.  
Several enterprise-level features were considered, but deliberately not implemented:

- **Offsite Backup:**  
  Proxmox Backup Server and `rclone` to encrypted cloud storage were considered.  
  For now, local ZFS redundancy and snapshots are sufficient. Offsite backup remains a future option.

- **Monitoring:**  
  A full monitoring stack (Prometheus + Grafana) was evaluated.  
  While powerful, it was deemed unnecessary overhead for a small homelab. It remains an area for learning if the project is expanded.

- **Security:**  
  Options such as VLAN segmentation, disk encryption, and advanced access control were considered.  
  For a home environment, these were seen as excessive, but they are standard practices in professional IT environments.

By including these considerations, the project shows awareness of best practices, while staying within a realistic scope for a homelab.

---

## Final System

### Features
| Feature                        | Status   |
|--------------------------------|----------|
| 8 TB redundant ZFS mirror      | ✅ Active |
| SMB/NFS access for PCs & VMs   | ✅ Active |
| Automatic photo backup (Immich)| ✅ Active |
| Media streaming (Jellyfin)     | 🔄 Planned |
| ZFS snapshots & scrubs         | ✅ Active |
| Cloud-sync backup              | 🔄 Planned |

---

## Reflection and Lessons Learned
- Gained hands-on experience with ZFS: pools, snapshots, scrub, resilver  
- Learned how to passthrough raw disks into a VM in Proxmox  
- Practiced failover and recovery with ZFS mirrors  
- Deployed and managed self-hosted apps (Immich, Jellyfin)  
- Understood trade-offs between GUI-driven solutions (TrueNAS) and CLI setups (native ZFS in Proxmox)  
- Considered but scoped out more advanced features (monitoring, offsite backup, VLAN security) → showing awareness of best practices  

---

## Future Work
- Implement encrypted offsite-backup with `rclone`  
- Add alerting via Grafana or TrueNAS reporting  
- Evaluate SSD/NVMe caching for faster NAS performance  
- Scale storage by adding new ZFS mirrors (e.g., 2×16 TB)  
