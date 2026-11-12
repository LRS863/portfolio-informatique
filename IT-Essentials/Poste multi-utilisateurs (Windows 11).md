---
title: Poste multi-utilisateurs (Windows 11)

---

# Rapport technique — Poste multi-utilisateurs (Windows 11)

## 1. Objectif du projet

Configurer un poste Windows 11 avec plusieurs comptes utilisateurs cloisonnés, un compte d'administration dédié à la maintenance, et une politique de sécurité minimale (mot de passe + verrouillage de session).

---

## 2. Installation et configuration de base

- Système d'exploitation : **Windows 11**
- Un compte administrateur local existant (`Loris`) a servi de point de départ pour créer les comptes nécessaires au projet.

---

## 3. Organisation des comptes utilisateurs

### 3.1 Création du compte administrateur de maintenance

Le compte **Admin-maintenance** a été créé via *Paramètres → Comptes → Autres utilisateurs → Ajouter un compte*, en tant que compte local (sans compte Microsoft), puis promu **Administrateur**.


![admin1](https://hackmd.io/_uploads/Bkm3EKIFMl.png)



![admin2](https://hackmd.io/_uploads/rJ56VtIYfx.png)


![admin3](https://hackmd.io/_uploads/r1ARNYLtGg.png)

### 3.2 Création des comptes utilisateurs standards

Trois comptes utilisateurs standards ont été créés selon la même procédure, sans droits administrateur : **étudiant1**, **étudiant2**, **étudiant3**.

![admin4](https://hackmd.io/_uploads/rk2lBtUYfe.png)

---

## 4. Cloisonnement effectif des données

### 4.1 Principe

Chaque compte dispose de son propre profil (`C:\Users\NomUtilisateur`), créé automatiquement à la première connexion. Les permissions NTFS par défaut de Windows empêchent un utilisateur standard d'accéder au dossier personnel d'un autre utilisateur.

### 4.2 Préparation des tests

Un dossier de test a été créé dans les Documents de chaque étudiant, en étant connecté sur son propre compte, afin d'avoir un élément concret à retrouver (ou non) depuis un autre compte.


![admin5](https://hackmd.io/_uploads/ryIQHK8FMg.png)

### 4.3 Tests d'accès croisés

Depuis un autre compte, une tentative d'accès direct au dossier personnel d'un autre utilisateur (`C:\Users\NomUtilisateur`) déclenche un refus d'accès.


![admin7](https://hackmd.io/_uploads/B1jVBtUYfe.png)

![admin8](https://hackmd.io/_uploads/r1MUHK8Fzx.png)


**Conclusion** : le cloisonnement des données entre comptes utilisateurs est fonctionnel — aucun compte standard ne peut consulter les fichiers d'un autre compte sans élévation de privilèges administrateur.

---

## 5. Politique de sécurité

### 5.1 Verrouillage automatique de session

Configuration de l'écran de veille via `desk.cpl,@screensaver`.


![admin0_compact](https://hackmd.io/_uploads/HkLePtUKfg.jpg)


![admin10_q85](https://hackmd.io/_uploads/HJVdDY8Ffg.jpg)



La configuration finale du verrouillage automatique est la suivante :
- **Délai** : 1 minute
- **À la reprise, demander l'ouverture de session** : Activé

### 5.2 Politique de mot de passe

Ouverture de la console `secpol.msc` → Stratégies de comptes → Stratégie de mot de passe.


![admin11_q85](https://hackmd.io/_uploads/HJDZutUKMg.jpg)

![admin12](https://hackmd.io/_uploads/rkaV_YIKGl.png)

![admin13](https://hackmd.io/_uploads/SJbFOKLFzx.png)


![admin14](https://hackmd.io/_uploads/HklJFF8tzg.png)

La politique de mot de passe finale est la suivante :
- **Longueur minimale du mot de passe** : 8 caractères
- **Le mot de passe doit respecter des exigences de complexité** : Activé

---

## 6. Livrables

- [x] Machine configurée avec 1 compte admin-maintenance + 3 comptes utilisateurs standards
- [x] Cloisonnement testé et démontré par captures d'écran
- [x] Politique de mot de passe (8 caractères minimum + complexité activée)
- [x] Verrouillage automatique (délai 1 minute + demande d'ouverture de session)
- [x] Présent rapport technique

---

## 7. Récapitulatif selon les critères d'évaluation

| Critère | Pondération | État |
|---|---|---|
| Installation et configuration de base | 15% | ✅ Fait |
| Organisation des comptes utilisateurs | 25% | ✅ Fait |
| Cloisonnement effectif des données | 30% | ✅ Fait, testé et prouvé |
| Politique de sécurité (mots de passe, verrouillage) | 15% | ✅ Fait |
| Documentation et preuves fournies | 15% | ✅ Ce rapport |