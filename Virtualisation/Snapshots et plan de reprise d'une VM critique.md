---
title: Snapshots et plan de reprise d'une VM critique

---

# Projet 2 — Snapshots et plan de reprise d'une VM critique

**Classe :** E1A
**Niveau :** Intermédiaire
**Durée :** 1 semaine (PPE)
**Cours :** Virtualisation — Mettre en place et exploiter une plateforme de virtualisation
**Hyperviseur :** VMware Workstation

---

## 1. Contexte

Le serveur applicatif virtualisé d'une entreprise doit pouvoir être restauré vite.
Deux cas sont visés : une mauvaise mise à jour et une panne.

## 2. Objectifs

- Comprendre le mécanisme des snapshots.
- Mettre en place une politique de snapshots réguliers.
- Simuler un incident et restaurer la VM.
- Documenter un plan de reprise simple.

---

## 3. Comprendre les snapshots

Un snapshot fige l'état d'une VM à un instant donné.
Il peut contenir :

- l'état du disque ;
- l'état de la mémoire (VM allumée) ;
- la configuration de la VM.

### Comment ça marche

1. Au moment du snapshot, le disque d'origine devient **en lecture seule**.
2. VMware crée un fichier **delta** (`.vmdk` de type `-000001`).
3. Toutes les nouvelles écritures vont dans ce fichier delta.
4. Restaurer = jeter le delta et revenir au disque d'origine.

### Fichiers créés par VMware

| Fichier | Rôle |
|---|---|
| `.vmx` | Configuration de la VM |
| `.vmdk` | Disque virtuel |
| `-0000xx.vmdk` | Disque delta (changements depuis le snapshot) |
| `.vmsd` | Métadonnées des snapshots |
| `.vmsn` | État du snapshot (dont la mémoire) |

### Limites à connaître

- Un snapshot **n'est pas une sauvegarde**. Il dépend du disque d'origine.
- Si le disque est perdu, le snapshot l'est aussi.
- Plus il y a de snapshots, plus la VM ralentit.
- Le fichier delta grossit avec le temps.
- VMware Workstation **Player** ne gère pas les snapshots. Il faut **Workstation Pro**.

---

## 4. Étape 1 — Créer la VM de test

1. Ouvrir VMware Workstation.
2. `File` → `New Virtual Machine`.
3. Choisir une installation de l'OS.
4. Nommer la VM : `[nom de la VM]`.
5. Installer l'OS.
6. Installer **une application simple** : `Serveur web`.
7. Vérifier que l'application fonctionne.

![2026-10-06_13h14_21](https://hackmd.io/_uploads/ryZhcwfiMe.png)

---

## 5. Étape 2 — Prendre un snapshot avant modification

### Méthode interface

1. `VM` → `Snapshot` → `Take Snapshot`.
2. Nom : `avant-modification`.
3. Description : état de la VM avant la mise à jour.
4. Valider.

### Méthode ligne de commande

```bash
vmrun -T ws snapshot "<chemin_vmx>" "avant-modification"
```

| Élément | Explication |
|---|---|
| `vmrun` | Outil en ligne de commande de VMware |
| `-T ws` | Type d'hôte : Workstation |
| `snapshot` | Action : créer un snapshot |
| `"<chemin_vmx>"` | VM concernée |
| `"avant-modification"` | Nom du snapshot |

### Vérifier que le snapshot existe méthode commande

```bash
vmrun -T ws listSnapshots "<chemin_vmx>"
```

`listSnapshots` affiche le nombre de snapshots et leurs noms.

### méthode interface

VM->snapshot->snapshot manager 

![2026-10-06_13h21_59](https://hackmd.io/_uploads/BJpT9wzozl.png)

---

## 6. Étape 3 — Faire la modification volontaire

Dans la VM, installer une mise à jour.

- Mise à jour installée : 2026-09 correctif de securité
- Date et heure : 13h24

![2026-10-06_13h55_43](https://hackmd.io/_uploads/SJZrivfiGl.png)

---

## 7. Étape 4 — Simuler un incident

Deux incidents sont demandés par le sujet : suppression de fichier et panne applicative.

### Incident A — Suppression de fichier

1. Choisir un fichier de l'application : `[fichier choisi]`.
2. Le supprimer dans la VM.
3. Constater que l'application ne fonctionne plus correctement.

### Incident B — Panne applicative

1. Arrêter ou casser l'application : `Suppression de fichier (suppression du fichier de la page d'accueil`.
2. Constater la panne.

![2026-10-06_14h11_19](https://hackmd.io/_uploads/HJP9iDMjGx.png)

---

## 8. Étape 5 — Restaurer et mesurer le temps

### Principe de la mesure

On mesure le temps entre :

- **début** : lancement de la restauration ;
- **fin** : application de nouveau fonctionnelle.

### Méthode interface (chronomètre à la main)

1. Lancer un chronomètre.
2. `VM` → `Snapshot` → `Revert to Snapshot`.
3. Attendre que la VM démarre.
4. Vérifier que l'application fonctionne.
5. Arrêter le chronomètre.

### Méthode ligne de commande (mesure automatique)

Sur un hôte **Windows** (PowerShell) :

```powershell
Measure-Command {
  vmrun -T ws revertToSnapshot "<chemin_vmx>" "avant-modification"
  vmrun -T ws start "<chemin_vmx>"
}
```

Sur un hôte **Linux** :

```bash
time (vmrun -T ws revertToSnapshot "<chemin_vmx>" "avant-modification" && vmrun -T ws start "<chemin_vmx>")
```

| Élément | Explication |
|---|---|
| `revertToSnapshot` | Revient à l'état du snapshot |
| `start` | Démarre la VM |
| `Measure-Command` / `time` | Mesure la durée d'exécution |

> ⚠️ Cette mesure s'arrête quand la VM est lancée. Il faut aussi chronométrer le moment où l'application répond.

### Résultats

| Essai | Incident | Temps de restauration | Application OK ? |
|---|---|---|---|
| 1 | A — fichier supprimé | 2;56;01 minute | innaccesible |

![image_paysage_hackmd](https://hackmd.io/_uploads/ByVNpDMjze.jpg)
![2026-10-06_14h11_19](https://hackmd.io/_uploads/B13Kkdzjze.png)


---

## 9. Politique de snapshots réguliers

Principes :

- un snapshot **avant** chaque changement (mise à jour, modification) ;
- un snapshot à intervalle régulier ;
- peu de snapshots conservés (les anciens sont supprimés) ;
- des noms clairs avec la date.

### Convention de nommage

```
AAAA-MM-JJ_hhmm_motif
```

### Automatiser avec `vmrun`

Windows : le Planificateur de tâches lance un script.
Linux : une tâche `cron` lance un script.

Commande à planifier :

```bash
vmrun -T ws snapshot "<chemin_vmx>" "<nom_du_snapshot>"
```

### Supprimer un ancien snapshot

```bash
vmrun -T ws deleteSnapshot "<chemin_vmx>" "<nom_du_snapshot>"
```

`deleteSnapshot` fusionne le delta dans le disque et libère de la place.

### Politique retenue

| Règle | Valeur |
|---|---|
| Fréquence | `[à compléter]` |
| Snapshots conservés | `[à compléter]` |
| Snapshot avant changement | Oui |
| Responsable | `[à compléter]` |

---

## 10. Plan de reprise (1 page)

### Définitions simplifiées

- **RPO** (*Recovery Point Objective*) : quantité de données qu'on accepte de perdre. Elle dépend de la fréquence des snapshots.
- **RTO** (*Recovery Time Objective*) : durée maximale acceptable d'indisponibilité. Elle se compare au temps de restauration mesuré.

### Fiche

| Champ | Contenu |
|---|---|
| VM critique | Windows 11 |
| Rôle | Serveur applicatif |
| Risques couverts | Mauvaise mise à jour, panne applicative, suppression de fichier |
| **RPO** | 3 |
| **RTO** | entre 3 et 5|
| Temps de restauration mesuré | 2;56;01 Minutes |


### Procédure de reprise

1. **Détecter** l'incident.
2. **Prévenir** le responsable.
3. **Identifier** le dernier snapshot sain : `vmrun -T ws listSnapshots "<chemin_vmx>"`.
4. **Restaurer** : `vmrun -T ws revertToSnapshot "<chemin_vmx>" "<nom_du_snapshot>"`.
5. **Démarrer** la VM : `vmrun -T ws start "<chemin_vmx>"`.
6. **Vérifier** que l'application fonctionne.
7. **Noter** l'incident, l'heure et le temps de reprise.

### Limites du plan

- Les snapshots sont sur le même stockage que la VM.
- Une panne du disque de l'hôte perd tout.
- Une vraie **sauvegarde externe** reste nécessaire.

---

## 11. Conclusion

- Les snapshots permettent un retour arrière rapide.
- Temps de restauration mesuré : 2;56;01 Minutes.
- Le plan de reprise fixe un RPO et un RTO.
- Un snapshot ne remplace pas une sauvegarde.