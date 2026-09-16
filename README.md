# 2026-Y2-S1-KU-28-NSL-KDD
IT2011 AI/ML — NSL-KDD Data Preprocessing and EDA — Progress Review I

Group: **2026-Y2-S1-KU-28**

## Purpose
Prepare the assigned NSL-KDD dataset for future binary intrusion detection. Scope is cleaning, preprocessing and EDA only. This version replaces the earlier complex implementation with short, sequential notebook cells and simple explanations. It contains seven individual notebooks and one integrated notebook, all with executed outputs.

## Members
| IT number | Name | Responsibility |

| IT25102665 | Withanawasam M.W.S.T. | Categorical encoding |
| IT25102687 | Amaranayake D.R. | Feature_scaling |
| IT25102702 | Madugalla G.N.M.W.S.N.B. | Binary_target_preparation |
| IT25300049 | Mahawithana M.A.S.N.M. | Missing_data_and_duplicates |
| IT25102827 | Palamakumbura W.W.M.D.N. | Outliers_and_log_transformation |
| IT25103036 | Wijeweera W.A.A.A. | Data_types_and_constant_features |
| IT25102835 | Wanigasekara W.M.T.S.B. | Correlation_feature_selection |

## Start here
1. Extract this ZIP completely.
2. Local Jupyter: open the group folder and install pandas, numpy, matplotlib, scikit-learn and jupyterlab using `python -m pip install -r requirements.txt`. Start with `python -m jupyterlab`.
3. Open an individual notebook, or group_pipeline.ipynb, and run cells in order.
4. For Google Colab, follow the setup instructions at the start of every notebook. Store the full extracted folder in Drive, mount Drive, and set ROOT to its actual path. Uploading notebooks alone does not upload their datasets. Each separate Colab runtime needs access to Drive. The code was executed locally; the cloud setup has not been tested here.

## What became simpler
- No custom preprocessing functions, ColumnTransformer, sparse matrices or saved transformer objects.
- One-hot encoding uses pandas.get_dummies; reindex keeps the training column schema.
- Outlier handling demonstrates log1p for src_bytes only.
- Feature selection uses one named pair and a visible 0.95 correlation rule.
- Complete processed data are compressed CSV files, readable with pandas.read_csv.
- Small cells contain one task each, with explanations immediately below them.

The selection and log-transform choices differ from the earlier package. Do not mix old output statistics with this version. Shorter code does not reduce the need to understand each method.

## Verified results
- Training: 125,973 rows; test: 22,544 rows.
- No missing cells or exact full-row duplicates in the supplied files. Feature-only repetitions can have different labels and are not deleted.
- 67,343 normal and 58,630 attack training records.
- Training-constant num_outbound_cmds removed.
- srv_serror_rate removed because its training correlation with serror_rate exceeds 0.95. This is a simple chosen rule, not a validated optimal selection.
- Final output: **120 features + binary_label**, so each saved CSV has 121 columns.
- Every original row and its order are preserved. Test statistics never fit the scaler or select columns.

## Outputs
`results/outputs/processed_train.csv.gz` and `processed_test.csv.gz` contain the full data, not samples. `binary_label` is the target and must be excluded from X during later modeling.
```python
import pandas as pd
data = pd.read_csv('results/outputs/processed_train.csv.gz')
X = data.drop(columns='binary_label')
y = data['binary_label']
```
`results/eda_visualizations/` contains seven charts. `results/logs/execution.json` records execution and checks. Each individual notebook includes its own chart and interpretation; the group notebook combines preprocessing and shows the class chart.


## Review and viva
See docs/SIMPLE_VIVA_GUIDE.md. Present technique → reason → code/output → chart interpretation. All seven members have assigned sections, but actual contribution and understanding must be demonstrated personally. The supplied specification says six members; confirm approval of the seven-person roster. Prepare for 15 minutes because the review PDF also mentions 20 minutes elsewhere.

## Limitations
This is a teaching pipeline for the provided files, not a production validator. It assumes their documented schema and valid nonnegative byte values. Unknown categories in test become all-zero blocks under the training schema. Correlation-based removal and log transformation require future training-validation comparison, and no accuracy improvement is claimed. For later cross-validation, learn scaling and feature selection separately inside each fold from raw data. NSL-KDD is historical and does not represent all modern traffic.

## References
- Supplied Progress Review I specification and Group Assignment Specification (included under docs).
- Supplied Lab Sheets 01, 02, 03, 04, 06 and 07: teaching style and relevant techniques.
- NSL-KDD dataset: https://www.unb.ca/cic/datasets/nsl.html
- User's dataset distribution: https://www.kaggle.com/datasets/hassan06/nslkdd
- Tavallaee, Bagheri, Lu and Ghorbani (2009), A Detailed Analysis of the KDD CUP 99 Data Set, IEEE CISDA, DOI 10.1109/CISDA.2009.5356528.

