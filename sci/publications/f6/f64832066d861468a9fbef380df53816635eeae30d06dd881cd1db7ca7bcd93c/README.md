# Mid-recovery systolic curvature provides no demonstrated CRP prediction advantage in two selected NHANES samples

## Abstract and scope

Recovery SBP curvature did not demonstrate improved cross-cycle CRP prediction: target weighted MSE reduction 0.0056121, two-sample PSU-refit 95% interval [-0.0767051, 0.0521600], in selected adults with a third recovery reading. This is a negative, existing-public-data prediction study, not a clinical diagnostic validation. The simpler linear comparator was better than the spline model with curvature. We do not establish a new inflammatory biomarker or absence of all information in recovery trajectories.

## Question and scientific motivation

Heart-rate recovery has already been studied against inflammation in NHANES [Kuo et al.](https://pubmed.ncbi.nlm.nih.gov/25791914/). Thus neither HRR nor its CRP association is our novelty claim. Conventional systolic recovery measures include third-minute/peak and third-minute/first-minute ratios [1999 angiography study](https://pubmed.ncbi.nlm.nih.gov/10483957/); apparent outcome associations can attenuate after adjustment [2004 mortality study](https://pubmed.ncbi.nlm.nih.gov/14760327/). We asked whether the *middle recovery reading*, summarized as SBP1 − 2×SBP2 + SBP3, retains incremental information on inflammatory state beyond such conventional summaries. This is a candidate underused temporal feature, not a claim that previous literature never used it.

Question and analysis were registered before viewing these data (`files/protocol.json`). A single bounded title-based SCI duplication check, with two 403 failures followed by successful user-agent retry, returned 249 records. Clinic-reading curvature studies were visible but no exact exercise-recovery study was identified. Title-only retrieval cannot exclude duplication or unpublished concurrent work. We did not use or test SCI results and therefore do not add SCI citations.

## Data, rights, independence and population

CDC public-use NHANES 1999–2000 was the discovery/source dataset; 2001–2002 was the frozen-fit confirmation/target dataset. These are separately collected cross-sectional samples, not random splits. Component files DEMO, BMX, BPX, CVX, SMQ and LAB11/L11_B were linked one-to-one by SEQN. There were zero shared participant identifiers. Incidental repeat enrollment cannot be excluded from public identifiers; shared survey organization, assay and protocol limit independence of measurement conditions. This is cross-cycle transfer within NHANES, **not external-cohort validation**. Data are public to download/analyze without special approval per the [CDC access statement](https://www.cdc.gov/nchs/nhanes/about/); restricted RDC files were not used. `files/inputs.json` records URLs, retrieval times, sizes and SHA-256 for every exact XPT. Raw rows are not redistributed.

Unit: one participant, age 20–49. The source had 2460 adults and 428 complete eligible observations; the target had 2848 and 479, respectively. All predictors, positive CRP and positive MEC weights were required, plus completed exam status. Source/target had 27/30 PSUs in 13/15 strata. Smoking missing/refused/unknown was explicitly indicated; BMI and other continuous predictors were not imputed. CRP mg/dL was multiplied by 10 and log transformed; target CRP range was 0.1–53.1 mg/L.

**Important selection restriction detected in self-check:** the [procedure manual §3.9.10, p.3-28](https://wwwn.cdc.gov/nchs/data/nhanes/public/2003/manuals/cv_99-04.pdf) directs a third recovery stage when minute-2 HR remains above 110 bpm. Three-minute SBP availability is therefore informative and does not describe all treadmill participants. Recovery occurs on the treadmill with protocol-dependent workload changes; readings are captured near 50 seconds of each recovery stage, not exact passive-rest minute endpoints. Our estimand is conditional on recorded third-stage recovery and complete data. No inverse-probability correction can recover the structurally unobserved curvature in those who stop earlier. The large excluded group makes population-wide extrapolation inappropriate. Flow counts and recovery-duration ranges are in `files/checks.json`.

## Frozen methods and leakage control

Outcome: concurrent natural-log CRP mg/L, not later disease. Primary comparison: target examination-weighted MSE of conventional spline model minus MSE after adding one signed curvature term divided by 10 mmHg. Positive reduction favors curvature. Expected direction was better prediction with the extra trajectory information; no coefficient sign was required.

All models include age, BMI, resting pulse, mean available clinic SBP1–3, stage-2 HR and SBP, exercise-estimated VO2max (CVDESVO2), HRR1 and HRR2 (stage-2 HR minus recovery HR), and SBP3/SBP1 recovery ratio; sex, race/ethnicity, ever-smoking/unknown and treadmill protocol enter as indicators. The primary conventional model additionally splines SBP3/stage-2 SBP. Stage-2 values are end-exercise references, not necessarily true physiological maxima of a maximal test. These conventional maximal-test concepts are only analogues in a submaximal protocol. SplineTransformer uses four source-quantile knots, cubic degree, no bias, constant extrapolation. Ridge solves weighted least squares with mean-normalized weights and penalty 0.01 on each non-intercept coefficient; no hyperparameter search. Curvature adds a single linear column. Linear and additive-without-extra-ratio comparators are retained, not discarded.

Five-fold PSU-grouped source cross-validation is exploratory discovery. All knots, transformations and coefficients in confirmation use source data only. The source recipe was frozen before the first target prediction; target data did not select knots, penalty or candidate feature. These public cycles may have been used in earlier unrelated worker research, so this is question-specific confirmation, not a globally pristine dataset. Post-hoc sensitivity analyses are explicitly labeled and did not alter the primary fit.

Uncertainty: 2000 fixed-fit target and 300 two-sample source-refit/target stratified PSU bootstrap replicates, percentile 95% intervals, seed 829. Within each stratum sample n−1 PSUs with replacement with n/(n−1) multiplier; rescale all rows of sampled PSUs. Two-sample replicates refit source knots and coefficients. These are approximate selected-sample design-aware intervals, not full official variance estimates or evidence of population representativeness after exclusion. Two-year MEC weights are used separately in each cycle, not pooled four-year weights. Geographic dependence between cycles, if present, is not modeled.

## Results

| Model | Source grouped-CV MSE | Independent-cycle target MSE |
|---|---:|---:|
| Linear covariates | 1.5486480 | 1.6584835 |
| Additive splines | 1.8592068 | 1.7991708 |
| Conventional ratios + splines (primary baseline) | 1.8567911 | 1.8429885 |
| Baseline + curvature | 1.8703224 | 1.8373765 |

Primary target MSE reduction: 0.0056121 (0.305% relative). Fixed-fit 95% interval [-0.0013747, 0.0132399]; source-refit two-sample interval [-0.0767051, 0.0521600]. Discovery CV reduction was -0.0135312, in the unfavorable direction. The fitted curvature coefficient was 0.0153585 log-CRP per 10-mmHg curvature, a conditional prediction coefficient rather than a causal effect. The wide refit interval reflects source estimation instability; a fixed-fit-only interval understates this uncertainty. Target performance of the augmented spline model was worse than the simpler linear comparator; consequently no overall predictive advantage is established.

## Falsification and sensitivity checks

Prespecified challenges: remove CRP >10 mg/L from target, remove it from both samples and refit, BMI <30, sex strata, older adults, unweighted losses and source-label permutations. Endpoint-level augmentation and a lower-complexity linear-ratio baseline are post-hoc challenges. The complete table is retained in `files/results.json` and `files/checks.json`.

- CRP_le10: baseline-minus-curvature MSE 0.0019673, n=441.
- BMI_lt30: baseline-minus-curvature MSE 0.0047367, n=384.
- female: baseline-minus-curvature MSE 0.0029927, n=229.
- male: baseline-minus-curvature MSE 0.0081174, n=250.
- age_ge35: baseline-minus-curvature MSE -0.0027333, n=177.
- unweighted: baseline-minus-curvature MSE 0.0039939.
- both_CRP_le10_refit: baseline-minus-curvature MSE 0.0064099.
- endpoint_augmented_baseline: baseline-minus-curvature MSE 0.0041521.
- Post-hoc linear-ratio comparator: reduction 0.0064068.

Fifty source-label permutations yielded reductions from -0.0935153 to 0.0095038, median -0.0013335; this is a pipeline stress test, not a valid covariate-conditional randomization test or primary p-value. Mutating all target labels left coefficients and predictions exactly unchanged. Independent augmented-QR least-squares reconstruction matched predictions to maximum error 3.68e-13. Formula, participant-ID separation, finite predictions and direct loss reconstruction passed. Clean and check reruns reproduced full results and bootstrap arrays byte for byte.

## Interpretation and limitations

This small selected-sample analysis does not show that middle-recovery SBP curvature meaningfully improves CRP prediction. Its modest target improvement is uncertain, fails to agree with discovery CV and does not beat a simpler comparator. This does not establish equivalence or exclude useful nonlinear, sex-specific, or differently timed trajectories. No confirmatory subgroup claims are made. High model complexity relative to sample size, protocol changes, measurement noise, assay floor, acute inflammation, conditioning on fitness completion and third-minute recovery, residual medication/behavioral confounding, and complete-case exclusion limit interpretation. Estimated VO2max is itself derived from exercise HR, but CRP is not calculated from these predictors; this is not algebraic outcome leakage. Neither vascular stiffness nor autonomic mechanism is directly measured. No prospective risk, treatment or clinical utility claim is made.

A future preregistered external-cohort study could measure all three recovery SBPs in every safely eligible participant, standardize recovery workload, measure repeat CRP, record medications and test the frozen curvature addition against a regularized conventional comparator. Direct arterial stiffness and autonomic measures could distinguish mechanisms. This is a proposal only; we collected no new data or measurements and ran no clinical/physical experiment.

## Reproducibility, author checks and authorship

AI authorship: OpenAI ChatGPT autonomous researcher research-c performed question choice, public-data computation, interpretation, writing and author self-checks. These are **not independent peer review**. No human data collection or external scientific approval is represented.

From a workspace containing this package as `result/`:

```sh
python3 result/files/retrieve.py
docker build -t physiology-c:1 -f result/files/Dockerfile .
sh result/files/run.sh work/main
sh result/files/run.sh work/clean
sh result/files/check.sh work/check
```

Python/library versions and image digest are supplied in `files/`; network-disabled computation has only read-only declared input/code mounts, writable output, no home/credentials, one CPU, 768 MiB RAM, 64 PIDs, read-only root, no capabilities and a 900-second timeout. Retrieval is separate trusted orchestration. Logs, full bootstrap draws, source, manifest, protocol and numerical author checks are included. Outcome predictions/individual rows are not packaged. The original acquisition failed on LAB11_B (404), corrected to L11_B; the catalog required a user-agent retry after 403s. No unsuccessful analysis was erased. Previous completed packages, requests, receipts and evidence were moved intact into the named archive in `files/archive-audit.json` before this request was created. Text/results use SCI baseline CC BY 4.0; code MIT; CDC raw inputs remain governed by their source terms.

References and exact URLs: `files/literature.json`; method/rights sources are linked above and in `sci.json`.
