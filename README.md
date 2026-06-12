# 🛡️ DisasterShield — Real-Time Reddit Rumor Detection System

An end-to-end NLP system that analyzes Reddit posts in real time to detect and classify disaster-related rumors during crisis events. Built with a DistilBERT + SVM ensemble, deployed with a FastAPI backend, PostgreSQL database on Render, and a React Native (Expo) mobile frontend.

---

## 📌 Table of Contents

- [Overview](#overview)
- [Problem Statement](#problem-statement)
- [Key Highlights](#key-highlights)
- [System Architecture](#system-architecture)
- [NLP Pipeline](#nlp-pipeline)
- [Model Details](#model-details)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [How to Run](#how-to-run)
- [API Endpoints](#api-endpoints)
- [Results](#results)
- [Team](#team)

---

## 🧠 Overview

During disaster events, social media platforms like Reddit are flooded with a mix of factual updates and unverified rumors. DisasterShield is an AI-powered system that:

- Continuously monitors Reddit posts related to disaster events.
- Classifies posts as **rumor** or **non-rumor** in real time using a fine-tuned NLP model.
- Stores results in a PostgreSQL database for tracking and analysis.
- Provides a mobile-friendly interface for end users to view classified posts.

---

## ❗ Problem Statement

During crises, misinformation spreads rapidly on social media, causing public panic and hampering emergency response. Manually filtering rumors from thousands of posts is infeasible. An automated, real-time NLP pipeline is needed to identify and flag unverified disaster-related claims as they emerge.

---

## ⭐ Key Highlights

- Analyzed **15,000+ Reddit posts** for disaster rumor classification.
- Achieved **93% classification accuracy** using a DistilBERT + SVM ensemble.
- Applied **pseudo-labeling** via topic modeling and zero-shot classification to fine-tune DistilBERT on limited labeled data.
- Full-stack deployment: **FastAPI** backend + **PostgreSQL** on Render + **React Native (Expo)** frontend.

---

## 🏗️ System Architecture

```
Reddit API (PRAW)
    │
    ▼
Data Collection & Preprocessing
    │   (tokenization, cleaning, stopword removal)
    ▼
Feature Extraction
    │   (DistilBERT embeddings)
    ▼
Classification (DistilBERT + SVM Ensemble)
    │   (Rumor / Non-Rumor)
    ▼
FastAPI Backend  ───►  PostgreSQL (Render)
    │
    ▼
React Native (Expo) Frontend
    │
    ▼
User: View & Filter Classified Posts
```

---

## 🔬 NLP Pipeline

### 1. Data Collection
- Reddit posts scraped using **PRAW** (Python Reddit API Wrapper) from disaster-related subreddits.
- Over **15,000 posts** collected across multiple disaster event threads.

### 2. Preprocessing
- Text cleaning: removal of URLs, special characters, and noise.
- Tokenization and stopword removal.
- Normalization for consistent input to the model.

### 3. Pseudo-Labeling (for Limited Labeled Data)
- **Topic Modeling** used to cluster posts by disaster-related themes.
- **Zero-Shot Classification** applied to assign soft labels to unlabeled posts.
- Pseudo-labeled data used to fine-tune DistilBERT, significantly expanding the effective training set.

### 4. Model — DistilBERT + SVM Ensemble
- **DistilBERT** (distilbert-base-uncased) fine-tuned on the pseudo-labeled dataset to generate contextual sentence embeddings.
- **SVM** (Support Vector Machine) trained on top of DistilBERT embeddings for final classification.
- Ensemble approach combines the deep contextual understanding of DistilBERT with the strong decision boundary of SVM.

### 5. Classification Output
- Each post labeled as: `Rumor` or `Non-Rumor`
- Confidence scores stored alongside predictions in PostgreSQL.

---

## 🤖 Model Details

| Component | Details |
|---|---|
| Base Model | DistilBERT (distilbert-base-uncased) |
| Classifier | SVM (on top of DistilBERT embeddings) |
| Labeling Strategy | Pseudo-labeling via Topic Modeling + Zero-Shot Classification |
| Training Data | 15,000+ Reddit posts |
| Classification Accuracy | 93% |
| Task | Binary classification (Rumor / Non-Rumor) |

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| NLP / ML | Python, DistilBERT (HuggingFace Transformers), Scikit-learn (SVM) |
| Data Collection | PRAW (Python Reddit API Wrapper) |
| Topic Modeling | Scikit-learn / Gensim |
| Zero-Shot Classification | HuggingFace Transformers |
| Backend | FastAPI (Python) |
| Database | PostgreSQL (hosted on Render) |
| Frontend | React Native (Expo) |
| Deployment | Render (backend + DB) |

---

## 📁 Project Structure

```
DisasterShield/
├── backend/
│   ├── main.py              # FastAPI app entry point
│   ├── routes/              # API route handlers
│   ├── models/              # DB models (SQLAlchemy)
│   ├── schemas/             # Pydantic schemas
│   ├── pipeline/            # NLP pipeline (preprocessing, prediction)
│   ├── reddit_scraper.py    # PRAW-based Reddit data collection
│   └── requirements.txt
├── frontend/
│   ├── App.js               # React Native (Expo) entry
│   ├── screens/             # UI screens
│   ├── components/          # Reusable components
│   └── package.json
├── models/
│   ├── distilbert_finetuned/  # Fine-tuned DistilBERT weights
│   └── svm_classifier.pkl     # Trained SVM model
├── .gitignore
└── DisasterShield Project.code-workspace
```

---

## 🚀 How to Run

### Prerequisites
- Python 3.8+
- Node.js 16+
- PostgreSQL instance (or use the Render-hosted DB)
- Reddit API credentials (PRAW)

### 1. Clone the repository
```bash
git clone https://github.com/VrajPatel-16/DisasterShield.git
cd DisasterShield
```

### 2. Backend Setup
```bash
cd backend
pip install -r requirements.txt
```

Create a `.env` file in the `backend/` folder:
```env
DATABASE_URL=your_postgresql_connection_string
REDDIT_CLIENT_ID=your_reddit_client_id
REDDIT_CLIENT_SECRET=your_reddit_client_secret
REDDIT_USER_AGENT=DisasterShield/1.0
```

Start the FastAPI server:
```bash
uvicorn main:app --reload
```

### 3. Frontend Setup
```bash
cd frontend
npm install
npx expo start
```

Scan the QR code with the Expo Go app on your mobile device, or run on an emulator.

---

## 📡 API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/posts` | Fetch all classified posts |
| `GET` | `/posts/{id}` | Fetch a specific post by ID |
| `POST` | `/classify` | Classify a new Reddit post |
| `GET` | `/stats` | Get rumor/non-rumor statistics |
| `POST` | `/scrape` | Trigger Reddit scraping for new posts |

---

## 📊 Results

| Metric | Value |
|---|---|
| Total Posts Analyzed | 15,000+ |
| Classification Accuracy | **93%** |
| Model | DistilBERT + SVM Ensemble |
| Labeling Approach | Pseudo-labeling (Topic Modeling + Zero-Shot) |
| Deployment | FastAPI + PostgreSQL on Render |



---

## 📄 License

This project is for academic purposes. Feel free to fork and build upon it with attribution.
