## Immich – Paths, IPs og arkitektur

### IP-adresser
- **Immich VM (VM 106):** `192.168.0.113`
- **TrueNAS VM (VM 101):** `192.168.0.55`

### Immich VM
- **OS:** Debian 12
- **App path:** `/opt/immich`
- **Media mountpoint:** `/srv/immich`
- **Immich web UI:** `http://192.168.0.113:2283`

### TrueNAS
- **Pool:** `immichpool`
- **Dataset:** `immich-data`
- **NFS export path:** `/mnt/immichpool/immich-data`

### Persistent mount
Immich VM mounts the TrueNAS dataset via NFS to:

```fstab
192.168.0.55:/mnt/immichpool/immich-data /srv/immich nfs defaults,_netdev,nofail 0 0
```

### Storage layout
- **Database:** local to the Immich VM
- **Docker / Compose / config:** local to the Immich VM
- **Photos / videos / thumbnails / encoded media / backups:** stored on TrueNAS via NFS mount at `/srv/immich`

### Architecture
- **Proxmox** runs both the Immich VM and the TrueNAS VM.
- **Immich** runs in a dedicated Debian VM using Docker Compose.
- **PostgreSQL** remains on the VM disk for stability and simplicity.
- **Media storage** is offloaded to a dedicated TrueNAS mirror pool.
- **TrueNAS** provides the storage over NFS to the Immich VM.

This separates:
- **Application + DB** from **bulk media storage**
- **Compute** from **NAS storage**
- **VM system disk** from **photo/video library**

### Validation completed
- Immich web interface reachable
- Test uploads successful
- Files confirmed under `/srv/immich/upload`
- TrueNAS pool usage increased after upload
- NFS mount survives reboot
- All Immich containers healthy after reboot
