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
