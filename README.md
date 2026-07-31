# PCRA-Index's Validation & Application

This repository contains the full set of Jupyter notebooks used to construct, validate, and apply the **Physical Climate Risk Attention (PCRA) index**, decomposed into its persistent level component (**SSI**, Salience State Index) and its transitory component (**CSP**, Climate Surprise Proxy).

**Each notebook is independent.** They all load the constructed index and any required financial/event data directly from disk at the top of the notebook (`Config — set Paths` / `Load data` cells), so none of them needs to be run in the same kernel session as another. The only hard dependency is that `Index_Construction.ipynb` must have been run at least once to produce the index files that every other notebook reads in.

## Suggested run order

### 1. `Index_Construction.ipynb`
The production pipeline. Builds the PCRA index for the US, full sample (2008–2025), end to end:
- Stage A — retrieval of candidate articles
- Stage B — syndication cleanup
- Phase 2 — article-level scoring
- Phase 3 — novelty weighting
- Full-sample production run

This is the source of the index files consumed by every notebook below. Run this first (or confirm the index outputs already exist) before running anything else.

### 2. `Article_Level.ipynb`
Two parts: an incremental reconstruction of the index at the article level (volume count, uniformity, novelty, EM-DAT coincidence), and the human audit workflow (filter-pass/filter-reject sampling, direct scoring, combination scoring, Spearman range checks) used to validate the article-classification step.

### 3. `Index-Level_Validity.ipynb`
The core statistical validation chapter. SSI construction and CSP distributional checks, orthogonality, stationarity (Phillips–Perron, variance ratio), whiteness, ARCH-LM, CUSUM, spectral density, half-life and AR-order sensitivity, window/offset sensitivity, EM-DAT permutation test, and convergent validity, plus the numbered figures used in the thesis.

### 4. `Discriminant_Validity.ipynb`
Tests whether CSP is genuinely distinct from CPU and EPU: pairwise and full correlation matrices, lead-lag cross-correlations, shock overlap, rolling correlation, distribution comparison, and PCA (CSP vs CPU, CSP vs EPU, CSP vs CPU vs EPU).

### 5. `Predeterminedness.ipynb`
Checks whether CSP is predetermined with respect to same-day yield moves: Granger causality, joint Wald test, distance correlation, and a visual/summary check.

### 6. `Placebo_Test.ipynb`
Constructs a placebo SSI/CSP using the Loughran–McDonald negative sentiment lexicon in place of the climate lexicon, runs the same EM-DAT coincidence permutation test and PCA, and compares against the true index as a falsification check.

### 7. `Sovereign_Investigation.ipynb`
The bond-side asset-pricing application. Builds the master panel and runs Jordà local projections on Treasury yields (2Y/5Y/10Y/30Y): baseline and long-horizon LPs, term-structure gradient, IRF plots, response-surface heatmaps, yield-spread LP, sub-sample stability, SUR joint tests, CSP-vs-SSI pricing comparison, event studies around top CSP spikes, asymmetry tests, and lag-structure robustness (including a GPR cross-check).

### 8. `Industry_Investigation.ipynb`
The equity-side asset-pricing application. Builds FF12 and FF49 industry exposure scores and panels, runs the daily panel regressions and compares taxonomies, tests temporal-aggregation and cross-validation robustness, sub-sample stability, and lagged-return checks, and breaks results down by exposure gamma, power/severity/volatility, firm level, and conditionality (plus a Faccini-style monthly/weekly comparison).

## Notes
- Notebooks 2–8 are each self-contained validation or application exercises that read the index built in step 1 — you can run them in any order relative to each other, but all of them depend on step 1 having been completed.
- `Sovereign_Investigation.ipynb` and `Industry_Investigation.ipynb` together form the asset-pricing validation referenced throughout the thesis against the CPU paper (Gavriilidis, Känzig, Raghavan & Stock, NBER WP 34762).
