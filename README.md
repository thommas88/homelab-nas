# Homelab NAS – Proxmox + ZFS + Samba

## Oversikt
Dette prosjektet er en del av min homelab, hvor jeg har satt opp en NAS-løsning basert på Proxmox og ZFS.  
Målet er å bygge en stabil og fleksibel lagringsplattform som gir sikkerhet, redundans og en plattform for videre eksperimentering.

## Mål
- Sikker lagring med diskredundans  
- Tilgang fra flere enheter (Windows, Linux)  
- Snapshots og enkel restore  
- Image-backup av VM-er og containere  
- Et stabilt fundament for videre prosjekter  

## Arkitektur
**Maskinvare:** i9-9900K, 32GB RAM, 2×8TB HDD (NAS), 2×2TB HDD (foto-backup), 250GB SSD (OS/VM)  

**Programvare og tjenester:**  
- Proxmox VE som hypervisor  
- ZFS med separate pools for NAS og foto-backup  
- Samba for nettverksdeling  
- Immich for foto-backup  
- Proxmox Backup Server for image-backup  
- Prometheus + Grafana for overvåkning  

## Hva jeg lærte
- Oppsett og administrasjon av ZFS mirrors i praksis  
- Hvordan datasets og snapshots kan brukes til organisering og databeskyttelse  
- Grunnleggende Samba-konfigurasjon for deling i nettverk  
- Integrasjon av image-backup og snapshots i en helhetlig strategi  
- Overvåkning av ressurser og dataintegritet med Prometheus/Grafana  

## Neste steg
- Fullføre oppsett av offsite-backup med rclone (kryptert)  
- Varsler i Grafana (Discord/Telegram/webhooks)  
- Vurdere caching med SSD/NVMe for bedre NAS-ytelse  

---

Detaljert prosjektrapport finnes her:  
👉 [PROJECT_REPORT.md](./PROJECT_REPORT.md)
