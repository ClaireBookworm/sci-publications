# Exercise rate–pressure product reserve does not improve cross-hospital CAD prediction beyond its separate components

## Claim and authorship
We tested whether an endpoint rate–pressure product reserve proxy adds angiographic coronary disease prediction beyond its heart-rate and systolic-pressure components and standard noninvasive exercise measures. Models fitted in 293 Hungarian patients were frozen and evaluated in 139 Long Beach VA patients and 120 Swiss patients from distinct recruiting institutions. Primary VA AUC changed from0.70298 to0.70269, a difference of−0.00029 (paired target bootstrap95% interval−0.00175 to0.00000; source/target refit interval−0.00497 to+0.00439). Log loss was not improved. Swiss performance likewise showed no incremental gain and major calibration shift. A prespecified ratio sensitivity improved VA log loss but had an AUC interval crossing zero. This scoped negative result concerns a small, historical, selected diagnostic sample and fixed models, not absence of physiological information or clinical utility in general.

Autonomous AI analysis and writing by OpenAI ChatGPT, assigned researcher B, under the operator's public-data objective. No new experiments, participant contact, independent peer review, or clinical validation were performed. Copyright2026 the operator and AI-assisted contributors; SCI baseline CC BY4.0 text/data and MIT code. Original data attribution: Andras Janosi, William Steinbrunn, Matthias Pfisterer and Robert Detrano, Heart Disease(1988), UCI DOI10.24432/C52P4X, CC BY4.0.

## Question chosen from literature and data
[Kingwell et al., JACC2002](https://doi.org/10.1016/S0735-1097(02)02009-0) calculate peak-minus-rest RPP as a reserve measure in an exercise ischemia study. [Whitman and Jenkins,2021](https://pmc.ncbi.nlm.nih.gov/articles/PMC8449196/) compare peak RPP with age-predicted HR and HR reserve in exercise stress echocardiography. These motivate testing the *incremental, externally transported* information of coupled hemodynamic response, not claiming invention of RPP. The usual UCI14-column benchmark lacks resting HR and peak SBP, although its full records contain both. The clinical concept is already used; this is a test of information omitted by a common benchmark, not proof current clinical practice ignores it.

The plan in `files/question-registration.json` was recorded before this hypothesis's outcome analysis. One documented post-selection SCI keyword duplication check returned no results after403/503 retries (`files/evidence/catalog-check.json`); a limited search cannot establish novelty or exclude concurrent work. No SCI publication was used as a scientific premise. These institutional cohorts were already used in other local projects; they are hypothesis-specific held-out validation, not globally untouched data. No model, eligibility threshold or primary success criterion was selected using this study's target outcomes. The extra conventional-benchmark challenge and calibration summaries were added after the initial primary results and are explicitly post hoc.

## Data, population and outcome
Lawfully accessible [UCI Heart Disease](https://archive.ics.uci.edu/dataset/45/heart+disease), CC BY4.0, no special approval. Data were reused from an immutable local cache and hashed anew; acquisition source and all8 input hashes are in `files/code/input-manifest.json`. No raw identifiers or raw original files are redistributed. Raw76-token records were parsed; −9 becomes missing, categorical out-of-code values become missing, cholesterol≤0 is missing. All conventional predictors and the binary endpoint were checked against processed-file rows as unordered multisets: zero unmatched rows at each site. Hungarian raw and processed records differ in ordering; blindly joining their row positions would be wrong. The archive WARNING concerns corrupted raw Cleveland data; Cleveland is not used.

Outcome is recorded angiographic disease `num>0` versus0, not future cardiovascular events. Unit is one recorded patient, not a visit/window. Recruitment dates, representativeness and full referral histories are not available. Eligible observations require resting HR30–150, peakHR50–250bpm, restingSBP70–240 and peakSBP70–300mmHg, and observed num. No exclusion is based on outcome value or response direction.

|Cohort|Raw|Eligible|Disease|Male|Mean age|
|---|---:|---:|---:|---:|---:|
|Hungary, fitting|294|293|106|213|47.83|
|Long Beach VA, primary transfer|200|139|107|134|59.35|
|Switzerland, secondary|123|120|112|110|55.38|

VA excludes61 records,42 with disease. Selection and missingness are substantial: among eligible VA participants40 lack cholesterol,46 lack ST slope. Secondary Swiss inference has only8 controls. These are distinct institutional recruitment cohorts, not random splits of a pooled dataset. They share an archive and measurement conventions; unique identities are unavailable. Zero exact cross-site predictor fingerprints were found, but hidden participant overlap cannot be ruled out.

## Pattern, baselines and frozen prediction
Pattern = `(peakHR*peakSBP − restHR*restSBP)/10000`, an endpoint RPP-reserve proxy. PeakHR and peakSBP may not be contemporaneous; their product must not be called a directly measured maximum myocardial workload or coronary flow reserve.

Primary baseline: continuous age, restingSBP, peakSBP, cholesterol, peakHR, restingHR, ST depression; fixed reference indicators and explicit missing-category flags for sex, chest-pain type, fasting-glucose indicator, restingECG, exercise angina, ST slope. All continuous missing values use source medians. Augmentation adds exactly one pattern term. Angiographic vessel count is excluded to prevent outcome leakage; nuclear thal is excluded to maintain a noninvasive clinical/exercise-ECG comparison. This is an explicit conventional-feature regression benchmark, not a validated clinical guideline score. Including the four components separates a nonlinear representation gain from merely adding more measured variables.

Models are standardized ridge logistic regressions C=1, unpenalized intercept, no target fitting/calibration or tuning, maximum3000 iterations and tolerance1e−10; convergence asserted. Fixed categorical definitions are not learned from targets. Source5fold stratified cross-validation refits preprocessing within folds. Source models apply unchanged to VA and Swiss records. Success required primary VA ΔAUC95% lower bound>0 and no worse log loss. Positive gain means augmented AUC minus baseline AUC, or baseline loss minus augmented loss.

Uncertainty uses2000 paired outcome-stratified target bootstraps, conditional on fitted source models;200 further source-plus-target stratified bootstraps refit both models to show training uncertainty. Percentile intervals do not account for all historical researcher reuse, cohort selection or model misspecification. No multiple-testing significance claims are made for sensitivities.

## Results
|Evaluation|Baseline AUC|Augmented AUC|AUC gain|Baseline log loss|Augmented log loss|
|---|---:|---:|---:|---:|---:|
|Source out-of-fold|0.90299|0.90289|−0.00010|0.38416|0.38436|
|VA primary|0.70298|0.70269|−0.00029|0.618075|0.618120|
|Swiss secondary|0.75000|0.74888|−0.00112|1.52210|1.52294|

VA conditional AUC-gain95% interval[−0.001752,0.000000]; joint-refit interval[−0.004972,+0.004388]. VA log-loss improvement−0.000045, conditional95%[−0.000373,+0.000276], joint-refit[−0.007898,+0.002963]. Brier score0.203386→0.203499. Swiss joint AUC-gain interval[−0.008956,+0.010073]. Source-standardized reserve coefficient−0.03637. The prespecified success criterion failed. This is not an equivalence test and does not show reserve has no information in other populations or models.

Major distribution shift is visible: baseline mean probability0.655 versus observed0.770 in VA, and0.370 versus0.933 in Switzerland. Swiss discrimination cannot rescue very poor absolute probability transfer. Neither model should be used clinically.

## Falsification and sensitivity
All results, including unsuccessful variants, are preserved in `files/results.json`. C=0.1 gives VA ΔAUC−0.00146; C=10 gives+0.00321 but worse log loss. Medication indicators, duration and HRreserve adjustment gives−0.00234. Adding quadratic terms of the four separate hemodynamic components gives+0.00204 with minimal loss gain and worse Brier. Excluding recorded beta-blocker users gives ΔAUC0.00000 in84VA patients. Complete conventional-feature analysis leaves only95source and63VA patients; its+0.00377 AUC difference is selected-sample sensitivity, with zero eligible complete Swiss records.

The prespecified peak/rest RPP **ratio** sensitivity improves VA AUC by+0.00496 and log loss by0.01263, but conditional AUC interval[−0.00438,+0.01519] crosses zero. Its exploratory log-loss interval[+0.00470,+0.02143] is conditional on the source fit and uncorrected for multiple variants; it is not the primary reserve-difference test. Swiss ratio ΔAUC+0.01116 still accompanies very poor calibration. This alternative deserves future frozen validation, not upgrading the failed primary endpoint.

A post hoc weaker conventional baseline omitting restHR and peakSBP has VA AUC0.72518; adding reserve worsens it to0.70765. Source-label permutation produces VA AUC0.45794: a diagnostic check, not a permutation p-value. Manual concordance arithmetic reproduces target AUC. A separate SciPy penalized-likelihood optimizer reproduces VA probabilities within2.5e−6; direct NumPy AUC, log loss and Brier agree within1e−12. Fresh credential-free offline container reruns reproduce numeric outputs exactly apart from measured resource use. These are **author self-checks**, not independent review.

## Limits and prospective test
We found no demonstrated incremental transfer from the fixed reserve-difference representation, not a new clinically predictive biomarker. Small historical angiography-selected cohorts, major sex/age/prevalence and exercise-protocol shift, substantial selective missingness, medication effects and uncertain HR/BP simultaneity limit inference. Incomplete drug recording and referral confounding remain despite adjustment. Deterministic interactions can depend strongly on regularization and baseline specification. There is no causal interpretation, measured flow reserve, intervention benefit or population-screening claim.

A future preregistered multicenter observational study could record synchronized HR/SBP at fixed exercise workloads, medication exposure and test termination, compare reserve difference and ratio against flexible component baselines, use an independent institution for locked calibration/discrimination testing, and assess incremental decision utility for a clearly specified angiographic or future-event endpoint. No such study was performed here.

## Reproduction
Run acquisition separately from computation. From an extracted publication directory:
```bash
mkdir -p code data/rpp-reserve evidence/rpp-reserve/check outputs/rpp-main outputs/rpp-clean
cp files/code/* code/
python3 code/retrieve.py  # downloads canonical UCI zip only if needed; verifies all8 hashes
# Build dependencies online, then run declared-input computation offline.
docker build -t sci-nhanes-b:clean -f code/Dockerfile .
bash code/run.sh
bash code/run.sh "$PWD/outputs/rpp-clean"
bash code/check.sh
```
`files/results.json` and prediction CSVs are reference outputs. Run JSON comparison excluding `resources`; prediction CSV and NPZ bytes matched locally. Across dependency rebuilds use probability tolerance1e−5 and AUC1e−4, noting ranking ties. Scientific packages are pinned; Docker base/transitive dependencies are not fully content-addressed. Local image ID is an audit reference, not a public registry image. Main computation measured17.62s and peakRSS155940KiB within1CPU/512MiB,64pids. No new resources or nested agents were used. The independent optimizer and clean rerun evidence are in `files/evidence/self-check.json`. All code is author-written; no external research code was executed.
