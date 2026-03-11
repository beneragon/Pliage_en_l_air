# PROJ901 - Explication du code

Explication du code de l'analyse du pliage en l'air par DIC.

## Objectif du programme

Ce code permet d'analyser une séquence d'images d'un essai de pliage en l'air afin de mesurer automatiquement l'évolution de l'angle de pliage d'une tôle.

L'objectif est de suivre la déformation de la pièce pendant le procédé et de calculer le retour élastique **(springback)** une fois que le poinçon est retiré.

Pour cela, le code utilise des méthodes de vision par ordinateur et de suivi de points inspirées de la corrélation d'images (DIC, Digital Image Correlation).
A partir des images du pliage, le programme va suivre le mouvement de la tôle, mesurer l'angle formé entre les deux ailes et tracer l'évolution de cet angle au cours du temps.

A la fin de l'analyse, le programme fournit :
- une vidéo montrant le suivi du pliage,
- un graphique de l'évolution de l'angle mesuré,
- la valeur du retour élastique.
