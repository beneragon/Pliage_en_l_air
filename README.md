# Pliage_en_l_air
Analyse de l'angle de pliage par corrélation d'images numériques

Ce projet permet de mesurer l'évolution de l'angle de pliage d'une plaque de tôle en temps réel à partir d'une séquence d'images.

Fonctionnalités :
- Tri automatique des images BMP.
- Suivi de texture par Optical Flow (Lucas-Kanade).
- Calcul de l'angle entre les deux ailes.
- Export des résultats en tableau.

Installation :
Installez les dépendances :bash pip install opencv-python numpy

Dans le code, modifiez le chemin du folder ligne 218 : main_dic_analysis(r'image_folder')

Lancez le script


# PROJ901 - Pliage en l'air

Suivi du pliage en l'air par corrélation d'images (DIC).

## Contexte

Le pliage de tôles est un procédé fondamental utilisé dans de nombreusx domaines : automobile, aéronautique, tôlerie, industrie de précision. Parmi eux, le pliage en l'air est l'une des méthodes les plus utilisées en raison de sa rapidité de mise en oeuvre, de sa compatibilité avec une large gamme de matériaux et d'épaisseurs et de sa capacité à produire une large gamme d'angles avec un même jeu poinçon-matrice.

Lors du pliage, un poinçon applique un effort sur la tôle positionnée sur une matrice en V : elle déforme la pièce jusqu'à obtenir l'angle de pliage souhaité. La géométrie finale dépend :
- de la course du poinçon,
- de l'ouverture de la matrice en V,
- des propriétés mécaniques du matériau.

Cependant, l'un des principaux problèmes lié au pliage en l'air est le retour élastique (appelé springback). Lors du retrait du poinçon, une partie de la déformation élastique accumulée pendant la mise en forme est relâchée, ce qui entraîne une variation entre l'angle final obtenu et l'angle sous charge souhaité. Il devient alors essentiel de déterminer ce springback afin de limiter les coûts et les temps de cycle.

## Objectif du projet

Concevoir un dispositif de mesure in-line basé sur la vision par ordinateur permettant :
- de suivre l'évolution de l'angle de pliage en temps réel,
- d'identifier l'angle maximal sous charge,
- de mesurer l'angle final après déchargement,
- de quantifier automatiquement le springback.

## Solution retenue

Pour mesurer l'angle de pliage, nous utilisons la Corrélation d'Images Numériques (DIC). La DIC est une méthode de vision par ordinateur qui permet de mesurer les déplacements d'un objet en comparant les images prises à différents instants.

Principe :
1. On applique un motif aléatoire (appelé speckle) sur la surface de la tôle.
2. Pendant le pliage, des images sont prises en continu, de façon frame-by-frame.
3. Le programme découpe l'image en petits carrés de pixels.
4. Il suit le déplacement de ces petits carrés d'une image à l'autre.
5. A partir de ces déplacements, on calcule :
   - la rotation des segments extérieurs,
   - l'évolution de l'angle de pliage,
   - puis le retour élastique.

## Architecture du projet

///

## Résultat attendu

Le programme renvoie : 
- la courbe des angles mesurés au cours du temps,
- la valeur de l'angle maximal sous-charge,
- la valeur de l'angle final après retour élastique,
- la valeur du retour élastique.
