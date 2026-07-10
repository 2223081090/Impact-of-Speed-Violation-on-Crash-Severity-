IMPACT OF SPEED VIOLATION ON CRASH SEVERITY
Complete Machine Learning Workflow Package
=============================================

FOLDER STRUCTURE
----------------
Reports/
  - Dataset_Processing_Report.docx   : Full data validation, cleaning, feature engineering,
                                        skewness/correlation/multicollinearity, and split methodology
  - Model_Performance_Report.docx    : Model comparison, filtering results, metrics, and figures

Figures/                              : All 14 generated charts (PNG) — correlation heatmap, ROC/PR
                                         curves, confusion matrices, SHAP plots, feature importance,
                                         learning curve, EDA charts on speed-violation risk vs severity

Tables/
  - all_models_results.csv            : Raw metrics for all 10 trained models
  - all_models_results_filtered.csv   : Metrics + KEPT/REMOVED status per filtering rules
  - final_model_comparison.csv        : Metrics for the 9 retained models only
  - feature_importance_best_model.csv : Top-20 feature importances for the best model (LightGBM)

Models/                                : Trained model files (.pkl, joblib format) for all 10 models
                                          plus the fitted StandardScaler

Processed_Data/
  - processed_full_dataset.csv        : Final processed dataset — all 46 original columns preserved,
                                         unmodified, plus 98 engineered/encoded columns (144 total)

KEY RESULTS
-----------
Best Model: LightGBM — Test Accuracy 94.00%, ROC-AUC 97.20%, F1 92.35%
9 of 10 models retained after applying the <80% / >97% / >5%-gap filtering rules
(MLP Classifier removed for a 5.23% train-test accuracy gap).

The engineered "Speed_Violation_Risk_Score" (a composite proxy built from road class,
absence of speed-reducing infrastructure, night-time driving, and low visibility — since the
raw dataset has no explicit speed-limit/violation field) is the single strongest correlate of
crash severity (r ≈ 0.455) and ranks among the top SHAP/feature-importance drivers, directly
supporting the thesis.

METHODOLOGY NOTES
------------------
- All 46 original dataset columns were preserved unchanged throughout (per project rules).
- Random seed = 42 used for all splits, models, and cross-validation for reproducibility.
- Train/Val/Test = 70/15/15, stratified, with 5-fold Stratified CV on the training set.
- Scaling fit on train only (no leakage). Outlier capping via IQR, not row removal.
- Target reframed as binary (Low: Severity 1-2 / High: Severity 3-4) due to extreme rarity
  of classes 1 and 4 (6 cases each) in the original 4-class label; original Severity column retained.
