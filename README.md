# REVID — Pipeline de données audio et entraînement IA

![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)
![IA](https://img.shields.io/badge/IA-Deep%20Learning-orange)
![Domaine](https://img.shields.io/badge/Domaine-Classification%20Audio-2E8B57)

**REVID** est un projet visant à identifier des modèles de véhicules et leurs types de motorisation à partir de leurs empreintes sonores, grâce au traitement du signal audio et à l'intelligence artificielle.

Ce dépôt contient le pipeline de traitement des données utilisé pour collecter, prétraiter et préparer les enregistrements audio destinés à l'entraînement des modèles d'apprentissage automatique.

## Vue d'ensemble

Le pipeline a pour objectif de transformer des enregistrements audio bruts en données exploitables pour entraîner un réseau de neurones convolutif (CNN).

Les principales étapes sont :

1. **Collecte des données** — Récupération d'enregistrements audio de moteurs de véhicules.
2. **Prétraitement audio** — Nettoyage, découpage et normalisation des signaux audio.
3. **Extraction des caractéristiques** — Transformation des signaux audio en spectrogrammes.
4. **Entraînement du modèle** — Apprentissage des caractéristiques sonores à l'aide d'un modèle d'intelligence artificielle.
5. **Exportation du modèle** — Sauvegarde des poids du modèle entraîné pour une utilisation ultérieure.

## Architecture du pipeline

```text
Enregistrements audio bruts
          |
          v
Prétraitement audio
          |
          v
Extraction des caractéristiques
   (Spectrogrammes)
          |
          v
Préparation du jeu de données
          |
          v
Entraînement du réseau de neurones
          |
          v
Modèle entraîné
          |
          v
Classification des véhicules
```

## Technologies utilisées

Le pipeline s'appuie sur les technologies suivantes :

| Technologie | Utilisation |
|---|---|
| Python | Langage de programmation principal |
| NumPy | Calculs numériques |
| Pandas | Manipulation des jeux de données |
| Librosa | Traitement audio et extraction de caractéristiques |
| SciPy | Traitement du signal |
| Matplotlib | Visualisation des données |
| PyTorch / TensorFlow | Entraînement des modèles d'apprentissage profond |
| EfficientNetB0 | Architecture de réseau de neurones envisageable pour la classification des spectrogrammes |

Les bibliothèques et l'architecture réellement utilisées dépendent de l'implémentation présente dans le dépôt.

## Installation

### Prérequis

- Python 3.10 ou version ultérieure
- Git
- Pip

### 1. Cloner le dépôt

```bash
git clone https://github.com/flavi1lv/revid-data-pipeline.git
cd revid-data-pipeline
```

### 2. Créer un environnement virtuel

**Windows**

```bash
python -m venv .venv
.venv\Scripts\activate
```

**Linux / macOS**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Installer les dépendances

Si le dépôt contient un fichier `requirements.txt` :

```bash
pip install -r requirements.txt
```

Si ce fichier n'existe pas, installe les dépendances nécessaires selon la configuration réelle du projet.

## Structure du projet

Voici une organisation indicative des différents composants du pipeline. Les noms de dossiers peuvent varier selon l'implémentation réelle.

```text
revid-data-pipeline/
├── data_scraping/     # Collecte des données audio
├── preprocessing/     # Prétraitement des fichiers audio
├── training/          # Entraînement des modèles
├── models/            # Sauvegarde des modèles entraînés
├── requirements.txt   # Dépendances Python
├── README.md
└── LICENSE
```

## Approche d'apprentissage automatique

Le projet explore la classification de véhicules à partir de leurs caractéristiques acoustiques.

Les enregistrements de moteurs présentent des particularités sonores susceptibles de permettre la distinction entre différents modèles de véhicules et types de motorisation.

Les spectrogrammes représentent l'évolution des fréquences d'un signal audio au cours du temps. Ils permettent notamment aux réseaux de neurones convolutifs d'apprendre des motifs acoustiques pertinents.

Une architecture telle qu'**EfficientNetB0** peut être adaptée à la classification de spectrogrammes sous forme d'images, à condition que la représentation des données et la configuration du modèle soient compatibles.

Les performances dépendent notamment de la qualité du jeu de données, de l'équilibre entre les classes, des conditions d'enregistrement et de la méthode d'évaluation.

## Qualité des données

Pour obtenir des résultats fiables, plusieurs éléments doivent être pris en compte :

- La cohérence des étiquettes associées aux véhicules.
- Les différences d'environnement et de matériel d'enregistrement.
- Les bruits parasites et les sons superposés.
- Le déséquilibre éventuel entre les catégories de véhicules.
- La séparation des données d'entraînement, de validation et de test afin de limiter les fuites de données.

## Reproductibilité des expériences

Pour faciliter la reproduction des expériences, il est recommandé de documenter :

- Les sources et versions des jeux de données.
- Les paramètres de prétraitement audio.
- La configuration de l'extraction des caractéristiques.
- L'architecture du modèle et les paramètres d'entraînement.
- Les graines aléatoires et les métriques d'évaluation.

Les jeux de données volumineux, les enregistrements bruts et les poids des modèles peuvent être stockés séparément du code source lorsque cela est pertinent.

## Perspectives d'évolution

Plusieurs pistes peuvent être envisagées pour faire évoluer le projet :

- Enrichir le jeu de données audio.
- Améliorer la réduction du bruit et le prétraitement.
- Comparer différentes représentations des signaux audio.
- Évaluer plusieurs architectures de réseaux de neurones.
- Mesurer les performances dans différentes conditions d'enregistrement.
- Intégrer les modèles entraînés dans une application de reconnaissance de véhicules.

## Licence

La licence du projet est définie par le fichier `LICENSE` présent à la racine du dépôt.
---

*REVID — La reconnaissance des véhicules par le son et l'intelligence artificielle.*
