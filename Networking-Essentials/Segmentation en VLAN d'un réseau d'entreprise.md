---
title: Segmentation en VLAN d'un réseau d'entreprise

---

# Projet 2 : Segmentation en VLAN d'un réseau d'entreprise

**Cours :** Networking Essentials, Infrastructure informatique et réseau d'une petite entreprise
**Classe :** E1A
**Niveau :** Intermédiaire
**Outil :** GNS3

---

## 1. Contexte

Une entreprise veut séparer le trafic de son service comptabilité de celui du reste des employés. La raison est la sécurité.

La solution retenue est la segmentation en VLAN sur un switch.

## 2. Objectifs

- Comprendre le principe des VLAN.
- Configurer au moins 2 VLAN sur un switch.
- Vérifier l'étanchéité entre les VLAN.

## 3. Principe d'un VLAN

Un VLAN (Virtual LAN) découpe un switch en plusieurs réseaux logiques séparés. Les postes d'un VLAN communiquent entre eux. Ils ne communiquent pas avec les postes des autres VLAN. Il n'y a pas besoin de switch physique supplémentaire.

Ici, la comptabilité est isolée des autres employés.

## 4. Topologie

La topologie contient :

- 1 switch : **Switch1** (switch Ethernet intégré à GNS3).
- 4 postes : **PC1, PC2, PC3, PC4** (VPCS).

Chaque poste est relié à un port du switch.

![2026-10-13_11h37_12](https://hackmd.io/_uploads/H1dyktsofe.png)

## 5. Plan d'adressage et VLAN

Les 4 postes utilisent le masque 255.255.255.0.

| Poste | Adresse IP | Masque | Port du switch | VLAN | Service |
|---|---|---|---|---|---|
| PC1 | 192.168.1.11 | 255.255.255.0 | 1 (e1) | 2 | Comptabilité |
| PC2 | 192.168.1.12 | 255.255.255.0 | 2 (e2) | 2 | Comptabilité |
| PC3 | 192.168.1.13 | 255.255.255.0 | 3 (e3) | 3 | Employés |
| PC4 | 192.168.1.14 | 255.255.255.0 | 4 (e4) | 3 | Employés |

Les autres ports du switch restent dans le VLAN 1 (VLAN par défaut).

## 6. Configuration

### 6.1 Configuration du switch

Le switch Ethernet intégré à GNS3 n'a pas de console. La configuration se fait dans la fenêtre **Node properties** du switch.

Pour chaque port, j'ai renseigné :

- **Port** : le numéro du port.
- **VLAN** : le numéro du VLAN.
- **Type** : `access`.

Puis j'ai cliqué sur **Add**. À la fin, j'ai validé avec **Apply** puis **OK**.

Résultat :

| Port | VLAN | Type |
|---|---|---|
| 1 | 2 | access |
| 2 | 2 | access |
| 3 | 3 | access |
| 4 | 3 | access |

![2026-10-13_11h42_58](https://hackmd.io/_uploads/BJhbJFsiGg.png)


### 6.2 Configuration des postes

Sur la console de chaque PC (VPCS), j'ai configuré l'adresse IP avec la commande `ip <adresse> <masque>`. Puis j'ai enregistré avec `save`.

Exemple pour PC1 :

```
ip 192.168.1.11 255.255.255.0
save
```

Vérification avec `show ip` :

![2026-10-13_11h43_49](https://hackmd.io/_uploads/ryemkYojMe.png)


## 7. Tests et validation

### 7.1 Tests dans le même VLAN (doivent réussir)

| Test | Source | Destination | Résultat attendu | Résultat obtenu |
|---|---|---|---|---|
| 1 | PC1 (VLAN 2) | PC2 (VLAN 2) | Réussi | Réussi |
| 2 | PC3 (VLAN 3) | PC4 (VLAN 3) | Réussi | Réussi |

![2026-10-13_11h46_42](https://hackmd.io/_uploads/Sy4S1toszx.png)


![2026-10-13_11h47_08](https://hackmd.io/_uploads/r1yUktsifx.png)


### 7.2 Tests entre VLAN (doivent échouer)

| Test | Source | Destination | Résultat attendu | Résultat obtenu |
|---|---|---|---|---|
| 3 | PC1 (VLAN 2) | PC3 (VLAN 3) | Échec | Échec |
| 4 | PC4 (VLAN 3) | PC2 (VLAN 2) | Échec | Échec |

![2026-10-13_11h47_34](https://hackmd.io/_uploads/ryiv1Fjife.png)

![2026-10-13_11h48_39](https://hackmd.io/_uploads/HyTvkYssfx.png)

## 8. Analyse

Les postes d'un même VLAN communiquent entre eux. Les postes de VLAN différents ne communiquent pas.

Les 4 postes ont pourtant des adresses du même réseau 192.168.1.0/24. L'échec des pings entre VLAN ne vient donc pas de l'adressage. Il vient de la séparation faite par le switch.

Cela prouve l'étanchéité entre le VLAN 2 (comptabilité) et le VLAN 3 (employés).

## 9. Conclusion

Les objectifs sont atteints :

- 2 VLAN sont configurés sur le switch (VLAN 2 et VLAN 3).
- Les ports sont assignés à chaque VLAN.
- L'étanchéité entre les VLAN est prouvée par les tests de ping.
- La configuration est documentée dans ce rapport.
