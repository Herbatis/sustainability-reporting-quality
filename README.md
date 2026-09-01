# Predictive Analysis of Sustainability Reporting Quality Using Web-Scraped Data

## Project Overview

This project investigated whether textual characteristics extracted from corporate reports could predict sustainability-reporting quality among UK companies.

The study combined web-based report collection, natural language processing (NLP), statistical analysis and machine learning. Eight textual features were used to predict a separately constructed Reporting Quality Index (RQI). The resulting predictions were then compared with LSEG ESG scores.

The project was developed as part of an MSc Business Analytics dissertation at the University of Greenwich.

**Important distinction:** The RQI measures reporting evidence. It does not independently verify corporate claims, identify dishonest companies or measure actual sustainability performance.

## Research Questions

1. **RQ1:** To what extent can NLP-derived textual features from sustainability and annual reports predict ESG reporting quality among UK firms?
2. **RQ2:** How closely do machine-learning-generated reporting-quality predictions align with third-party ESG ratings?

## Research Objectives

The study aimed to:

1. Develop a reproducible corpus of corporate reports from a candidate sample of 150 UK companies.
2. Engineer eight NLP features and construct a separately defined reporting-quality index.
3. Examine statistical relationships, overlapping predictors and sector differences.
4. Develop and compare regression and machine-learning models.
5. Evaluate alignment with LSEG ESG scores and document the framework’s limitations.

## Data and Sample

The source population was downloaded from LSEG Datastream through university-authorised access.

| Sampling stage                                    | Number |
| ------------------------------------------------- | -----: |
| Records in the source dataset                     |    352 |
| Companies after removing two non-company records  |    350 |
| Eligible companies with available LSEG ESG scores |    335 |
| Proportionately stratified candidate sample       |    150 |
| Companies in the final analytical sample          |    100 |

Companies were sampled across ten sectors using The Refinitiv Business Classification (TRBC). A fixed random seed of 42 supported reproducibility.

One annual, sustainability or ESG report was selected per company from the 2022–2025 reporting period. Annual reports were prioritised. Documents were collected from official company websites and associated document domains, then checked for PDF validity, duplication and sufficient extractable text.

The final dataset contained **100 companies and 18 columns, including eight NLP predictors**. The number of dataset columns should not be confused with the number of textual predictors.

No Kaggle dataset was used.

## Notebook Workflow

Run the notebooks in the following order. Each stage produces outputs required by later stages.

| Notebook                                                   | Purpose                                                                                                                                      |
| ---------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| **01 — Population Sampling and Report Collection**         | Prepare the eligible population, select the candidate sample, discover report URLs and collect corporate reports.                            |
| **02 — PDF Validation, Text Extraction and Preprocessing** | Validate PDFs, identify duplicates, extract text, apply eligibility checks and prepare the final corpus.                                     |
| **03 — NLP Feature Engineering and Automated RQI**         | Calculate the eight textual predictors and score the separately defined reporting-evidence criteria.                                         |
| **04 — Statistical Analysis and Predictive Modelling**     | Examine distributions, correlations and sector differences; compare models; assess feature importance; and generate out-of-fold predictions. |
| **05 — External Validation and Final Results**             | Compare observed and predicted RQI with LSEG ESG scores and consolidate the final results.                                                   |

## NLP Features

| Feature                             | What it measures                                           |
| ----------------------------------- | ---------------------------------------------------------- |
| **Word Count**                      | The amount of extracted report text.                       |
| **ESG Keyword Density**             | The concentration of predefined ESG-related terms.         |
| **Flesch Reading Ease**             | The linguistic readability of the report.                  |
| **Sentiment Score**                 | The tone of the extracted text.                            |
| **Lexical Diversity**               | Vocabulary variety, measured through the type–token ratio. |
| **Jargon Density**                  | The concentration of specialised reporting terminology.    |
| **Thematic Richness**               | Coverage of predefined sustainability themes.              |
| **Quantitative Disclosure Density** | The frequency of detected numerical patterns.              |

TRBC sector was included as a control variable. LSEG ESG Score was reserved for external comparison rather than included as a predictor.

## Reporting Quality Index

The dependent variable was an automated Reporting Quality Index constructed from eight reporting-evidence criteria:

1. Governance and accountability.
2. Materiality and stakeholder engagement.
3. Strategy and risk integration.
4. Targets and time horizons.
5. Quantitative performance indicators.
6. Comparative performance.
7. Reporting-framework alignment.
8. External assurance.

Each criterion received:

* **0:** No indicator was detected.
* **1:** One or two indicators were detected.
* **2:** At least three indicators were detected.

The total score was converted to a percentage:

**Final RQI = (Sum of the eight criterion scores ÷ 16) × 100**

Supporting evidence passages were retained for inspection. A ten-company pilot, with one company from each sector, supported a procedural review before the unchanged scoring rules were applied to the full sample.

The RQI was not calculated by adding the eight NLP predictors. Separate scoring rules reduced direct circularity, although the predictors and outcome still came from the same reports.

## Analytical Methods

### Statistical analysis

The analysis included:

* Descriptive statistics and distribution plots.
* Shapiro–Wilk normality tests.
* Interquartile-range outlier checks.
* Spearman rank correlations.
* Benjamini–Hochberg adjustment for multiple testing.
* Variance Inflation Factor diagnostics.
* Kruskal–Wallis tests for sector differences.

Plausible outlying observations were retained rather than automatically removed.

### Predictive modelling

Three models were compared:

* Multiple Linear Regression.
* Random Forest.
* XGBoost.

Evaluation used an 80:20 training–testing split and five-fold cross-validation. Performance was assessed using Mean Absolute Error (MAE), Root Mean Squared Error (RMSE) and R².

Random Forest was selected using cross-validation performance. Permutation importance assessed predictive feature contributions, and out-of-fold predictions were generated for all 100 companies.

## Main Findings

### Final predictive performance

| Measure             | Random Forest out-of-fold result |
| ------------------- | -------------------------------: |
| Companies evaluated |                              100 |
| MAE                 |                             9.50 |
| RMSE                |                            11.65 |
| R²                  |                            0.718 |

The model explained **71.8% of the variation in the automated RQI**. Word Count, Thematic Richness and Lexical Diversity were the most influential predictors.

The model-selection cross-validation results were calculated on the 80-company training sample. The final out-of-fold assessment covered all 100 companies; these evaluations should therefore be reported separately.

### External validation

| Comparison                          | Spearman correlation |
| ----------------------------------- | -------------------: |
| Actual RQI versus predicted RQI     |                0.788 |
| Actual RQI versus LSEG ESG Score    |                0.506 |
| Predicted RQI versus LSEG ESG Score |                0.513 |

All three associations remained statistically significant after multiple-testing adjustment, with adjusted p-values below 0.001.

The findings supported the predictive value of report text and moderate alignment with the external benchmark. They did not establish that the RQI and LSEG measured identical constructs.

## Running the Project

The analysis uses Python and Jupyter Notebook.

Core analytical packages include pandas, NumPy, PyMuPDF, NLTK, textstat, SciPy, statsmodels, scikit-learn, XGBoost, Matplotlib, seaborn and openpyxl. Consult each notebook’s import and setup cells for its complete dependencies and required resources.

To reproduce the workflow:

1. Obtain authorised access to the required source data.
2. Review the notebook setup cells and configure the project paths for your computer.
3. Install the required packages and any specified NLP resources.
4. Run the notebooks sequentially from Notebook 01 to Notebook 05.
5. Check each stage’s validation results before continuing.
6. Retain intermediate datasets so later stages can be reproduced without repeating report collection.

Live report collection may produce different results if company websites, document URLs or available reports change.

## Data Access and Responsible Use

LSEG data were accessed through a university arrangement. Access to this repository does not provide a licence to use or redistribute those data.

Before publishing or reusing project materials:

* Check the applicable permissions for LSEG exports and derived datasets.
* Respect copyright and website access conditions for corporate reports.
* Remove credentials, access tokens and unnecessary personal information.
* Review notebook outputs for restricted data before making them public.

Full reproduction requires the relevant input files and appropriate access permissions. Public availability of a report does not automatically permit unrestricted redistribution.

## Limitations

* The analytical sample contained 100 companies, with unequal sector sizes.
* The test set contained only 20 companies.
* Reports covered different years and were predominantly annual reports.
* Automated evidence detection could miss context or reward longer documents.
* Predictors and RQI shared the same underlying reports.
* Model selection and tuning could introduce optimism without nested validation.
* LSEG scores provided an external benchmark, not an independent audit of corporate claims.

Future work could incorporate expert scoring, larger longitudinal samples, more context-sensitive NLP and independent validation datasets.

## Practical Value

The framework demonstrates how unstructured corporate reports can be transformed into an auditable analytical dataset.

It could support analysts and reporting teams in prioritising documents for closer review. It should be used as an **evidence-assisted screening tool**, alongside professional judgement and other sources of sustainability information.

## Author

**Damilola Abati**
MSc Business Analytics
University of Greenwich

## Licence

Refer to the repository’s licence file, if provided, for code-reuse terms. Any code licence does not automatically extend to LSEG data, corporate reports or other third-party materials.
