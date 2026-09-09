---
title: Installation de Windows 11 sur VMware Workstation Pro 17
tags: VMware, Windows 11, Installation
---

# Installation de Windows 11 sur VMware Workstation Pro 17

[TOC]

---

## 1. Création de la machine virtuelle

Dans VMware Workstation Pro : **New Virtual Machine** → **Typical** → sélectionner l'ISO de Windows 11 → système invité **Microsoft Windows / Windows 11 x64**.

**Configuration matérielle recommandée :**
- **Disque dur : 64 Go**, en "disque scindé en plusieurs fichiers" (plus facile à déplacer/copier la VM ensuite qu'un fichier monolithique de 64 Go).
- **Mémoire vive : 4 Go minimum**, 6 Go conseillé si la machine hôte le permet (Windows 11 tourne correctement à partir de 4 Go, mais 6-8 Go évite les ralentissements une fois plusieurs applications ouvertes).
- **Processeurs : au moins 2 cœurs**, idéalement 4 à 8 selon le CPU de la machine hôte (Windows 11 exige officiellement 2 cœurs minimum).
- **TPM (Trusted Platform Module) : obligatoire**. Windows 11 refuse de s'installer sans TPM activé — VMware en fournit un virtuel automatiquement, mais demande de définir un mot de passe de chiffrement pour celui-ci. **Ce mot de passe est à conserver**, sans lui la VM ne redémarre plus.


![WhatsApp Image 2026w-09-09 at 15.13.07](https://hackmd.io/_uploads/HkOEvVJFfg.jpg)

---

## 2. Démarrage sur l'ISO et lancement de l'installation

Démarrer la VM. Au message **"Press any key to boot from CD or DVD"**, appuyer sur une touche immédiatement (délai très court, sinon la VM tente de démarrer sur le disque vide et il faut relancer la VM).

Choisir **Installer Windows 11**. Si aucune licence n'est disponible, cliquer sur **"Je n'ai pas de clé de produit"** — Windows s'installera quand même et pourra être activé plus tard (ou restera en mode non activé avec filigrane, sans limitation fonctionnelle bloquante). Accepter les conditions de licence pour continuer.


![WhatsApp Image 2026-09-092 at 15.13.07](https://hackmd.io/_uploads/ByJRENkFMx.jpg)


---

## 3. Partitionnement et installation

Sélectionner l'espace disque non alloué créé à l'étape 1 (ici 64 Go), cliquer sur **Suivant**. Windows copie les fichiers et redémarre plusieurs fois automatiquement (comportement normal, ne pas interrompre).

![WhatsApp Image 2026-09-209 at 15.13.08](https://hackmd.io/_uploads/S1Z-SVJtzx.jpg)


---

## 4. Contourner l'obligation de connexion réseau/compte Microsoft

**Point le plus utile de ce guide.** Depuis une mise à jour récente, Windows 11 impose une connexion Internet et un compte Microsoft pendant la configuration initiale (OOBE), même en usage local/hors ligne. Pour l'éviter :

1. Sur l'écran de connexion réseau (ou celui imposant un compte Microsoft), appuyer sur **Maj + F10** pour ouvrir une invite de commandes.
2. Taper : OOBE\BYPASSNRO

3. Lancer la commande de contournement (celle qui débloque l'option **"Je n'ai pas de connexion Internet"**).
4. Fermer l'invite de commandes : la configuration continue normalement, avec un compte local classique au lieu d'un compte Microsoft.

![WhatsApp Image 23026-09-09 at 19.49.58](https://hackmd.io/_uploads/ryT_rEJtMl.jpg)


---
## 5. Finalisation et premier bureau

Une fois le compte configuré (compte local ou Microsoft selon le choix), Windows termine l'installation en récupérant les dernières mises à jour de sécurité (plusieurs redémarrages possibles, c'est normal). Le bureau final s'affiche ensuite : l'installation est terminée et la VM est utilisable.

![WhatsApp Image 2026-09-029 at 17.44.22](https://hackmd.io/_uploads/rkiiBEyKGl.jpg)

# 6.Preuve que la VM marche


![2026-09-09_22h13_55](https://hackmd.io/_uploads/BkGGgS1tze.png)

# 6.2 Renommage du PC

![2026-09-09_22h16_12](https://hackmd.io/_uploads/SyqkxrJtzg.png)

