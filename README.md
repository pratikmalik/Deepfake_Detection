# Deepfake Detection System

A high-performance Flask web application for detecting Deepfake videos and AI-manipulated images using Xception, ResNet-50, and EfficientNet deep learning models.

---

## 📁 Project Architecture

The repository is modularly partitioned into clear, separated layers:

```text
deepfake/
├── backend/
│   ├── server.py              # Main Flask server & prediction API endpoints
│   ├── models.py              # SQLAlchemy Database User model
│   └── suppress_output.py     # Terminal log suppressor utility
├── frontend/
│   ├── static/                # CSS styles, heatmaps, and frame thumbnail outputs
│   └── templates/             # HTML templates (home, detect, image, login, signup)
├── models/
│   ├── best_model-v3.pt       # EfficientNet-B0 PyTorch weights (Image analysis)
│   └── df_92_v2.h5            # Xception Keras weights (Video analysis)
├── Uploaded_Files/            # Temporary directory for uploaded media
├── instance/                  # SQLite database location (users.db)
├── .gitignore                 # Excludes environments, caches, and large media
├── requirements.txt           # Python dependency requirements
└── README.md                  # Project documentation & setup instructions
```

---

## ⚡ Quick Start Guide (Run Step-by-Step)

Follow these steps to set up and run the application from scratch:

### 1. Prerequisites
- Python 3.10 – 3.12 installed on your system.
- Git (optional, for version control).

### 2. Create & Activate Virtual Environment

**Windows (PowerShell):**
```powershell
python -m venv .venv
.\.venv\Scripts\activate
```

**Linux / macOS:**
```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

### 4. Run the Application
Execute the backend server:
```bash
python backend/server.py
```

### 5. Access the Web Interface
Open your browser and navigate to:
👉 **[http://localhost:5000](http://localhost:5000)** (or `http://127.0.0.1:5000`)

---

## 🎯 Features & Models Used

- **Video Analysis**: Upload MP4, AVI, or MOV files. Analyzes 15 representative video frames in under 3 seconds using the **Xception CNN architecture** (`df_92_v2.h5`).
- **Image Analysis**: Upload single images to evaluate facial manipulation using **EfficientNet-B0** (`best_model-v3.pt`).
- **Temporal Heatmaps**: Generates visual frame-by-frame risk distribution heatmaps using `seaborn` / `matplotlib`.
- **User Authentication**: Built-in user signup, login, and secure session management powered by `Flask-Login` and `Flask-SQLAlchemy`.

---

## 🛠️ API & Endpoints

| Endpoint | Method | Description |
| :--- | :--- | :--- |
| `/` | `GET` | Homepage & feature overview |
| `/detect` | `GET`, `POST` | Video upload & Deepfake analysis |
| `/image-detect` | `GET`, `POST` | Single image manipulation detection |
| `/login` | `GET`, `POST` | User authentication login |
| `/signup` | `GET`, `POST` | New user registration |
| `/health` | `GET` | System & model health check status |
| `/test` | `GET` | Quick diagnostic ping endpoint |
