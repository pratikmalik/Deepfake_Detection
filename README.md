# Deepfake Detection System

A high-performance Flask web application for detecting Deepfake videos and AI-manipulated images using Xception, ResNet-50, and EfficientNet deep learning architectures.

---

## 🛠️ Technologies Used

### Backend & Server Framework
- **Python 3.12**: Core programming language.
- **Flask**: Lightweight WSGI web application framework.
- **Flask-SQLAlchemy & SQLite**: Object-Relational Mapping (ORM) for managing user accounts (`users.db`).
- **Flask-Login**: User authentication and session management.
- **Werkzeug**: Password hashing (`generate_password_hash`, `check_password_hash`) and secure filename handling.

### Deep Learning & AI Frameworks
- **TensorFlow 2.16+ & `tf_keras`**: Executing Keras-based deep convolutional neural network models.
- **PyTorch & torchvision**: Running PyTorch neural network architectures and vision transformations.

### Computer Vision & Face Processing
- **OpenCV (`opencv-python 4.x`)**: Frame extraction, video decoding/encoding, image resizing, and Haar Cascade face detection.
- **MTCNN (Multi-task Cascaded Convolutional Networks)**: High-accuracy deep learning face detector.
- **Pillow (PIL) & scikit-image**: Image manipulation, color space conversion, and array transformations.

### Visualization & Analytics
- **Seaborn & Matplotlib**: Rendering temporal confidence heatmaps showing frame-by-frame risk metrics.
- **NumPy & SciPy**: Vectorized image array normalization and mathematical calculations.

### Frontend
- **HTML5 & CSS3**: Modern responsive UI layout with custom stylesheet (`index.css`, `admin.css`).
- **Jinja2**: Server-side template rendering engine.

---

## 🧠 AI Models & Purpose Breakdown

| Model File | Architecture | Framework | Primary Purpose | Input & Output Details |
| :--- | :--- | :--- | :--- | :--- |
| **`df_92_v2.h5`** | **Xception CNN** | Keras / TensorFlow | **Video Deepfake Detection (`/detect`)** | **Input:** 224x224 RGB cropped face frames normalized to `[-1.0, 1.0]`.<br>**Purpose:** Evaluates temporal video sequences for facial boundary artifacts, blending inconsistency, and AI synthesis.<br>**Output:** Sigmoid score \(p \in [0, 1]\). \(p \ge 0.5 \implies\) **FAKE**, \(p < 0.5 \implies\) **REAL**. |
| **`best_model-v3.pt`** | **EfficientNet-B0** | PyTorch | **Single Image Detection (`/image-detect`)** | **Input:** 224x224 RGB image with ImageNet normalization.<br>**Purpose:** Analyzes static photos for generative AI artifacts, Deepfake face swaps, and digital manipulation.<br>**Output:** Binary classification logits (`Class 0` = Real, `Class 1` = Fake). |
| **`df_model.pt`** | **ResNeXt50 + LSTM** | PyTorch | **Sequence Model Architecture** | **Input:** 5D Video Tensors `[Batch, Frames, Channels, H, W]`.<br>**Purpose:** Combines ResNeXt-50 feature extraction with Recurrent LSTM layers for frame sequence feature mapping. |
| **MTCNN & Haar Cascade** | **Cascaded CNN / Haar** | OpenCV & MTCNN | **Face Detection & Cropping** | **Input:** Raw video frames & uploaded photos.<br>**Purpose:** Automatically locates human faces, applies a 20% margin buffer, and resizes cropped face regions for CNN models. |

---

## 📁 Project Directory Structure

```text
deepfake/
├── backend/
│   ├── server.py              # Main Flask server & API routes (/detect, /image-detect, /health)
│   ├── models.py              # SQLAlchemy User database model
│   └── suppress_output.py     # Terminal log suppressor utility
├── frontend/
│   ├── static/                # CSS styles, generated heatmaps, and frame thumbnail outputs
│   └── templates/             # HTML templates (home, detect, image, login, signup, privacy, terms)
├── models/
│   ├── best_model-v3.pt       # EfficientNet-B0 PyTorch weight file (Image detector)
│   └── df_92_v2.h5            # Xception Keras weight file (Video detector)
├── Uploaded_Files/            # Temporary upload storage directory
├── instance/                  # SQLite database directory (users.db)
├── .gitignore                 # Excludes environments, caches, and heavy media
├── requirements.txt           # Python package dependencies
└── README.md                  # Project documentation & setup instructions
```

---

## ⚡ Step-by-Step Setup & Execution Guide

Follow these commands to run the application step-by-step:

### 1. Prerequisites
- Python 3.10 – 3.12 installed on your system.

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

### 4. Run the Backend Server
```bash
python backend/server.py
```

### 5. Access the Web Interface
Open your browser and go to:
👉 **[http://localhost:5000](http://localhost:5000)** (or `http://127.0.0.1:5000`)

---

## 🌐 Web Routes & Endpoints

| Endpoint | Method | Description |
| :--- | :--- | :--- |
| `/` | `GET` | Main application landing page |
| `/detect` | `GET`, `POST` | Upload videos for Deepfake analysis using Xception |
| `/image-detect` | `GET`, `POST` | Upload static images for AI manipulation detection using EfficientNet |
| `/login` | `GET`, `POST` | User login authentication |
| `/signup` | `GET`, `POST` | User registration |
| `/health` | `GET` | Health check endpoint returning model & system status |
| `/test` | `GET` | Quick diagnostic ping endpoint |
