---
title: Déploiement standardisé d'un poste (image système)

---

# Projet 5-Déploiement standardisé d'un poste (image système)


# 1. Contexte

Une entreprise doit équiper 10 nouveaux postes identiques. Ce document décrit une méthode standardisée de déploiement rapide, basée sur une image système obtenue par clonage d'une machine virtuelle de référence sous VMware.

# 2. Cahier des charges du poste standard

* Navigateur:Google Chrome ou Mozilla Firefox
* Suite Bureautique:LibreOffice
* Lecteur PDF: lecteur intégré Windows
* Antivirus:Windows Defender (inclus nativement)
* Mises à jour Windows:entièrement installées
* configuration réseau: DHCP (adress IP automatique)
* Compte utilisateur:nom générique(ex."Utilisateur")

# 3.Préparation du poste de référence

La VM de référence a été configurée sous VMware Workstation avec Windows 11, puis tous les logiciels du cahier des charges ont été installés et les mises à jour Windows appliquées intégralement
![Dure1](https://hackmd.io/_uploads/Sk9sUn5tMx.jpg)

# 4. Création de l'image système (Sysprep+clonage)

**4.1 Généralisation avec Sysprep**

Avant le clonage, l'outil Sysprep
(C:\Windows\System32\Sysprep\sysprep.exe) a été utilisé afin de généraliser l'installation: suppression des identifiants uniques (SID,nom de machine) pour permettre un déploiement sur plusieurs postes sans conflits.

**Option utilisées**: action système "Entrer en mode OOBE", case "Généraliser" cochée, arrêt du système en fin de traitement.

Une erreur ("Sysprep n'a pas pu valider votre installation de Windows") a été rencontrée lors d'une première tentative, causée par des applications du Microsoft Store non provisionnées pour tous les comptes (Xbox,Skype,BingWeather,Solitaire). Ces applications ont été supprimées via PowerShell:

![dure4](https://hackmd.io/_uploads/Hygc53qYfx.png)

![dure2](https://hackmd.io/_uploads/HytH_h5FMe.png)

**4.2 Clonage de la machine virutelle**

Une fois la VM éteinte par Sysprep, un clone complet ("Full clone") a été créé dans VMware Workstation via Manage>Clone, à partir de l'état courant de la machine. Le clone constitue l'image système déployable, indépedante de la VM d'origine.
![2026-09-18_15h22_24](https://hackmd.io/_uploads/Skwrjh5Kzg.png)

# 5. Test du déploiement sur une seconde machine

le clone ("Clone-of-PC04_LP") a été démarré pour simuler le déploiement sur un second poste. L^écran de configuration initiale (OOBE) s'est présenté comme sur un poste neuf:langue,région,compte utilisateur, connexion réseau.

Après configuration, le bureau du clone a été vérifié:tous les logiciels du cahier des charges (Chrome/Firefox,LibreOffice) et les mises a jour étaient présents, identiques à la VM de référence. Le test est concluant
![clone_desktop](https://hackmd.io/_uploads/rkERj25FMl.jpg)


---

## 6. Comparatif des temps : installation manuelle vs déploiement automatisé

| Tâche | Manuel | Automatisé (clonage) |
|:--|:-:|:-:|
| Installation de Windows 10/11 | 25-30 min | Inclus dans l'image (0 min) |
| Installation des logiciels (Chrome, LibreOffice, 7-Zip) | 10-15 min | Inclus dans l'image (0 min) |
| Mises à jour Windows | 20-30 min | Déjà à jour (0 min) |
| Configuration du compte / réseau | 5 min | Écran OOBE (~5 min) |
| Clonage de la VM (VMware, full clone) | — | 10-15 min |
| **Total estimé** | **≈ 70 min** | **≈ 17 min** |

### Calcul du gain de temps

| Étape du calcul | Résultat |
|:--|:--|
| Temps manuel | 70 minutes |
| Temps automatisé | 17 minutes |
| Temps gagné | 70 − 17 = **53 minutes** |
| Gain en % | 53 ÷ 70 × 100 ≈ **76 %** |

Le déploiement automatisé via image système permet donc un gain de temps très significatif (environ 76 %), particulièrement rentable pour équiper les 10 postes identiques demandés dans le contexte de l'entreprise.

# 7. Conclusion

La méthode par image système (clonage de VM, précédée d'une généralisation Sysprep) permet de déployer rapidement et de façon fiable un poste standardisé conforme au cahier des charges. Elle réduit fortement le temps de mise en service par rapport à une installation manuelle répétée sur chaque poste, ce qui est particulièrement adapté au déploiement de 10 postes identiques.
