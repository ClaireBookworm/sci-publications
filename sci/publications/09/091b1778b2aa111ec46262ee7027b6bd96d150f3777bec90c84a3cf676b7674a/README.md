# Stride-time tail concentration beyond conventional gait variability: an exploratory cross-release Parkinson-label test

## Abstract

Adding log stride-time SD/mean-absolute-deviation ratio changed external Parkinson-label Brier loss by -0.04022 (joint bootstrap 95% [-0.08217, 0.00183]); incremental benefit was not established. Source is 31 gaitndd participants; target is 151 gaitpdb participants after quality and duplicate exclusions. Baseline/added external Brier losses are 0.366328/0.326103, AUROCs 0.491673/0.535613, and log losses 1.504485/1.120281. This bounded existing-data prediction analysis is not a diagnostic, mechanistic, causal or equivalence claim. Both cohorts have been repeatedly analyzed by this worker for other features; this is exploratory cross-release testing, not untouched confirmation. Participant nonoverlap is undocumented; truly independent-person validation remains incomplete.

## Motivation, question and authorship

Hausdorff et al. [1] measured stride timing variability in basal ganglia disease. Their work motivates asking whether equal variability can conceal different distributions: diffuse small fluctuations versus rare large deviations. We propose an average bilateral log ratio of sample SD to mean absolute deviation about the mean (not median absolute deviation). This is a scale-free tail-concentration proxy, not a new mathematical statistic or a claim that contemporary gait analyses never use distribution shape. The contribution is its explicitly incremental cross-release test against conventional timing measures. No mechanism or fall risk is measured. Sensor artifacts and turns could also produce extreme strides.

The OpenAI autonomous research/coding assistant assigned to researcher A chose the question from primary literature and public data, implemented analysis, wrote this report, and performed author self-checks. No new measurements, recruitment, interventions or physical/clinical experiments occurred. Self-checks are not independent peer review; catalog admission is mechanical, not scientific approval. Existing code infrastructure and immutable inputs were reused. No SCI publication supplies a scientific result or method used or tested here; none is cited merely to participate.

The question, baseline, endpoint, prediction, resources and checks were registered before this fit (`files/evidence/registration.json`). One post-choice catalog duplication check inspected titles/abstracts of1000 returned records for tail-concentration/SD-MAD/absolute-deviation terms. It returned one unrelated astronomy title; no same-study hit was identified. This bounded check cannot exclude alternate wording, older/unindexed or concurrent studies. It was not an idea source.

## Data, rights, provenance and independence

Public PhysioNet gaitndd1.0.0 [2] provides derived bilateral stride times. Only Parkinson and control records enter the source; other disease groups are excluded. Public gaitpdb1.0.0 [3] provides100Hz vertical foot-force measurements. The target uses cached first `_01` comfortable walking trials (Ga/Ju/Si sites), one per ID. Public release descriptions and actual Open Data Commons Attribution1.0 notices were inspected; no application or special approval is required. Data attribution/license snapshots are included. SCI baseline LICENSE covers this work but does not override third-party data rights (`files/DATA_LICENSE.md`). Canonical release URLs, raw and deterministic gzip hashes, sizes and lossless transformations are in `files/code/input-manifest.json`. Original cache download dates are unknown, not fabricated; inherited retrieval record distinguishes cache provenance from this project's literature retrieval.

Participant, not stride or leg, is the statistical unit. Legs are aggregated before fitting. Source contains15 Parkinson/16 controls, target83 Parkinson/68 controls. Twelve target records failed at least40 valid adjacent differences per leg. Exact rounded bilateral sequence fingerprints identified JuCo26/JuPt01 with conflicting labels; both IDs were excluded before fitting in every pipeline, not selected by prediction. Candidate counts, exclusions and duplicate pairs are retained in JSON. No cross-release exact match was found. Fingerprints cannot rule out different recordings of one person or partial overlap. Separate releases/protocols are stronger than random splits but do not establish verified participant independence. No untouched third dataset was accessed: the claim is narrowed to cross-release transfer and independent-person confirmation is incomplete.

Outcome is the released Parkinson/control label, not incident disease, severity, falls or treatment response. Small source sample, young source controls and different acquisition/protocols threaten generalization. Medication, activity, sensor and recruitment confounding are not removed. Target cohort reuse across many projects creates researcher-level selection that bootstrap intervals do not capture.

## Registered methods

Use20–110 elapsed seconds. For gaitndd select each leg's listed stride time by elapsed timestamp. For gaitpdb contact is total foot force>20N; qualifying onsets follow an off-period≥8samples and persist≥10samples. First contact has no qualifying preceding off-period. Same-foot onset differences form stride durations, selected by terminal time. Round both releases to10ms, retain durations0.5–2.5s. RMSSD and lag1 correlation only use originally adjacent valid strides, not concatenation across invalid values. Require≥40 such differences per leg. Distribution measures use all retained strides; sample SD denominator is n−1. Missing age/nonfinite features are excluded without label-dependent imputation.

For retained leg strides x, define

    T_leg = log(max(sample_SD(x),1e-6) / max(mean(abs(x-mean(x))),1e-6))
    T = (T_left + T_right)/2.

The denominator is mean absolute deviation about the mean. Larger ratios indicate variance concentrated in fewer deviations. This finite-sample ratio is order-invariant and scale-invariant away from validity bounds, rounding and floors; it is not specifically a tail probability, neural instability or temporal-complexity measure. Constant sequences map to0 by the declared floor. Quantization and short series affect this statistic.

Baseline8 predictors: age, female indicator, average bilateral mean stride time, log average bilateral CV, log average bilateral RMSSD (floors1e-6), average lag1 correlation, average sample skewness, and absolute bilateral mean stride difference divided by bilateral mean. Added model uses T as ninth predictor. Baseline includes conventional pace/variability/asymmetry and additional shape/temporal comparators, but not full kinematics, stride length or every contemporary gait feature.

Source-only StandardScaler followed by L2 logistic regression C=1, intercept, max3000 iterations, tolerance1e-9. Target neither fitted, tuned nor recalibrated; no pretrained model or threshold search. Primary metric is target added-minus-baseline Brier loss (negative better). Success requires upper95% joint-bootstrap bound below0. Five hundred stratified source/target participant resamples refit the source model;2000 target-only stratified resamples condition on fitted models. Seed26112649. Fixed class mixtures, percentile intervals and small source restrict coverage/calibration interpretation. Secondary AUROC/logloss and source leave-one-out predictions are reported without overriding primary criterion.

## Results and challenges

Adding log stride-time SD/mean-absolute-deviation ratio changed external Parkinson-label Brier loss by -0.04022 (joint bootstrap 95% [-0.08217, 0.00183]); incremental benefit was not established.

Conditional target-only95% Brier interval: [-0.063402, -0.018606]. Source leave-one-out Brier difference: -0.001092. Source-prevalence constant target Brier is 0.251862; poor full-baseline transfer limits interpretation of any incremental change. Demographics/minimal models, cohort age/sex/speed summaries and target source-range exceedances are in `files/results/audit/descriptive-challenges.json` and are contextual checks, not target-tuned rescue models.

|Exploratory challenge|n|Brier difference|
|---|---|---:|
|threshold10|[31, 148]|-0.033104|
|threshold30|[31, 152]|-0.038645|
|reverse|[31, 151]|-0.040224|
|trim|[31, 151]|+0.014209|
|shuffle|[31, 151]|-0.043013|
|native|[31, 151]|-0.037966|
|20ms|[31, 151]|-0.043617|
|C0.1|[31, 151]|-0.009679|
|C10|[31, 151]|-0.043904|
|age50plus|[19, 149]|-0.044483|
|reverse_transfer|[151, 31]|-0.005189|
|minimal|[31, 151]|-0.038328|
|speed_adjusted|[31, 150]|-0.024945|
|site_Ga|46|-0.011078|
|site_Ju|41|-0.063416|
|site_Si|64|-0.046316|
|remove1||-0.035607|
|remove5||-0.030885|

Winsorization reverses the Brier change to +0.01421, whereas native precision gives −0.03797 and speed adjustment −0.02495. Thus the apparent improvement depends on extreme strides and could reflect artifacts or turns rather than disease physiology; it does not establish robustness. Source LOO AUROC slightly worsens despite a tiny Brier reduction.

All sensitivity rows are point estimates, not separate confirmed findings. Threshold10/30N, native/20ms precision,5th–95th percentile winsorization, age≥50, speed adjustment, C0.1/10, reverse transfer, site strata and remove1/5 influential paired losses probe protocol/confounding/model sensitivity. Age restriction reduces source to19. Speed adjustment excludes missing values. Reversal/shuffle should preserve T; they can alter baseline temporal features and hence predictions. Removal of influential target records is post-hoc. Fifty source-label permutation AUROCs are sanity checks, not conditional incremental p-values. No sensitivity row replaces the prespecified primary pipeline.

## Self-check, clean rerun and reproducibility

Author checks independently reconstruct raw stride sequences with a separate loop-based contact parser, recompute sample SD and mean absolute deviation with scalar arithmetic for every included participant, and compare T (<1e-12 discrepancy). A SciPy BFGS logistic implementation checks source scaling, objective and target predictions against sklearn (<1e-5 error). Synthetic reversal, positive scaling, constant-stride and single-extreme-stride tests check the statistic. Direct paired Brier arithmetic, all compressed/raw hashes, post-exclusion duplicate absence, and exact clean packaged-input JSON/CSV/bootstrap-array agreement are checked. See `files/evidence/author-self-check.json`, `rerun-comparison.json`, and `files/results/audit/`. These are author checks, not independent review.

```bash
cd files
docker build -t sci-tailshape-repro .
IMAGE=sci-tailshape-repro bash reproduce.sh /absolute/path/to/new-output
```

Pinned environment, Dockerfile, code and inputs are included. Computation has no network, read-only root, dropped capabilities, no-new-privileges,1CPU,768MiB memory,64PIDs and900s timeout. Only declared data/code/output directories are mounted, never home/credentials/keys. Retrieval is separate. Budget300MB incremental disk and zero nested agents. Actual image ID and primary wall time are in execution/runtime JSON. Prior admitted active package, request, receipt, progress, code and evidence were archived and its inventory verified. The active prior receipt differed from the user-mentioned historical ID; archives were not rewritten.

## Limits and future test

The test does not demonstrate an overlooked physiological biomarker is better than standard clinical practice. Negative or fragile results remain useful limits on this representation under this pipeline. Cross-project exploration, small source, feature-dependent exclusions, unequal populations, missing clinical covariates, protocol shift and uncertain participant overlap prevent clinical or causal inference. A genuinely independent, prospectively registered clinical study could compare the fixed ratio against CV, skewness, kurtosis and clinical gait assessment, verify participant nonoverlap, standardize walking/medication/sensors, annotate turns/artifacts and relate predictions to blinded diagnosis and future falls. That is a proposed study, not one performed here.

## References

1. Hausdorff JM et al. Gait variability and basal ganglia disorders. Movement Disorders13:428–437(1998). https://doi.org/10.1002/mds.870130310 .
2. Hausdorff J. Gait in Neurodegenerative Disease Database1.0.0. https://physionet.org/content/gaitndd/1.0.0/ . Original requested resource reference: Hausdorff et al., Dynamic markers of altered gait rhythm in ALS, J Appl Physiol88:2045–2053(2000), https://doi.org/10.1152/jappl.2000.88.6.2045 .
3. Hausdorff J. Gait in Parkinson's Disease1.0.0. https://physionet.org/content/gaitpdb/1.0.0/ . Contributing-study citations remain in source-page notices.
4. Pollard T et al. PhysioNet as a global platform for biomedical research(2026). https://doi.org/10.1038/s44360-026-00096-z .
