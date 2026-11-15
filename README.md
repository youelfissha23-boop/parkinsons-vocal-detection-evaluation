# Parkinson's Voice Screening Evaluation

Small machine-learning study on a public voice dataset (195 recordings from
32 people, 24 with Parkinson's and 8 healthy). The goal was not a high score but
an honest one. Educational project, not medical research.

## Key finding
The dataset has about six recordings per person. A random split puts the same
person in both training and test sets, which inflates the score:
random-split AUC 0.955 (recording-level) vs. person-level AUC 0.745 (averaged over 5 seeds using random forest).

## Method
- Splits are made by person, never by recording (to prevent data leakage).
- Models: logistic regression, random forest, SVM, XGBoost (with tuned hyperparameters).
- 5 random repeats of 4-fold cross-validation; all predictions are out-of-fold.
- Metrics: ROC-AUC, and recall and specificity at different cut-offs.

## Results (person-wise, mean ± std over 5 seeds)
| Model | Person-level AUC |
|---|---|
| Random forest | [0.745 ± 0.052] |
| XGBoost | [0.759 ± 0.052] |
| Logistic regression | [0.722 ± 0.037] |
| SVM | [0.660 ± 0.058] |

![ROC](reports/figures/roc_models.png)

Random forest at different cut-offs: 
|Threshold|Recall|Specificity|Flagged|
|---|---|---|---|
|0\.3|0\.993|0\.250|182|
|0\.4|0\.980|0\.271|179|
|0\.5|0\.952|0\.271|175|
|0\.6|0\.925|0\.313|169|
|0\.7|0\.905|0\.375|163|
|0\.8|0\.803|0\.542|140|
No cut-off gives both high recall and high specificity.

## Limitations
- Only 32 people (8 healthy): scores swing a lot depending on which people land in which fold.
- The three top models are statistically tied; only SVM is clearly worse.
- One data source, controlled recordings, no external validation.
- Not clinical evidence: this cannot show that a model reliably detects Parkinson's.

## Next steps
Person-level evaluation with confidence intervals, a permutation test,
and testing on a second dataset or raw audio.

## Credits
Dataset: UCI Parkinsons voice data (see data/README.md for the citation).
Built as a learning project with guidance from an AI.
