---
title: Prohet 4-Dépannage et diagnostic d'un poste en panne

---

# Projet 4-Dépannage et diagnostic d'un poste en panne

# 1. Présentation du problème

**Scénario**

Un camarade a choisi une panne sur mon PC, il a choisi un problème avec une clé USB.

La clé est branchée sur le poste, mais elle n'apparait dans l'Explorateur de fichiers.

**Symptôme constaté**:

La clé USB est branchée, mais elle n'est pas accessible dans "Ce PC".

# 2. Création de la panne

Pour simuler la panne, mon camarade a désactivé le lecteur USB d'une clé USB reconnue normalement par Windows.

Dans le **L'explorateur de fichier,** la clé USb apparaît sous: 
**Ce PC->General Udisk USB Device**

Avant la création de la panne, le périphérique était correctement détecté.

![panne1](https://hackmd.io/_uploads/SkOv_mYtMg.jpg)

Mon camarade a ensuite volontairement désactivé le périphérique afin de simluer une panne.

Pour cela: 

Aller dans gestionnaire de péripheriques->lecteur de disque->Périphériques portables->autres périphériques, regarder si votre clé USB apparaît.

Puic lique droit sur votre clé USB et désactiver l'appareil.

La clé USB n'est donc plus accesible dans l'Explorateur de fichiers.

# 3. Constatation de la panne

Après la désactivation du périphériques, nous avons vérifié l'Explorateur de fichiers.

La clé USB n'apparaît plus dans **Ce PC**.

![panne3](https://hackmd.io/_uploads/r1ndc7KYMg.jpg)


**Problème constaté:**

Le périphérique USB est branché, mais Windows ne permet plus d'accéder à la clé USB.

On a essayer de brancher la clé USB sur le PC d'un camarade et ce dernier avait accès a la clé USB, donc le problème est belle est bien du PC hote.
![panne7](https://hackmd.io/_uploads/BJDJimYYMe.jpg)

# 4. Diagnostic

**Hypothèse 1 - Problème de connexion USB**

Nous avons vérifié que la clé USB était toujours branché et qu'elle était détectée par Windows.

Le périphérique **General UDisk USB Device (la clé USB)** apparaît toujours dans le Gestionnaire de périphériques.

Cette hypothèse n'explique donc pas complètement le problème.

**Hypothèse 2 - Périphérique désactivé**

Nous avons ouvert les propriétés de la clé USB dans le Gestionnaire de périphériques.

Dans l'état du périphérique, Windows indique:
**Ce périphérique est désactivé(code 22).**
![panne4](https://hackmd.io/_uploads/HJts2XYKMl.jpg)
Cette information permet d'identifer la cause de la panne.

**Hypothèse 3 - Problème lié au pilote**

Nous avons également vérifié l'onglet **Pilote** du périphérique afin de consulter les informations du pilote.

![panne6](https://hackmd.io/_uploads/ByWcTQKKze.jpg)
Le problème identifié ne vient pas d'une absence du périphérique: celui-ci est bien reconnu par Windows, mais il est désactivé.


# 5. Solution

La cause du problème étant identifiée, nous avons réactiver le périphérique.

Pour cela:

1. Ouvrir le **Gestionnaire de périphériques.**
2. Aller dans Lecteurs de disque.
3. Faire un clic droit sur le nom de la clé USB 
4. Ouvrir Propiétés. 
5. cliquer sur **Activer le périphérique**

Le périphérique a alors été réactiver.

# 6. Vérification finale

Après avoir réactiver le périphérique, nous avons vérifié l'explorateur de fichiers.

La clé USB est de nouveau détectée et son contenu est accessible.
![panne8](https://hackmd.io/_uploads/BJqBkNFYMe.png)
Nous avons également vérifié que le périphérique était de nouveau actif dans le Gestionnaire de périphériques.

# 7. Résumé des hypothèses

| Hypothèse                 | Test                                                            | Résultat                               |
| ------------------------- | --------------------------------------------------------------- | -------------------------------------- |
| Problème de connexion USB | Vérification du branchement et du Gestionnaire de périphériques | Hypothèse écartée                      |
| Périphérique désactivé    | Vérification de l'état du périphérique                          | **Cause identifiée**                   |
| Problème lié au pilote    | Vérification de l'onglet Pilote                                 | Pilote présent, périphérique désactivé |

# 8. Conclusion

Le problème venait de la désactivation du périphérique General UDisk USB Device (Clé USB) dans le Gestionnaire de périphériques.

Le **Code 22** indiquait que le périphérique était désactivé.

Après avoir réactivé le périphérique, la clé USB a de nouveau été reconnue par Windows et son contenu était accessible.

Le post est donc de nouveau fonctionnel.


