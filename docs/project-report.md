# Homelab NAS Project with Proxmox and TrueNAS SCALE

## Planning Phase

### Requirements
- Storage of data and photos in a safe and robust way.  
- Redundancy (able to survive a single disk failure without data loss).  
- Automatic photo backup from Android phone.  
- Media server (Jellyfin) for movies and TV shows on the living room TV.  
- Access to data from both physical PCs and VMs.  

### Alternatives Considered
- **ZFS directly in Proxmox** with a lightweight Linux container sharing via Samba/NFS.  
- **TrueNAS SCALE as a VM** on Proxmox, with raw disk passthrough.  
- **UnRAID** for flexibility with adding single disks, but without true bitrot protection.  

### Chosen Solution
I decided to run **TrueNAS SCALE as a VM in Proxmox** with raw-disk passthrough.  
Reasons:  
- TrueNAS provides a robust GUI and many built-in services (SMB, NFS, iSCSI, app catalog).  
- ZFS is managed in one place (within TrueNAS), making the system simpler to operate.  
- Strong for portfolio purposes → combination of hypervisor (Proxmox) and enterprise storage solution (ZFS).  

---

## Setup Phase

### Proxmox Installation
- Installed Proxmox on a dedicated system disk (SSD).  
- Left the two 8 TB HDDs untouched so they could be passed raw into TrueNAS.  

*(Screenshot: Proxmox installation)*  

### TrueNAS VM
- Created VM with 8 GB RAM, CPU type `host`, VirtIO NIC.  
- Disks added via passthrough using `/dev/disk/by-id/...`.  

*(Screenshot: VM settings in Proxmox)*  

### ZFS Pool
- Created ZFS pool `tank` in TrueNAS as a mirror (RAID1) of the two 8 TB disks.  
- Result: 8 TB usable capacity with redundancy.  

*(Screenshot: ZFS mirror vdev in TrueNAS)*  

### Network Sharing
- Configured SMB shares for access from Windows.  
- Tested connection from a Windows VM and a physical PC.  

*(Screenshot: Windows connecting to `\\truenas\share`)*  

### Apps
- **Immich** for automatic Android photo backup → pointing to `tank/photos`.  
- **Jellyfin** as a media server → pointing to `tank/media/movies` and `tank/media/tv`.  

*(Screenshot: Immich web-GUI + Jellyfin web-GUI)*  

---

## Testing and Operation

### Failover Test
- Physically disconnected one disk during operation.  
- ZFS marked the pool as `DEGRADED`, but all data remained available.  
- When the disk was reinserted, the rebuild started automatically.  

### Snapshots
- Configured snapshots on `tank/photos`.  
- Test: deleted a file and rolled back snapshot → file successfully restored.  

### Performance
- Ran `iperf3` between Windows PC and NAS → ~1 Gbit/s, as expected.  
- File copy test (large file) → ~110 MB/s over SMB.  

---

## Final System

### Architecture Diagram
*(Insert simple diagram: Proxmox → TrueNAS VM → ZFS Mirror → SMB/Immich/Jellyfin)*  

### Features
- 8 TB redundancy with ZFS mirror.  
- Access via SMB/NFS for PCs and VMs.  
- Automatic photo backup from Android via Immich.  
- Jellyfin media server for movies/TV to the living room.  
- ZFS snapshots and scrubs for data integrity.  
- Optional cloud-sync backup for photos.  

---

## Reflection
- The project provided hands-on experience with both a hypervisor (Proxmox) and a storage platform (TrueNAS + ZFS).  
- I learned about design trade-offs between flexibility (UnRAID) and robustness (ZFS).  
- The setup can be scaled by adding new mirror pairs for increased capacity.  
