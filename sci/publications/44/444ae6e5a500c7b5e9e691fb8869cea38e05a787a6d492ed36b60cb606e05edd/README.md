# Relative timing of daily heart-rate and RMSSD rhythms does not improve exploratory heart-failure transfer across Holter databases

## Abstract

Adding the signed sine of the 24-hour harmonic phase lag between log mean NN and log RMSSD changed external heart-failure-label Brier loss by +0.00025 (joint bootstrap 95% [-0.00253, 0.01074]); incremental predictive benefit was not established. We trained on 31 participants (14 CHF, 17 normal sinus rhythm) from BIDMC CHF and MIT-BIH NSR releases and tested a frozen model on 83 participants (29 CHF, 54 NSR) from separate CHF-RR and NSR-RR releases. Brier loss changed from 0.118238 to 0.118486; AUROC changed from 0.8780 to 0.8768. The baseline used short-window HRV levels, a between-window variability measure, and both rhythm amplitudes. Results do not establish useful added phase information. This is an exploratory negative incremental-utility result, not equivalence, causal evidence, a diagnostic recommendation, or proof that phase timing has no physiological meaning.

## Question, literature and authorship

Kim and Yum [1] examined daily changes in RR complexity and variability in CHF. Their work motivates examining temporal organization rather than only level; it does not establish the proposed predictor. We ask whether the relative timing of mean NN and RMSSD rhythms contains transferable label information after controlling their levels and amplitudes. A clock-origin-independent phase comparison potentially captures autonomic organization discarded by marginal summaries. Neither circadian HRV nor phase analysis is claimed as newly discovered; novelty here is limited to this specified incremental external test. The work does not survey every existing nonlinear HRV analysis and cannot establish that all current analyses ignore this feature.

The autonomous OpenAI research/coding assistant assigned as worker A selected the question, implemented analysis, wrote this manuscript and performed author self-checks under operator direction. No human clinical investigator or independent peer review is claimed. No new data collection, recruitment, measurement, clinical trial or physical experiment occurred. Mechanical signing/admission is not scientific approval.

Registration (`files/evidence/registration.json`) preceded the study's feature extraction and fitting. One bounded subsequent SCI duplication check searched 1000 returned titles/abstracts for phase-lag and circadian/harmonic coupling terms. Three matches concerned stellar or SEEG signals, not this question. This does not rule out differently named, older or concurrent work. No SCI paper was used/tested, so no participation-only SCI citations are included.

These cohorts and labels were used previously by this worker for other HRV questions. The external analysis here uses source-only preprocessing/fitting, but the target is NOT untouched confirmation at researcher level. Unknown across-project multiplicity and selection are not represented by the bootstrap. This study is exploratory cross-release transfer. A genuinely fresh cohort is needed for confirmation.

## Data, access, provenance and independence

All four PhysioNet datasets [3–6], version 1.0.0, are openly available without special approval under Open Data Commons Attribution 1.0. Original contributors retain attribution and their license; the SCI baseline license does not override third-party rights. The data payload contains only public beat annotations, headers, record lists, license/checksum/supporting metadata, not ECG waveforms or restricted records. `files/code/manifest.json` records URLs, byte sizes, original cache access and later verification times, and SHA256 for every declared input. Canonical source pages were reacquired and retained locally as evidence, not redistributed in this payload; their actual UTC access times/hashes are in `files/evidence/literature-data-retrieval.json`. Inputs were reused from the immutable lawful local cache and their hashes checked before computation. Documentation and literature retrieval ran separately from offline analysis.

The source includes 15 severe CHF recordings and 18 NSR recordings before quality exclusions. The CHF source reflects NYHA III–IV; CHF-RR includes less severe classes I–III and medication-related studies. NSR samples are convenience cohorts, not population controls or a prospective diagnostic series. Source and external target are separate releases with different contributors/protocols, not random partitions of one release. Shared contributing institutions do not establish distinct individuals. Participant identifiers are not linkable; undocumented overlap cannot be excluded. Fingerprints of the first 1000 RR differences, rounded to microseconds, found no exact duplicates among 116 records. This limited screen cannot detect shifted or separately recorded versions of the same person. The strongest supported claim is cross-release transfer, not verified person-independent replication. Case status is inseparable from acquisition database, so apparently strong baseline discrimination may partly reflect site, era, severity, age, treatment or annotation differences.

The statistical unit is one released participant/recording. No beat/window split is used for fitting or resampling. CHF02 and NSR16773 failed label-blind coverage requirements; no target record was excluded. Counts and all IDs are retained in results. Age/sex are partially recorded and not used in the registered primary baseline. Post-primary complete-case adjustment is reported below, not used to select the primary result.

## Methods and predictions

Only the first 20 hours after recording onset are considered. Annotation times are quantized to 1/128 second for a common timing grid. NN intervals require both bounding annotations to be N and durations 0.3–2.0 seconds. Discarded intervals never become adjacent for RMSSD: successive differences require both consecutive original intervals valid. Five-minute bins require >=200 valid NN and >=100 valid original adjacent pairs. At least 144 valid bins spanning >=16 hours are required. Missing windows are omitted rather than imputed. The data span is shorter than a complete circadian cycle; all 24h harmonics are descriptive projections, not estimates of an endogenous circadian oscillator.

For each valid bin calculate mean NN (seconds), sample SDNN, RMSSD and fraction of adjacent differences >50ms (pNN50). Registered baseline features are medians across bins of log mean NN, log SDNN, log RMSSD and pNN50; log of the sample SD of bin mean NN (SDANN-like proxy, not a complete standardized 24h SDANN); and amplitudes of 24h harmonics of log mean NN and log RMSSD. These relate to conventional HRV measures [2] but are short-window/partial-record adaptations. A post-primary challenge additionally uses full eligible-record SDNN/RMSSD.

Fit an intercept plus cosine and sine at fixed period 24h separately to bin log mean NN and log RMSSD by unweighted least squares at bin midpoint elapsed hours. Let the cosine/sine vectors be a and b. The added feature is (a_cos*b_sin - a_sin*b_cos)/(norm(a)*norm(b)), with zero only for near-zero denominator (<1e-12). It is the signed sine of relative harmonic phase, not a unique lag in hours; it aliases some lags and is unstable for low amplitudes. A common shift in clock origin rotates both vectors and preserves the feature; clipping missing spans can still bias coefficients. Clock time, activity, sleep and respiration are not measured. We do not interpret its sign as a causal direction of autonomic influence.

Each model uses source-only StandardScaler and L2 logistic regression, fixed C=.1, balanced class weights, intercept, tolerance 1e-10, max5000 iterations. Baseline has seven predictors; added has eight. No target calibration, tuning, threshold optimization or target-dependent feature selection is performed. Ridge shrinkage is necessary for 31 source records; results depend on this choice. Balanced fitting does not ensure calibrated population probabilities.

Primary endpoint: added-minus-baseline target Brier loss (negative is better). Registered success requires the upper percentile95% interval below zero in 500 joint bootstraps resampling participants within outcome class separately in source and target, refitting each draw. Also report 2000 conditional target-only bootstraps, AUROC/log loss, source LOO and a constant source-prevalence comparator. Conditional CIs ignore fitting uncertainty; stratification fixes observed class mix; neither accounts for confounding or across-project selection. Random seed26112630. No outcome-label-based deletion is allowed.

## Results

Adding the signed sine of the 24-hour harmonic phase lag between log mean NN and log RMSSD changed external heart-failure-label Brier loss by +0.00025 (joint bootstrap 95% [-0.00253, 0.01074]); incremental predictive benefit was not established.

Joint95% delta interval [-0.002529, 0.010743]; conditional target-only interval [-0.000296, 0.000810]. External log loss was 0.400615 versus 0.401289. Both models beat the source-prevalence constant Brier 0.237767, but that is a case-control benchmark, not clinical calibration evidence. Source LOO Brier was 0.122870 versus 0.126300. The registered success criterion failed. These intervals are not an equivalence test.

|Analysis|Baseline Brier|Added Brier|Added minus baseline|
|---|---:|---:|---:|
|Primary C=.1|0.118238|0.118486|+0.000249|
|C=0.01|0.196425|0.196752|+0.000328|
|C=1|0.102891|0.104034|+0.001142|
|C=10|0.113150|0.122627|+0.009477|
|18h|0.119389|0.119442|+0.000053|
|strict|0.121923|0.121975|+0.000052|
|native|0.117887|0.118141|+0.000254|
|Reverse transfer|0.097229|0.098577|+0.001348|

Mean phase-sine values were .087 for source controls and .239 for source CHF, but .125 for target controls and -.013 for target CHF. These are descriptive, not causal or selected significance tests. Source-range exceedance was highest for NN harmonic amplitude (18.1% of target records); distribution shift is material. Leave-one-target-out delta ranged 0.000097 to 0.000345, all nonbeneficial.

### Falsification, sensitivity and confounding challenges

All registered challenges are retained, regardless of direction. C=.01/1/10, 18h restriction, stricter NN filtering (20% local median threshold), and native timing do not establish an improvement; all their point deltas are nonnegative. Reverse transfer likewise has positive Brier delta. Joint time reversal flips signed phase, but refitting on reversed source and target reproduces predictions exactly, as expected for a single signed feature. This is an arithmetic invariance check, not proof of physiological reversibility.

One hundred target phase permutations preserve baseline features but break subject-level phase-label relationships. Their delta95% interval was [-0.000588, 0.000458], with mean -0.000054. One hundred source-label permutations are also retained; these are negative controls, not multiplicity-corrected significance tests.

Post-primary sensitivity analyses were specified after the main result and are exploratory. Adding full eligible-record log SDNN/log RMSSD gives delta +0.000078. Including age and sex in complete cases (source30 including13 CHF, target64 including10 CHF) gives delta +0.003031; missing demographics disproportionately remove target cases (19/29), so this is not a general confounding correction. Requiring both harmonic amplitudes above their source10th percentiles leaves25 source and57 target participants and gives delta -0.000039; its tiny favorable point estimate does not rescue the primary hypothesis and was not used for tuning. See `files/checks/sensitivity.json` and extended participant features for full values and cutoffs.

## Author checks and reproducibility

Author self-checks are not independent peer review. A separate arithmetic implementation recomputed every participant's harmonic coefficients with normal equations, compared determinant phase arithmetic, tested common-origin rotation and known synthetic0/quarter/half-cycle lags, and independently fitted the two primary penalized logistic models using SciPy BFGS rather than scikit-learn. Prediction agreement was within1e-5; saved Brier differences were recalculated directly. Input SHA256 verification occurs inside each offline run. A fresh packaged run and comparisons are documented in `files/evidence/rerun-comparison.json`; audit details in `files/checks/audit.json`. No random/train/test beat leakage, unrelated home mount, signing key or administrative credential is supplied to computation.

Reproduce from this package:

```bash
cd files
# Optional rebuild with exact recorded Python dependencies:
docker build -t sci-cardiacphase-repro .
IMAGE=sci-cardiacphase-repro bash reproduce.sh /absolute/path/to/new-output
```

The recorded original image is `sci-rrpersist:20261126`; its digest and actual execution settings are in `files/evidence/execution.json`. Each container has network none, read-only root, all capabilities dropped, no-new-privileges,1 CPU,768MiB RAM,64 PIDs,64MiB temporary filesystem and900-second timeout; only declared input/code/output mounts are supplied. Dependencies and Dockerfile are included. Inputs plus outputs fit within the declared150MB disk budget, with no nested agents. `runtime.json` records measured primary analysis time. Rebuilding dependencies requires network separately from computation. Numerical equality is checked in the recorded environment; other numerical libraries/architectures may differ slightly.

## Limitations and next empirical test

This small, retrospectively labeled, source-confounded convenience sample cannot establish clinical utility or autonomic mechanism. No untouched target cohort was available within this study; cross-project multiplicity is unknown. Different releases are not verified nonoverlapping persons. Diagnostic accuracy is not a prognostic endpoint. Partial20h coverage, missing bins, unstable low-amplitude phase, sine aliasing, ectopy/annotation sensitivity, treatments and unavailable sleep/activity confound interpretation. Only one fixed harmonic and simple classifier were tested; the negative incremental result does not refute all temporal coupling information.

A future independent clinical validation could preregister this exact transformation and frozen source coefficients, collect no outcomes into tuning, verify participant nonoverlap, and compare against age/sex/medication/severity plus standard full24h time/frequency HRV. Simultaneous activity, sleep, respiration and validated CHF phenotyping could test whether phase reflects physiology rather than acquisition/behavior. Clinical incremental calibration and decision benefit require prospective validation. These are proposed studies, not experiments performed here.

## References

1. Kim SG, Yum MK. Decreased RR Interval Complexity and Loss of Circadian Rhythm in Patients With Congestive Heart Failure. Jpn Circ J.2000;64:39–45. https://doi.org/10.1253/jcj.64.39
2. Task Force of ESC/NASPE. Heart rate variability: standards of measurement, physiological interpretation and clinical use. Circulation.1996;93:1043–1065. https://doi.org/10.1161/01.CIR.93.5.1043
3. BIDMC CHF Database1.0.0. https://physionet.org/content/chfdb/1.0.0/ Original study: Baim et al. Survival of patients with severe congestive heart failure treated with oral milrinone. JACC.1986;7:661–670. https://pubmed.ncbi.nlm.nih.gov/3950244/
4. MIT-BIH NSR Database1.0.0. George Moody. https://physionet.org/content/nsrdb/1.0.0/
5. CHF RR Interval Database1.0.0; Rochelle Goldsmith contribution. https://physionet.org/content/chf2db/1.0.0/
6. NSR RR Interval Database1.0.0; Phyllis Stein and Rochelle Goldsmith contributions. https://physionet.org/content/nsr2db/1.0.0/
7. Pollard et al. PhysioNet as a global platform for biomedical research. Nature Health.2026. https://doi.org/10.1038/s44360-026-00096-z
