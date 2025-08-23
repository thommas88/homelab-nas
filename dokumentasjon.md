# Overskrift (H1)
## Underoverskrift (H2)🔹 Når du bør dokumentere

Planleggingsfasen

Hva var behovet (lagring, redundans, auto-backup, media)?

Hvilke alternativer vurderte du (ZFS i Proxmox vs TrueNAS, UnRAID vs ZFS)?

Hvorfor du valgte TrueNAS-VM på Proxmox → dette viser refleksjon og at du kan vurdere trade-offs.

Under oppsett

Når du installerer Proxmox → skjermbilde av installasjon, notat om at HDDs ble holdt “rå”.

Når du lager TrueNAS-VM → screenshot av VM-innstillinger i Proxmox (CPU, RAM, disk-passthrough).

Når du setter opp ZFS pool i TrueNAS → screenshot + forklaring av “mirror vdev” for redundans.

Når du konfigurerer SMB share → bilde av share-oppsett og Windows som kobler til.

Når du setter opp Immich/Jellyfin → et skjermbilde av appen i TrueNAS og at det fungerer.

Testing / drift

Test av failover: f.eks. “Jeg dro ut en disk, og ZFS rapporterte degraded men dataene var fortsatt tilgjengelige.”

Snapshot-demo: slett en testfil, rull tilbake snapshot.

Ytelsestest: enkel iperf3 eller kopier test for å vise nettverksfart.

Ferdig system

Arkitekturdiagram (enkelt, men profesjonelt).

Oppsummering av funksjoner:

8 TB redundans

SMB/NFS for PC-tilgang

Immich auto-backup av mobilbilder

Jellyfin media streaming

ZFS snapshots + scrubs

Cloud-sync backup

🔹 Hvordan du dokumenterer

Skjermbilder + korte forklaringer (ikke bare walls of text).

Diagrammer (bruk draw.io / Excalidraw / Lucidchart).

Markdown eller PDF (så du kan bruke det på GitHub eller ha en “case study”).

Beskriv det nesten som en liten prosjektrapport:

Introduksjon (hva, hvorfor)

Arkitektur/designvalg

Implementasjon (med skjermbilder)

Testing / resultater

Refleksjon / læring

🔹 Bonus for CV / intervju

På GitHub/portfolio kan du ha en mappe som heter homelab-nas-project med README.md som forteller historien.

Hvis du blir spurt i intervju:

“Hvordan håndterte du redundans?” → du forklarer speil vdev.

“Hvordan testet du backup?” → du viser at du prøvde å gjenopprette slettede filer.

“Hva ville du gjort annerledes om du hadde flere disker?” → snakk om utvidelse med flere vdevs.
