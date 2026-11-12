---
title: Procédure de sauvegarde et de réinstallation de Windows 11.

---

# Procédure de sauvegarde et de réinstallation de Windows 11.

# 1. Préparation des données

Avant de réinstaller Windows 11, j'ai identifié les données qu'il était nécessaire de conserver.
J'ai donc créé un dossier **Sauvegarde_Test** sur le Bureau contenant deux fichiers texte de test.

Ces fichiers servent à vérifier que la sauvegarde et que la restauration fonctionnent correctement.

![sauvegarde1](https://hackmd.io/_uploads/H10fDg_Yzl.png)

# 2. Inventaire des données

J'ai ensuite vérifié le contenu de mon profil urilisateur dans:

**C: \Users\NomPC**

J'ai identifié les principaux dossiers susceptibles de contenir des données personnelles, comme le Bureau, les Documents, les Téléchargements et les Images.


# 3. Préparation du support de sauvegarde

Comme la réinstallation allait supprimer les données présentes sur le disque de la VM, j'ai utilisé un dossier partagé VMware situé sur l'ordinateur hôte.

J'ai activé les dossiers partagés dans VMware Workstation et sélectionné l'option Always enabled.

J'ai ensuite créé le dossier partagé **Sauvegarde_VM**

![sauvegarde2](https://hackmd.io/_uploads/S1-f9gdYGe.jpg)


# 4. Réalisation de la sauvegarde

J'ai copié les fichiers présents dans **Sauvegarde_Test** vers:

**\\\vmware-host\Shared Folders\Sauvegarde_VM**

Les fichiers sont ainsi conservés en dehors du disque principal de la VM.
![sauvegarde3](https://hackmd.io/_uploads/rklzsxdYGl.jpg)

# 5. Vérification de la sauvegarde

Avant de réinstaller Windows, j'ai vérifié que les fichiers sauvegardés étaient bien accessibles et que leur contenu était toujours présent.
![Sauvegarde5](https://hackmd.io/_uploads/BJ0aoxOFMx.jpg)

# 6. Réinstallation de Windows 11

J'ai arrêté la VM afin de pouvoir démarrer sur l'image ISO de Windows 11.

Dans les paramètres VMware, j'ai vérifier que l'ISO de Windows 11 était bien sélectionné dans le **lecteur CD/DVD**,

![sauvegarde6](https://hackmd.io/_uploads/HJZOneuFMe.png)

J'ai ensuite démarré la VM et sélectionné le démarrage sur le lecteur CD/DVD afin de lancer l'installation de Windows 11.
![sauvegarde7](https://hackmd.io/_uploads/B1Brpl_Kfe.png)

Lors du choix de l'emplacement d'installation, j'ai sélectionné le disque virtuel principal de la VM.
![sauvegarde8](https://hackmd.io/_uploads/SJdYag_Kzx.png)

L'installation de Windows 11 a ensuite commencé.
![sauvegarde10](https://hackmd.io/_uploads/S10s6x_KGl.png)

Les données importantes avaient été sauvegardées avant cette étape afin d'éviter leur perte pendant la réinstallation.

# 7. Configuration de WIndows 11

Une fois l'installation terminée, j'ai effectué la configuration initiale de Windows 11.

J'ai configuré les différents paramètres demandés, notamment la région, le clavier, le réseau et le compte utilisateur

Une fois arrivé sur le Bureau, j'ai vérifié que Windows fonctionnait correctement.

![sauvegarde11](https://hackmd.io/_uploads/rkM6AeOYzx.jpg)


# 8. Restauration des données

Après la réinstallation, j'ai réinstaller VMware Tools afin de pouvoir accéder au dossier partagé.

J'ai ensuite ouvert:

**\\\vmware-host\Shared Folders\Sauvegarde_VM**

Les fichiers sauvegardés étaient toujours présents.

![sauvegarde12](https://hackmd.io/_uploads/rJHxW-dFzl.jpg)

J'ai ensuite copié ces fichiers depuis la sauvegarde vers le Bureau de la nouvelle installation de Windows 11.
![sauvegarde13](https://hackmd.io/_uploads/B1nzx-_YMl.jpg)

# 9. Vérification de la restauration

J'ai ouvert les fichiers restaurés afin de vérifier que leur contenu était toujours présent.

Les fichiers pouvaient être ouverts et les données présentes avant la réinstallation avaient bien été récupérées.
![sauvegarde14](https://hackmd.io/_uploads/rk-r--utGe.jpg)


# 10. Vérification finales

Pour terminer, j'ai effectué plusieurs vérifications du système.

**Windows Update**

J'ai vérifié Windows Update afin de m'assurer que Windows pouvait rechercher et installer les mises à jour.
![sauvegarde16](https://hackmd.io/_uploads/BJ_gzb_FMl.jpg)

**Applications**

J'ai vérifier la liste des applications installées après la réinstallation. 
![sauvegarde17](https://hackmd.io/_uploads/ByHvzWuKGx.jpg)

**Fonctionnement général**

J'ai également vérifié que:

.Windows démarre correctement;
.la connexion Internet fonctionne;
.les fichiers réstaurés sont accessibles;
.les applications nécessaires sont présentes;
.la sauvegarde est toujours disponible.

# Conclusion

Cette procédure m'a permis de réaliser une sauvegarde complète de Windows 11, puis de restaurer les données sauvegardées.

La vérification finale montre que les fichiers ont été correctement récupérés et que le système Windows 11 fonctionne normalement après la réinstallation.