# Homelab NAS-prosjekt med Proxmox og TrueNAS SCALE

## Planleggingsfasen

### Behov
- Lagring av data og bilder på en trygg og robust måte.  
- Redundans (tåle at én disk ryker uten datatap).  
- Automatisk bildebackup fra Android-telefon.  
- Mediaserver (Jellyfin) for filmer og TV-serier til stue-TV.  
- Tilgang til data fra både fysiske PC-er og VMer.  

### Vurderte alternativer
- **ZFS i Proxmox direkte** med en lett Linux-container som deler via Samba/NFS.  
- **TrueNAS SCALE som VM** på Proxmox, med disker passthrough.  
- **UnRAID** for fleksibilitet med én-og-én disk, men uten ekte bitrot-beskyttelse.  

### Valgt løsning
Jeg valgte å kjøre **TrueNAS SCALE som en VM i Proxmox** med rå-disk passthrough.  
Grunnene:  
- TrueNAS gir et ferdig, robust GUI og mange innebygde tjenester (SMB, NFS, iSCSI, app-katalog).  
- ZFS håndteres ett sted (i TrueNAS), som gjør systemet enklere å drifte.  
- Godt å vise i portefølje → kombinasjon av hypervisor (Proxmox) og enterprise lagringsløsning (ZFS).  

---

## Under oppsett

### Proxmox installasjon
- Installerte Proxmox på egen systemdisk (SSD).  
- Lot de to 8 TB HDD-ene stå urørte slik at de kunne gis rått til TrueNAS.  

*(Skjermbilde: Proxmox installasjon)*  

### TrueNAS VM
- Opprettet VM med 8 GB RAM, CPU type `host`, VirtIO NIC.  
- Diskene lagt til som passthrough via `/dev/disk/by-id/...`.  

*(Skjermbilde: VM-innstillinger i Proxmox)*  

### ZFS Pool
- Opprettet ZFS-pool `tank` i TrueNAS som et mirror (RAID1) av de to 8 TB diskene.  
- Resultat: 8 TB brukbar kapasitet med redundans.  

*(Skjermbilde: ZFS mirror vdev i TrueNAS)*  

### Nettverksdeling
- Konfigurerte SMB-shares for tilgang fra Windows.  
- Testet tilkobling fra en Windows-VM og en fysisk PC.  

*(Skjermbilde: Windows kobler til `\\truenas\share`)*  

### Apper
- **Immich** for automatisk bildebackup fra Android → peker til `tank/photos`.  
- **Jellyfin** for mediaserver → peker til `tank/media/movies` og `tank/media/tv`.  

*(Skjermbilde: Immich web-GUI + Jellyfin web-GUI)*  

---

## Testing og drift

### Failover-test
- Dro ut én disk fysisk under drift.  
- ZFS markerte pool som `DEGRADED`, men alle data var tilgjengelige.  
- Etter at disken ble satt inn igjen, rebuild startet automatisk.  

### Snapshots
- Konfigurerte snapshots på `tank/photos`.  
- Test: slettet en fil og rullet tilbake snapshot → fil gjenopprettet.  

### Ytelse
- Kjørte `iperf3` mellom Windows-PC og NAS → ~1 Gbit/s, som forventet.  
- Filkopieringstest (stor fil) → ~110 MB/s over SMB.  

---

## Ferdig system

### Arkitekturdiagram
*(Legg inn et enkelt diagram: Proxmox → TrueNAS VM → ZFS Mirror → SMB/Immich/Jellyfin)*  

### Funksjoner
- 8 TB redundans med ZFS mirror.  
- Tilgang via SMB/NFS for PC-er og VMer.  
- Automatisk bildebackup fra Android via Immich.  
- Jellyfin mediaserver for filmer/serier til TV.  
- ZFS snapshots og scrubs for dataintegritet.  
- Cloud-sync backup av bilder (valgfritt).  

---

## Refleksjon
- Prosjektet ga erfaring med både hypervisor (Proxmox) og lagringsplattform (TrueNAS + ZFS).  
- Jeg lærte om designvalg mellom fleksibilitet (UnRAID) og robusthet (ZFS).  
- Oppsettet kan skaleres ved å legge til nye mirror-par for mer kapasitet.  
