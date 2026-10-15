# Telco Customer Churn — SML Experiment

This project explores the **Telco Customer Churn** dataset through exploratory data analysis and a reproducible preprocessing pipeline. The goal is to prepare customer service and account information for a binary classification task that predicts whether a customer will churn.

The target column is `Churn`, encoded as:

- `0` — No churn
- `1` — Churn

## Dataset

- Source: [Kaggle — Telco Customer Churn](https://www.kaggle.com/datasets/blastchar/telco-customer-churn)
- Raw dataset in this repository: `telco_churn_raw.csv`

The raw dataset is kept unchanged. Data preparation is performed by the notebook and the automated preprocessing script.

## Experiment tracking

MLflow experiment tracking and run details are available in the project’s DagsHub repository:

- [View MLflow experiments on DagsHub](https://dagshub.com/kuzanf3b/telco-customer-churn)

## Repository structure

```text
experiment-sml/
├── .github/
│   └── workflows/
│       └── main.yml
├── preprocessing/
│   ├── Eksperimen_Muhammad_Fauzan_Febrian_Wibowo.ipynb
│   ├── automate_kuzanf3b.py
│   ├── artifacts/
│   │   ├── feature_names.json
│   │   ├── preprocessing_metadata.json
│   │   └── preprocessor.joblib
│   └── telco_customer_churn_preprocessing/
│       ├── telco_churn_train.csv
│       └── telco_churn_test.csv
├── requirements.txt
├── telco_churn_raw.csv
└── README.md
```

## Notebook

`preprocessing/Eksperimen_Muhammad_Fauzan_Febrian_Wibowo.ipynb` contains the manual experiment workflow, including:

1. Dataset introduction and loading
2. Exploratory data analysis
3. Data quality checks
4. Target distribution analysis
5. Feature distribution visualizations
6. Data preprocessing

The notebook reads the raw dataset from the repository using a relative path. Run it with the repository root as the working directory.

## Automated preprocessing

The preprocessing script is located at `preprocessing/automate_kuzanf3b.py`. It performs the following steps:

- Converts `TotalCharges` to a numeric column
- Removes duplicate rows
- Removes the `customerID` identifier column
- Encodes `Churn` as `0` or `1`
- Splits the data into stratified training and test sets
- Imputes missing numeric values with the median
- Imputes missing categorical values with the most frequent value
- Applies one-hot encoding to categorical features
- Standardizes numeric features
- Saves the fitted preprocessor and preprocessing metadata

Run the script from the repository root:

```bash
python preprocessing/automate_kuzanf3b.py
```

Optional path overrides are also supported:

```bash
python preprocessing/automate_kuzanf3b.py \
  --input-path telco_churn_raw.csv \
  --output-dir preprocessing/telco_customer_churn_preprocessing \
  --artifacts-dir preprocessing/artifacts
```

The default configuration uses a `0.2` test split and `random_state=42`.

### Generated outputs

Processed datasets:

- `preprocessing/telco_customer_churn_preprocessing/telco_churn_train.csv`
- `preprocessing/telco_customer_churn_preprocessing/telco_churn_test.csv`

Preprocessing artifacts:

- `preprocessing/artifacts/preprocessor.joblib`
- `preprocessing/artifacts/feature_names.json`
- `preprocessing/artifacts/preprocessing_metadata.json`

## Installation and usage

The project uses Python 3.12 and pinned dependencies from `requirements.txt`.

```bash
python3.12 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

To launch JupyterLab:

```bash
python -m jupyter lab
```

Then open the notebook under `preprocessing/`.

## Continuous integration

The GitHub Actions workflow in `.github/workflows/main.yml` runs the automated preprocessing pipeline when:

- It is triggered manually with `workflow_dispatch`
- Changes are pushed to `main` and affect the raw dataset, preprocessing script, dependencies, or workflow
- A pull request changes the relevant preprocessing files

The workflow installs the dependencies, runs the preprocessing script, and uploads the generated datasets and artifacts as the `telco-churn-preprocessed` workflow artifact.
