---
title: 3  Formulaire de contact accessible

---

# Projet 3 – Formulaire de contact accessible

## 1. Présentation du projet

L'objectif de ce projet était de créer un formulaire de contact accessible pour une association. Le formulaire permet aux visiteurs d'envoyer un message facilement, y compris avec le clavier uniquement ou avec un lecteur d'écran.

Le formulaire a été réalisé en HTML et CSS avec Visual studio code.

## 2. Livrables

### 2.1 Fichiers du formulaire

Les fichiers réalisés sont :
- `index.html` : contient la structure et le contenu du formulaire.
- `style.css` : contient la mise en forme et le design du formulaire.

### 2.2 Capture d'écran – Formulaire

![2026-11-06_08h48_25](https://hackmd.io/_uploads/rkDHO-spze.png)

## 3. Choix de structure et de conception

**HTML** : J'ai choisi une structure HTML sémantique pour que le formulaire soit clair et compréhensible par un lecteur d'écran.
- `<label>` : chaque champ a un label lié à son `id`.
- `<fieldset>` et `<legend>` : regroupent les champs par thème (coordonnées, demande, mode de réponse).
- `required`, `type="email"`, `type="tel"` et `pattern` : valident les champs sans JavaScript.
- `autocomplete` : aide à remplir les coordonnées.
- `<form action="mailto:…">` : au clic sur le bouton, la messagerie s'ouvre avec le message prérempli.

### 3.1 Aperçu du code HTML

0![2026-11-06_08h47_38](https://hackmd.io/_uploads/Sy50OWiTGg.png)


**CSS** : Pour la partie CSS, j'ai choisi des couleurs avec un fort contraste : texte noir sur fond blanc et bouton bleu foncé avec texte blanc. J'ai utilisé des unités relatives (`rem`) pour que le texte puisse être agrandi. Enfin, j'ai ajouté un contour jaune épais avec `:focus-visible` pour que l'on voie toujours où l'on se trouve au clavier.

### 3.2 Aperçu du code CSS
![2026-11-06_08h48_11](https://hackmd.io/_uploads/BJUHY-oTze.png)

## 4. Preuves que le formulaire fonctionne
Pour prouver que le formulaire fonctionne j'ai utiliser un nom, prenom et adresse e-mail fictives.
![2026-11-06_08h48_47](https://hackmd.io/_uploads/SyNycWo6Gl.png)
## 4.1 le bouton fonctionne
Quand on appuie sur le bouton la messagerie s'ouvre et on apparait directement avec le champ qu'on peut remplir poue envoyer un message.
![2026-11-06_08h49_16](https://hackmd.io/_uploads/By8Jq-jaGx.png)
## 4.2 L'email arrive au destinataire
Vérification que j'ai bien reçu l'email
![2026-11-06_08h49_28](https://hackmd.io/_uploads/Hydk9WjaMe.png)

## 5. Conclusion

Ce projet m'a permis de mettre en pratique mes connaissances en HTML et CSS. J'ai appris à construire un formulaire accessible, à valider les champs avec HTML5 et à tester la navigation au clavier. Une limite : `mailto:` dépend de la messagerie de l'utilisateur, et il faudrait un serveur pour un envoi réel.