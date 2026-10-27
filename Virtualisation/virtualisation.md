# Virtualisation (hyperviseur de type 1)

Couvre : OSI-700-1 à 5, SO-OSI-700-C21 à C25, ASI-5, ASI-6.

> La grille cite VMware et Proxmox VE ; votre infra tourne sur **Hyper-V**
> (SRV-HYPER-V, GROUPE5-HYPERV). Les notions sont les mêmes : savoir les
> nommer dans les trois.

## OSI-700-1 — Hyperviseur de type 1

| Type | Où il tourne | Exemples |
|---|---|---|
| **Type 1 (bare-metal)** | directement sur le matériel | VMware ESXi, Proxmox VE (KVM), Hyper-V, XCP-ng |
| Type 2 (hébergé) | sur un OS hôte | VirtualBox, VMware Workstation |

Type 1 = meilleures performances, isolation, usage en production. Gestion
centralisée : vCenter (VMware), cluster Proxmox, Failover Cluster / SCVMM
(Hyper-V).

## OSI-700-2 — Clonage

- **Clone complet** : copie indépendante des disques.
- **Clone lié** (*linked clone*) : partage le disque parent, rapide mais
  dépendant.
- **Après un clonage Windows** : **Sysprep** (`/generalize`) pour régénérer
  le **SID** et éviter les conflits dans le domaine ; nouvelle adresse MAC,
  nouveau nom, nouvelle IP. Sous Linux : régénérer `machine-id`, clés SSH.
- **Modèle** (*template*) : VM généralisée, figée, dont on déploie de
  nouvelles VM (niveau « Maîtrisé » d'OSI-700-3).

## OSI-700-3 / C23 — Ressources et création des VM

vCPU, RAM (éviter le surengagement en production), disques (provisionnement
**fin** ou **épais**), carte réseau, ISO. Pour la production : outils
invités (VMware Tools, QEMU guest agent, services d'intégration Hyper-V),
démarrage automatique, ordre de démarrage (AD avant SQL avant API).

## OSI-700-4 / C22 — Réseaux virtuels

| VMware | Proxmox | Hyper-V |
|---|---|---|
| vSwitch standard / distribué | Linux bridge `vmbr0` / OVS | Commutateur virtuel **externe / interne / privé** |
| Groupe de ports (VLAN) | VLAN-aware bridge | VLAN ID sur la carte de la VM |
| Carte physique (uplink) | interface physique liée au bridge | carte physique liée au commutateur externe |

Séparer les flux : administration, production, DMZ, stockage, réplication
(Always On).

## C24 — Stockage

Local (LVM, ZFS), partagé (NFS, iSCSI, SMB 3), Ceph (Proxmox). Prévoir
des espaces distincts pour les **disques de VM**, les **ISO**, les
**modèles** et les **sauvegardes**.

## C25 — Sécurisation et sauvegardes

Comptes nominatifs et rôles, accès d'administration sur un réseau dédié,
mises à jour, **sauvegardes planifiées** (Proxmox Backup Server, Veeam,
Windows Server Backup) et **tests de restauration** (c'est ce qui fait
« Maîtrisé »). Règle **3-2-1** : 3 copies, 2 supports, 1 hors site.

## OSI-700-5 / ASI-5 / ASI-6 — Documenter

Voir [Architecture d'Argus](../Architecture/architecture-argus.md) : schéma
physique (hyperviseurs, VM, réseaux virtuels) + schéma logique (rôles et
flux), ressources de chaque VM, plan d'adressage, procédure de reprise.
