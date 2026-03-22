<div align="center">
  <img src="logo.jpeg" alt="Loop Logo" width="300"/>

  <h1>🔁 Loop — Feel the Rhythm, Find Your Sound</h1>

  <p>
    A smart, Spotify-powered music recommendation engine that helps you discover songs you'll love — built with a hybrid ML approach, an intuitive Streamlit interface, and a full MLOps pipeline.
  </p>

  ![Python](https://img.shields.io/badge/Python-3.12-blue?logo=python)
  ![Streamlit](https://img.shields.io/badge/Streamlit-1.41-red?logo=streamlit)
  ![Docker](https://img.shields.io/badge/Docker-containerized-blue?logo=docker)
  ![DVC](https://img.shields.io/badge/DVC-data%20versioned-945DD6?logo=dvc)
  ![CI/CD](https://img.shields.io/badge/CI%2FCD-GitHub%20Actions-black?logo=github-actions)
  ![AWS](https://img.shields.io/badge/Deploy-AWS%20CodeDeploy-orange?logo=amazon-aws)

</div>

---

## 📖 Overview

**Loop** is an end-to-end music recommendation system that takes any song name as input and returns personalised suggestions with playable Spotify audio previews — right inside the browser.

The system intelligently selects the best recommendation strategy based on data availability:

- If the song is in the collaborative filtering dataset, Loop activates the **Hybrid Recommender System**, blending content-based and collaborative signals with a user-controlled diversity slider.
- Otherwise, it falls back to pure **Content-Based Filtering**, still delivering relevant recommendations using audio feature similarity.

---

## ✨ Features

- 🎵 **Instant Recommendations** — Enter any song and get 5, 10, 15, or 20 suggestions in seconds
- 🎧 **In-App Audio Previews** — Listen to Spotify 30-second previews without leaving the page
- ⚖️ **Diversity Slider** — Tune the balance between *personalised* (same vibe) and *diverse* (explore new) recommendations
- 📊 **Live Mix Ratio Chart** — See your personalisation vs. diversity split rendered as an interactive bar chart
- 🤖 **Auto-Strategy Selection** — The app automatically picks the best recommendation method for each song
- 🐳 **Docker Ready** — Fully containerised for consistent local or cloud deployment
- 🔄 **CI/CD Pipeline** — Automated testing and deployment via GitHub Actions and AWS CodeDeploy
- 🗂️ **DVC Data Versioning** — All training data and artefacts tracked and reproducible

---

## 🧠 How It Works

### Content-Based Filtering
Uses **cosine similarity** on a TF-IDF / feature-transformed matrix of song audio attributes (energy, tempo, valence, etc.) to find songs with the most similar sonic fingerprint to your input.

### Collaborative Filtering
Builds a **user–song interaction matrix** and finds songs that listeners with similar tastes have enjoyed — surfacing tracks you might not have found through audio features alone.

### Hybrid Recommender System
Combines both approaches with a **weighted scoring formula**:

```
final_score = (w_content × content_similarity) + (w_collab × collab_similarity)
```

Both similarity vectors are **min-max normalised** before combination, ensuring neither method dominates purely because of scale. The weights are controlled by the diversity slider in the UI — slide towards *Personalised* (w_content = 1.0) or *Diverse* (w_collab = 1.0), or anything in between.

---

## 🗂️ Project Structure

```
loop/
├── app.py                        # Streamlit web application
├── content_based_filtering.py    # Content-based recommendation logic
├── collaborative_filtering.py    # Collaborative filtering logic
├── hybrid_recommendations.py     # HybridRecommenderSystem class
├── data_cleaning.py              # Data preprocessing pipeline
├── transform_filtered_data.py    # Feature engineering & matrix creation
├── test_app.py                   # Unit tests
│
├── data/                         # DVC-tracked datasets & artefacts
│   ├── cleaned_data.csv          # Cleaned Spotify song metadata
│   ├── collab_filtered_data.csv  # Songs with collaborative filtering support
│   ├── transformed_data.npz      # Content-based feature matrix (sparse)
│   ├── transformed_hybrid_data.npz  # Hybrid feature matrix (sparse)
│   ├── interaction_matrix.npz    # User–song interaction matrix (sparse)
│   └── track_ids.npy             # Track ID index array
│
├── notebooks/                    # EDA and experimentation notebooks
├── deploy/                       # Deployment scripts for AWS CodeDeploy
│
├── Dockerfile                    # Container definition
├── appspec.yml                   # AWS CodeDeploy configuration
├── dvc.yaml                      # DVC pipeline definition
├── dvc.lock                      # Locked DVC pipeline state
└── requirements.txt              # Python dependencies
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.12+
- Docker (optional, for containerised run)
- DVC (optional, for pulling data from remote)

### 1. Clone the Repository

```bash
git clone https://github.com/BayesianDecoder/loop.git
cd loop
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Pull Data with DVC

If you have access to the DVC remote (e.g. S3), pull the processed data artefacts:

```bash
dvc pull
```

> **Note:** If you don't have DVC remote access, you can run the pipeline locally to regenerate data files:
> ```bash
> dvc repro
> ```

### 4. Run the App

```bash
streamlit run app.py
```

The app will open at `http://localhost:8501`.

---

## 🐳 Docker

### Build the Image

```bash
docker build -t loop-app .
```

### Run the Container

```bash
docker run -p 8000:8000 loop-app
```

The app will be available at `http://localhost:8000`.

---

## 🧪 Running Tests

```bash
pytest test_app.py
```

---

## ⚙️ CI/CD Pipeline

The repository includes a **GitHub Actions** workflow (`.github/workflows/`) that automatically:

1. Runs tests on every push
2. Builds the Docker image
3. Deploys to AWS via **CodeDeploy** using `appspec.yml`

---

## 📦 Key Dependencies

| Library | Purpose |
|---|---|
| `streamlit` | Interactive web UI |
| `scikit-learn` | Cosine similarity, preprocessing |
| `scipy` | Sparse matrix operations |
| `pandas` / `numpy` | Data manipulation |
| `dvc` + `dvc-s3` | Data versioning & remote storage |
| `boto3` | AWS S3 integration |
| `pytest` | Unit testing |

---


## 📄 License

This project is open source. See the repository for details.

---

<div align="center">
  Made with 🎵 by <a href="https://github.com/BayesianDecoder">BayesianDecoder</a>
</div>
