# REVID — Pipeline audio et reconnaissance de véhicules

![Python](https://img.shields.io/badge/Python-3.10%20%7C%203.11-3776AB?logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Interface-Flask-000000?logo=flask&logoColor=white)
![TensorFlow](https://img.shields.io/badge/IA-TensorFlow%202.15.1-FF6F00?logo=tensorflow&logoColor=white)
![Licence](https://img.shields.io/badge/Licence-MIT-blue)

**REVID** est un projet de machine learning qui vise à reconnaître des modèles de véhicules à partir du son de leur moteur. Il combine un pipeline de collecte et de préparation des données audio, un filtrage des extraits pertinents, la génération de spectrogrammes et l'entraînement d'un modèle de classification.

Un tableau de bord web développé avec Flask permet de lancer le pipeline, de suivre son avancement, d'entraîner le modèle et de tester une prédiction à partir d'un fichier audio.

## Fonctionnalités

- **Collecte des données** : recherche de vidéos YouTube correspondant au véhicule demandé, jusqu'à atteindre une durée audio cible.
- **Nettoyage des métadonnées** : normalisation des noms de véhicules avec le modèle `llama-3.1-8b-instant` via l'API Groq.
- **Préparation audio** : téléchargement des pistes, découpage en segments de 3 secondes et suppression des segments trop silencieux.
- **Filtrage acoustique** : utilisation de YAMNet pour conserver les extraits susceptibles de contenir des sons de moteurs et écarter notamment la musique et la parole.
- **Génération des spectrogrammes** : conversion des segments audio en spectrogrammes Mel de 224 × 224 pixels avec Librosa.
- **Entraînement** : apprentissage d'un classifieur EfficientNetB0 avec TensorFlow/Keras à partir des spectrogrammes organisés par classe.
- **Prédiction** : envoi d'un fichier audio depuis le tableau de bord et retour de la classe prédite avec un score de confiance.
- **Suivi du pipeline** : affichage des journaux et de l'état d'avancement ; sauvegarde de l'état des étapes pour permettre une reprise après interruption.

## Architecture

## Architecture

```text
Collecte des vidéos et des métadonnées
  |
  v
Nettoyage des titres avec Groq
  |
  v
Extraction et découpage audio
  |
  v
Filtrage des segments avec YAMNet
  |
  v
Génération des spectrogrammes
  |
  v
Préparation du jeu de données
  |
  v
Entraînement d’EfficientNetB0
  |
  v
Modèle entraîné et prédiction
```

Le prétraitement utilisé pour la prédiction reprend les paramètres de génération des spectrogrammes employés pour l'entraînement afin de conserver une représentation cohérente des données.

## Technologies

| Technologie | Rôle dans le projet |
|---|---|
| Python | Langage principal |
| Flask et Flask-CORS | Tableau de bord et API locale |
| yt-dlp | Recherche de vidéos YouTube et récupération des pistes audio |
| Groq (`llama-3.1-8b-instant`) | Nettoyage des noms de véhicules dans les métadonnées |
| pydub | Découpage et normalisation des fichiers audio |
| YAMNet, TensorFlow Hub | Filtrage des segments selon leur contenu acoustique |
| Librosa, NumPy, Matplotlib | Traitement du signal et génération des spectrogrammes Mel |
| TensorFlow / Keras, EfficientNetB0 | Entraînement et utilisation du modèle de classification |
| SQLite | Stockage des vidéos collectées et de l'état du pipeline |

## Installation

### Prérequis

- Python **3.10 ou 3.11** ;
- Git et pip ;
- **FFmpeg**, installé séparément et accessible dans le `PATH` ;
- une connexion Internet pour la collecte des vidéos et le téléchargement initial du modèle YAMNet ;
- une clé API Groq pour l'étape de nettoyage des métadonnées.

La version de TensorFlow déclarée dans `requirements.txt` nécessite de conserver Python 3.10 ou 3.11 pour cette configuration.

### 1. Cloner le dépôt

```bash
git clone https://github.com/flavien-prm/revid-data-pipeline.git
cd revid-data-pipeline
```

### 2. Créer et activer un environnement virtuel

**Windows — PowerShell**

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

**Linux / macOS**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Installer les dépendances Python

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

FFmpeg n'est pas installé par cette commande. Vérifier sa disponibilité avec :

```bash
ffmpeg -version
```

### 4. Configurer la clé API Groq

Créer un fichier `.env` à partir du modèle fourni :

**Windows — PowerShell**

```powershell
Copy-Item .env.example .env
```

**Linux / macOS**

```bash
cp .env.example .env
```

Dans `.env`, renseigner la clé API :

```dotenv
GROQ_API_KEY=clé_api_groq
```

Ne jamais publier ce fichier ni y inscrire une clé dans le code source. Le fichier `.env` est exclu du dépôt par `.gitignore`.

### 5. Lancer le tableau de bord

Depuis la racine du dépôt, avec l'environnement virtuel activé :

```bash
python src/app.py
```

Ouvrir ensuite [http://127.0.0.1:5000](http://127.0.0.1:5000) dans un navigateur.

## Utilisation

Depuis le tableau de bord :

1. **Constituer un jeu de données** : indiquer le véhicule recherché et la durée audio cible, puis lancer la collecte et le traitement.
2. **Suivre l'exécution** : consulter les journaux et l'état des étapes au fur et à mesure de leur traitement.
3. **Entraîner le modèle** : lancer l'entraînement sur les spectrogrammes générés. Le dossier de données doit contenir plusieurs classes exploitables.
4. **Tester une prédiction** : envoyer un fichier audio depuis l'interface. Un modèle entraîné doit être disponible dans `models/`.

Le nettoyage des titres nécessite une clé Groq valide. La collecte dépend de la disponibilité des vidéos sur YouTube et peut varier selon les recherches effectuées.

## Données et fichiers générés

Les données audio et les modèles entraînés ne sont pas inclus dans le dépôt. Les dossiers `data/` et `models/` sont ignorés par Git et sont créés ou alimentés au cours de l'utilisation du projet.

| Chemin | Contenu |
|---|---|
| `data/03_audio/` | Fichiers audio téléchargés |
| `data/04_audio_chunks/` | Segments audio découpés |
| `data/05_audio_valide/` | Segments retenus après filtrage acoustique |
| `data/06_spectrograms/` | Spectrogrammes organisés par classe de véhicule |
| `models/` | Modèles entraînés utilisés pour la prédiction |

La base SQLite conserve notamment les métadonnées des vidéos et l'état du pipeline. Une première exécution doit constituer le jeu de données avant qu'un entraînement et une prédiction représentatifs soient possibles.

## Structure du dépôt

```text
revid-data-pipeline/
├── src/
│   ├── app.py                     # Tableau de bord Flask et routes API
│   ├── orchestrator.py             # Orchestration et reprise du pipeline
│   ├── scraper.py                  # Recherche de vidéos YouTube
│   ├── cleaner_groq.py             # Nettoyage des noms avec Groq
│   ├── downloader.py               # Téléchargement audio
│   ├── audio_cutter.py             # Découpage et normalisation audio
│   ├── yamnet_filter.py             # Filtrage acoustique avec YAMNet
│   ├── spectrogram_generator.py     # Génération des spectrogrammes Mel
│   ├── train_model.py               # Entraînement EfficientNetB0
│   ├── predict.py                   # Chargement du modèle et prédiction
│   ├── db.py                        # Accès à la base SQLite
│   └── templates/
│       └── dashboard.html           # Interface web
├── data/                            # Données générées, ignorées par Git
├── models/                          # Modèles entraînés, ignorés par Git
├── .env.example                     # Modèle de configuration
├── .gitignore
├── requirements.txt
├── LICENSE
└── README.md
```

## Limites et évaluation

Le résultat d'une prédiction dépend des classes présentes dans le jeu de données, de la qualité des enregistrements et des conditions de prise de son. Le score de confiance retourné pour un fichier ne constitue pas, à lui seul, une mesure de la précision globale du modèle. Une évaluation fiable nécessite un jeu de test distinct des données d'entraînement.

## Licence

Ce projet est distribué sous licence [MIT](LICENSE).
