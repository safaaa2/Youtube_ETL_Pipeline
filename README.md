# 🎬 MrBeast YouTube Analytics Pipeline

<div align="center">

![Python](https://img.shields.io/badge/Python-3.11+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![YouTube API](https://img.shields.io/badge/YouTube_Data_API-v3-FF0000?style=for-the-badge&logo=youtube&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-2.0+-150458?style=for-the-badge&logo=pandas&logoColor=white)
![Power BI](https://img.shields.io/badge/Power_BI-Dashboard-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

**Pipeline ETL complet pour l'analyse des données de la chaîne YouTube MrBeast**

*Extraction · Transformation · Visualisation*

[Vue d'ensemble](#-vue-densemble) · [Installation](#-installation) · [Structure](#-structure-du-projet) · [Utilisation](#-utilisation) · [Dashboard](#-dashboard-power-bi)

</div>

---

## 📌 Vue d'ensemble

Ce projet implémente un **pipeline de Data Engineering** de bout en bout qui collecte, transforme et visualise les données publiques de la chaîne YouTube de **MrBeast** ([@MrBeast](https://www.youtube.com/@MrBeast)), l'une des plus grandes chaînes du monde avec plus de **300 millions d'abonnés**.

### Objectifs analytiques

- 📊 Analyser la **performance des vidéos** (vues, likes, commentaires)
- 📈 Étudier l'**évolution temporelle** de l'engagement
- 🎯 Identifier les **formats de contenu** les plus performants
- 🔍 Comprendre les **patterns de publication** optimaux

### Architecture du pipeline

```
YouTube Data API v3
        │
        ▼
┌─────────────────┐
│  Extraction     │  ← requests, python-dotenv
│  Python Script  │
└────────┬────────┘
         │  JSON brut
         ▼
┌─────────────────┐
│  Stockage brut  │  ← data/raw/
│  JSON files     │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Transformation │  ← pandas, isodate
│  & Nettoyage    │
└────────┬────────┘
         │  CSV structuré
         ▼
┌─────────────────┐
│   Power BI      │  ← Dashboards analytiques
│   Dashboard     │
└─────────────────┘
```

---

## 🏗️ Structure du projet

```
mrbeast-youtube-pipeline/
│
├── 📁 src/
│   ├── extract.py          # Extraction via YouTube API
│   ├── transform.py        # Transformation et nettoyage
│   └── pipeline.py         # Orchestration du pipeline complet
│
├── 📁 data/
│   ├── raw/                # Données brutes JSON (gitignored)
│   │   ├── channel_info.json
│   │   ├── video_ids.json
│   │   └── videos_raw.json
│   └── processed/          # Dataset final CSV
│       └── mrbeast_videos.csv
│
├── 📁 docs/
│   ├── SETUP.md            # Guide de configuration API
│   ├── DATA_DICTIONARY.md  # Description des champs
│   └── screenshots/        # Captures Power BI
│
├── 📁 notebooks/
│   └── exploration.ipynb   # Exploration exploratoire
│
├── 📁 tests/
│   └── test_extract.py     # Tests unitaires
│
├── .env.example            # Template variables d'environnement
├── .gitignore
├── requirements.txt
└── README.md
```

---

## ⚙️ Installation

### Prérequis

- Python 3.11 ou supérieur
- Compte Google Cloud Platform (gratuit)
- Power BI Desktop (gratuit)

### Étape 1 — Cloner le dépôt

```bash
git clone https://github.com/votre-username/mrbeast-youtube-pipeline.git
cd mrbeast-youtube-pipeline
```

### Étape 2 — Environnement virtuel

```bash
# Créer l'environnement virtuel
python -m venv venv

# Activer (Windows)
venv\Scripts\activate

# Activer (macOS / Linux)
source venv/bin/activate
```

### Étape 3 — Dépendances

```bash
pip install -r requirements.txt
```

### Étape 4 — Configuration de l'API

```bash
# Copier le template
cp .env.example .env

# Éditer .env avec votre clé API
nano .env  # ou code .env
```

Contenu du fichier `.env` :
```env
YOUTUBE_API_KEY=votre_clé_api_ici
CHANNEL_ID=UCX6OQ3DkcsbYNE6H8uQQuVA
```

> 📖 Voir [docs/SETUP.md](docs/SETUP.md) pour obtenir une clé API Google.

---

## 🚀 Utilisation

### Exécution complète du pipeline

```bash
python src/pipeline.py
```

### Étapes individuelles

```bash
# Extraction uniquement
python src/extract.py

# Transformation uniquement
python src/transform.py
```

### Paramètres disponibles

```bash
python src/pipeline.py --max-videos 500 --output data/processed/output.csv
```

---

## 📡 Endpoints YouTube API utilisés

| Endpoint | Usage | Quota |
|----------|-------|-------|
| `channels.list` | Métadonnées de la chaîne | 1 unité |
| `playlists.list` | Playlist principale | 1 unité |
| `playlistItems.list` | IDs des vidéos (paginé) | 1 unité/page |
| `videos.list` | Détails vidéo (50 par batch) | 1 unité/batch |

> ⚠️ **Quota journalier** : 10 000 unités/jour (gratuit). Le pipeline gère automatiquement le batching pour optimiser la consommation.

---

## 📊 Dataset final — Champs extraits

| Champ | Type | Description |
|-------|------|-------------|
| `video_id` | string | Identifiant unique YouTube |
| `title` | string | Titre de la vidéo |
| `published_at` | datetime | Date de publication (UTC) |
| `duration_seconds` | int | Durée en secondes |
| `duration_formatted` | string | Format HH:MM:SS |
| `view_count` | int | Nombre de vues |
| `like_count` | int | Nombre de likes |
| `comment_count` | int | Nombre de commentaires |
| `engagement_rate` | float | (likes+comments)/views |
| `like_ratio` | float | likes/views |
| `year` | int | Année de publication |
| `month` | int | Mois de publication |
| `day_of_week` | string | Jour de publication |

---

## 📈 Dashboard Power BI

Le dashboard inclut les visualisations suivantes :

### 🔢 KPIs Globaux
- Total des vues de la chaîne
- Nombre total de vidéos analysées
- Taux d'engagement moyen
- Durée moyenne des vidéos

### 📊 Graphiques
- **Évolution des vues** dans le temps (courbe)
- **Top 10 vidéos** les plus vues (barres horizontales)
- **Distribution des durées** (histogramme)
- **Engagement vs Vues** (scatter plot)
- **Performance par jour de publication** (heatmap)
- **Évolution mensuelle** des likes et commentaires

### 📸 Screenshots

<img width="1167" height="650" alt="image" src="https://github.com/user-attachments/assets/47f7b69e-3396-4b60-95a6-df744bb306d3" />

<img width="1275" height="717" alt="image" src="https://github.com/user-attachments/assets/3f7ce83d-5717-4331-bd0b-789b418b3cf1" />






## 🔐 Sécurité

- ✅ La clé API est stockée dans `.env` (non commité)
- ✅ `.gitignore` exclut les fichiers `.env` et `data/raw/`
- ✅ Aucune credential n'est hardcodée dans le code source

---

## 📦 requirements.txt

```
google-api-python-client==2.111.0
python-dotenv==1.0.0
pandas==2.1.4
isodate==0.6.1
requests==2.31.0
tqdm==4.66.1
```

---

## 🤝 Contribution

Les contributions sont les bienvenues !

1. Forker le projet
2. Créer une branche (`git checkout -b feature/nouvelle-fonctionnalite`)
3. Commiter (`git commit -m 'feat: ajout nouvelle fonctionnalité'`)
4. Pusher (`git push origin feature/nouvelle-fonctionnalite`)
5. Ouvrir une Pull Request

---

## 📄 Licence

Distribué sous licence MIT. Voir `LICENSE` pour plus d'informations.

---

## 🙏 Remerciements

- [YouTube Data API v3 Documentation](https://developers.google.com/youtube/v3)
- [MrBeast](https://www.youtube.com/@MrBeast) pour l'inspiration du contenu
- [Google Cloud Platform](https://console.cloud.google.com/)

---

<div align="center">

**Réalisé dans le cadre d'un projet de Data Engineering**

⭐ N'oubliez pas de mettre une étoile si ce projet vous a été utile !

</div>
