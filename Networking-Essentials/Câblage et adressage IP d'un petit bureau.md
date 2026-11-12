---
title: Câblage et adressage IP d'un petit bureau

---

# Projet 1 — Câblage et adressage IP d'un petit bureau
**Cours** : Networking Essentials — Infrastructure informatique et réseau d'une petite entreprise
**Classe** : E1A — **Niveau** : Débutant


## 1. Contexte
Un bureau de 8 postes doit être câblé et adressé en IP statique sur un même sous-réseau.
Le travail est réalisé sous GNS3.

Objectifs :
- Concevoir un plan d'adressage IP cohérent.
- Câbler et connecter switch, routeur et postes.
- Vérifier la connectivité de bout en bout.

## 2. Plan d'adressage

| Élément | Valeur |
|---|---|
| Réseau | 192.168.1.0/24 |
| Masque | 255.255.255.0 |
| Passerelle (R1, g0/0) | 192.168.1.1 |
| Broadcast | 192.168.1.255 |

| Équipement | Adresse IP | Connecté à |
|---|---|---|
| R1 | 192.168.1.1 | Switch1 |
| PC1 | 192.168.1.11 | Switch1 |
| PC2 | 192.168.1.12 | Switch1 |
| PC3 | 192.168.1.13 | Switch1 |
| PC4 | 192.168.1.14 | Switch1 |
| PC5 | 192.168.1.15 | Switch2 |
| PC6 | 192.168.1.16 | Switch2 |
| PC7 | 192.168.1.17 | Switch2 |
| PC8 | 192.168.1.18 | Switch2 |

Les switchs n'ont pas d'adresse IP.

## 3. Câblage

| Équipement A | Port | Équipement B | Port |
|---|---|---|---|
| PC1 | e0 | Switch1 | e1 |
| PC2 | e0 | Switch1 | e2 |
| PC3 | e0 | Switch1 | e3 |
| PC4 | e0 | Switch1 | e4 |
| R1 | g0/0 | Switch1 | e5 |
| Switch1 |e0 | Switch2 | e0 |
| PC5 | e0 | Switch2 | e1|
| PC6 | e0 | Switch2 | e2 |
| PC7 | e0 | Switch2 | e3 |
| PC8 | e0 | Switch2 | e4 |

![2026-10-12_10h30_32](https://hackmd.io/_uploads/S1xY2z5sfe.png)


## 4. Schéma réseau annoté

![2026-10-12_10h37_01](https://hackmd.io/_uploads/rJdv2G9ifl.png)

## 5. Configuration

### 5.1 Routeur R1
```
enable
configure terminal
hostname R1
interface g0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
end
write memory
show ip interface brief
```

![2026-10-12_08h53_09](https://hackmd.io/_uploads/H17qnfcsfg.png)

### 5.2 Postes (VPCS)
Commande utilisée sur chaque poste (exemple PC1) :
```
ip 192.168.1.11 255.255.255.0 192.168.1.1
save
show ip
```

![2026-10-12_08h55_01](https://hackmd.io/_uploads/rJE32Mcifx.png)
![2026-10-12_08h55_55](https://hackmd.io/_uploads/rJInhMqjGx.png)
![2026-10-12_08h56_35](https://hackmd.io/_uploads/H1Y3hGqoMx.png)
![2026-10-12_08h57_13](https://hackmd.io/_uploads/SJT33Gqsfx.png)
![2026-10-12_08h58_20](https://hackmd.io/_uploads/B171az5izl.png)
![2026-10-12_08h58_57](https://hackmd.io/_uploads/SyUkaM9szg.png)
![2026-10-12_09h11_28](https://hackmd.io/_uploads/rytJTfqiMl.png)
![2026-10-12_09h12_10](https://hackmd.io/_uploads/Bkj16Gcize.png)


## 6. Tests de connectivité

### 6.1 Vers la passerelle
Commande : `ping 192.168.1.1`

![2026-10-12_09h12_39](https://hackmd.io/_uploads/rk6WTfqiGg.png)
![2026-10-12_09h13_00](https://hackmd.io/_uploads/rkZGTf5sMe.png)
![2026-10-12_09h13_23](https://hackmd.io/_uploads/rJPMpzqjfx.png)
![2026-10-12_09h13_45](https://hackmd.io/_uploads/BydGaz5sMg.png)
![2026-10-12_09h14_05](https://hackmd.io/_uploads/Hy5zTM5iMx.png)
![2026-10-12_10h13_14](https://hackmd.io/_uploads/HyCGaf9jMl.png)
![2026-10-12_10h13_35](https://hackmd.io/_uploads/H1xXpG9iMg.png)
![2026-10-12_10h13_58](https://hackmd.io/_uploads/SJz7pfqsMl.png)


### 6.2 Entre les postes
Chaque poste a pingué les 7 autres postes.

![2026-10-12_10h17_47](https://hackmd.io/_uploads/rkoHTzcsMl.png)
![2026-10-12_10h18_55](https://hackmd.io/_uploads/Hk0H6fcjGl.png)
![2026-10-12_10h20_17](https://hackmd.io/_uploads/rkxLTGcjfg.png)
![2026-10-12_10h21_22](https://hackmd.io/_uploads/B1fU6G5oMl.png)
![2026-10-12_10h22_28](https://hackmd.io/_uploads/H1E8pM9iMg.png)
![2026-10-12_10h24_17](https://hackmd.io/_uploads/S1BIaf9iGe.png)
![2026-10-12_10h25_58](https://hackmd.io/_uploads/rkwIpf5ifx.png)
![2026-10-12_10h27_24](https://hackmd.io/_uploads/SyKLaM5ize.png)


### 6.3 Matrice de connectivité

| Source \ Cible | PC1 | PC2 | PC3 | PC4 | PC5 | PC6 | PC7 | PC8 | R1 |
|---|---|---|---|---|---|---|---|---|---|
| PC1 | – | OK | OK | OK | OK | OK | OK | OK | OK |
| PC2 | OK | – | OK | OK | OK | OK | OK | OK | OK |
| PC3 | OK | OK | – | OK | OK | OK | OK | OK | OK |
| PC4 | OK | OK | OK | – | OK | OK | OK | OK | OK |
| PC5 | OK | OK | OK | OK | – | OK | OK | OK | OK |
| PC6 | OK | OK | OK | OK | OK | – | OK | OK | OK |
| PC7 | OK | OK | OK | OK | OK | OK | – | OK | OK |
| PC8 | OK | OK | OK | OK | OK | OK | OK | – | OK |

Résultat : la connectivité est totale entre les 8 postes et la passerelle.

## 7. Problèmes rencontrés

### 7.1 Postes injoignables (PC6 puis PC5)
- **Symptôme** : `host not reachable` vers PC6, puis vers PC5.
- **Diagnostic** : `show ip` correct (IP, masque, passerelle). Les autres postes répondaient.
- **Actions** : liens supprimés et recréés, switch recréé, nouveau projet.
- **Résolution** : Suppression puis Réinstallation de GNS3, avec redémarrage du pc. Puis nouveau projet crée.


### 7.2 Erreur Dynamips au démarrage
- **Symptôme** : `unable to create UDP NIO` à la création du projet.
- **Diagnostic** : `netstat -ano -p udp` montre les ports UDP 10010 et 10011 utilisés par `NVIDIA Overlay.exe` et `nvcontainer.exe`.
- **Résolution** : modification de la plage de ports UDP dans GNS3 (Edit, Preferences, Server) : 30000 à 40000.

## 8. Conclusion
Le réseau de 8 postes est câblé et adressé en IP statique sur 192.168.1.0/24.
La connectivité a été vérifiée entre tous les postes et avec la passerelle.
