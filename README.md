# Bidabi Clone Alone

Projet de classification d'images de produits alimentaires à l'aide d'un modèle ResNet-18, avec versionnement du code (Git) et des données (DVC).

## Structure du projet

'''
bidabi-clone-alone/
├── src/
│   ├── asyscrapper.py       # Scrapper pour collecter les données
│   ├── classificator.py     # Pipeline d'entraînement ResNet-18
│   └── data_loader.py       # Chargement des données
├── data/
│   ├── raw/
│   │   ├── images/          # Images par catégorie
│   │   │   ├── bread/
│   │   │   ├── butter/
│   │   │   ├── milk/
│   │   │   └── sugar/
│   │   └── metadata_<categorie>.csv
│   └── localstore/          # Remote DVC local
├── requirements.txt
├── .gitignore
└── README.md
'''

## Prérequis

- Python 3.12
- Git
- DVC

## Installation

1. Cloner le dépôt
```bash
git clone git@github.com:<votre-utilisateur>/bidabi-clone-alone.git
cd bidabi-clone-alone
```

2. Créer et activer l'environnement virtuel
```bash
py -3.12 -m venv .venv
.venv\Scripts\Activate
```

3. Installer les dépendances
```bash
pip install -r requirements.txt
```

4. Récupérer les données avec DVC
```bash
dvc pull
```

## Collecte des données

Pour collecter les images d'une catégorie, modifier `CATEGORY` dans `src/asyscrapper.py` :

```python
CATEGORY = "sugar"  # "bread", "milk", "butter"
```

Puis lancer :
```bash
python src/asyscrapper.py
```

Les images seront sauvegardées dans `data/raw/images/<categorie>/` et les métadonnées dans `data/raw/metadata_<categorie>.csv`.

## Entraînement du modèle

```bash
python src/classificator.py
```

Le pipeline effectue :
- Chargement du dataset (4 catégories)
- Séparation train/val/test (60/20/20)
- Entraînement ResNet-18 avec MixUp et augmentations
- Early stopping (patience=3)
- Évaluation : matrice de confusion, courbes ROC, accuracy par classe
- Sauvegarde du meilleur modèle dans `best_model_resnet18_finetuned.pth`

## Versionnement des données avec DVC

```bash
dvc add data/raw
git add data/raw.dvc
git commit -m "Update dataset"
dvc push
```

## Versions du projet

| Tag | Description |
|-----|-------------|
| v1.0 | Version initiale du projet |
| v2.0 | Nouveau scrapper et données RAW |
| v2.1 | Dataset RAW versionné avec DVC |
| v3.0 | Pipeline complet d'entraînement |