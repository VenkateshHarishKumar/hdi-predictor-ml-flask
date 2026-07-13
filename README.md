# A Comprehensive Measure of Well-Being — Human Development Index Predictor

A Comprehensive Measure of Well-Being is a machine learning–powered web application that predicts a country's Human Development Index (HDI) score and corresponding developmental tier based on healthcare (Life Expectancy at Birth), education (Mean Years of Schooling), and economic output (Gross National Income per Capita). It helps development economists, policy researchers, and students make data-driven projections to model national progress.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Problem Statement](#problem-statement)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Architecture](#project-architecture)
- [Dataset](#dataset)
- [Project Workflow](#project-workflow)
- [Installation](#installation)
- [Usage](#usage)
- [Model Performance](#model-performance)
- [Folder Structure](#folder-structure)
- [Future Enhancements](#future-enhancements)
- [License](#license)

---

## Overview

**A Comprehensive Measure of Well-Being (HDI Predictor)** integrates key developmental dimensions — health, knowledge, and standard of living — to calculate projected HDI rankings. By combining linear regression, socio-economic databases, and a clean, responsive glassmorphism interface, the application translates policy goals directly into measurable indices.

### Use Cases

| Scenario | Description |
|---|---|
| **Target HDI Simulation** | A researcher inputs targeted health, education, and income benchmarks to project future HDI scores instantly. |
| **Country Baseline Profiling** | A user searches for any country in the autocomplete dropdown to pre-fill inputs with the nation's latest historical indicators. |
| **Socio-Economic Scenario Testing** | Policy planners simulate changes (e.g. increasing schooling budget) to evaluate shifts in developmental tiers. |

---

## Problem Statement

Policy planners and academic researchers often lack a quick, interactive mechanism to estimate how multi-sector targets translate to overall national rankings, leading to:

- Inefficiencies when evaluating socio-economic policy drafts
- Overreliance on static global spreadsheets and historical reports
- High entry barriers requiring programming skills to run baseline target simulations
- Difficulties in visualizing developmental tier thresholds dynamically

This application addresses these challenges by employing a pre-trained regression model to calculate and categorize HDI scores instantly from 3 simple parameters.

---

## Features

- ✅ Predicts numeric HDI scores ($0.000$ to $1.000$) using Linear Regression
- ✅ Dynamically maps predictions to UNDP tiers: *Low*, *Medium*, *High*, or *Very High* development
- ✅ Autocomplete country search pre-loads indicators for 195 nations
- ✅ Premium glassmorphism single-page user interface (fully mobile responsive)
- ✅ Bundled Python scripts for modular model training and database preprocessing

---

## Tech Stack

| Layer | Technology |
|---|---|
| Language | Python 3.9+ |
| Data Processing | Pandas, NumPy |
| Machine Learning | scikit-learn (Linear Regression) |
| Backend | Flask |
| Frontend | HTML5, CSS3, Vanilla JavaScript |
| Model Persistence | Joblib / Pickle |

---

## Project Architecture

```
                    ┌────────────────────────┐
                    │   Historical Dataset   │
                    │  (Life Exp, Schooling, │
                    │   GNI per Capita)      │
                    └───────────┬────────────┘
                                │
                    ┌───────────▼────────────┐
                    │   Data Preprocessing   │
                    │   Null check · Feature │
                    │   correlation scaling  │
                    └───────────┬────────────┘
                                │
                    ┌───────────▼────────────┐
                    │    Model Training      │
                    │   Linear Regression    │
                    │    Weight Fitting      │
                    └───────────┬────────────┘
                                │
                    ┌───────────▼────────────┐
                    │     Flask Web App      │
                    │  home.html → /predict  │
                    │  → indexnew.html       │
                    └────────────────────────┘
```

---

## Dataset

The model is trained on the **Human Development Index Dataset** containing socio-economic records of 195 countries.

| Column | Description | Range |
|---|---|---|
| `Life Expectancy` | Life expectancy at birth (years) | 20 – 100 |
| `Schooling` | Mean years of schooling (years) | 0 – 25 |
| `GNI per Capita` | Gross National Income per Capita (USD) | 100 – 150,000 |
| `HDI` (Target) | Composite Human Development Index value | 0.000 – 1.000 |

**Tiers covered:**
- **Low Human Development:** $\text{HDI} < 0.550$
- **Medium Human Development:** $0.550 \leq \text{HDI} < 0.700$
- **High Human Development:** $0.700 \leq \text{HDI} < 0.800$
- **Very High Human Development:** $\text{HDI} \geq 0.800$

---

## Project Workflow

1. **Define Problem & Scope** — project goal, literature study, and target user persona mapping (Empathy Maps)
2. **Data Sourcing & Cleaning** — load files, perform univariate/bivariate checks, clean null indicators
3. **Model Selection & Fitting** — evaluate regression algorithms, evaluate R² and MSE errors, and serialize model coefficients
4. **Core Application Build** — develop Flask REST endpoints, connect database pre-fills, and code frontend templates
5. **Deployment & Review** — test application queries under load limits, configure WSGI, and deploy to live cloud services

---

## Installation

### Prerequisites
- Python 3.9 or higher
- pip package manager

### Steps

```bash
# 1. Clone the repository
git clone https://github.com/v-harishkumar/hdi-predictor-ml-flask.git
cd hdi-predictor-ml-flask

# 2. Create a virtual environment
python -m venv venv
source venv/bin/activate      # On Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt
```

### requirements.txt

```
numpy
pandas
scikit-learn
flask
gunicorn
```

---

## Usage

### Run the web application locally

```bash
python Flask/app.py
```

Then open your browser at:
```
http://127.0.0.1:5000
```
Search for a country to populate standard values, adjust indicator variables, and click **Predict HDI** to run a simulation.

### Retrain the model (optional)

To retrain the model weights with new datasets:
```bash
python Training/train_model.py
```
This script saves the updated model file (`Flask/model.pkl` or similar) containing target coefficients.

---

## Model Performance

| Metric | Score |
|---|---|
| Model Algorithm | Linear Regression (Ordinary Least Squares) |
| R² (Coeff of Determination) | ~0.94+ |
| MSE (Mean Squared Error) | < 0.005 |
| Features | Life Expectancy, Schooling, GNI per Capita |

---

## Folder Structure

```
hdi-predictor-ml-flask/
│
├── Flask/
│   ├── templates/
│   │   ├── home.html         # Core input form template with searchable country dropdown
│   │   ├── index.html        # Legacy baseline form template
│   │   └── indexnew.html     # High-fidelity glassmorphism results dashboard
│   │
│   ├── app.py                # Main Flask server running routing and model inference logic
│   └── update_dropdown.py    # Preprocessing helper script for country list validation
│
├── Dataset/
│   └── HDI.csv               # Dataset containing historical indicators for 195 countries
│
├── Training/
│   ├── HumDevIndex.ipynb     # Jupyter Notebook documenting data cleaning and training steps
│   └── train_model.py        # Script to train and serialize the model to model.pkl
│
├── 1. Brainstorming & Ideation/
├── 2. Requirement Analysis/
├── 3. Project Design Phase/
├── 4. Project Planning Phase/
├── 5. Project Development Phase/
├── 6.Project Testing/
├── 7.Project Documentation/
├── 8.Project Demonstration/
│
├── .gitignore                # Git exclusion settings
├── requirements.txt          # List of dependencies
└── README.md                 # Setup guide and project deployment instructions
```

---

## Future Enhancements

- [ ] Integrate a dynamic global SVG map for spatial country selection
- [ ] Add a side-by-side grid panel to track multiple simulation histories simultaneously
- [ ] Set up live integrations with UNDP APIs for real-time baseline data synchronization
- [ ] Migrate database structures to secure relational cloud engines (PostgreSQL)

---

## License

This project is open source and available under the [MIT License](LICENSE).

---

## Acknowledgements

- Dataset: Kaggle/UNDP Human Development Index Indicators
- Built as part of the phase-based documentation workflow spanning brainstorming, design, testing, and deployment.
