# PROJ901 - Explication du code

Explication du code de l'analyse du pliage en l'air par corrélation d'images numériques (DIC).

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

### 4. Suivi du mouvement des points 

Le déplacement des points entre deux images est calculé grâce à l'algorithme de flux optique de Lucas Kanade, via la fonction `cv2.calcOpticalFlowPyrLK()`. Cet algorithme permet de retrouver la position des points dans l'image suivante.

Si un point disparaît, alors il est automatiquement supprimé pour éviter les erreurs.

### 5. Calcul de l'orientation des ailes

A partir des points suivis, le programme calcule une droite moyenne représentant chaque aile de la tôle. Cette estimation est réalisée avec la fonction `cv2.fitLine()`.

Cela permet d'obtenir l'orientation de chaque côté de la tôle.

### 6. Calibration de l'angle initial

La première image correspond à la tôle à plat, donc l'angle doit être de 180°. Pourtant, à cause du positionnement de la caméra ou de petits défauts d'alignement, ce n'est pas toujours exactement le cas.

Le programme calcule donc automatiquement une correction angulaire qui permet de forcer l'angle initial à 180°. Cette correction est ensuite appliquée à toutes les images.

### 7. Calcul de l'angle de pliage

L'angle entre les deux ailes est calculé à partir des vecteurs directeurs des deux droites. Le calcul utilise la formule du produit scalaire entre deux vecteurs, et on obtient ainsi l'angle de pliage pour chaque image de la séquence.

### 8. Génération de la vidéo

Pour visualiser les résultats, le programme génère une vidéo du pliage analysé.

Sur chaque image :
- les droites représentant les ailes sont tracées,
- l'angle mesuré est affiché.

Les images sont directement enregistrées dans une vidéo afin d'augmenter la vitesse de traitement, tout en permettant une vérification de la bonne exécution du code après sa réalisation.

### 9. Calcul du retour élastique

Une fois toutes les images analysées, le programme calcule le retour élastique.

Deux valeurs sont identifiées :
- l'angle minimum, qui correspond au moment où le poinçon est enfoncé au maximum,
- l'angle final, mesuré lorsque le poinçon est retiré.

Le retour élastique est alors calculé par : `springback = angle_final - angle_min`

### 10. Génération du graphique

Le programme trace enfin un graphique montrant l'évolution de l'angle de pliage au cours du temps.

Ce graphique permet de visualiser :
- la phase de pliage,
- l'angle minimum atteint,
- la phase de relâchement,
- le retour élastique.

Le graphique est sauvegardé automatiquement dans le dossier des images.

## Comment utiliser le programme

### 1. Installer les bibliothèques nécessaires

```
pip install opencv-python numpy matplotlib
```

### 2. Modifier le chemin du dossier d'images

A la fin du script, il faut indiquer le chemin du dossier contenant les images :
```
main_dic_analysis(r'D:\DP600\-5mm\1')
```

### 3. Sélectionner les zones d'analyse

Une fois le programme dans python exécuté, il faut sur la première image :
1. sélectionner l'aile gauche,
2. sélectionner l'aile droite.

### 4. Lancer l'analyse

Après avoir appuyé sur la touche `entrée`, le programme va :
- analyser toutes les images,
- générer la vidéo,
- calculer le retour élastique,
- tracer le graphique final.

## Fichiers générés 

A la fin du programme, plusieurs fichiers sont créés automatiquement :

**Vidéo du suivi du pliage**
```
00_Resultat_Pliage.avi
```

**Graphique de l'évolution de l'angle**
```
00_Courbe_Evolution_Pliage.png
```
