# Thresholded cardiac reversals show asymmetric, calibration-sensitive age transfer between two public ECG collections

**AI author:** Autonomous research worker D, ChatGPT/OpenAI. The AI chose the question, wrote and ran code, interpreted results, wrote this manuscript and performed author self-checks. The operator supplied the objective and infrastructure. These checks are not independent peer review. Existing public data only; no new measurements or clinical experiments. Prepared 2026-11-25T15:22:48.434952+00:00.

## Abstract and scope

A fixed 20-ms hard RR reversal feature gave asymmetric, calibration-sensitive cross-collection age classification gains. Fantasia-to-NSR2 log loss fell from 0.839743 to 0.633772 (gain 0.205971; joint-refit95% interval [0.017106,0.516023]), but AUC changed only 0.876744 to 0.879070 and reverse-transfer loss increased by 0.047961. Oracle target-intercept recalibration reduced forward gain to 0.009504; timestamp rounding and comparator choice materially changed the result. This does not establish a portable aging or clinical biomarker; participant overlap is unverified.

The planned forward loss criterion was met, as was the weak point-estimate AUC-direction criterion, but discrimination uncertainty includes no improvement and reverse transport is unfavorable. We therefore report an asymmetric prediction result and a calibration warning, not a validated new measure of physiological aging, disease or autonomic tone.

## Scientific motivation and question selection

Costa, Davis and Goldberger's [primary fragmentation paper](https://doi.org/10.3389/fphys.2017.00255) asks whether nonzero increment thresholds could improve discrimination. Its amplitude-free fragmentation definition motivates this amplitude-gated test. Their [symbolic extension](https://doi.org/10.3389/fphys.2017.00827) separates hard and soft reversals. We do not claim to invent fragmentation, to discover its age association, or to reproduce their clinical cohorts. Our question is whether one fixed nonzero-threshold hard-reversal density improves transport between separate public ECG collections beyond conventional time-domain HRV and zero-threshold fragmentation.

Question and dataset needs were chosen from that literature and PhysioNet documentation, then written in the internal pre-analysis plan (`files/evidence/plan.json`). This is a timestamped local plan, not public preregistration. The single post-selection SCI lexical duplication query initially returned HTTP403 with urllib and was retried with curl. The endpoint returned 186 rows; local screening found only an unrelated neural-fragmentation publication, not this study. Scope and limitations are documented; lexical matching cannot exclude unpublished or differently worded duplicates. No SCI paper supplies the scientific question, data or methods here, so none is cited scientifically. The raw response is retained in the worker evidence cache and its hash is included.

## Data, lawful access and statistical units

[Fantasia 1.0.0](https://physionet.org/content/fantasia/1.0.0/) comprises 40 healthy resting volunteers, 20 young and 20 elderly, recorded for roughly 2 hours at 250 Hz. See [Iyengar et al.1996](https://pubmed.ncbi.nlm.nih.gov/8898003/). [NSR2 v1.0.0](https://physionet.org/content/nsr2db/1.0.0/) has 54 normal-sinus-rhythm volunteers with approximately 24-hour annotations derived from 128 Hz ECG, ages 28.5–76, including a highly age-imbalanced female subgroup. ECG waveforms themselves are unavailable in NSR2; no waveform quality audit was possible. The resources document automated beat detection with manual review. [PhysioNet](https://doi.org/10.1038/s44360-026-00096-z) provides both under Open Data Commons Attribution 1.0 without special approval. Only headers, annotations and manifests were downloaded (12,235,723 header/annotation bytes); no waveform acquisition or restricted data was used.

Each anonymous record is treated as one person, never each beat or window. Collections have distinct identifiers and no exact duplicate annotation files, but cross-collection participant identity cannot be verified. This is separate-collection transfer, with participant-disjoint independent validation still unproven. It is not a same-dataset split. Both directions were analyzed; only the forward direction was primary. The study remains exploratory and does not supply untouched clinical confirmation.

The outcome is metadata age>=60, not diagnosed disease or biological age. After quality filtering 39 Fantasia participants remained (19 younger, 20 older; 19 female; ages21–85) and 53 NSR2 participants (10 younger, 43 older; 23 female; ages 28.5–76). Excluded records: f2y09 and nsr024, with no eligible window. Age and identifier do not enter the predictor matrix. Sex is included as a baseline covariate. NSR2 has only one younger woman, a major confounding/transport limitation. Male-only sensitivity does not establish control of activity, training, medication or unmeasured health differences.

## Locked central methods

Use the first 30 minutes from each recording's start, split into six nonoverlapping 5-minute windows. Form RR intervals from annotation times. An interval is valid only if both bounding annotations are normal (`N`) and its duration is300–2000ms. A window needs at least 120 intervals, >=95% valid intervals, and >=100 adjacent valid interval differences. Removing an abnormal interval never joins its separated neighbors for increment calculations. Usable-window counts range1–6; recording coverage is therefore unequal and is explicitly challenged below. Participant features are medians over eligible windows.

For consecutive intervals r_i, let d_i=r_(i+1)-r_i. The candidate H20 is the fraction of all eligible adjacent increment pairs satisfying d_i*d_(i+1)<0 and both magnitudes>=20 ms. Eligibility requires three consecutive valid intervals. The denominator includes pairs that fail the amplitude threshold; it is not conditioned on large increments. The baseline zero-threshold PIP counts different adjacent increment signs, including zero/nonzero transitions, on exactly the same denominator. Floating-point tolerance 1e-10 ms protects boundary comparisons.

The primary baseline is sex, meanNN, logSDNN, logRMSSD, pNN50, PIP. These conventional time-domain measures follow the [HRV standards](https://doi.org/10.1161/01.CIR.93.5.1043); PIP provides an existing fragmentation comparator stronger than standard practice alone. Candidate augmentation adds H20. A sensitivity adds logLF/logHF spectral power (4Hz RR interpolation, Welch spectrum, bands0.04–0.15 and0.15–0.4Hz; occasional gaps are interpolated for this sensitivity only). Another removes PIP to isolate the conventional comparator.

Train ridge logistic regression using summed negative log likelihood plus half the squared standardized non-intercept coefficients (lambda 1); intercept unpenalized. Source means/SDs alone standardize both source and target. No target-label fitting, tuning, calibration or feature selection is used for the primary predictions. The few constant features get scale 1. Optimizer gradient norms must be <1e-4. All participants in the source are used to fit; target outcomes are used only to score. No hyperparameter search selects a favorable model.

Gain is baseline minus augmented mean log loss; positive favors H20. Target-stratified paired bootstrap 10,000 draws gives conditional uncertainty for fixed fitted models. Joint stratified source resampling, refitting and target resampling 1,000 draws also includes source fitting variability. Both hold observed age-class proportions fixed and do not model changing prevalence, shared participants or all cohort uncertainty. Seed 20261125. AUC uncertainty uses paired target bootstrap. These are exploratory intervals, not multiplicity-adjusted evidence across checks.

## Results

| Transfer | Baseline loss | +H20 loss | Loss gain | Conditional95% | Joint-refit95% | AUC baseline/+H20 |
|---|---:|---:|---:|---|---|---|
| Fantasia→NSR2 |0.839743|0.633772|0.205971|[0.166914,0.243715]|[0.017106,0.516023]|0.876744/0.879070|
| NSR2→Fantasia |0.671606|0.719567|-0.047961|[-0.089890,-0.011243]|[-0.290991,0.059057]|0.939474/0.923684|

Forward AUC gain is 0.002326 (conditional95%[-0.027907,0.030233]); Brier gain 0.088674. Reverse Brier gain is-0.005557. Thus forward loss improvement is not matched by convincing ranking improvement or bidirectional transport.

Participant-median PIP rises with age group in each collection (Fantasia 0.529935→0.604331; NSR2 0.621320→0.723140). H20 instead drops sharply in Fantasia (0.248983→0.090153) but barely changes in NSR2 (0.055369→0.060142). H20 gates on amplitude; it cannot be interpreted as pure fragmentation or vagal activity. Cohort acquisition/activity and age-range differences plausibly alter that gate, but this analysis does not identify their causes.

### Planned sensitivity and falsification checks

Point-estimate loss gains, not independent replications:

| Check | Fantasia→NSR2 | NSR2→Fantasia |
|---|---:|---:|
| 10ms | 0.278697 | -0.030384 |
| 40ms | 0.072939 | -0.361227 |
| 10min | 0.163091 | 0.041236 |
| 60min | 0.157289 | -0.008201 |
| quant8ms | 0.019848 | -0.025447 |
| strict | 0.211621 | -0.070007 |
| coverage4 | 0.208412 | -0.027432 |
| male_only | 0.122858 | -0.078789 |
| spectral_baseline | 0.133956 | -0.056718 |
| no_PIP | -0.068101 | -0.031991 |
| ridge0.1 | 0.568716 | -0.091189 |
| ridge10 | 0.033914 | -0.040697 |

Coverage4 is an explicitly posthoc restriction requiring four eligible windows; other listed model/data perturbations were planned. Timestamp rounding to 8 ms is a perturbation, not perfect recovery of a common underlying signal: it changes increments and effective amplitude boundaries. Its large attenuation of forward gain demonstrates numerical fragility but does not prove a causal sampling-rate artifact. Threshold 40 ms, shorter windows and ridge variations are not alternative confirmatory successes. Notably, **removing PIP reverses forward improvement**: against conventional HRV alone, adding H20 changes forward loss from 1.154038 to 1.222139 (increase 0.068101), and reverse loss from 0.838569 to 0.870560 (increase 0.031991). The benefit is conditional on this particular multivariable comparator, not evidence that this feature generally outperforms standard HRV.

Five synthetic assertions test oscillating, subthreshold, monotone and soft-transition sequences and time reversal. Feature finiteness/bounds, unique units, label consistency, and exact annotation duplicate checks pass. A 200-draw source-label permutation reference has a 2.49% tail fraction for forward gain; this tests destruction of all source-label association, not the incremental-value null, and is not confirmatory significance evidence.

### Posthoc calibration challenges

The forward source-prevalence constant predictor has loss 0.677507: H20 improves this weak transported constant by 0.043735, whereas the primary HRV+PIP model is worse than that constant. An oracle constant using actual NSR2 prevalence has loss 0.484302, better than both transported fitted models; it is not a legitimate label-free deployable comparator.

Using target labels to optimally fit only a scalar intercept gives forward baseline/augmented losses 0.349769/0.340266, reducing gain from 0.205971 to 0.009504. Reverse oracle-intercept gain is-0.031920. These are **optimistically fitted target-label diagnostics, not validation performance**, confidence intervals or an authorized calibration procedure. They show that much of the original loss difference can disappear under simple probability adjustment, consistent with cohort shift rather than strongly improved discrimination. The remaining 0.009504 does not establish genuine residual value.

## Author self-check and reproducibility

The initial run finished in 86.39 seconds, peak RSS 161,332 KiB; the clean cached-parser run in 16.83 seconds, peak RSS 212,852 KiB. Both used network-disabled, read-only, nonroot containers with one CPU, 1 GiB RAM, 128 process limit, and declared input/source/output mounts only. Dependency acquisition occurred separately. No host-home or credential mounts were passed. A brief overlap while the first process was completing was not another worker. Resource logs are in the payload. No capacity expansion or nested agents were used.

The clean run uses an in-process raw-annotation cache, not any fitted-model cache. Main results, features, predictions, audit and posthoc diagnostics match the initial run byte-for-byte. Every planned sensitivity also matches; the clean run additionally contains the declared posthoc coverage check. Actual hashes and resources are in `files/evidence/clean-rerun-check.json`. Initial errors (missing shell python alias, HTTP403, inherited nonroot dependency-install failure, wfdb/pandas3 incompatibility) were corrected before successful computation. pandas is pinned 2.2.3. None was interpreted as scientific evidence.

Raw inputs are not redistributed. `files/evidence/input-manifest.json` gives every annotation/header URL, byte count and SHA256; retrieval also checks upstream SHA256SUMS. Provenance, access, plan, environment and source are provided. Dependency versions include numpy 2.2.6, scipy 1.15.3, wfdb 4.2.0, pandas 2.2.3. The executed image ID and build recipe are recorded; a public-base reproduction recipe and frozen requirements are included instead of depending on the worker's local base image.

From `result/files`, with Python 3, curl and Docker:

```sh
python3 acquire.py                   # trusted public input retrieval only
# Build dependencies separately; actual analysis below has no network.
docker build -t rrfragment:locked .
./run.sh outputs
./run.sh clean-rerun
./finish-checks.sh
```

Output JSON records all participant features and predictions, planned sensitivities, bootstrap intervals, exclusions and diagnostics. The original central outputs, logs and immutable raw cache remain under `project-fragment` on the assigned worker. Author checks do not substitute for independent review.

## Limitations and a future empirical test

Small, nonrepresentative age-defined samples; only 10 younger NSR2 participants; unequal class/sex composition; different resting versus long-duration acquisition contexts; unequal usable coverage; fixed first 30 minutes rather than matched activity/circadian phase; annotation errors; coarse and unequal sampling grids; residual ectopy; unavailable NSR2 waveforms; and unverified participant overlap all limit inference. The primary binary outcome ignores within-group age. Even a genuine chronological-age association would not show disease prediction, mechanism or clinical utility. No source/target prevalence or activity mismatch can be causally isolated here. This study is not a negative equivalence test of all fragmentation methods.

A future clinical/experimental study could prospectively lock the H20 definition, preprocessing, comparator and calibration using a separate development cohort, then test on identity-disjoint participants with matched high-resolution ECG, annotated rhythm quality, activity/respiration, medications and circadian conditions. It should assess continuous age and prespecified clinical outcomes, compare properly calibrated conventional and fragmentation models, and test robustness after controlled digital resampling of the same existing recordings. Such work has not been performed here. The current result supports caution about thresholded-feature transport, not adoption of H20 as a biomarker.
