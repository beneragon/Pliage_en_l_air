# PROJ901 - Explication du code

Explication du code de l'analyse du pliage en l'air par DIC.

## Objectif du programme

Ce code permet d'analyser une séquence d'images d'un essai de pliage en l'air afin de mesurer automatiquement l'évolution de l'angle de pliage d'une tôle.

L'objectif est de suivre la déformation de la pièce pendant le procédé et de calculer le retour élastique (springback) une fois que le poinçon est retiré.

Pour cela, le code utilise des méthodes de vision par ordinateur et de suivi de points inspirées de la corrélation d'images (DIC, Digital Image Correlation).
A partir des images du pliage, le programme va suivre le mouvement de la tôle, mesurer l'angle formé entre les deux ailes et tracer l'évolution de cet angle au cours du temps.

A la fin de l'analyse, le programme fournit :
- une vidéo montrant le suivi du pliage,
- un graphique de l'évolution de l'angle mesuré,
- la valeur du retour élastique.

## Bibliothèques utilisées

Le programme est écrit en Python et utilise plusieurs bibliothèques :
- **OpenCV (cv2)** : utilisée pour le traitement des images et le suivi des points,
- **NumPy** : utilisée pour les calculs mathématiques,
- **Matplotlib** : utilisée pour tracer le graphique final,
- **glob / os / re** : utilisées pour gérer et trier les fichiers,
- **time** : utilisée pour mesurer le temps de calcul du code.

## Principe général du programme

Le fonctionnement du programme peut être résumé en dix étapes.

### 1. Chargement et tri des images

Le programme commence par charger toutes les images présentes dans le dossier de l'essai.

Les fichiers sont ensuite triés dans le bon ordre grâce à une fonction appelée `natural_sort_key`.

Cette fonction permet d'éviter les erreurs de tri, et de considérer `image_2` après `image_10`, et donc de garantir que les images sont bien analysées dans l'ordre chronologique du pliage (et non dans l'ordre alphabétique du nom des fichiers).

### 2. Sélection des zones à analyser

Sur la première image, l'utilisateur doit sélectionner deux zones :
- une zone sur l'aile gauche de la tôle,
- une zone sur l'aile droite.

Ces zones sont appelées ROI (Region Of Interest).

Elles doivent être placées sur des zones rigides de la tôle, et non sur la zone du pli, afin de mesurer correctement la rotation des deux parties. Cette sélection est réalisée grâce à la fonction `cv2.selectROI()`.

### 3. Détection des points à suivre 

Une fois les zones sélectionnées, le programme détecte automatiquement des points de texture dans ces régions. Cette étape est assurée via la fonction `cv2.goodFeaturesToTrack()`.

Ces points correspondent aux zones de l'image où il y a du contraste (speckle). Le speckle (texture) sert de repère visuel qui pourra être suivi dans les images suivantes.
