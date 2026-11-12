---
title: 3 Mise en place d'un routeur/pare-feu pour une petite entreprise

---

# Projet 3 — Mise en place d'un routeur/pare-feu pour une petite entreprise

**Cours :** Networking Essentials — Infrastructure informatique et réseau d'une petite entreprise
**Niveau :** Intermédiaire
**Durée :** 1 semaine (PPE)
**Date :** 14 octobre 2026

---

## 1. Contexte et objectifs

Une petite entreprise veut connecter son réseau local à Internet de façon sécurisée. Elle utilise un pare-feu basique.

Objectifs :

- Configurer un routeur pour la connexion Internet.
- Mettre en place des règles de pare-feu simples.
- Tester l'accès Internet et le blocage de trafic non autorisé.

Outils : GNS3, routeur Cisco (R1), PC virtuel VPCS (PC-LAN), nœud Cloud1.

---

## 2. Topologie

`PC-LAN (e0) — (g0/0) R1 (g1/0) — Cloud1 (Ethernet)`

- Le LAN relie PC-LAN à R1 par `g0/0`.
- Le WAN relie R1 à Cloud1 par `g1/0`.
- Cloud1 utilise l'adaptateur **VMware Network Adapter VMnet8**.

![2026-10-14_08h33_37](https://hackmd.io/_uploads/H1AN5shifl.png)

---

## 3. Plan d'adressage

| Équipement | Interface | Adresse IP | Masque | Rôle |
|---|---|---|---|---|
| PC-LAN | e0 | 192.168.1.11 | 255.255.255.0 | Poste du LAN |
| R1 | g0/0 | 192.168.1.1 | 255.255.255.0 | LAN (passerelle) |
| R1 | g1/0 | 192.168.202.138 (DHCP) | 255.255.255.0 | WAN |

- Passerelle du PC-LAN : `192.168.1.1`.
- Passerelle du WAN (route par défaut) : `192.168.202.2`.

---

## 4. Configuration du routeur

### 4.1 Interfaces LAN et WAN

```
interface GigabitEthernet0/0
 description LAN
 ip address 192.168.1.1 255.255.255.0
 no shutdown

interface GigabitEthernet1/0
 description WAN
 ip address dhcp
 no shutdown
```
![2026-10-14_08h38_08](https://hackmd.io/_uploads/rkS9Ajhofg.png)

Sur PC-LAN :

```
ip 192.168.1.11 255.255.255.0 192.168.1.1
```
![2026-10-14_08h38_46](https://hackmd.io/_uploads/Skv3RshjGl.png)

Vérifications :

- `show ip interface brief` : g0/0 a `192.168.1.1`, g1/0 a `192.168.202.138` (méthode DHCP), les deux sont `up/up`.
- `show ip route` : route par défaut `S* 0.0.0.0/0 via 192.168.202.2`.
- `ping 192.168.202.2` depuis R1 : 5/5 réussis.

![2026-10-14_08h39_08](https://hackmd.io/_uploads/HJPPcinsMg.png)

![2026-10-14_08h52_14](https://hackmd.io/_uploads/Bkl95onoGx.png)

### 4.2 NAT (PAT)

```
interface GigabitEthernet0/0
 ip nat inside
interface GigabitEthernet1/0
 ip nat outside

access-list 1 permit 192.168.1.0 0.0.0.255
ip nat inside source list 1 interface GigabitEthernet1/0 overload
```

- `g0/0` est le côté `inside` (LAN).
- `g1/0` est le côté `outside` (WAN).
- L'ACL 1 désigne le réseau `192.168.1.0/24`.
- `overload` partage l'adresse WAN entre les postes du LAN.

---

## 5. Test d'accès à Internet (autorisé)

Depuis PC-LAN :

```
ping 8.8.8.8
```

Sur R1 :

```
show ip nat translations
show ip nat statistics
```

Résultats :

- Les 5 pings ICMP de `192.168.1.11` vers `8.8.8.8` sont traduits en `192.168.202.138`.
- `show ip nat statistics` : 5 traductions dynamiques, 10 hits, 0 miss.
![2026-10-14_08h54_15](https://hackmd.io/_uploads/ryTh9onjzx.png)

![2026-10-14_08h54_54](https://hackmd.io/_uploads/r1DRcj3sfl.png)

---

## 6. Règle de pare-feu

### 6.1 Configuration

```
ip access-list extended FILTRE-LAN
 deny icmp 192.168.1.0 0.0.0.255 any echo
 permit ip any any

interface GigabitEthernet0/0
 ip access-group FILTRE-LAN in
```

### 6.2 Règles documentées

| N° | Action | Protocole | Source | Destination | Rôle |
|---|---|---|---|---|---|
| 10 | deny | ICMP (echo) | 192.168.1.0/24 | any | Bloque le ping sortant du LAN |
| 20 | permit | IP | any | any | Autorise tout le reste |

- L'ACL est appliquée en **entrée** (`in`) sur `g0/0`.
- Le trafic est filtré dès son entrée dans le routeur depuis le LAN.
- L'ordre compte. Le `deny` est placé avant le `permit`.
- La règle bloque tous les pings sortant du LAN, y compris ceux vers `192.168.1.1`.

---

## 7. Test du blocage (bloqué)

Depuis PC-LAN :

```
ping 8.8.8.8
```

Résultat : chaque ping est refusé par `192.168.1.1` avec le message `ICMP type:3, code:13, Communication administratively prohibited`.

Ce message indique un blocage volontaire par une ACL.

Sur R1 :

```
show access-lists
show ip interface g0/0
```

Résultats :

- `10 deny icmp 192.168.1.0 0.0.0.255 any echo` : **5 matches** (les 5 pings refusés).
- `20 permit ip any any` : en place.
- `access-list 1` (NAT) : 17 matches.
- `Inbound access list is FILTRE-LAN` sur `g0/0`.

> [Capture à insérer : ping refusé depuis PC-LAN]
![2026-10-14_09h02_30](https://hackmd.io/_uploads/BJaeii2sGl.png)

![2026-10-14_09h21_38](https://hackmd.io/_uploads/H17xRs3iMg.png)

---

## 8. Synthèse des tests

| Test | Attendu | Résultat |
|---|---|---|
| `ping 192.168.202.2` depuis R1 | Réussi | Réussi (5/5) |
| `ping 8.8.8.8` depuis PC-LAN, avant la règle | Réussi | Réussi, NAT visible |
| `ping 8.8.8.8` depuis PC-LAN, après la règle | Bloqué | Bloqué (code 13) |
| Compteur du `deny` | Augmente | 5 matches |

---

## 9. Configuration complète du routeur

![2026-10-14_09h05_23](https://hackmd.io/_uploads/BkYIRs2iGg.png)

![2026-10-14_09h05_30](https://hackmd.io/_uploads/Sy-D0jnsGg.png)

![2026-10-14_09h05_38](https://hackmd.io/_uploads/SknvRi3ozg.png)

![2026-10-14_09h05_45](https://hackmd.io/_uploads/H1oOAo3sMg.png)

---

## 10. Conclusion

Le routeur R1 relie le LAN `192.168.1.0/24` à Internet. Le NAT (PAT) traduit les adresses du LAN vers l'adresse WAN `192.168.202.138`. L'ACL `FILTRE-LAN` bloque le ping sortant du LAN. Les tests montrent un accès autorisé avant la règle et un accès bloqué après la règle.