# WNMFDDA: Hypothesis Testing and Evidence-Aware Architecture Development

This repository documents a research project investigating the assumptions behind **WNMFDDA**, a weighted non-negative matrix factorisation approach for drug–disease association prediction, and developing an evidence-aware extension.

The focus is not only the final architecture. The notebooks preserve the progression from mathematical and implementation checks, through hypothesis-driven experiments and rejected alternatives, to the final multi-scale latent-agreement approach.

## Research overview

Drug–disease association datasets are sparse. The original method combines weighted K-nearest-neighbour (WKNN) reconstruction with graph-regularised non-negative matrix factorisation. WKNN propagates information through similar drugs and diseases; graph regularisation encourages connected entities to have compatible latent representations.

The project asks which assumptions contribute useful predictive information, when graph information helps or oversmooths representations, and whether using the available evidence more selectively can improve predictions.

## What was investigated

- **Implementation and mathematical checks:** WKNN reconstruction invariants, multiplicative-update behaviour, and block-wise curvature properties.
- **Component contributions:** WKNN reconstruction, graph regularisation, and the value of factorisation relative to simpler baselines.
- **Graph behaviour:** graph smoothness, regularisation strength, graph alignment versus permutation, and Laplacian normalisation.
- **Generalisation:** cold-start settings, reduced training density, neighbour information, and the proximity channel.
- **Architectural alternatives:** ECW-NMF, spectral/heat-kernel variants, MS-PU-GNMF, AG-RGNMF, CGFF variants, latent-agreement re-graphing, and the final multi-scale approach.

## Proposed architecture

The final proposal is **EA-MS-LAR-WNMFDDA** (Evidence-Aware Multi-Scale Latent-Agreement Re-graphing WNMFDDA). It combines three modifications:

1. **Availability-aware fusion:** adjusts the contribution of drug-side and disease-side reconstructions according to whether relevant training evidence is available.
2. **Evidence-gated latent-agreement re-graphing:** refines the original similarity graph using agreement with learned latent representations, while falling back to the original similarity when association evidence is insufficient.
3. **Multi-scale prediction:** averages predictions across neighbourhood sizes \(K=p\in\{3,5,8\}\).

## Main result

In pair-held-out evaluation, the proposed architecture improved mean AUC, AUPR, and Recall@10 over the multi-scale baseline on both Cdataset and Fdataset, for both random and NNDSVDar initialisation. The reported AUPR improvements over that baseline range from approximately **0.0075 to 0.0117**.

Cold-start results are mixed: AUC sometimes improves, but AUPR and Recall@10 do not improve consistently. The results therefore support a pair-held-out improvement, not a claim that the method solves cold-start prediction.

The final summary reports mean and standard deviation across nine outer splits per dataset, setting, and initialisation. These are descriptive results; the exported summary does not provide a complete fold-level table for independently reproducing inferential tests.

## Repository contents

```text
notebooks/   Six original experiment notebooks, with their code and saved outputs
results/     Consolidated metrics, hypothesis findings, architecture screens, and figures
data/        Notes about the external benchmark data (data files are not included)
```

### Notebooks

| Notebook | Main role |
|---|---|
| `01_supporting_arguments_S1-S3.ipynb` | WKNN invariants, optimisation behaviour, and block-wise curvature checks |
| `02_component_and_graph_hypotheses_R1-R3.ipynb` | Component ablations, factorisation baselines, and graph smoothness |
| `03_coldstart_density_neighbour_hypotheses_RH1-RH3.ipynb` | Cold-start transfer, training-density reduction, and neighbour-based evidence |
| `04_regularisation_and_graph_hypotheses_RH6-RH8.ipynb` | Regularisation interactions, graph permutation, and Laplacian normalisation |
| `05_proximity_channel_hypothesis_RH11.ipynb` | Controlled neighbour-blinding / proximity-channel experiment |
| `06_architecture_development_and_final_EA-MS-LAR.ipynb` | Architectural alternatives and final model comparison |

The uploaded notebook set does not include RH4–RH5 or RH9–RH10; the numbering is intentionally preserved rather than implying that every hypothesis notebook is present.

## Results files

- `summary_metrics.csv` — final architecture and baselines, with mean, standard deviation, and split count.
- `paired_delta_summary.csv` — differences between the reported EA-MS-LAR and multi-scale baseline means on the same outer splits.
- `hypothesis_verdicts.csv` — hypothesis outcomes and the scope of the supporting evidence.
- `ecw_*.csv`, `intermediate_lar_*.csv`, `spectral_heat_kernel_screen.csv`, and `ag_rgnmf_screen.csv` — separate exploratory architecture results. These are not pooled into the final-model leaderboard because their run settings and evaluation protocols differ.
- `results/figures/` — plots generated from saved notebook outputs. Error bars show the variability reported by the notebooks; they are not confidence intervals unless explicitly identified as such in the figure.

## Data and reproducibility

The notebooks refer to the Cdataset and Fdataset benchmark files (`DataC.mat` and `DataF.mat`) from an external data location. The dataset files are **not** included in this repository. See [`data/README.md`](data/README.md) for the data note. Before public release, add the verified dataset source and check its terms of use.

The notebooks preserve their experimental code and saved outputs. Some experiments use quick profiles with limited seeds or folds; consult the individual notebook cells and exported result metadata when interpreting them. Unobserved drug–disease pairs are treated as negatives in these evaluations, although some may be undiscovered positives.

## Reference

Wang, M.-N., Xie, X.-J., You, Z.-H., Ding, D.-W. & Wong, L. “A weighted non-negative matrix factorization approach to predict potential associations between drug and disease.” *Journal of Translational Medicine* 20, 552 (2022). https://doi.org/10.1186/s12967-022-03757-1

Please cite the original work when using or adapting the baseline method. The proposed modifications and experimental comparisons in this repository are part of this project.
