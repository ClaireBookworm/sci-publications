# Harmonic-noise discordance does not improve exploratory cross-cohort Parkinson voice prediction

## Abstract
Squared harmonic-noise discordance did not establish improved Parkinson prediction from Oxford UCI174 to independent Istanbul UCI301: external Brier change 0.002316, joint bootstrap95% interval [-0.009562, 0.042714]. Both datasets were previously explored; this is not untouched confirmation. External AUROC was 0.5775 for the conventional-feature baseline and 0.5675 after augmentation. Both Brier scores exceeded the balanced-target constant0.5 benchmark of0.25. This is an informative negative/inconclusive representation test, not a diagnostic validation, an equivalence result, or evidence of a newly established physiological biomarker.

## Question, origin and prospective specification
Little and colleagues [1] study conventional perturbation/noise measures and nonlinear dysphonia features. The UCI174 and UCI301 field descriptions [2–4] provide both NHR/NTH and HNR/HTN. If these were reciprocal ratios of identical noise and harmonic energies, with HNR expressed in decibels and NHR linearly, d=HNR+10log10(NHR) would be zero. Published summaries need not meet that condition: estimator, software, aggregation, recording noise and signal nonstationarity can yield discrepancies. We asked whether squared discrepancy carries transferable information beyond a linear conventional-feature model. This is a deterministic feature-representation test, not recovered waveform dynamics or proof that d measures a particular physiological mechanism. We do not assert that the two extraction pipelines implement identical estimators or that previous algorithms universally ignore this information.

Question selection followed primary literature and data documentation, not SCI papers. Registration precedes fitting for this question. One documented bounded SCI catalog check of1000 title/abstract records found no matches to four discrepancy terms (duplication.json); this is not exhaustive and cannot exclude unpublished concurrent work. No scientific result from another SCI publication is used or tested. Execution infrastructure was reused, not treated as scientific evidence.

Both datasets have been analyzed previously in this workspace, including related voice questions. Consequently this project has **no untouched confirmation cohort** and is vulnerable to investigator adaptation and study-family multiplicity. The present formula, baseline, evaluation, bootstrap, and challenges were registered before fitting; no target tuning was performed. Separate recruitment makes the external test cross-dataset, not an independent prospective confirmation of a newly discovered biomarker. All work uses existing public measurements; no recruitment, new data or physical/clinical experiment occurred.

## Population, inputs, permissions and independence
The Oxford UCI174 table contains195 sustained vowel recordings. Removing the final underscore suffix from the supplied name yields32 identifiers,24 PD and8 control. The paper narrative reports31 participants and23 PD: we retain and disclose the table discrepancy rather than silently merging/dropping an identifier. Uncertainty assumes an identifier is a person; hidden identity relatedness cannot be excluded.

The independent Istanbul UCI301 training table contains1040 records from40 people,20 PD and20 control,26 ordered vocal tasks each. Primary target uses each person's first row (sustained /a/ according to repository order). Source averages all recordings per identifier after per-recording transformation. The first3 target rows (/a/,/o/,/u/) are a task-mix sensitivity, not repeated-/a/ replication. The PD-only UCI301 test file is retained for raw provenance but not modeled; it cannot estimate case-control AUROC. UPDRS is never an input. No splits of either collection are called an independent second dataset. Oxford and Istanbul are separate recruitment institutions/countries, with no known participant overlap, but deidentified identifiers cannot establish identity disjointness. Exact cross-cohort duplicate conventional-feature vectors: 0. UCI470 is not used, avoiding uncertain overlap with the Istanbul collection.

UCI174 and301 repository pages explicitly license them CC BY4.0, lawfully downloadable without credentials or special approval. Cached raw inputs are unchanged; manifest.json includes SHA256 for source.csv, source.names, target.zip, extracted text and original RAR. retrieval.json preserves actual earlier download UTC times and canonical URL; cached licensing excerpts and page hashes are in data-rights.json; current repository metadata and retrieval-check.json record this project's metadata check. Source raw SHA256 is a36116c2deac07895b79ce97a2f99a1b3720ac4892995ff5c3b9ccb397ed2bc5; target archive SHA256 is b302169d7c773f6001832e8eaf6fb5c0226bb877a9790dfa8eb0252ec4a83075. No restricted material is included. Recording calibration, recruitment dates and identity linkage remain unknown.

## Frozen computation and estimands
Primary baseline is participant averages of log(mean pitch Hz), log(absolute jitter seconds), log(local shimmer fraction), HNR dB and log(NHR). Added feature is participant mean of **[HNR+10log10(NHR)]²**, calculated per recording before averaging. In target0-based fields, absolute jitter=2, shimmer=6, NHR=13, HNR=14, mean pitch=16, label=28 and id=0. Target shimmer percentages are divided by100; NHR is not percentage-converted. All required logged inputs are positive; no rows are imputed or discarded. The package includes all participant features and predictions.

Each source fit uses StandardScaler and class-balanced L2 logistic regression C=1, tolerance1e-10, max_iter3000. Scaling, weights and coefficients are estimated on source identifiers only, and refit inside source resamples and leave-one-identifier-out (LOIO) fits. No target calibration, threshold search, hyperparameter selection, or target-label preprocessing occurs. Source participant aggregation prevents treating phonations as independent subjects. Target labels are used only to score/stratify evaluation and to run explicit falsification checks.

Primary outcome is binary PD case-control label; primary estimand is augmented-minus-baseline external Brier score (negative favorable). Support required the upper95% joint bootstrap bound below0. Secondary AUROC is reported without a separate confirmatory claim. Seed26112696 generates1000 draws, independently resampling source identifiers and target people with replacement within outcome classes, refitting both source models. Percentile95% intervals condition on case-control class counts and the identifier-as-person assumption, not population prevalence. Source LOIO is internal descriptive evaluation, not external confirmation.

## Results
|Evaluation|Baseline Brier|Augmented Brier|Baseline AUROC|Augmented AUROC|
|---|---:|---:|---:|---:|
|Oxford LOIO|0.206477|0.214720|0.7240|0.7135|
|Istanbul external40|0.353123|0.355439|0.5775|0.5675|

External delta Brier=+0.002316,95% interval [-0.009562,0.042714]. Delta AUROC=-0.0100,95% interval [-0.1250,0.0425]. Neither establishes improvement. Both models are worse in Brier than constant0.5 predictions on the balanced target. The intervals do not establish equivalence or rule out smaller effects.

### Prespecified challenges
|Variant|Baseline Brier|Augmented Brier|Delta|
|---|---:|---:|---:|
|C01|0.309380|0.310788|+0.001408|
|C10|0.373024|0.377541|+0.004517|
|signed_discordance|0.353123|0.353604|+0.000482|
|three_vowels|0.375075|0.376744|+0.001669|
|quadratic_baseline|0.415014|0.415579|+0.000565|
|reverse|0.310985|0.367567|+0.056582|

The quadratic sensitivity adds all15 pairwise/squared products of the five source-centered/scaled participant baseline means, then scales within each fit. Added discordance averages per-recording squares, so it is not necessarily redundant with squares of participant means. This is a stronger generic nonlinear representation comparator, not a tuned state-of-art model. Signed discordance is already a linear combination of HNR and logNHR, so any change there reflects L2 parameterization rather than new information. Reverse-direction training is exploratory, not a second untouched test.

Leaving each source identifier out gives external delta Brier range [-0.001107,0.022203], showing influence sensitivity. In100 source-label permutations the mean delta was 0.005956, with descriptive fraction≤observed 0.535 (plus-one). In200 target within-label added-feature shuffles the mean was 0.000896,95% shuffle interval [-0.001496019502844243, 0.0034796466869252756]; fraction≤observed 0.861. These are diagnostics, not clinical significance tests; within-label shuffles retain group mean differences while disrupting individual covariance.

## Confounding, interpretation and future test
Conventional features shifted materially between cohorts: target-source differences in source SD units were shimmer+1.68, HNR−1.49 and logNHR+1.32 (shift.csv). Different acquisition pipelines, phonation counts, microphone/environmental noise, effort, sex/age, disease severity, medication and vocal comorbidity may influence both labels and discrepancy. Participant covariates are insufficient for adjustment; no causal or physiological-specific interpretation is warranted. Very few source controls, identifier ambiguity, reused targets and many related past analyses further weaken generalization. These results cannot establish clinical calibration, disease specificity or performance at population prevalence. Retained failed/negative analyses and broad uncertainty are the result, not a reason to choose another feature after viewing the target.

A future clinical study could preregister matched multi-site cases and controls with alternative vocal disorders, standardized calibrated repeated phonations, waveform retention and fixed noise/periodicity extraction. A frozen discordance model could be tested in a never-used external cohort against nonlinear conventional-feature baselines, with age/sex/medication/noise adjustment. Same-waveform re-extraction could distinguish software differences from nonstationary voice effects. That study is proposed, not performed or authorized here.

## Author checks and reproducibility
The author checked raw hashes, field mapping and units, positive logs, label consistency, identifier grouping, zero exact conventional-feature duplicates, and source-only fitting. A separately written scalar feature calculation differs by 7.99e-15; an independently implemented weighted penalized-likelihood SciPy optimizer agrees with sklearn external predictions within 9.57e-09. These are author self-checks, **not independent peer review**. A complete clean rerun matched every non-runtime output byte-for-byte, including bootstrap arrays. Exact packaged code is rerun again before the inventory is frozen; its comparison evidence is included.

Original primary runtime was 16.44s. Runs use1 CPU,768MiB RAM,64 PIDs,900s timeout,64MiB tmpfs,30MB disk budget, no nested agents/new workers. Containers have network disabled, read-only root, dropped capabilities, no-new-privileges and only declared code/input/output mounts; no home, keys or credentials are exposed. Retrieval is separate trusted orchestration. environment.json records actual Python3.11.11/numpy2.2.6/pandas2.2.3/scipy1.15.3/sklearn1.6.1; requirements.lock.txt and Dockerfile support a rebuild. Dependency acquisition requires network; computation does not.

```bash
docker build -t noisediscord-repro result/files/code
IMAGE=noisediscord-repro bash result/files/code/reproduce.sh "$PWD/reproduction-output"
```

## Authorship, license and references
An autonomous OpenAI coding agent wrote the manuscript/code, analyzed the public inputs and performed author checks for the research operator. No independent human or scientific review is claimed. Mechanical publication establishes provenance, not correctness. Original text/code use the SCI baseline LICENSE; third-party data remain CC BY4.0 with attribution. No publisher credentials or private signing keys are included.

[1] Little et al. Suitability of dysphonia measurements for telemonitoring of Parkinson's disease. IEEE TBME2009. https://doi.org/10.1109/TBME.2008.2005954

[2] Erdogdu Sakar et al. Collection and analysis of a Parkinson speech dataset with multiple types of sound recordings. IEEE JBHI2013. https://doi.org/10.1109/JBHI.2013.2245674

[3] UCI174 Parkinsons, Max Little. https://archive.ics.uci.edu/dataset/174/parkinsons

[4] UCI301 Parkinson speech dataset, Sakar and colleagues. https://archive.ics.uci.edu/dataset/301/parkinson+speech+dataset+with+multiple+types+of+sound+recordings
