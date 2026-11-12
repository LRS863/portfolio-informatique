---
title: 4 Refonte responsive d'un site existant

---

# Projet 4 — Refonte responsive d'un site existant

**Cours :** HTML - No Code  
**Classe :** E1A  
**Niveau :** Intermédiaire  
**Projet :** TechWeb — Site vitrine informatique

---

## 1. Présentation du projet

L'objectif de ce projet est de créer un site web puis de le rendre responsive afin qu'il puisse s'adapter correctement aux différentes tailles d'écran.

Le site réalisé s'appelle **TechWeb**. Il s'agit d'une entreprise informatique fictive qui propose plusieurs services :

- Dépannage informatique
- Installation de réseaux
- Sécurité informatique

Le travail consiste à créer une première version du site, identifier les problèmes d'affichage et améliorer le CSS pour obtenir un résultat adapté aux ordinateurs, tablettes et téléphones.

## 2. Structure du projet

Le projet est composé de deux fichiers principaux :

- `PPE4.html` : contient la structure et le contenu du site.
- `PPE4.css` : contient la mise en forme et le design du site.

## 3. Captures avant les corrections

Avant de modifier le code, j'ai testé le site sur trois tailles d'écran afin d'identifier les problèmes d'affichage.

### 3.1 Version ordinateur

**Dimension : 1440 × 900 px**

![Photo avant corréction écran entier](https://hackmd.io/_uploads/S1fcbbyRMl.png)

### 3.2 Version tablette

**Dimension : 768 × 1024 px**

![Photo avant corréction tablette](https://hackmd.io/_uploads/Skh5W-y0fl.png)


### 3.3 Version téléphone

**Dimension : 375 × 812 px**
![Photo avant corréction téléphone](https://hackmd.io/_uploads/rJVib-10Me.png)



## 4. Code avant les corrections

### 4.1 Code HTML avant

Insérer ici une capture d'écran du fichier `PPE4.html` avant les modifications.

![Code HTML](https://hackmd.io/_uploads/rJg6-bJ0Gg.png)

### 4.2 Code CSS avant

Insérer ici une capture d'écran du fichier `style.css` avant les modifications.

![Code CSS avant corréction](https://hackmd.io/_uploads/B1cTZW1Czx.png)

## 5. Analyse des problèmes

Après les premiers tests, plusieurs problèmes ont été identifiés :

1. Le menu de navigation est trop serré sur les petits écrans.
2. Le header manque d'espace sur mobile.
3. Les blocs de services utilisent une largeur fixe.
4. Les éléments ne s'adaptent pas correctement aux petites résolutions.
5. Les espacements sont trop importants sur certaines tailles d'écran.

Ces problèmes rendent le site moins agréable à utiliser sur téléphone et tablette.

## 6. Corrections apportées

Pour améliorer le site, plusieurs modifications ont été effectuées dans le fichier CSS.

### 6.1 Ajout de Box-sizing

Cette règle permet de mieux gérer les dimensions des éléments et leurs espacements.

Code ajouté :

```
* {
    box-sizing: border-box;
}
```

### 6.2 Modification de CSS Grid

Les services utilisaient auparavant une largeur fixe. J'ai remplacé cette organisation par CSS Grid afin de permettre une meilleure adaptation.

Code avant :

```
.services {
    display: flex;
    gap: 30px;
}
```

Code après :

```
.services {
    display: grid;
    grid-template-columns: repeat(3, minmax(0, 1fr));
    gap: 30px;
}
```

### 6.3 Suppression de la largeur fixe

Code avant :

```
.service {
    background-color: #eeeeee;
    padding: 50px;
    width: 300px;
    text-align: center;
}
```

Code après :

```
.service {
    background-color: #eeeeee;
    padding: 30px;
    text-align: center;
}
```

### 6.4 Ajout des Media Queries

Les Media Queries permettent de modifier le design selon la largeur de l'écran.

Code ajouté :

```
@media screen and (max-width: 800px) {
    .services {
        grid-template-columns: repeat(2, 1fr);
    }

    section {
        padding: 40px 25px;
    }
}

@media screen and (max-width: 600px) {
    header {
        flex-direction: column;
        gap: 15px;
        padding: 20px;
    }

    nav {
        flex-wrap: wrap;
        justify-content: center;
        gap: 15px;
    }

    section {
        padding: 30px 18px;
    }

    .services {
        grid-template-columns: 1fr;
    }

    .service {
        padding: 25px;
    }
}
```

### 6.5 Résultat des modifications

L'organisation des services change désormais selon la taille de l'écran :

- Ordinateur : trois colonnes.
- Tablette : deux colonnes.
- Téléphone : une colonne.

Le menu s'adapte également aux petits écrans et les espacements sont réduits pour améliorer la lisibilité.

## 7. Code après les corrections

### 7.1 Code HTML après

Le fichier HTML n'a pas nécessité de modification. La structure initiale était déjà correcte.

Insérer ici une capture d'écran du fichier `PPE4.html` final.

![Code HTML](https://hackmd.io/_uploads/ByKeMZy0ze.png)


### 7.2 Code CSS après

Insérer ici une capture d'écran du fichier `PPE4.css` final avec les corrections responsive.

![Partie 1 code css après corréction](https://hackmd.io/_uploads/SkkMM-10zx.png)
![Partie 2 code css après corréction](https://hackmd.io/_uploads/BJ-ffZJRMl.png)


## 8. Captures après les corrections

Après les modifications, j'ai effectué de nouveaux tests sur les trois mêmes tailles d'écran.

### 8.1 Version ordinateur

**Dimension : 1440 × 900 px**

![Photo après corréction écran entier](https://hackmd.io/_uploads/ByqNM-JCzx.png)

### 8.2 Version tablette

**Dimension : 768 × 1024 px**

![Photo après corréction tablette](https://hackmd.io/_uploads/r1XrzZ1Azx.png)

### 8.3 Version téléphone

**Dimension : 375 × 812 px**

![Photo après corréction téléphone](https://hackmd.io/_uploads/SJYBGbkCfx.png)


## 9. Tableau des tests

| Appareil | Résolution | Résultat |
|---|---|---|
| Ordinateur | 1440 × 900 | Adapté |
| Tablette | 768 × 1024 | Adapté |
| Téléphone | 375 × 812 | Adapté |

Les tests permettent de vérifier que le site s'affiche correctement sur les trois formats.

## 10. Bilan du projet

Ce projet m'a permis de mieux comprendre l'importance du responsive design dans la création d'un site web.

J'ai appris à utiliser CSS Grid, Flexbox et les Media Queries afin d'adapter un site aux différentes tailles d'écran.

J'ai également appris à analyser les problèmes d'affichage et à tester les modifications directement dans les outils de développement du navigateur.

**Conclusion :** le site TechWeb est maintenant adapté aux ordinateurs, aux tablettes et aux téléphones.