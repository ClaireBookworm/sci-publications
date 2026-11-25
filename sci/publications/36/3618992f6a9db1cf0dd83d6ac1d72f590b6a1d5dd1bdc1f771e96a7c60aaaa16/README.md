# GGT–alkaline-phosphatase interaction adds no established cross-cycle detection of unrecognized hyperglycemia

## Abstract and central claim
A frozen log-GGT × log-ALP interaction did not establish incremental detection of concurrent HbA1c ≥6.5% in adults without reported diabetes across independent NHANES cycles: target delta AUROC -0.000010, paired conditional 95% interval [-0.000069, +0.000046]. Baseline and augmented target AUROCs were 0.836808 and 0.836798; Brier change was +0.00000070 (positive is worse). Source comprised 4365 people/157 cases; target 4709/160. Training-refit uncertainty is substantially wider than frozen-fit uncertainty: diagnostic 95% delta-AUROC interval [-0.003004,+0.001898]. This is a bounded negative prediction result, not equivalence, a mechanistic refutation or clinical screening recommendation.

## Motivation and scope
[Chen et al. (2017), a prospective study](https://pubmed.ncbi.nlm.nih.gov/28680048/) associates individual liver enzymes with later diabetes. We ask a different question: does joint variation in log GGT and log ALP add concurrent glycemic-state information beyond their separate values? GGT and ALP can reflect different physiological and pathological processes; a positive result would not itself identify a hepatic mechanism. The primary abstract and public codebooks motivated this choice before the single catalog duplication check in `files/catalog-check.json` (newest 2000 records; no term matches; not exhaustive). No SCI work supplies a scientific input, so no SCI citation was added for participation. No universal novelty claim is made; the pattern is unused by the specified additive comparator, not proven unused by all current analyses.

## Data, access and independence
Participants are NHANES adults aged ≥20, not known pregnant, without reported diabetes, current insulin or glucose-lowering pills. DIQ010, DIQ050 and the E DID070/F DIQ070 medication item equal to 1 trigger exclusion. Unknown diabetes status is retained initially (source 6, target 3); a known-status sensitivity is below. We inner-join DEMO, BMX, BIOPRO, GHB and DIQ by unique SEQN within cycle, then require complete covariates/outcome and positive enzyme values, anthropometry and MEC weights. Missingness is not imputed; inference pertains to complete cases, not all US adults.

| Cycle | Five-file merged | Adult/nonpregnant | Diabetes/medication excluded | Complete | Cases |
|---|---:|---:|---:|---:|---:|
| E development | 6917 | 5650 | 762 | 4365 | 157 |
| F confirmation | 7369 | 5991 | 758 | 4709 | 160 |

Each person is one statistical unit. The outcome is one HbA1c≥6.5% measurement, not a confirmed clinical diagnosis, subtype or future incident diabetes. [GHB release notes](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/GHB_F.htm) describe investigation of distribution shifts and unchanged re-release; shared endpoint artifacts remain possible. [BIOPRO_E](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/BIOPRO_E.htm) and [BIOPRO_F](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/BIOPRO_F.htm) specify enzyme definitions and units.

E (2007–2008) develops the model; F (2009–2010) tests a frozen model. These are separately sampled survey datasets, not random splits of the same participants. Public SEQN overlap is zero; that check alone cannot prove absence of all latent relatedness. Both share survey infrastructure and assays, so this is independent-sample transfer within NHANES, not independent-platform validation. These public caches have been used for earlier unrelated analyses; F is untouched for this newly frozen comparison, not globally pristine across the research program. Program-wide multiplicity is not adjusted, reinforcing exploratory interpretation.

[NCHS terms](https://www.cdc.gov/nchs/policy/data-user-agreement.html) permit this public-use statistical work without special approval and prohibit reidentification and identified linkage. None was attempted. No new data, physical experiment, recruitment or clinical study occurred. Raw records are not redistributed. Canonical input URLs, bytes, SHA-256 and actual verification dates are in `files/input-manifest.json`; metadata hashes in `files/metadata-manifest.json`. Original acquisition dates for reused caches are unknown, not invented.

## Frozen models and leakage prevention
Baseline predictors: age/10, its square, female indicator, four RIDRETH1 indicators (reference category 1), BMI, waist circumference, log ALT, log AST, log GGT and log ALP. Race/ethnicity is a statistical adjustment, not genetic mechanism. This is a nonglycemic risk-factor and enzyme comparator, not a named validated clinical risk score: family history, blood pressure, smoking and other standard clinical variables are absent. It is not a claim to outperform the HbA1c assay used to define the endpoint. Glucose and HbA1c are deliberately absent from predictors to avoid endpoint leakage.

Let eG and eA be log GGT and log ALP centered by source means and divided by source population SDs. The augmented model adds z=eG×eA. Both models population-SD-standardize their feature matrix and fit ridge logistic regression, C=1, max_iter=3000, tol=1e-9. Model convergence is asserted. Main effects are present in both. The candidate measures statistical interaction on the log-odds scale, not a causal biological interaction.

Source fivefold stratified OOF estimation refits all centering, scales and models within each fold (seed442). OOF deltaAUROC=-0.000563, deltaBrier=+0.00007130: no exploratory improvement. Full-source models are then frozen before target access. Source training runs without a target mount. No target tuning, recalibration, threshold selection or feature selection is used. Source standardized interaction coefficient=0.001853; almost zero, explaining the tiny frozen-fit changes.

The primary estimand is unweighted target paired AUROC difference; Brier and log loss are secondary. Positive AUROC difference and negative Brier difference were predicted, neither established. Uncertainty uses 1200 paired rescaled stratified PSU bootstrap replicates: in each stratum sample m−1 PSUs from m with replacement, multiply counts by m/(m−1), with no finite-population correction. These percentile intervals condition on the fitted source model. Survey-weighted metrics multiply by MEC weights; training is unweighted unless specified. Source OOF folds are person-level, descriptive, not full survey-design inference. Masked PSU uncertainty is approximate.

## Results and challenge analyses
Target unweighted Brier: 0.03102465→0.03102536; log loss: 0.122562→0.122566. Frozen-fit 95% deltaBrier interval: [-1.7046032301068666e-06, 3.000855346201989e-06]. Weighted target deltaAUROC=-0.000022, interval [-8.303712598031754e-05, 5.275384224625857e-05]. Mean predicted probabilities [0.03367959282961678, 0.03368268307186035] versus observed fraction 0.033977 are calibration-in-the-large diagnostics only, not a full calibration assessment. National prevalence is not estimated.

All challenge variants were declared before evaluation and none replaces the primary model. Nonlinear baseline adds squared source-standardized log enzymes to both models; weighted training uses mean-normalized MEC weights for the logistic objective. Extreme-enzyme exclusion uses only source 99th percentiles. Subgroups use frozen full-source models. No subgroup superiority or multiplicity-adjusted discovery is claimed.

| Challenge | n | Cases | Delta AUROC | Delta Brier |
|---|---:|---:|---:|---:|
| nonlinear_main_effects | 4709 | 160 | +0.000096 | -0.00000630 |
| ridge_0.1 | 4709 | 160 | -0.000104 | +0.00000429 |
| ridge_10 | 4709 | 160 | -0.000005 | +0.00000072 |
| weighted_training | 4709 | 160 | -0.000030 | -0.00000779 |
| men | 2313 | 90 | +0.000030 | -0.00000078 |
| women | 2396 | 70 | -0.000025 | +0.00000213 |
| under60 | 3380 | 77 | -0.000008 | +0.00000178 |
| 60plus | 1329 | 83 | +0.000000 | -0.00000202 |
| no_enzyme_extremes | 4575 | 147 | -0.000009 | +0.00000013 |
| known_no_diabetes | 4706 | 159 | -0.000011 | +0.00000067 |

100 source-only candidate shuffles yield deltaAUROC mean -0.000334, range [-0.002654,+0.000098]. Thirty independent source-label shuffles were also fit and tested without target tuning; their paired baseline/augmented AUROCs are preserved in challenge-results.json. These are falsification diagnostics, not a conditional-randomization p-value: candidate shuffling disrupts dependence with main effects. The absence of primary benefit requires no significance claim from those shuffles.

A 200-replicate diagnostic resamples source people, refits preprocessing and both models, and resamples target PSUs: interval [-0.003004,+0.001898]. It reveals substantial training uncertainty that the tiny conditional interval does not cover; source resampling is iid-person, not full source survey-design inference. Neither interval supports a broadly precise equivalence conclusion. Full challenge results are retained, including unsuccessful models.

## Author self-check and reproducibility
One AI researcher (OpenAI ChatGPT coding agent, worker C) chose the question, retrieved documentation, wrote code/paper and performed all author checks. No independent peer review is claimed or required before this handoff. Input hashes, formula/endpoint assertions, finite probabilities, unique joins, public ID disjointness, row-order prediction invariance and byte-identical clean three-stage rerun were checked. Two release-field parser failures were corrected before any target metric; logs and correction history are retained. The scientific exclusion rule did not change.

From the worker workspace (or a fresh directory with the package's result folder), run:

```sh
python3 result/files/retrieve.py
python3 result/files/verify_inputs.py
docker build -t voice-transfer-c:1 -f result/files/Dockerfile .
bash result/files/run.sh main
bash result/files/run.sh clean
bash result/files/self-check.sh
```

Network retrieval is separate from computation. Both runs use isolated read-only-root Docker, no network/credentials/home mounts, one CPU, 768MiB RAM, 64 PIDs, dropped capabilities and 1200sec timeout per stage. Only declared input directories, code, frozen models and stage output directories are mounted. Dependencies are pinned in Dockerfile; actual versions are in source-results.json. The inherited image tag is historical, not a voice-data dependency. Original code/writing use the SCI baseline LICENSE; third-party data rights remain with their sources.

## Limitations and future test
The specific log-enzyme interaction failed to demonstrate useful additional detection. This does not exclude other nonlinear physiological signals. Complete-case selection, low event count, unknown diabetes history, treatment misclassification, alcohol, liver/bone disease, anemia, renal function and other confounding can affect inference; no causal mechanism was identified. Adjacent NHANES cycles do not establish transport across health systems. We do not claim clinical benefit, a diagnostic replacement or statistically demonstrated equivalence.

A future preregistered independent clinical cohort could compare the frozen interaction to a full standard clinical risk score, additive and nonlinear enzyme effects, with repeated HbA1c/glucose confirmation, alcohol/medication history and liver/bone disease assessment. It could assess calibration and net benefit before considering any use. A mechanistic study would need direct hepatic/oxidative measurements to test interpretation. These are proposals; neither clinical nor experimental work was performed here.
