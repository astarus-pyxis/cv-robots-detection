# Caméra déportée pour la détection de robots

## Objectifs du projet

Projet développé dans le cadre des activités du club de robotique de l'ISAE-SUPAERO.

La caméra déportée doit permettre de récupérer les positions des objets sur la plateforme et de les renvoyer aux robots par télémétrie.
Ce système se compose d'un Raspberry Pi 3 B+, d'une caméra grand angle et d'un module de télémétrie. Il est fixé au bord du terrain, sur un mât d'un mètre et demi de haut afin de voir l'ensemble de la plateforme.

## Avancement du projet

Toutes les fonctions de traitement d'image ont été implémentées avec succès. Le raspberry Pi prend une image, corrige les déformations dues à la lentille de la caméra, renormalize l'image pour la mettre à plat et détecte les tags Aruco des robots, le tout en une seconde environ.

L'envoi des données par module LoRa n'a pas été implémenté.

## Organisation du dépôt

+ src               
| aruco.py
| constants.py
| detectRobotsInsideAruco.py
| imageNormalizer.py
| test_capture_jpeg.py
| test_para.py
| test.py
| undistort.py
| calibrateCamera.py (présent dans cmd uniquement)

+ platformImages
+ normalizedImages
+ npz
+ json

- npz contient le fichier NPZ obtenu à la calibration de la caméra, utilisé par les codes Python

- json contient des fichiers JSON utilisés par les codes Python  

- platformImages contient l'image de référence de la plateforme et les images de la plateforme prises par la caméra telles quelles

- normalizedImages contient les images traitées

- src contient les codes Python:
    - aruco.py est une librairie utilisée pour détecter les tags Aruco
    - constants.py contient les constantes utiles au module Lora
    - detectRobotsIndideAruco.py détecte les robots et objets situés à l'intérieur du rectangle définir par les quatre tags Aruco de la plateforme
    - imageNormalizer.py traite les images pour corriger les déformations de la lentille de la caméra et aligner l'image prise avec l'image de référence, pour ensuite détecter les objets avec detectRobotsInsideAruco.py
    - test_capture_jpeg.py est un test pour prendre une image avec la caméra en utilisant picamera2
    - test.py effectue un test général des fonctions attendues par la caméra déportée (prise d'une image, traitement, détection des objets)
    - undistort.py est une librairie utilisée pour corriger les distorsions dues à la lentille de la caméra. Cette librairie est appelée par imageNormalizer.py
    - calibrateCamera.py sert à calibrer la caméra et obtenir le fichier NPZ.

## Fonctionnement

Le Pi doit contenir tous les codes de ce répertoire. Les packages doivent être installés dans un environnement virtuel, à activer à chaque utilisation de la Pi. Pour faire une démo des codes, se connecter en SSH à la Pi (mot de passe par défaut: raspberry) puis lancer les commandes suivantes:

```
cd ./raspberry
source ./rasbperry_venv/bin/activate
cd ./src
python test.py
```