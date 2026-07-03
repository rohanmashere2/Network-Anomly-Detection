<div align="center">
  <h1>🛡️ Network Anomaly Detection System</h1>
  <h3><em>Real-Time Network Intrusion Detection using Machine Learning</em></h3>
  <p>
    <strong>Author:</strong> Rohan Mashere<br>
    B.Tech, Artificial Intelligence and Data Science<br>
    Dr. D. Y. Patil School Vidyapeeth, Pune
  </p>
  <p>
    <img src="https://img.shields.io/badge/Python-3.10%2B-3776AB?style=flat&logo=python&logoColor=white" alt="Python"/>
    <img src="https://img.shields.io/badge/Model-Random%20Forest-2E8B57?style=flat&logo=scikitlearn&logoColor=white" alt="Model"/>
    <img src="https://img.shields.io/badge/Backend-FastAPI-009688?style=flat&logo=fastapi&logoColor=white" alt="Backend"/>
    <img src="https://img.shields.io/badge/Dashboard-Streamlit-FF4B4B?style=flat&logo=streamlit&logoColor=white" alt="Dashboard"/>
    <img src="https://img.shields.io/badge/Packet%20Capture-Scapy-FFA500?style=flat" alt="Packet Capture"/>
    <img src="https://img.shields.io/badge/Dataset-UNSW--NB15-6A5ACD?style=flat" alt="Dataset"/>
    <img src="https://img.shields.io/badge/License-Academic%20Use-4CAF50?style=flat" alt="License"/>
    <img src="https://img.shields.io/badge/Platform-Windows%20%7C%20Linux-808080?style=flat" alt="Platform"/>
  </p>
</div>
---
 
## 📌 Overview
 
**Network Anomaly Detection System** is an end-to-end, real-time intrusion detection pipeline that captures live network traffic, extracts flow-level features, and classifies traffic as **Normal** or **Attack** using a hybrid **Rule-Based + Machine Learning** approach. The system is built on the **UNSW-NB15** dataset and exposes predictions through a **FastAPI** backend, with a live-updating **Streamlit** dashboard for monitoring and visualization.
 
The project combines:
- A **Random Forest Classifier** trained on the UNSW-NB15 network intrusion dataset
- A **rule-based detection layer** for catching obvious attack patterns (DDoS spikes, bot bursts, abnormal data transfers) instantly
- **Live packet capture** using Scapy
- A **real-time monitoring dashboard** with traffic trends, threat levels, and alerts
---
 
## ✨ Key Features
 
- 🔍 **Hybrid Detection Engine** — Rule-based checks run first for fast, explainable detection of obvious attacks, with ML as a secondary layer for subtler anomalies.
- ⚡ **Real-Time Packet Capture** — Live traffic is sniffed from the network interface, aggregated into time windows, and converted into feature vectors.
- 🤖 **Machine Learning Core** — Random Forest Classifier trained on the UNSW-NB15 dataset with strong benchmark performance.
- 🌐 **REST API** — A FastAPI service exposes a `/predict` endpoint for real-time inference.
- 📊 **Live Dashboard** — A Streamlit dashboard auto-refreshes every 5 seconds, showing attack/normal traffic trends, threat levels, and time-sliced traffic history.
- 🚨 **Automated Alerting** — Visual alerts trigger when attack traffic crosses defined thresholds (40% / 70%).
- 🧩 **One-Command Orchestration** — `run_all.py` launches the API, dashboard, and packet capture together.
---
 
## 🏗️ System Architecture
 
```
                ┌─────────────────────┐
                │   Live Network       │
                │   Traffic (Scapy)     │
                └──────────┬───────────┘
                           │  Sniffed packets, aggregated every 2s
                           ▼
                ┌─────────────────────┐
                │  packet_capture.py    │
                │  Feature Extraction   │
                └──────────┬───────────┘
                           │  POST /predict (feature vector)
                           ▼
                ┌─────────────────────┐
                │      app.py            │
                │   FastAPI Backend      │
                │ ┌─────────────────┐   │
                │ │ Rule-Based Check │   │
                │ └────────┬────────┘   │
                │          ▼             │
                │ ┌─────────────────┐   │
                │ │  ML Model (RF)   │   │
                │ │ scaler + model   │   │
                │ └────────┬────────┘   │
                │          ▼             │
                │   Final Prediction     │
                │   + live_log.json      │
                └──────────┬───────────┘
                           │  Reads live_log.json
                           ▼
                ┌─────────────────────┐
                │   dashboard.py         │
                │  Streamlit Dashboard   │
                │  (auto-refresh 5s)     │
                └─────────────────────┘
```
 
---
 
## 📂 Project Structure
 
```
Network Anomaly Detection/
│
├── ML_SEM6.ipynb              # Model training & evaluation notebook
├── app.py                     # FastAPI backend (rule-based + ML inference)
├── dashboard.py                # Streamlit real-time monitoring dashboard
├── packet_capture.py           # Live traffic sniffer & feature extractor
├── run_all.py                  # Launches API, dashboard, and sniffer together
├── test_api.py                 # Simple script to test the /predict endpoint
│
├── unsw_model.pkl              # Trained Random Forest model
├── scaler.pkl                  # StandardScaler used during training
├── feature_names.pkl           # Ordered list of feature names
│
├── UNSW_NB15_training-set.csv   # Training data
├── UNSW_NB15_testing-set.csv    # Testing data
│
├── live_log.json               # Rolling log of live predictions (last 500)
├── live_data.json              # Live traffic snapshot data
│
└── requirements.txt             # Python dependencies
```
 
---
 
## 🧠 Machine Learning Model
 
The model is trained in `ML_SEM6.ipynb` on the **UNSW-NB15** dataset, a widely used benchmark dataset for network intrusion detection research.
 
### Pipeline
 
1. **Data Loading** — Training set (82,332 rows) and testing set (175,341 rows) loaded from UNSW-NB15 CSVs.
2. **Cleaning** — Column names stripped, infinite values removed, missing values dropped.
3. **Encoding** — Categorical columns (e.g., protocol, service, state) encoded using `LabelEncoder`, fit jointly across train and test sets for consistency.
4. **Feature/Target Split** — Features separated from the binary `label` column (0 = Normal, 1 = Attack).
5. **Scaling** — Features standardized using `StandardScaler`.
6. **Model Training** — A `RandomForestClassifier` (100 estimators) is trained on the scaled feature set.
7. **Evaluation** — Accuracy, classification report, confusion matrix, ROC-AUC curve, and feature importance are computed.
8. **Cross-Validation** — 5-fold cross-validation is performed to verify model stability.
9. **Persistence** — The trained model, scaler, and feature name order are saved via `joblib` for use in the live API.
### Model Performance
 
| Metric | Score |
|---|---|
| **Test Accuracy** | **95.10%** |
| **Precision (Attack class)** | 1.00 |
| **Recall (Attack class)** | 0.93 |
| **F1-score (Attack class)** | 0.96 |
| **Mean Cross-Validation Score (5-fold)** | 99.17% |
 
> Full classification report, confusion matrix, and ROC curve are available in `ML_SEM6.ipynb`.
 
---
 
## ⚙️ Rule-Based Detection Layer
 
Before the ML model is consulted, a lightweight rule engine flags obvious attack signatures for faster, more explainable detection:
 
| Condition | Detection | Severity |
|---|---|---|
| Packet rate > 800/s or total packets > 800 | High Traffic Spike (Possible DDoS) | HIGH |
| Duration < 1s with > 200 packets | Burst Traffic (Bot Activity) | MEDIUM |
| Total bytes transferred > 500,000 | Unusual Data Transfer | LOW |
| None of the above | Passed to ML model for classification | — |
 
If a rule is triggered, its verdict takes priority; otherwise, the Random Forest model's prediction is used as the final decision.
 
---
 
## 🚀 Getting Started
 
### Prerequisites
 
- Python 3.10+
- Administrator/root privileges (required for live packet sniffing via Scapy)
- Npcap (Windows) or libpcap (Linux/macOS) installed for packet capture
### 1. Clone the repository
 
```bash
git clone https://github.com/<your-username>/network-anomaly-detection.git
cd network-anomaly-detection
```
 
### 2. Create a virtual environment (recommended)
 
```bash
python -m venv venv
source venv/bin/activate        # On Windows: venv\Scripts\activate
```
 
### 3. Install dependencies
 
```bash
pip install -r requirements.txt
```
 
### 4. Run the full system
 
```bash
python run_all.py
```
 
This starts:
- **FastAPI backend** at `http://127.0.0.1:8000`
- **Streamlit dashboard** at `http://localhost:8501`
- **Live packet capture**, streaming predictions to the API every 2 seconds
> ⚠️ Packet sniffing requires elevated privileges. Run your terminal as Administrator (Windows) or with `sudo` (Linux/macOS).
 
### 5. Run components individually (optional)
 
```bash
# Start the API only
uvicorn app:app --reload
 
# Start the dashboard only
streamlit run dashboard.py
 
# Start packet capture only
python packet_capture.py
```
 
---
 
## 🔌 API Reference
 
### `POST /predict`
 
Classifies a network traffic feature vector as Normal or Attack.
 
**Request Body**
 
```json
{
  "features": [0.5, 1200, 0, 45, 0, 90, ...]
}
```
 
**Response**
 
```json
{
  "time": 1751567890.123,
  "prediction": 1,
  "message": "🚨 High Traffic Spike (Possible DDoS)",
  "severity": "HIGH"
}
```
 
### Testing the API
 
```bash
python test_api.py
```
 
---
 
## 📊 Dashboard Highlights
 
The Streamlit dashboard (`dashboard.py`) auto-refreshes every 5 seconds and displays:
 
- 🚨 **Live Metrics** — Attack count, normal count, attack percentage, and overall threat level
- ⏱️ **Current Time Slot Analysis** — Real-time safe/under-attack status for the active minute window
- 🧠 **Latest Detection** — The most recent prediction and its message
- 📈 **Traffic Analytics** — Trend line chart and pie chart distribution of normal vs. attack traffic
- 📊 **Historical Time Slots** — Minute-by-minute breakdown of the last 10 windows
- 🔔 **Alert Banners** — Automatic warnings when attack traffic exceeds 40% (suspicious) or 70% (critical)
---
 
## 🛠️ Tech Stack
 
| Category | Technology |
|---|---|
| **Language** | Python |
| **Machine Learning** | Scikit-learn (Random Forest) |
| **Backend API** | FastAPI, Uvicorn |
| **Dashboard/UI** | Streamlit |
| **Packet Capture** | Scapy |
| **Data Processing** | Pandas, NumPy |
| **Visualization** | Matplotlib, Seaborn |
| **Model Persistence** | Joblib |
| **Dataset** | UNSW-NB15 |
 
---
 
## 📈 Dataset
 
This project uses the **UNSW-NB15** dataset, developed by the Australian Centre for Cyber Security (ACCS), which contains a hybrid of real modern normal traffic and synthetically generated contemporary attack behaviors (DoS, exploits, fuzzers, reconnaissance, backdoors, and more).
 
- `UNSW_NB15_training-set.csv` — 82,332 records
- `UNSW_NB15_testing-set.csv` — 175,341 records
---
 
## 🔮 Future Enhancements
 
- [ ] Multi-class attack categorization (DoS, Exploits, Reconnaissance, etc.) instead of binary classification
- [ ] Deep learning models (LSTM/CNN) for sequential traffic pattern analysis
- [ ] Persistent database (e.g., PostgreSQL) instead of JSON-based logging
- [ ] Email/SMS/Webhook alerting for critical threats
- [ ] Containerization with Docker for simplified deployment
- [ ] Model explainability using SHAP values
---
 
## 📄 License
 
This project is intended for academic and research purposes. Feel free to fork and build upon it with attribution.
 
---
 
<div align="center">
  <p>Made with ❤️ by <strong>Rohan</strong></p>
</div>
 