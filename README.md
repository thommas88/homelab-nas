# Homelab NAS – Proxmox + ZFS + Samba

## Mål
Sette opp en NAS-løsning i hjemmenettverket mitt som gir:
- Sikker lagring (diskredundans).
- Tilgang fra flere enheter (Windows, Linux).
- Mulighet for snapshots og enkel restore.
- Et stabilt fundament for videre prosjekter i homelaben.

## Arkitektur
-  **Maskinvare:** i9-9900K, 32GB RAM, 2×8TB HDD (NAS), 2×2TB HDD (foto-backup), 250GB SSD (OS/VM).
- **Hypervisor:** Proxmox VE.
- **Lagring:** ZFS mirrors i to separate pools (`pool_nas`, `pool_photos`).
- **Tjenester:** LXC med Samba (NAS), LXC med Immich (foto-backup).
- **Backup:** ZFS snapshots ukentlig + offsite-kopi (planlagt med rclone).


## Implementasjon
1. Opprettet ZFS pool `pool_nas` i Proxmox (mirror av 2×8TB).
2. Laget egne datasets for ulike bruksområder:
   - `share` (daglig bruk).
   - `archives` (langtidslagring).
   - `backups` (Proxmox-backuper, dump-filer).
3. Opprettet en LXC-container (`nas-server`) basert på Debian.
4. Installerte og konfigurerte **Samba**:
   - Opprettet brukere og grupper.
   - Delte ut `\\nas\share` med passende rettigheter.
5. Testet tilgang fra Windows- og Linux-klienter.

## Sikkerhet og drift
- ZFS scrubs planlagt månedlig (sjekker dataintegritet).
- ZFS snapshots ukentlig (rotasjon på 3 måneder).
- Mail-varsler fra Proxmox ved diskfeil eller pool-feil.
- Planlagt offsite-backup med `rclone` (til skylagring, kryptert).

## Skjermbilder
- Proxmox dashboard med ZFS pool.
- Samba share tilgjengelig fra Windows Explorer.
- Snapshot-plan i Proxmox.

## Hva jeg lærte
- Hvordan sette opp og administrere ZFS mirrors i praksis.
- Hvordan datasets og snapshots kan brukes til å organisere og beskytte data.
- Grunnleggende Samba-konfigurasjon for deling over nettverk.
- Viktigheten av å planlegge backup-strategi utover lokal redundans.

---
**Neste steg:** Integrere NAS-oppsettet med monitoring (Prometheus/Grafana) og sette opp automatisk offsite-backup.

