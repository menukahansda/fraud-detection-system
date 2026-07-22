# fraud-detection-system
Flags fraudulent credit card transactions in real time using supervised ML on imbalanced transaction data


## Table of Contents
- [Project Structure](#project-structure--fraud-detection-system)
- [Setup](#setup)
- [Download Data](#download-data-set-from-cli)


## Project Structure — fraud-detection-system

```
fraud-detection-system/
│
├── README.md                     # project overview, setup instructions, results summary
├── .gitignore                    # ignores venv, __pycache__, data/raw, node_modules, etc.
├── requirements.txt              # Python dependencies (pandas, scikit-learn, fastapi, etc.)
│
├── data/
│   ├── raw/                      # original dataset, untouched (gitignored if large)
│   └── processed/                # cleaned/engineered data saved after preprocessing
│
├── notebooks/
│   ├── 01_eda.ipynb              # exploration: class imbalance, distributions, correlations
│   ├── 02_preprocessing.ipynb    # SMOTE vs class weighting experiments, feature engineering
│   └── 03_modeling.ipynb         # baseline vs Random Forest/XGBoost, evaluation, feature importance
│
├── ml/
│   ├── __init__.py               # marks ml/ as a Python package
│   ├── preprocessing.py          # cleaning + feature engineering functions (shared by notebooks & API)
│   ├── train.py                  # training script — trains model, saves artifact to models/
│   ├── model.py                  # loads saved model, wraps predict/score logic for reuse
│   └── evaluate.py               # precision/recall/F1, confusion matrix, feature importance plots
│
├── models/
│   └── fraud_model.pkl           # serialized trained model (output of train.py)
│
├── api/
│   ├── main.py                   # FastAPI app entrypoint, starts the server
│   ├── routes/
│   │   └── score.py              # POST /score-transaction endpoint — real-time fraud scoring
│   └── schemas.py                # Pydantic request/response models for the API
│
├── dashboard/                    # Next.js frontend
│   ├── app/                      # pages/routes (or pages/ depending on Next.js version)
│   ├── components/
│   │   ├── TransactionFeed.tsx   # live list of scored transactions, flags fraud in red
│   │   └── StatsPanel.tsx        # summary stats — fraud rate, precision/recall on test set
│   └── ...                       # standard Next.js config files (package.json, next.config.js)
│
├── docs/
│   ├── project_report.md         # working draft of the report (export to PDF for submission)
│   └── architecture.png          # optional diagram: data → model → API → dashboard
│
└── tests/
    └── test_preprocessing.py     # basic sanity checks on preprocessing functions
```


## Setup

1. Get a Kaggle API token and download the dataset (see CLI instructions below)
2. Install dependencies: `pip install -r requirements.txt`
3. (Optional) Explore the data: `notebooks/01_eda.ipynb`
4. Run `notebooks/02_preprocessing.ipynb` to generate:
   - Processed train/test data in `data/processed/`
   - Fitted scaler in `models/amount_scaler.pkl`



## Download Data Set From CLI:

Get a api token from kaggle and replace it with <token_key> :

``` bash
# save it to a file so that client can read automatically
mkdir -p ~/.kaggle && echo <token_key> > ~/.kaggle/access_token && chmod 600 ~/.kaggle/access_token

# download the dataset
kaggle datasets download -d mlg-ulb/creditcardfraud -p data/raw --unzip
```