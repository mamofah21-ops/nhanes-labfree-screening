# Laboratory-free screening for undiagnosed diabetes and hypertension (NHANES)

Code for the paper:

> P. Amofa et al., "Laboratory-Free Machine Learning Screening for Undiagnosed Diabetes and Hypertension: Temporal Validation, Fairness and Explanation Stability in NHANES 2011 to 2023," submitted to *IEEE Access*, 2026.

The notebooks build screening models that use only information a primary care clinic can collect without a laboratory: age, sex, body measurements, blood pressure, smoking, physical activity, education and medical history. The models are trained on one period of the US National Health and Nutrition Examination Survey (NHANES), tuned on a later period and tested once on the most recent period.

| Period | NHANES cycles | Role |
| --- | --- | --- |
| 2011 to 2016 | G, H, I | Development |
| 2017 to March 2020 | P (pre-pandemic) | Validation and tuning |
| August 2021 to August 2023 | L | Test |

## What the notebooks do

| Notebook | Contents | Run time on Colab |
| --- | --- | --- |
| `01_data_preparation.ipynb` | Downloads the NHANES files from the CDC, harmonises them across cycles, applies the eligibility criteria, defines the outcomes and saves `nhanes_analytic.csv` | About 5 minutes |
| `02_models_and_performance.ipynb` | Logistic regression, EBM, XGBoost and LightGBM; tuning, test performance, calibration and comparison with the ADA risk score | About 10 minutes |
| `03_decision_curves_fairness_shap_stability.ipynb` | Decision curve analysis, subgroup performance and SHAP stability (bootstrap, seeds, periods and model types) | 20 to 40 minutes |
| `04_sensitivity_and_weighted.ipynb` | Alternative outcome definitions, a diabetes model without blood pressure and survey-weighted performance | About 5 minutes |

## How to reproduce the results

1. Open `01_data_preparation.ipynb` in Google Colab and choose Runtime > Run all. It downloads the public data directly from the CDC, so nothing needs to be requested or registered. When it finishes, it saves `nhanes_analytic.csv`.
2. Open each of the next three notebooks on a fresh runtime (Runtime > Disconnect and delete runtime, then Runtime > Run all) and upload `nhanes_analytic.csv` when asked.
3. Each notebook saves its tables and figures to an output folder and offers them for download.

The notebooks also run locally with Python 3.10 or later. Install the packages with `pip install -r requirements.txt`.

## Expected results

If everything has run correctly, the test AUCs in notebook 2 should read:

| Outcome | Logistic regression | XGBoost |
| --- | --- | --- |
| Undiagnosed diabetes (HbA1c 6.5% or higher) | 0.781 | 0.799 |
| Undiagnosed hypertension (140/90 mmHg) | 0.709 | 0.713 |

All analyses use a fixed random seed of 2026. Results for logistic regression, XGBoost and LightGBM reproduce exactly. EBM results can differ slightly between machines because its fitting is not fully deterministic.

## Data

All data are public and are not redistributed here. The files come from the National Center for Health Statistics at https://wwwn.cdc.gov/nchs/nhanes/. NHANES protocols were approved by the NCHS Research Ethics Review Board, and all participants gave written informed consent.

## A note on reading the CDC files

`pandas.read_sas` reads a stored zero in some SAS transport files as a tiny number (about 5.4e-79) rather than exactly 0. Notebook 1 sets these values back to 0 when loading each file. Without this step, answers of "0 times" to the 2021 to 2023 physical activity questions are counted as "active", and diastolic readings of 0 mmHg are not recognised as missing.

## Citation

If you use this code, please cite the paper above. Full citation details will be added on publication.

## Contact

Prince Amofa, Department of Computer Science, University of Ghana (pamofa004@st.ug.edu.gh, ORCID 0009-0009-1092-8017)

## Licence

MIT. See `LICENSE`.
