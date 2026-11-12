---
title: 4 Migration d'un serveur physique vers une VM (P2V)
tags: [Virtualisation, ' P2V', ' VMware Workstation', ' E1A']

---

---
title: Projet 4 - Migration d'un serveur physique vers une VM (P2V)
tags: Virtualisation, P2V, VMware Workstation, E1A
---

# Projet 4 : Migration d'un serveur physique vers une VM (P2V)

| | |
|---|---|
| **Cours** | Virtualisation : mettre en place et exploiter une plateforme de virtualisation |
| **Hyperviseur** | VMware Workstation |
| **Date des manipulations** | 08/10/2026 |
|


---

## 1. Contexte et objectifs

Une entreprise possède un ancien serveur physique. Elle veut le virtualiser pour prolonger sa durée de vie logicielle et simplifier sa maintenance.

**Objectifs du projet**

- Comprendre le principe d'une migration physique vers virtuel (P2V).
- Réaliser ou simuler une migration P2V.
- Valider le fonctionnement identique après migration.
- Documenter les limites et les risques d'une migration P2V.

**Contrainte : aucune machine physique n'est disponible.**
Le serveur physique est donc **simulé** par une VM Windows 11 clonée. Cette VM joue le rôle du « vieux serveur ».

:::info
**Résumé du résultat**
La migration avec l'outil **Disk2vhd** n'a pas abouti. La VM source a planté à chaque tentative. Une **VM équivalente a été reconstruite** par clonage, ce que le sujet autorise (étape 2 : « choisir un outil de migration P2V ou reconstruire équivalent en VM »). Les deux VM ont ensuite été comparées.
:::

---

## 2. Vue d'ensemble de la démarche

| Phase | Contenu | Résultat |
|---|---|---|
| 1 | Création et configuration du serveur source simulé | OK |
| 2 | Inventaire et mesures « avant » | OK |
| 3 | Tentative de migration avec Disk2vhd | Échec (4 plantages) |
| 4 | Reconstruction d'une VM équivalente (`SRV-MIGRE`) | OK |
| 5 | Mesures « après » et comparatif | OK |
| 6 | Limites et risques | Rédigé |

---

## 3. Phase 1 : le serveur source simulé

### 3.1 Création de la VM `SRV-PHYS-SIM`

1. VM Windows 11 x64 éteinte.
2. Clic droit → **Manage** → **Clone**.
3. Choix : **The current state in the virtual machine**.
4. Choix : **Create a full clone**.
5. Nom : `SRV-PHYS-SIM`.

**Pourquoi un clone complet ?** Il est indépendant de la VM d'origine. Un clone lié dépendrait du disque parent. Ce serait moins réaliste pour un « serveur physique ».


### 3.2 Désinstallation des VMware Tools

Les VMware Tools ont été désinstallés (Paramètres → Applications), puis la VM a redémarré.

**Pourquoi ?** Un serveur physique n'a pas de VMware Tools. Ils seront réinstallés sur la VM migrée. Cette différence fait partie du comparatif.

![2026-10-08_09h35_57](https://hackmd.io/_uploads/SJjSClSifx.png)


![2026-10-08_09h40_51](https://hackmd.io/_uploads/rkxUReHizx.png)


### 3.3 Réseau : passage du LAN Segment au NAT

La VM était sur un **LAN Segment**. Ce réseau est isolé. Seules les VM du même segment s'y voient. L'hôte n'y a pas accès.

La carte réseau a été passée en **NAT** (VM → Settings → Network Adapter). La VM a obtenu l'adresse `192.168.202.135`.

![2026-10-08_10h08_47](https://hackmd.io/_uploads/Byxo0gHiGl.png)

### 3.4 Rôle du serveur : serveur de fichiers (SMB)

Le serveur source est un **serveur de fichiers**. Il partage le dossier `C:\Users\loris\Desktop\Partage` sous le nom `Partage`.

Le dossier contient trois dossiers de test (`Test1`, `Test2`, `Test3`) et un fichier de test `test.bin` de 500 Mo.

**Commande : création du fichier de test**

```powershell
fsutil file createnew C:\Users\loris\Desktop\Partage\test.bin 524288000
```

| Élément | Explication |
|---|---|
| `fsutil file createnew` | Crée un fichier vide d'une taille donnée. |
| `...\test.bin` | Chemin du fichier à créer. |
| `524288000` | Taille en **octets** : 500 × 1024 × 1024, soit 500 Mo. |

**Pourquoi ce fichier ?** Il sert à mesurer la vitesse d'accès au partage, avant et après la migration.

**Commande : vérification du partage**

```powershell
Get-SmbShare
```

Elle liste les partages SMB de la machine. Le partage `Partage` doit apparaître avec son chemin.

![2026-10-08_12h58_57](https://hackmd.io/_uploads/rkTXk-riMe.png)
### 3.5 Test d'accès depuis l'hôte

Le test depuis l'hôte demande une authentification. Le compte `loris` de la VM a été utilisé.

```powershell
net use \\192.168.202.135\Partage /user:loris *
dir \\192.168.202.135\Partage
```

| Commande | Explication |
|---|---|
| `net use` | Connecte l'hôte à un partage réseau. |
| `\\192.168.202.135\Partage` | Chemin réseau : `\\adresse-IP\nom-du-partage`. |
| `/user:loris` | Compte utilisé pour se connecter. |
| `*` | Demande le mot de passe sans l'afficher à l'écran. |
| `dir` | Liste le contenu du dossier distant. |

La commande a listé `Test1`, `Test2` et `Test3`. Le partage fonctionne par le réseau.

![2026-10-08_10h05_21](https://hackmd.io/_uploads/rJucy-rjfl.png)

---

## 4. Phase 2 : inventaire et mesures « avant »

### 4.1 Inventaire de la source

| Élément | Valeur |
|---|---|
| Nom | `SRV-PHYS-SIM` |
| Système | Windows 11 x64 (clone de la VM d'origine) |
| Adresse IP | `192.168.202.135` (NAT) |
| Disque 0 | 59,98 Go |
| Partitions | système (≈ 100 Mo), `C:` (59,01 Go, NTFS), récupération (898 Mo) |
| Rôle | Serveur de fichiers (partage SMB `Partage`) |
| VMware Tools | Non installés |
| RAM, CPU | _à compléter (propriétés de la VM)_ |
| Activation Windows | _à compléter (capture 10)_ |

**Commandes et outils utilisés pour l'inventaire**

| Outil | Rôle |
|---|---|
| `msinfo32` | Résumé du système : version de Windows, processeur, mémoire, BIOS/UEFI. |
| `ipconfig /all` | Configuration réseau complète : IP, masque, passerelle, DNS, adresse MAC. |
| `diskmgmt.msc` | Gestion des disques : partitions, tailles, état. |
| `services.msc` | Liste des services Windows et leur état. |
| Paramètres → Système → Activation | État de l'activation de la licence Windows. |


![2026-10-08_13h07_51](https://hackmd.io/_uploads/rk-Jg-Hszg.png)

![2026-10-08_12h58_06](https://hackmd.io/_uploads/BkBbgbBifg.png)

![2026-10-08_10h09_37](https://hackmd.io/_uploads/S12mxZHoGx.png)


![2026-10-08_10h08_22](https://hackmd.io/_uploads/HyYSlbBiMx.png)

![2026-10-08_13h08_43](https://hackmd.io/_uploads/r1D5l-riMe.png)

![2026-10-08_10h08_47](https://hackmd.io/_uploads/BkIplbHsfl.png)

### 4.2 Mesure : copie du fichier de test

La copie de `test.bin` depuis le partage vers l'hôte a été chronométrée.

```powershell
mkdir C:\Temp
Measure-Command { Copy-Item \\192.168.202.135\Partage\test.bin C:\Temp\ }
```

| Commande | Explication |
|---|---|
| `mkdir C:\Temp` | Crée le dossier de destination sur l'hôte. |
| `Copy-Item` | Copie le fichier du partage vers `C:\Temp\`. |
| `Measure-Command { ... }` | Exécute le bloc et mesure sa durée. |
| `TotalSeconds` | Champ du résultat : durée totale en secondes. |

**Résultat sur la source : 4,30 s** (`TotalSeconds` = 4,3039147).

:::warning
Une première mesure avait donné 0,04 s. Elle était **fausse** : la copie avait échoué car le dossier `C:\Temp` n'existait pas. Seule la mesure réussie est retenue.
:::

![2026-10-08_10h24_19](https://hackmd.io/_uploads/SySlWbBoMe.png)

### 4.3 Mesure : temps de démarrage

```powershell
Get-WinEvent -FilterHashtable @{LogName='Microsoft-Windows-Diagnostics-Performance/Operational'; Id=100} -MaxEvents 1 | Format-List Message
```

| Élément | Explication |
|---|---|
| `Get-WinEvent` | Lit un journal d'événements Windows. |
| `LogName='...Diagnostics-Performance/Operational'` | Journal des performances de Windows. |
| `Id=100` | Événement « Windows a démarré », avec la durée du démarrage. |
| `-MaxEvents 1` | Ne garde que l'événement le plus récent. |
| `Format-List Message` | Affiche le contenu du message. |

**Résultat sur la source : 122 074 ms** (≈ 122,1 s). `IsDegradation` = `false`.

![2026-10-08_13h20_08](https://hackmd.io/_uploads/SyJNbbrjze.png)


### 4.4 Point de sécurité : snapshot

La VM a été éteinte, puis un snapshot `avant-migration` a été pris (VM → Snapshot → **Take Snapshot**).

**Pourquoi ?** Il permet de revenir à l'état sain si la migration échoue.

![2026-10-08_10h37_17](https://hackmd.io/_uploads/HJnS-WSiMl.png)

---

## 5. Phase 3 : tentative de migration avec Disk2vhd

### 5.1 Choix de l'outil

| Outil | Remarque |
|---|---|
| VMware vCenter Converter Standalone | Téléchargement via un compte Broadcom. Non retenu. |
| **Disk2vhd (Sysinternals) v2.02** | Gratuit, sans compte, très léger. **Retenu.** |

**Principe de Disk2vhd :** il copie les volumes d'une machine **en fonctionnement** vers un fichier disque virtuel (VHDX). Il utilise le Volume Shadow Copy (VSS) pour obtenir une image cohérente.

![2026-10-08_10h50_20](https://hackmd.io/_uploads/H1fw--SjGx.png)

### 5.2 Configuration utilisée

| Option | Valeur |
|---|---|
| Volumes | Les trois volumes cochés (système, récupération, `C:`) |
| Use Vhdx | Coché |
| Use Volume Shadow Copy | Coché |
| Prepare for use in Virtual PC | Décoché |
| Espace requis | ≈ 21,5 Go (58 Mo + 782 Mo + 20,68 Go) |
| Destination | Partage réseau de l'hôte `\\192.168.202.1\P2V` |

L'adresse `192.168.202.1` est celle de l'hôte sur le réseau NAT (carte VMnet8).

**Commande : création du partage sur l'hôte**

```powershell
mkdir C:\P2V
New-SmbShare -Name P2V -Path C:\P2V -FullAccess "Tout le monde"
```

| Commande | Explication |
|---|---|
| `New-SmbShare` | Crée un partage SMB. |
| `-Name P2V` | Nom du partage. |
| `-Path C:\P2V` | Dossier partagé. |
| `-FullAccess "Tout le monde"` | Droits complets. Le nom du groupe dépend de la langue de Windows (`Everyone` en anglais). |

![2026-10-08_11h57_47](https://hackmd.io/_uploads/BJDFbZHofg.png)

### 5.3 Résultat : échec

La VM source a **planté et redémarré à 4 reprises**, à chaque fois vers la fin de la capture. Un fichier VHDX **incomplet** est resté sur le partage (113 246 208 octets, soit environ 108 Mo au lieu des ≈ 21 Go attendus).

Après un plantage, la connexion au partage était perdue. Disk2vhd a affiché une erreur d'authentification.


**Pistes essayées ou préparées**

- Reconnexion au partage avec `net use`, puis relance.
- Ajout d'un second disque virtuel de 40 Go à la VM, pour écrire en local au lieu du réseau (disque visible en « alloué » dans la gestion des disques). Il n'a finalement pas été utilisé.

![2026-10-08_13h49_44](https://hackmd.io/_uploads/rJWOXWBsfl.png)


:::danger
**Cause du plantage : non identifiée.** Le code d'erreur du journal système (événement 1001) n'a pas été relevé. Ce point est une limite de l'analyse.
:::

**Décision :** après 4 échecs, l'approche a changé. Le sujet autorise de **reconstruire une VM équivalente**.

---

## 6. Phase 4 : reconstruction de la VM `SRV-MIGRE`

### 6.1 Création par clonage du snapshot

1. Clic droit sur `SRV-PHYS-SIM` → **Manage** → **Clone**.
2. Choix : **An existing snapshot** → `avant-migration`.
3. Choix : **Create a full clone**.
4. Nom : `SRV-MIGRE`.

**Pourquoi le snapshot ?** Il contient l'état exact du serveur avant migration : le partage, les fichiers de test et la configuration.


### 6.2 Premier démarrage

- `SRV-PHYS-SIM` a été **éteinte** pour éviter un conflit de nom et d'adresse IP.
- La machine a été renommée `SRV-MIGRE` (Paramètres → Système → À propos).

**Résultat de `ipconfig /all` sur `SRV-MIGRE`**

| Élément | Valeur |
|---|---|
| Nom de l'hôte | `SRV-MIGRE` |
| Adresse IPv4 | `192.168.202.136` (DHCP) |
| Masque | `255.255.255.0` |
| Passerelle | `192.168.202.2` |
| Serveur DHCP | `192.168.202.254` |
| Carte réseau | Intel(R) 82574L Gigabit Network Connection |
| Adresse physique (MAC) | `00-0C-29-13-AA-D8` |

L'adresse IP et l'adresse MAC **changent** par rapport à la source. C'est normal pour un clone.

![2026-10-08_12h58_06](https://hackmd.io/_uploads/SyojXWSiMl.png)

### 6.3 Vérification du serveur

`Get-SmbShare` montre que le partage `Partage` existe, avec le même chemin : `C:\Users\loris\Desktop\Partage`.

![2026-10-08_13h00_15](https://hackmd.io/_uploads/HyxzVZBoMe.png)

![2026-10-08_13h01_36](https://hackmd.io/_uploads/S10EVWHoGg.png)

### 6.4 Test d'accès depuis l'hôte

```powershell
net use \\192.168.202.136\Partage /user:loris *
dir \\192.168.202.136\Partage
```

![2026-10-08_13h02_40](https://hackmd.io/_uploads/BJIwVbHjfg.png)


### 6.5 VMware Tools

Les VMware Tools sont installés sur la VM migrée (VM → **Install VMware Tools**, puis redémarrage). Ils apportent les pilotes adaptés à l'environnement virtuel.

- [ ] Installés avant les mesures « après »
- [x] Installés après les mesures « après »



![2026-10-08_13h06_23](https://hackmd.io/_uploads/SJBtEbHiGe.png)


---

## 7. Phase 5 : mesures « après » et comparatif

### 7.1 Mesures sur `SRV-MIGRE`

**Copie de `test.bin`** (même commande, même fichier, depuis l'hôte)

```powershell
Measure-Command { Copy-Item \\192.168.202.136\Partage\test.bin C:\Temp\ }
```

Résultat : **3,6 s**.

![2026-10-08_13h04_11](https://hackmd.io/_uploads/BkRR4bSofl.png)

> ![](https://)

**Temps de démarrage** (même commande `Get-WinEvent` que pour la source)

Résultat : **118 906 ms** (≈ 118,9 s). `IsDegradation` = `false`.

![2026-10-08_13h13_03](https://hackmd.io/_uploads/HktcrWHjze.png)

**Inventaire « après »**

![2026-10-08_13h08_04](https://hackmd.io/_uploads/BkZkIWSiGg.png)

![2026-10-08_13h08_29](https://hackmd.io/_uploads/S11m8-Hsfl.png)

![2026-10-08_13h09_04](https://hackmd.io/_uploads/HkINUWBjMg.png)

![2026-10-08_13h09_19](https://hackmd.io/_uploads/H1kvIWrozx.png)

![2026-10-08_14h04_54](https://hackmd.io/_uploads/HyQ7wZSjGe.png)

### 7.2 Tableau comparatif avant / après

| Critère | `SRV-PHYS-SIM` (avant) | `SRV-MIGRE` (après) | Écart |
|---|---|---|---|
| Adresse IP | 192.168.202.135 | 192.168.202.136 | Change |
| Adresse MAC | _à compléter_ | 00-0C-29-13-AA-D8 | Change (clone) |
| Rôle | Serveur de fichiers SMB | Serveur de fichiers SMB | Identique |
| Partage `Partage` | Oui | Oui | Identique |
| Contenu du partage | `Test1`, `Test2`, `Test3`, `test.bin` | `Test1`, `Test2`, `Test3`, `test.bin` | Identique |
| VMware Tools | Non | Oui | Différent |
| Copie de `test.bin` (500 Mo) | 4,30 s | 3,8 s | ≈ -0,5 s |
| Temps de démarrage | 122 074 ms | 118 906 ms | -3 168 ms (≈ -2,6 %) |
| CPU, RAM, disque au repos | _à compléter (capture 33)_ | _à compléter_ | _à compléter_ |
| Activation Windows | _à compléter_ | _à compléter_ | _à compléter_ |

### 7.3 Analyse

- Le **fonctionnement est identique** : même rôle, même partage, mêmes fichiers, accès depuis l'hôte réussi.
- Les **performances sont comparables**. L'écart sur le démarrage est faible (≈ 2,6 %).
- Les écarts mesurés restent faibles. Ils ne permettent pas de conclure que la VM migrée est plus rapide.
- Chaque mesure n'a été réalisée **qu'une fois** pour le démarrage. Les valeurs de la seconde mesure étaient identiques à la milliseconde. Elles relisaient très probablement le même événement. Elles n'ont pas été retenues.

---

## 8. Problèmes rencontrés et corrections

| Problème | Cause | Correction |
|---|---|---|
| L'hôte ne voyait pas la VM | Carte réseau en **LAN Segment** (réseau isolé) | Passage en **NAT** |
| `\\IP\Partage` tapé dans PowerShell : erreur « terme non reconnu » | Un chemin réseau seul est pris pour une commande | Utiliser `dir`, ou **Win + R** |
| `Test-Path` renvoyait `False` alors que le partage existait | Le partage demande une authentification | Se connecter avec `net use` |
| Copie échouée (« Nom de répertoire non valide ») | Dossier `C:\Temp` absent sur l'hôte | `mkdir C:\Temp` |
| Disk2vhd : « Error launching 64-bit version » | Programme lancé depuis le **zip**, non extrait | Extraire le zip, lancer `disk2vhd64.exe` |
| `New-SmbShare` : erreur 1332 | Nom de groupe `Everyone` inconnu sur un Windows en français | Utiliser `"Tout le monde"` |
| `net use` : erreur système 67 | Faute de frappe dans l'adresse IP (virgule au lieu d'un point) | Corriger l'adresse |
| Disk2vhd : « nom d'utilisateur ou mot de passe incorrect » | Connexion au partage perdue après un plantage de la VM | Refaire `net use`, puis relancer |
| Plantages de la VM pendant la capture | Non identifiée | Reconstruction de la VM par clonage |

---

## 9. Limites et risques d'une migration P2V

### 9.1 Limites de ce projet

- **Le serveur source est simulé.** Il s'agit d'une VM, pas d'une machine physique. Les vrais problèmes de matériel (pilotes de contrôleur disque, carte réseau, carte mère) n'ont pas pu être observés.
- **La migration par outil n'a pas abouti.** La VM `SRV-MIGRE` est un **clone**, pas le résultat d'un P2V. Le clonage valide la méthode de comparaison. Il ne valide pas le processus P2V.
- **La cause des plantages n'est pas identifiée.**
- **Les mesures sont peu nombreuses.** Une seule mesure de démarrage par VM.

### 9.2 Risques d'une migration P2V

| Risque | Description | Mesure possible |
|---|---|---|
| **Pilotes** | Le matériel virtuel diffère du matériel physique. Un Windows migré peut ne pas démarrer ou perdre des périphériques. | Installer les VMware Tools. Prévoir le retrait des pilotes inutiles. |
| **Licences** | Le changement de matériel peut invalider ou forcer la réactivation de Windows. Les licences logicielles liées au matériel sont aussi touchées. | Vérifier les contrats avant la migration. Contrôler l'activation après. |
| **Performances** | La VM partage les ressources de l'hôte. Les performances peuvent différer de la machine d'origine. | Mesurer avant et après. Dimensionner CPU, RAM et disque. |
| **Cohérence des données** | Une capture à chaud peut copier des fichiers en cours d'écriture. | Utiliser le Volume Shadow Copy. Arrêter les services sensibles. |
| **Stabilité pendant la capture** | La capture charge le serveur source. Dans ce projet, la VM source a planté à 4 reprises. | Tester hors production. Prévoir un snapshot ou une sauvegarde. |
| **Espace disque** | L'image demande autant de place que les données (≈ 21,5 Go ici), puis plus pour une conversion. | Prévoir l'espace avant de commencer. |
| **Conflits** | Deux machines identiques en réseau créent des conflits de nom et d'adresse IP. | Éteindre la source ou l'isoler avant de tester la copie. |
| **Retour arrière** | Si la migration échoue, il faut pouvoir revenir à l'état d'origine. | Garder la source intacte. Un snapshot a été pris ici. |
| **Sécurité du transfert** | L'image contient tout le système et les données. | Utiliser un réseau de confiance. Protéger les fichiers image. |

---

## 10. Conclusion

Une VM source simulant un serveur de fichiers a été créée, inventoriée et mesurée. La migration avec **Disk2vhd** a échoué à 4 reprises, la VM source plantant en fin de capture. Une VM équivalente, `SRV-MIGRE`, a été reconstruite par clonage du snapshot.

La comparaison montre un **fonctionnement identique** et des **performances comparables** : 4,30 s contre 3,8 s pour la copie de 500 Mo, et 122 074 ms contre 118 906 ms pour le démarrage.

Le projet montre aussi qu'un P2V **à chaud** comporte des risques réels. La source peut devenir instable pendant la capture. Il faut prévoir un snapshot, de l'espace disque, une procédure de retour arrière et des tests avant toute bascule.

---
