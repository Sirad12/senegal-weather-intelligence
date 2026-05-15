# 🌍 Sénégal Environmental Monitoring Pipeline

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Kafka](https://img.shields.io/badge/Apache_Kafka-231F20?style=flat&logo=apachekafka&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![MinIO](https://img.shields.io/badge/MinIO-C72E49?style=flat&logo=minio&logoColor=white)
![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=flat&logo=streamlit&logoColor=white)
![Parquet](https://img.shields.io/badge/Parquet-50ABF1?style=flat&logo=apache&logoColor=white)

> Pipeline Big Data temps réel de monitoring environnemental au Sénégal.  
> Scraping météo → Kafka (3 brokers) → MinIO (Parquet) → Dashboard Streamlit.

---

## 🏗️ Architecture

```
Open-Meteo API  ─┐
                  ├──► producer.py ──► Kafka (3 brokers) ──► store_to_minio.py ──► MinIO (Parquet)
OpenWeatherMap  ─┘                                       └──► app.py (Dashboard Streamlit)
                                                                       ↑
                                                               JupyterLab (analyse libre)
```

---

## ⚙️ Services

| Service | Port | Description |
|---------|------|-------------|
| Kafka broker 1 | 9092 | Broker principal |
| Kafka broker 2 | 9093 | Broker réplique |
| Kafka broker 3 | 9094 | Broker réplique |
| MinIO API | 9000 | Stockage objet S3-compatible (Parquet) |
| MinIO Console | 9001 | Interface web MinIO |
| Dashboard | 8501 | Streamlit — visualisation temps réel |
| JupyterLab | 8888 | Notebooks d'analyse |

---

## 🚀 Démarrage rapide

```bash
# 1. Cloner le repo
git clone https://github.com/Sirad12/senegal-weather-intelligence.git
cd senegal-weather-intelligence

# 2. Configurer les variables d'environnement
cp .env.example .env

# 3. Lancer tous les services
docker compose up --build -d

# 4. Attendre ~30s que Kafka soit healthy
docker compose ps
```

### Accéder aux interfaces

| Interface | URL | Identifiants |
|-----------|-----|--------------|
| 📊 Dashboard Streamlit | http://localhost:8501 | — |
| 🗄️ MinIO Console | http://localhost:9001 | minioadmin / minioadmin |
| 📓 JupyterLab | http://localhost:8888 | token: dakar2024 |

---

## 📁 Structure du projet

```
senegal-weather-intelligence/
├── .env                        # Variables sensibles
├── .gitignore
├── docker-compose.yml
├── producer/
│   ├── producer.py             # Scraping Open-Meteo + OWM → Kafka
│   ├── store_to_minio.py       # Kafka → MinIO (format Parquet)
│   ├── Dockerfile.producer
│   └── Dockerfile.worker
├── consumer/
│   ├── app.py                  # Dashboard Streamlit temps réel
│   └── Dockerfile.dashboard
└── notebooks/
    └── senegal_analysis_starter.ipynb
```

---

## 🪣 Stockage MinIO

| Bucket | Contenu |
|--------|---------|
| `raw-data` | Données brutes telles que reçues de Kafka |
| `processed-data` | Données nettoyées et validées |

**Chemin des fichiers :**
```
{region_code}/{source}/year={Y}/month={M}/day={D}/{HHmmss}.parquet
```

---

## 📡 Topic Kafka

- **Topic** : `senegal-meteo`
- **Partitions** : 3
- **Réplication factor** : 3

---

## 👩🏾‍💻 Auteure

**Ndeye Sira Dia** — Étudiante en Licence Informatique option Big Data  
Dakar Institute of Technology · Dakar, Sénégal

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=flat&logo=linkedin&logoColor=white)](https://linkedin.com/in/sira-dia)

---
