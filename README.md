# Homelab NAS – Proxmox + ZFS + Samba

## Oversikt
Dette prosjektet er en del av min homelab, hvor jeg har satt opp en NAS-løsning basert på Proxmox og ZFS.  
Målet er å bygge en stabil og fleksibel lagringsplattform som gir sikkerhet, redundans og en plattform for videre eksperimentering.

## Mål
- Sikker lagring med diskredundans  
- Tilgang fra flere enheter (Windows og Linux)  
- Snapshots og enkel restore  
- Image-backup av VM-er og containere  
- Et stabilt fundament for videre prosjekter  

## Arkitektur
**Maskinvare:** i9-9900K, 32GB RAM, 2×8TB HDD (NAS), 2×2TB HDD (foto-backup), 250GB SSD (OS/VM)  

**Programvare og tjenester:**  
- Proxmox VE som hypervisor  
- TrueNAS SCALE som NAS-VM (ZFS mirror pool)  
- Samba for nettverksdeling  
- Immich for foto-backup  
- Jellyfin for media-streaming  
- Proxmox Backup Server for image-backup  
- Snapshots og scrubs med ZFS  

## Hva jeg lærte
- Oppsett og administrasjon av ZFS mirrors i praksis  
- Hvordan datasets og snapshots kan beskytte data  
- Grunnleggende nettverksdeling med Samba  
- Erfaring med selvhostede tjenester som Immich og Jellyfin  
- Integrasjon av snapshots og image-backup i en helhetlig strategi  

## Videre arbeid
- Fullføre offsite-backup (rclone med kryptering)  
- Varsler i Grafana (Discord/Telegram/webhooks)  
- Vurdere SSD/NVMe caching for bedre NAS-ytelse  

---

## Om scope og vurderinger
Jeg har vurdert flere enterprise-funksjoner (for eksempel full overvåkning med Prometheus/Grafana og avansert sikkerhetstiltak som VLAN-segmentering).  
For en homelab i denne størrelsen ble dette vurdert som unødvendig komplekst, men er notert som mulige fremtidige utvidelser.  

---

Detaljert prosjektrapport finnes her:  
👉 [PROJECT_REPORT.md](./PROJECT_REPORT.md)
