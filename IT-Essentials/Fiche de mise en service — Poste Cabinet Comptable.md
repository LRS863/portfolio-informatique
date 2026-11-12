---
title: Fiche de mise en service — Poste Cabinet Comptable

---

# Projet 1 — Poste Cabinet Comptable

**Nom du poste :** CABCOMPTA-PC2
**Système d'exploitation :** Windows 10 Professionnel (22H2)
**Date d'installation :** 11/09/2026
**Date de mise en service :** 14/09/2026

---

## 1. Installation du système d'exploitation

- Installation de Windows 10 Professionnel (build 22H2) réalisée depuis le support fourni.
- Édition confirmée dans **Paramètres > Système > À propos de**.

---

## 2. Configuration des comptes utilisateurs

- Création d'un compte utilisateur standard via **Paramètres > Comptes > Famille et autres utilisateurs**.
  - Nom d'utilisateur : `Colaboratrice`
  - Mot de passe défini avec question de sécurité en cas d'oubli.
- Compte administrateur local conservé en parallèle pour la gestion du poste.
![comptable1](https://hackmd.io/_uploads/S1KavEBKze.png)

---

## 3. Logiciels installés

| Logiciel | Usage | Statut |
|---|---|---|
| PDF Suite 20 | Lecture/édition de PDF (alternative à Adobe Acrobat) | ✅ Installé |
| LibreOffice 26.8.0.3 | Suite bureautique (Writer, Calc, Impress) | ✅ Installé |
| Microsoft Edge | Navigateur web, favoris métiers configurés (Microsoft 365, Booking.com, CFF, MySwitzerland, etc.) | ✅ Opérationnel |

![comptable2](https://hackmd.io/_uploads/SJsydVHFGx.png)
![comptable3](https://hackmd.io/_uploads/S1UeuNHtze.png)
![comptable4](https://hackmd.io/_uploads/S1lbuNHFGe.png)
![comptable5](https://hackmd.io/_uploads/SJP-_4rKGe.png)
![comptable6](https://hackmd.io/_uploads/SJgGuVHtMx.png)
![comptable7](https://hackmd.io/_uploads/ryFfOVrYfx.png)

---

## 4. Configuration réseau et imprimante

### Réseau
- Nom du PC renommé : `PC05-W10` → **`CABCOMPTA-PC2`** (via Paramètres > Système > À propos de > Renommer ce PC).
![comptable8](https://hackmd.io/_uploads/SJPouVrtzl.png)
- Redémarrage effectué pour appliquer le nouveau nom.
![comptable9](https://hackmd.io/_uploads/SkPnO4SFzl.png)
- Configuration IPv4 de la carte Ethernet0 : obtention automatique de l'adresse IP et des serveurs DNS (DHCP).
![comptable11](https://hackmd.io/_uploads/SJGRdNSYGe.png)
![comptable12](https://hackmd.io/_uploads/HkUCdVBFGg.png)
- Groupe de travail vérifié dans **Propriétés système > Nom de l'ordinateur** : le poste `CABCOMPTA-PC2` est membre du groupe de travail **WORKGROUP**.
![comptable 13](https://hackmd.io/_uploads/ryvJFNHKfl.png)
### Imprimante
- Consultation de **Périphériques et imprimantes** : seules les imprimantes virtuelles/logicielles sont présentes (Fax, Microsoft Print to PDF, Microsoft XPS Document Writer, OneNote, PDF Suite 20).
![comptable14](https://hackmd.io/_uploads/HJBbYVSYfl.png)
- ⚠️ **Aucune imprimante physique partagée n'a pu être configurée ni testée** : la machine virtuelle utilisée pour ce projet ne disposait pas d'accès à une imprimante physique partagée sur le réseau local. L'étape « ajouter une imprimante réseau partagée et tester une impression » n'a donc pas pu être réalisée dans les conditions du labo.

---

## 5. Mises à jour, sécurité et pare-feu

- **Windows Update** : poste à jour (dernière vérification effectuée le jour de la mise en service).
![comptable15](https://hackmd.io/_uploads/SJULKNSFzx.png)
![comptable16](https://hackmd.io/_uploads/HkkvYESYGg.png)
- **Sécurité Windows / Pare-feu** :
  - Réseau avec domaine : pare-feu activé.
  - Réseau privé : pare-feu activé.
  - Réseau public (actif) : pare-feu activé.
- Aucune alerte de sécurité signalée dans le Centre de sécurité Windows.
![comptable17](https://hackmd.io/_uploads/SJ7Ot4StMx.png)
![comptable18](https://hackmd.io/_uploads/BkRuFEBYfe.png)
![comptable21](https://hackmd.io/_uploads/r1cKKEBtzx.png)
---

## 6. Récapitulatif des livrables

- [x] Poste opérationnel présenté au formateur.
- [x] Fiche de mise en service (ce document).
- [x] Captures d'écran des étapes clés (création de compte, installation logicielle, renommage du PC, configuration réseau, imprimantes, mises à jour, pare-feu).

---

## 7. Points d'attention / limites rencontrées

- Configuration de l'imprimante réseau partagée non testable faute d'imprimante physique disponible dans l'environnement virtuel.
- Le reste des étapes (installation OS, comptes, logiciels, réseau, mises à jour, sécurité) a été réalisé et vérifié avec succès.