---
title: Comparatif d'hyperviseurs

---

# Projet 5 — Comparatif d'hyperviseurs

**Classe :** E1A
**Cours :** Virtualisation
**Date :** `09/10/26`


## 1. Contexte

Un client hésite entre plusieurs solutions de virtualisation.
Je compare trois hyperviseurs :

- **VMware Workstation** (testé) ;
- **VirtualBox** (testé) ;
- **Proxmox VE** (étudié avec la documentation, non testé).

Je termine par une recommandation pour le scénario client.

## 2. Critères de comparaison

| Critère | Ce que j'observe |
|---|---|
| Coût | Prix de la solution |
| Licences | Conditions d'usage (personnel, commercial) |
| Facilité d'administration | Création de la VM, interface |
| Performances | Temps de démarrage, CPU et RAM de l'hôte |
| Écosystème | Éditeur, outils, type de solution |

## 3. Environnement de test

| Élément | Valeur |
|---|---|
| Processeur | Intel Core i7-12700H (14 cœurs, 20 processeurs logiques) |
| RAM | 16 Go DDR4, 3200 MHz |
| Virtualisation matérielle | Activée |
| OS invité | Windows 11 Pro |

**VM de test (identique sur les deux hyperviseurs) :**

| Paramètre | Valeur |
|---|---|
| vCPU | `2` |
| RAM | `4Go` |
| Disque | `60Go` |
| Réseau | NAT |

![2026-10-09_09h14_20](https://hackmd.io/_uploads/Byu7PmIize.png)

![2026-10-09_09h14_27](https://hackmd.io/_uploads/H1jXP7Iofl.png)

**Protocole de mesure :**

1. Éteindre la VM.
2. Lancer le chrono au clic sur le bouton de démarrage.
3. Arrêter le chrono quand le bureau est affiché.
4. Laisser la VM allumée 1 minute.
5. Relever CPU et RAM de l'hôte dans le Gestionnaire des tâches.

## 4. Test 1 — VMware Workstation

- Version : `VMware Workstation Pro 17`
- Création de la VM : `VM vréée depuis une ISO Windows 11`

![2026-10-09_09h14_37](https://hackmd.io/_uploads/ryZzKXIozl.png)

![2026-10-09_09h15_41](https://hackmd.io/_uploads/SkH_tX8iMl.png)

| Mesure | Résultat |
|---|---|
| Démarrage de la VM | **1 min 35 s 43** |
| CPU hôte (VM allumée) | 13 % puis 15 % |
| RAM hôte (VM allumée) | 14,2 Go sur 15,6 Go (91 %) |
| RAM disponible | 1,4 Go |


![WhatsApp Image 2026-10-209 at 10.37.31](https://hackmd.io/_uploads/rkJsKm8oGx.jpg)

**Observations :** `[3 phrases : facilité, interface, problèmes]`

## 5. Test 2 — VirtualBox

- Version : `VirtualBox 7.2.20`
- Création de la VM : VM créée depuis la même ISO Windows 11

![capture-6-version-virtualbox](https://hackmd.io/_uploads/SJn35mLizg.jpg)

![capture-7-parametres-virtualbox](https://hackmd.io/_uploads/S1-65XIiMe.jpg)


| Mesure | Résultat |
|---|---|
| Démarrage de la VM | **52 s 03** |
| CPU hôte (VM allumée) | 10 % puis 13 % |
| RAM hôte (VM allumée) | 14,5 à 14,7 Go sur 15,6 Go (93 à 94 %) |
| RAM disponible | 1,1 Go |

![WhatsApp Image 2026-10-09 at 10.37.31](https://hackmd.io/_uploads/ry7Vs78sMg.jpg)

**Observations :** `[3 phrases : facilité, interface, problèmes]`

## 6. Solution non testée — Proxmox VE

Informations tirées de sources publiques (voir section 11).

| Information | Détail |
|---|---|
| Éditeur | Proxmox Server Solutions (Autriche) |
| Technologie | Virtualisation KVM et conteneurs LXC |
| Administration | Interface web unique |
| Licence | AGPL v3, open source |
| Coût | Utilisable gratuitement, sans limite de CPU, de cœurs ni de VM |
| Abonnement | Optionnel : de 120 à 1 100 € par processeur et par an (support et dépôt de production) |
| Type d'hyperviseur | `[à vérifier dans la documentation officielle]` |

Point d'attention : sans abonnement, un message « No valid subscription » s'affiche à la connexion.

## 7. Licences et coûts des trois solutions

| Solution | Licence | Coût |
|---|---|---|
| VMware Workstation | Propriétaire (non open source) | Gratuit pour l'usage personnel depuis mai 2024. Selon Wikipédia, gratuit aussi pour l'usage commercial depuis novembre 2024.  |
| VirtualBox | Paquet de base : GPLv3, open source | Gratuit. L'Extension Pack (licence PUEL) est gratuit pour l'usage personnel et éducatif, payant pour l'usage commercial |
| Proxmox VE | AGPL v3, open source | Gratuit. Abonnement de support optionnel (120 à 1 100 € par processeur et par an) |

## 8. Tableau comparatif

| Critère | VMware Workstation | VirtualBox | Proxmox VE |
|---|---|---|---|
| Coût | Gratuit (voir section 7) | Gratuit (base) | Gratuit, support payant optionnel |
| Licences | Propriétaire | Open source (GPLv3) + Extension Pack PUEL | Open source (AGPL v3) |
| Administration | Interface graphique avec bibliothèque de VM assistant de création. |Interface graphique avec gestionnaires de machines et assistant de création (curseurs pour RAM, CPU,disque) | Interface web (doc) |
| Performances : démarrage | 1 min 35 s 43 | **52 s 03** | Non testé |
| Performances : CPU hôte | 13 à 15 % | 10 à 13 % | Non testé |
| Performances : RAM hôte | 91 % | 93 à 94 % | Non testé |
| Écosystème | Editer par VMware (Broadcom). Produit propriétaire, installé sur le poste de travail | Edité par Oracle. Open source (GPLv3. Fonctionne sur Windows, macOS et Linux.) | KVM + LXC |

### Différences notables observées

- VirtualBox démarre la VM plus vite que VMware sur mes essais.
- La consommation CPU est faible avec les deux.
- La RAM de l'hôte est très chargée dans les deux cas. L'écart est faible.

### Limites de mes mesures

- Une seule mesure de démarrage par hyperviseur.
- Le CPU varie pendant la mesure.
- Les captures de l'hôte sont des photos au téléphone.
- Proxmox n'est pas testé : les informations viennent de sources publiques.


