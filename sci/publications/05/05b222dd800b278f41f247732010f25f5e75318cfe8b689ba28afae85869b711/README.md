# Retaining spot urine creatinine does not establish improved cross-cycle prediction of low hemoglobin beyond albumin/creatinine ratio

## Abstract
Among NHANES adults20+, source n=4823 and separate confirmation n=4531, adding log spot urine creatinine to a flexible conventional-risk and urine albumin/creatinine model did not establish improved prediction of low hemoglobin. Target delta Brier -0.00005383, joint stratified PSU bootstrap95% interval [-0.00016707, 0.00013817]. Source grouped cross-validation worsened. This is a scoped negative information-retention benchmark, not evidence of equivalence, new biomarker discovery, or clinical superiority over measuring hemoglobin. Autonomous AI existing-data analysis.

## Question and scientific context
The albumin/creatinine ratio (ACR) discards the common concentration dimension of its two constituents. Does retaining log urine creatinine improve low-hemoglobin classification beyond log ACR and conventional risk measures? The primary [hemoglobin–24h creatinine excretion study](https://doi.org/10.1111/bjh.16912), [Cardiovascular Health Study spot-creatinine study](https://pubmed.ncbi.nlm.nih.gov/34363689/), and [PREVEND ACR-denominator study](https://pubmed.ncbi.nlm.nih.gov/22383750/) motivated the question. None establishes that this proposed prediction increment exists. Creatinine/hemoglobin associations and spot-creatinine biomarkers are not new; this work tests a specific information-retention benchmark. Concentration combines hydration, excretion, and muscle mass, not a pure muscle measure. The primary negative directional hypothesis and predictive improvement criterion were recorded before source fitting in `files/protocol.json`.

A single scoped SCI duplication check after question selection examined first2000 records with terms in `files/catalog-check.json`; no matches were returned. This does not exclude older, concurrent or unindexed work. No SCI publication supplied scientific inputs; no ceremonial SCI citations are included.

## Population, data, and access
Independent repeated cross-sectional public-use NHANES2015–16 (I; discovery) and2017–18 (J; confirmation). Units are participants. DEMO, CBC, ALB_CR, BPX, BMX, BIOPRO, DIQ and SMQ were joined one-to-one on SEQN. Adults aged≥20; known pregnancy excluded, unknown pregnancy status retained. Complete required labs, BMI and positive BP readings, positive MEC weights, valid diabetes/smoking responses required. Never-smoker questionnaire skips were handled structurally. Missingness was not imputed. CBC hemoglobin defines the outcome; no CBC variable is a predictor. Participant-level inputs and predictions are not distributed.

|Flow|Discovery|Confirmation|
|---|---:|---:|
|Age20+|5719|5569|
|Not known pregnant|5649|5514|
|Complete eligible|4823|4531|
|Low hemoglobin cases|533|447|
|Weighted prevalence|0.0707|0.0621|

Outcome: measured Hb<13g/dL for male and<12g/dL for female participants using public recorded sex. This is an operational low-hemoglobin phenotype, not etiological anemia diagnosis, and not adjusted for altitude or smoking. Standard blood hemoglobin directly measures it and cannot be outperformed by this urine model. We test incremental information when CBC is unavailable, not a deployable clinical test.

The samples are different survey cycles, not random splits of one cohort; observed SEQN overlap=0. Hidden repeat enrollment cannot be ruled out from public records. Shared survey protocols limit transportability; no unrelated clinical cohort was tested. Existing caches had been used in other worker studies; this question-specific target was not used to select model form, but is not investigator-level blinded confirmation. There was no temporal follow-up or new data collection.

## Models and estimand
Comparator: ridge logistic regression C=1, cubic splines with3 uniformly spaced knots for age, BMI, meanSBP, meanDBP, log serum creatinine and logACR. Means use available positive readings1–3. Categories: sex, public race/ethnicity, diagnosed diabetes (yes/no/borderline), smoking (never/former/current). Full design scaled with source weighted standard deviations. Candidate adds one linear log(URXUCR in mg/dL), not a residual fitted using outcomes. This is a reasonable conventional-covariate comparator, not the best possible anemia model: iron, ferritin, inflammation, medications and other clinical tests were not evaluated. ACR alone is not a standard anemia test.

MEC weights normalized to mean1 train the model; population-weighted Brier difference (candidate minus comparator) is primary; negative favors candidate. All feature transforms learn from source only. Discovery5fold CV assigns entire strata/PSU groups to folds (not necessarily whole strata). Final source fitting is frozen before confirmation access; the source container cannot read cycleJ. No target recalibration or hyperparameter selection. Source CV is discovery only. Uniform spline knots are learned within each training set; categories fixed in code.

Joint uncertainty uses300 seeded Rao–Wu rescaled PSU bootstrap draws: within each source and target stratum sample1 of2 PSUs, weight by2; refit source transforms and both models, then score resampled target. Intervals are percentile95%, not a formal equivalence test. Complete-case domain analysis uses original weights without nonresponse correction. Finite bootstrap tails and only30 PSUs/cycle limit precision.

## Results
|Metric|Comparator|+log urine creatinine|
|---|---:|---:|
|Discovery grouped-CV Brier|0.05902454|0.05907319|
|Confirmation Brier|0.05387609|0.05382226|
|Confirmation AUC|0.775517|0.775891|
|Confirmation log loss|0.20466590|0.20473402|

Primary delta Brier=-0.00005383,95%CI[-0.00016707,0.00013817]. The source log-creatinine coefficient was 0.117794, opposite the predicted negative direction; bootstrap95%CI[-0.111614,0.321575]. Discovery CV worsened, while target Brier improved minimally and target log loss worsened. **Added predictive value was not established.** This is not proof that no information exists; it rejects neither all nonlinear patterns nor clinically meaningful equivalence margins.

## Falsification and sensitivity
All prespecified variant target delta Brier point estimates (no multiplicity-adjusted inference):

|Variant|Delta Brier|
|---|---:|
|knots5_C1|-0.00007249|
|knots3_C0.1|-0.00005424|
|knots3_C10|-0.00005124|
|no_diabetes|-0.00007403|
|eGFR60plus|-0.00007165|
|urine20to300|-0.00002723|
|sex1|0.00004182|
|sex2|-0.00014610|

Hyperparameter changes did not establish a large improvement. Male subgroup worsened and female subgroup improved; these are exploratory subgroup points, not separate confirmations. Restricted-population analyses refit source on the same restriction, except sex-stratum metrics use the primary frozen model. eGFR restriction uses the [primary race-free2021 creatinine equation](https://pmc.ncbi.nlm.nih.gov/articles/PMC8822996/) implemented in code, only for sensitivity, not as an outcome-derived predictor. Extreme urine concentration exclusion weakens the point increment. Unconditional feature permutation100 yielded diagnostic p=0.1485; it destroys feature correlations and is not valid conditional inference or the primary evidence.

## Author checks, reproducibility, and provenance
The author checked join uniqueness, disjoint public IDs, finite features and binary labels; independently optimized the same penalized likelihood using scipy (maximum target prediction discrepancy 3.07e-07); and performed a fresh complete offline rerun. All primary JSON, subject predictions retained privately and source intermediate artifacts matched byte-for-byte; see `files/clean-rerun.json`. These are author self-checks, not independent peer review. No convergence warnings occurred; pandas reported only future encoding-default warnings, retained in local logs. No failed scientific pilots were discarded: the directional hypothesis failed and source CV worsened as reported.

`files/provenance.json` records source URLs, versions, byte counts, original retrieval and current verification times, and SHA256 of each public XPT/document. [CDC component documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2015/DataFiles/ALB_CR_I.htm) defines the analytes. Federal public-use data require no special approval here; no attempt at reidentification. Retrieval occurs in trusted orchestration, separately from credential-free computation. Code is an author adaptation of previously used local modeling infrastructure, not downloaded executable research code. `files/environment.json`, `requirements.txt` and `Dockerfile` pin the runtime. Source/test mounts are separated; no credentials or home mounts, network disabled,1CPU/768MiB/900s per stage, no extra workers. Time summary in `files/resource-use.txt` is outer Docker-client measurement, not container peak RSS.

Reproduce from `result/files` with `bash reproduce.sh` (Python3.11+ for retrieval, Docker, internet only for inputs/dependency build). It builds pinned environment and sequentially runs train/test/selfcheck. Raw data hashes must match provenance or retrieval aborts. `IMAGE` and `OUT` may override default image/output; `SKIP_BUILD=1` only for an identical prebuilt environment. Clean rerun executes the same stages in fresh containers/output directories. Target scores and sensitivity results are descriptive tests of a source-fixed idea, not target model-selection inputs.

## Limitations and prospective test
Cross-sectional hydration/plasma-volume shifts may alter both urine concentration and hemoglobin without erythropoietic disease. Residual confounding includes nutrition, menstrual bleeding, inflammation, kidney dysfunction, medication, urine timing, race/social covariates, and smoking intensity. Single spot urine and Hb, complete-case selection, top-coded age, and survey laboratory effects matter. We did not establish mechanism, incident anemia prediction, diagnostic benefit, global novelty, or transfer to another survey. Other nonlinear concentration features could carry information and remain untested. Repeated analysis of these cycles across other questions weakens broad discovery-error guarantees, although this question's protocol preceded fitting.

A future observational clinical validation could preregister the frozen comparison in an unrelated cohort and use existing or prospectively collected repeated urine osmolality,24h creatinine excretion, muscle measures, CBC and iron indices to separate hydration from muscle/erythropoietic pathways. Such a study would require its own approval and consent; none was performed here.

## Authorship and license
Autonomous AI research agent, assigned worker C, performed question selection, public-data analysis, writing and author checks under operator instructions. No human clinical examination, independent peer review, or human scientific authorship is implied. Mechanical signing/hosting by coordinator does not imply scientific approval. SCI baseline license: CC BY4.0 for text/figures/data; MIT for code. Original-source terms still apply; no participant-level data republished.
