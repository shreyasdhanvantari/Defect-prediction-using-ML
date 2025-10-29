# 🧠 Software Defect Prediction using Machine Learning  
[![Python](https://img.shields.io/badge/Python-3.11-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Build](https://img.shields.io/badge/Build-Passing-brightgreen.svg)]()
[![Platform](https://img.shields.io/badge/Platform-Colab%20%7C%20VS%20Code-orange.svg)]()

> Automatically predict defect-prone source code files by analyzing code metrics and version-control history.  
> Built entirely in **Python**, with results verified using a real GitHub repository (`psf/requests`).

---

## 📋 Overview

This project predicts **which source code files are most likely to contain defects (bugs)** by combining:
- 🧮 **Static metrics** – Lines of code, cyclomatic complexity, maintainability index, PEP8 violations  
- 🧠 **Process metrics** – Commit frequency, code churn, authors, last modification timestamp  
- 🐞 **Defect labels** – Generated automatically by scanning commit messages containing “fix”, “bug”, etc.  

Using these metrics, two ML models (Logistic Regression and Random Forest) are trained to produce a **risk score (0-1)** for each file.

---

## 🧩 Project Structure

defect-predictor/
│
├── data/                 # Extracted datasets
│   ├── static.csv        # Code metrics
│   ├── process.csv       # Git metrics
│   ├── labels.csv        # Bug-fix labels
│   └── merged_dataset.csv
│
├── src/
│   ├── extract/
│   │   ├── static_metrics.py     # Extract static code metrics
│   │   └── process_metrics.py    # Extract process metrics
│   ├── label/
│   │   └── labeler.py            # Generate defect labels
│   ├── models/
│   │   ├── train.py              # Train ML models
│   │   └── predict.py            # Predict file risk
│   ├── report/
│   │   ├── make_report.py        # Generate HTML report
│   │   └── app.py                # Flask dashboard (optional)
│   └── utils/                    # Helper functions (optional)
│
├── artifacts/            # Saved trained models & metrics
│   ├── rf.pkl
│   ├── logreg.pkl
│   └── metrics.csv
│
├── reports/              # Predictions & visual reports
│   ├── risk.csv
│   └── report.html
│
├── requirements.txt
└── README.md

---

## ⚙️ Setup Instructions

### 1️⃣ Clone the Repository
git clone https://github.com/<your-username>/defect-predictor.git
cd defect-predictor

### 2️⃣ Install Dependencies
pip install -r requirements.txt

### 3️⃣ Clone a Target Repository for Analysis
git clone https://github.com/psf/requests.git target_repo

### 4️⃣ Extract Metrics & Labels
python -m src.extract.static_metrics --repo ./target_repo --out data/static.csv
python -m src.extract.process_metrics --repo ./target_repo --out data/process.csv
python -m src.label.labeler --repo ./target_repo --out data/labels.csv

### 5️⃣ Train Models
python -m src.models.train --features data/static.csv data/process.csv --labels data/labels.csv --out artifacts

### 6️⃣ Predict Defect Risk
python -m src.models.predict --model artifacts/rf.pkl --features data/static.csv data/process.csv --out reports/risk.csv

### 7️⃣ Generate HTML Report
python -m src.report.make_report --risk reports/risk.csv --out reports/report.html

Then open **reports/report.html** to view the risk chart and file-wise predictions.

---

## 🧠 Results Summary

| Model | ROC-AUC | PR-AUC | F1-Score |
|--------|----------|---------|-----------|
| Logistic Regression | 0.74 | 0.63 | 0.58 |
| Random Forest | **0.86** | **0.71** | **0.69** |

### 🔍 Insights
- High **code churn** and **commit frequency** correlate strongly with defects.  
- Large, complex files (high LOC, avg_CC) are riskier.  
- Multiple contributors (**authors**) also increase defect likelihood.

---

## 📊 Example Output

| file | risk | defect_prone |
|------|------|---------------|
| requests/models.py | 0.87 | 1 |
| requests/adapters.py | 0.76 | 1 |
| requests/utils.py | 0.24 | 0 |

### 🎨 Feature Importance
1. Code Churn  
2. Commit Count  
3. Lines of Code (LOC)  
4. Cyclomatic Complexity  
5. Number of Authors  

---

## 🧰 Tech Stack

| Category | Tools |
|-----------|-------|
| Language | Python 3.11 |
| ML Libraries | scikit-learn, pandas, numpy |
| Static Analysis | radon, pycodestyle |
| Repo Mining | PyDriller |
| Visualization | matplotlib, seaborn |
| Web/Dashboard | Flask |
| Environment | Google Colab / VS Code |

---

## 🚀 Future Enhancements
- Integrate with CI/CD for automatic defect scanning on each push.  
- Use NLP to improve commit message labeling.  
- Apply temporal validation (train on older commits, predict on newer).  
- Extend to multiple repositories for cross-project learning.

---

## 👨‍💻 Author

**Shreyas Dhanvantari**  
Master of Engineering – Electrical & Computer Engineering  
Ontario Tech University, Canada  

📫 Email: hanvantari.svd@gmail.com

---

## 📜 License
Distributed under the **MIT License**. See [LICENSE](LICENSE) for more information.

---

⭐ **If you found this useful, give it a star on GitHub!**
