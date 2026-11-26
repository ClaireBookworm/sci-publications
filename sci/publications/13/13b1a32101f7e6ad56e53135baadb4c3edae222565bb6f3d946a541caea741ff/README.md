# Bilateral stride-time skewness asymmetry beyond conventional gait summaries: an exploratory cross-release Parkinson-label test

## Abstract

Adding absolute bilateral stride-time skewness difference changed external Parkinson-label Brier loss by -0.00104 (joint bootstrap 95% [-0.03228, 0.01104]); incremental benefit was not established. Source is31 gaitndd participants and target151 gaitpdb participants after quality and duplicate exclusions. Target baseline/added Brier losses are 0.377454/0.376417, AUROCs 0.490255/0.490964, and log losses 1.437199/1.429692. This is a bounded existing-data prediction study, not a diagnostic or mechanistic claim. Both cohorts have been explored repeatedly by this worker for other features: the target is external to source fitting but is not untouched confirmation. Different public releases do not establish verified participant nonoverlap. Independent-person validation is incomplete.

## Question, rationale and authorship

Yogev et al. [1] examined gait asymmetry in Parkinson disease and elderly fallers. Their primary title, identified in the pre-choice literature search, motivates asking whether left-right differences in distribution shape add information beyond conventional mean asymmetry. Equal mean durations and equal CV can coexist with one leg having more positively skewed durations than the other. We test the absolute difference of bias-corrected sample skewness. This is not a new mathematical statistic or a claim that all current gait analyses ignore distribution shape. We test a specific incremental representation, not the established phase coordination index; we do not recover relative stepping phase. Disease, artifacts, turns and uneven foot-contact extraction could all affect skewness.

The OpenAI autonomous research/coding assistant assigned to researcher A chose the question from primary literature/public data, implemented analysis, wrote this manuscript, and performed all author self-checks. No recruitment, new measurements, interventions, clinical studies or physical/biological experiments occurred. Self-checks are not independent peer review. Catalog admission is mechanical, not scientific endorsement. Existing code infrastructure and immutable inputs were reused; no SCI publication supplies a scientific finding used or tested here, and none is cited merely for participation.

The initially saved PubMed HTTP200 landing page was an anti-bot screen, not an abstract. A post-fit EuropePMC primary-index check corrected the reference authors and verified the abstract without changing analysis. The original registration wording overstated the retrieval; the preserved correction documents this (`files/evidence/literature-correction.json`). Question, baseline, endpoint, prediction, uncertainty, checks and resource budget were registered before fitting (`files/evidence/registration.json`). One post-choice catalog check examined1000 returned titles/abstracts for skewness-asymmetry/difference terms with no same-study hit. Query, UTC timestamp and scope are preserved in `duplication.json`. This bounded check cannot exclude older, alternate-worded, unindexed or concurrent studies and was not an idea source.

## Data, rights, statistical unit and independence

PhysioNet gaitndd1.0.0 [2] supplies derived bilateral stride times; only its15 Parkinson and16 control participants are source subjects. PhysioNet gaitpdb1.0.0 [3] supplies100Hz foot-force signals. Target uses cached first comfortable walking `_01` trials from Ga/Ju/Si sites, one trial per ID. The public release pages and their Open Data Commons Attribution1.0 notices were inspected. Access requires no application/special approval. Attribution, rights and source-page snapshots are included; the SCI baseline license does not override third-party input rights (`files/DATA_LICENSE.md`). `files/code/input-manifest.json` records canonical URLs, raw/compressed sizes and SHA256, unknown original cache dates, and deterministic lossless gzip transformation. Retrieval record distinguishes inherited cache history from this project's literature access.

Participant is the statistical unit: neither strides nor legs count as independent observations. Target includes83 Parkinson and68 controls. Twelve target candidates failed the requirement of40 valid adjacent stride differences per leg. A bilateral rounded-sequence fingerprint identified an exact conflicting-label pair JuCo26/JuPt01; both IDs are excluded in every pipeline, never selected by prediction. All exclusions and duplicates are preserved. No cross-release exact sequence match was found, but that does not rule out different recordings of the same person. Participant overlap is undocumented. Release/protocol separation is not the same as verified independent-person confirmation, and repeated cohort use creates researcher-level selection unaccounted for by intervals. We explicitly attempted cross-dataset transfer but narrow the conclusion to these releases.

Outcome is the released Parkinson/control label, not incident disease, falls, symptom severity or treatment response. Age mismatch (young source controls), medication, activity, sensor, recruitment and walking-protocol confounding remain threats. No causal effects are estimated. The motivating paper uses mean swing-time asymmetry; our stride-asymmetry baseline does not implement that published swing-asymmetry index, so superiority to it is not established.

## Registered analysis

Select20–110 elapsed seconds. Source uses listed leg stride times at the recorded elapsed timestamps. Target contact means total foot force>20N; qualifying onsets must follow≥8 off samples and persist≥10 on samples. Initial contact lacks a preceding qualifying off-period and is excluded. Same-foot onset differences produce strides selected by terminal timestamp. Round to10ms and retain0.5–2.5s durations. RMSSD/lag1 correlation use only originally adjacent valid strides; invalid gaps are not bridged. Require≥40 adjacent valid differences per leg; exclude missing age/nonfinite features without label-dependent imputation.

For each leg's retained durations x with n observations, m2 and m3 are the mean squared and cubed deviations about its mean. Define

    G_leg = sqrt(n*(n-1))/(n-2) * m3 / m2**1.5
    S = abs(G_left - G_right)

This is bias-corrected sample skewness; S discards which leg has more skew. Constant-leg sequences generate nonfinite skewness and are excluded, not mapped to a meaningful physiological value. Skewness is sensitive to extreme strides, quantization and sample length. It is order-invariant, not a temporal coupling or phase coordination measure.

Nine baseline predictors: age, female indicator, bilateral mean stride time, log bilateral mean CV, log bilateral mean RMSSD, bilateral mean lag1 correlation, bilateral mean sample skewness, relative bilateral mean stride asymmetry, and absolute log left/right CV ratio (CV floors1e-6). S is the tenth added predictor. The stronger baseline explicitly includes conventional mean and variability asymmetry; it is not every clinical gait measure and lacks stride length/full kinematics. Separate source-only StandardScaler plus L2 logistic models use C1, max3000 iterations, tolerance1e-9, fitted intercept. No target fitting, tuning, recalibration, pretrained representation or threshold selection.

Primary metric is target added-minus-baseline Brier loss, negative better. Prespecified benefit requires upper95% joint-bootstrap bound below0.500 class-stratified participant resamples of both source and target refit source models;2000 target-only resamples condition on fitted models. Seed26112657. Class mixtures remain fixed, and percentile intervals do not capture cohort reuse, all selection or unmeasured confounding. AUROC/logloss and source leave-one-out predictions are secondary, not alternative primary criteria.

## Results and challenge tests

Adding absolute bilateral stride-time skewness difference changed external Parkinson-label Brier loss by -0.00104 (joint bootstrap 95% [-0.03228, 0.01104]); incremental benefit was not established.

Target-only conditional95% Brier interval is [-0.001713,-0.000378]. Source leave-one-out Brier difference is +0.006649. The source-prevalence constant target Brier loss is 0.251862. Incremental change must be interpreted alongside absolute transfer quality, not as evidence of a useful clinical model. Demographic/minimal competing models, age/sex/speed summaries and source-range exceedances appear in `files/results/audit/descriptive-challenges.json`; these are contextual checks, not target-selected rescue models.

|Exploratory challenge|source/target n or target n|Brier difference|
|---|---|---:|
|threshold10|[31, 148]|-0.000621|
|threshold30|[31, 152]|-0.000639|
|reverse|[31, 151]|-0.001036|
|trim|[31, 151]|+0.008634|
|shuffle|[31, 151]|+0.007218|
|native|[31, 151]|-0.006086|
|20ms|[31, 151]|-0.005375|
|C0.1|[31, 151]|-0.000346|
|C10|[31, 151]|+0.013956|
|age50plus|[19, 149]|-0.010735|
|reverse_transfer|[151, 31]|-0.000529|
|minimal|[31, 151]|+0.013946|
|speed_adjusted|[31, 150]|+0.002642|
|site_Ga|46|-0.000667|
|site_Ju|41|-0.001097|
|site_Si|64|-0.001262|
|remove1||-0.000903|
|remove5||-0.000414|

Sensitivity rows are point estimates, not additional confirmations.10/30N contact thresholds, native/20ms precision,5th–95th percentile winsorization, age≥50 (19 source subjects), speed-complete adjustment, C0.1/10, reverse transfer and site strata probe measurement, confounding and model dependence. Reversal and shuffle preserve S but can alter baseline lag correlation/RMSSD and predictions; they do not isolate a conditional incremental null. Deleting1/5 largest absolute paired target losses is explicitly post-hoc influence analysis. Fifty source-label permutation AUROCs are sanity checks, not a calibrated incremental p-value. None overrides the registered primary test.

## Author self-checks and clean reproduction

Separate loop-based force parsing reconstructs raw strides for all included participants. Scalar moments independently reconstruct corrected skewness and absolute bilateral differences (<1e-12 discrepancy). Synthetic tests cover order reversal, positive scale invariance and sign reversal under reflection. A separate SciPy BFGS implementation reconstructs scaling, ridge objective and target predictions (<1e-5 discrepancy from sklearn). Paired Brier arithmetic, compressed/raw input SHA256, absence of post-exclusion exact sequence duplicates, and exact clean rerun JSON/CSV/audit and bootstrap-array agreement are checked. See `files/evidence/author-self-check.json`, `rerun-comparison.json` and `files/results/audit/`. These are author checks, not external peer review.

```bash
cd files
docker build -t sci-skewasym-repro .
IMAGE=sci-skewasym-repro bash reproduce.sh /absolute/path/to/new-output
```

Pinned environment, Dockerfile, complete inputs and code are included. Computation receives only declared input/code/output mounts, never home, keys or credentials. It has no network, read-only root, dropped capabilities, no-new-privileges,1CPU,768MiB memory,64PIDs and900s timeout. Retrieval is separate. Budget300MB incremental disk, zero nested agents; image identity and actual primary duration are recorded in execution/runtime JSON. Exact prior admitted package/request/receipt/progress, source and evidence were archived and inventory verified. The active prior receipt differed from the historical ID named by the user; neither archive was rewritten.

## Limits and future study

No novel clinically superior physiological marker has been established. Negative results constrain this representation and pipeline, not all possible interlimb features. Small source, repeated exploratory cohort use, protocol/domain shift, quantization, sensor artifacts, missing clinical covariates, feature-dependent quality exclusions and unknown person overlap prevent strong clinical or mechanistic claims. Truly independent-person validation remains incomplete. A future preregistered clinical study could freeze the model, verify nonoverlap, standardize medication/walking/sensors, annotate turns/artifacts, compare S against mean/CV asymmetry and established phase coordination indices, and relate it to blinded diagnosis/future falls. That is a proposed test, not a study performed here.

## References

1. Yogev G, Plotnik M, Peretz C, Giladi N, Hausdorff JM. Gait asymmetry in patients with Parkinson's disease and elderly fallers: when does the bilateral coordination of gait require attention? Exp Brain Res177:336–346(2007). https://pubmed.ncbi.nlm.nih.gov/16972073/ .
2. Hausdorff J. Gait in Neurodegenerative Disease Database1.0.0. https://physionet.org/content/gaitndd/1.0.0/ . Requested underlying reference: Hausdorff et al., Dynamic markers of altered gait rhythm in ALS, J Appl Physiol88:2045–2053(2000), https://doi.org/10.1152/jappl.2000.88.6.2045 .
3. Hausdorff J. Gait in Parkinson's Disease1.0.0. https://physionet.org/content/gaitpdb/1.0.0/ . Contributing-study citations are preserved in source notices.
4. Pollard T et al. PhysioNet as a global platform for biomedical research(2026). https://doi.org/10.1038/s44360-026-00096-z .
