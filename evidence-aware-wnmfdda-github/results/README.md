# Results

`summary_metrics.csv` contains the final EA-MS-LAR comparison and its baselines. It reports mean and standard deviation over the recorded outer splits. `paired_delta_summary.csv` contains differences between the reported means for EA-MS-LAR and the multi-scale baseline.

`hypothesis_verdicts.csv` records the supporting checks and hypothesis outcomes with their scope and source notebook. The remaining architecture CSVs are kept as separate exploratory results; they should not be merged into the final-model comparison because their experiment settings differ.

Figures in `figures/` were created from saved notebook outputs. The final EA-MS-LAR comparison figures show mean values with the standard deviations reported in the notebook. The ECW figures retain the confidence intervals reported by that experiment.
