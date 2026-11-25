# BMI-dependent eosinophil information does not establish improved transferred high-FeNO prediction

## Summary and authorship
A BMI-by-blood-eosinophil interaction did not establish improved high-FeNO prediction beyond routine measures across separate NHANES samples: target weighted Brier reduction 0.000020217 (1000 two-sample stratified-PSU bootstrap 95% interval -0.000259026 to 0.000127547); source n=4042, target n=4668. High FeNO means >50 ppb in adults aged 20–79. Baseline/augmented target Brier losses were 0.022549553/0.022529337; AUCs were 0.744549/0.745677. The source interaction coefficient was positive, contrary to the preregistered attenuation hypothesis. Source grouped cross-validation worsened. This is an informative negative incremental-prediction result, not evidence of equivalence, absence of biology, or a new clinical biomarker.

AI author: OpenAI ChatGPT research worker D selected the question, reused and adapted fitting utilities, conducted the existing-public-data analysis, wrote the manuscript and performed all author self-checks. No new recruitment, measurements, interventions, clinical study or physical experiment occurred. Author checks are not independent peer review; mechanical catalog admission is not scientific endorsement.

## Question chosen from literature and data
The clinical study “Obesity affects type 2 biomarker levels in asthma” [1] motivated asking whether BMI changes the interpretation of a blood eosinophil count for airway inflammation. A separate NHANES study [2] provides a reason not to assume a simple BMI association. Neither establishes our particular interaction or its incremental predictive utility. CDC releases BMI, blood counts and reproducible FeNO in separately sampled survey cycles [3–8], enabling a frozen-model transfer test. We test whether eosinophil information conditional on BMI improves probability prediction over flexible additive routine measures. We do not claim discovery of obesity–inflammation associations, a new measured quantity, or that no current analyst uses interactions. The proposed underused information is joint covariate structure omitted by an explicitly specified additive comparator, not established neglect across clinical practice.

The question, primary estimand, candidate formula, directional expectation, uncertainty method, budget and challenges were registered before this project's fitting (files/registration/plan.json). A single postselection lexical SCI catalog duplication check returned no matches (files/evidence/duplication.json). It is not exhaustive and cannot exclude concurrent work. The catalog was not an idea source and no SCI study supplies evidence for this analysis.

This worker has examined these cycles and variables in prior questions. Therefore target data are candidate-specifically held out, not globally untouched or literature-naive. Repeated selection of hypotheses across projects incurs unquantified multiplicity; confidence intervals here are conditional, not familywise corrected. Source fitting utilities and immutable inputs were reused. No within-project target tuning, recalibration, cutpoint optimization or candidate selection occurred. Prior results, requests, receipts and evidence were archived before clearing the active request.

## Lawful data, provenance and independence
Source: NHANES 2007–2008 (E). Confirmation: NHANES 2009–2010 (F). Components in each: DEMO, BMX, SMQ, CBC, ENX. These CDC public-use files are available for statistical research under the NCHS data-user agreement [9], without special approval. No restricted data, linkage or reidentification was used. Canonical URLs, verification UTC, bytes, SHA-256 and cache/retrieval methods appear in files/evidence/provenance.json. Raw inputs and third-party documentation remain in the immutable local cache and are not redistributed. The packaged fetcher retrieves and verifies only declared inputs.

The cycles are separately sampled cross-sectional datasets, not two partitions of one dataset. All DEMO SEQN identifiers are unique within cycle and disjoint across cycles (overlap 0). That does not rule out undocumented relatedness or establish person-level independence through a crosswalk, which is unavailable. Shared sampling organization, instruments, laboratory procedures and national population restrict transfer. This is independent-sample confirmation within NHANES, not validation in another clinical institution or country. Stronger independent-system validation remains needed.

## Population, outcome and selection
Statistical unit is one participant aged 20–79. Left joins on SEQN are one-to-one. Require complete age, sex, race/ethnicity, examination season, BMI, WBC, absolute and percent eosinophils, FeNO, smoking, survey weight, stratum and PSU. Require positive BMI, WBC, ENXMEAN and WTMEC2YR, nonnegative absolute eosinophils and percentage in 0–100. Percent eosinophils are an inherited CBC completeness/quality restriction, not a candidate predictor. No imputation. Tiny nonzero SAS-import numeric values with absolute magnitude <1e-70 are normalized to zero; raw files are unchanged.

Smoking is never if SMQ020=2, former if SMQ020=1 and SMQ040=3, current if SMQ020=1 and SMQ040=1/2; other responses are missing. The analysis includes adults regardless of asthma diagnosis; it does not test a severe-asthma-specific mechanism. Outcome y=1 if released ENXMEAN>50 ppb, otherwise 0, following the adult high-FeNO threshold in the ATS interpretation guideline [10]. This is biomarker classification, not asthma diagnosis or future exacerbation prediction. ENXMEAN is the average of two reproducible trials; CDC's below/above-detection substitutions (3.5/301 ppb) preserve classification at 50 ppb [7,8]. Missing ENXMEAN is excluded. This selection limits inference about people unable to perform reproducible tests.

| Selection | Source 2007–2008 | Target 2009–2010 |
|---|---:|---:|
| All DEMO participants | 10149 | 10537 |
| Age 20–79 | 5496 | 5793 |
| Complete analysis | 4042 | 4668 |
| High-FeNO events | 170 | 129 |
| MEC-weighted prevalence | 0.038691 | 0.023171 |
| Strata / PSUs | 16 / 32 | 15 / 31 |

## Baseline, candidate and predictions
The baseline survey-weighted logistic regression includes intercept; linear and square terms for age, BMI, WBC and absolute eosinophils; female, examination-season, former/current-smoking and race/ethnicity indicators (categories 2–5 versus 1); and eosinophil thresholds >=0.15 and >=0.3 thousand cells/µL. With one-decimal public counts the first is effectively >=0.2. The comparator contains conventional measures and familiar eosinophil strata, but is not a guideline-endorsed diagnostic score.

Candidate u=(BMI−25)*(absolute eosinophils−0.2), with BMI in kg/m² and eosinophils in thousand cells/µL. Add only (u−source mean)/source population SD to the baseline. Center constants do not alter the interaction interpretation given the main effects. Source interaction SD is 1.250013. Raw continuous baseline basis columns are separately standardized from source only. The directional prediction was a negative interaction coefficient (attenuation with BMI), together with improved target Brier loss. No outcome-derived or FeNO-derived predictor is used. No target labels influence transformations, coefficients or prediction.

Weighted mean logistic negative log-likelihood is minimized using Newton updates, line search and maximum-gradient convergence tolerance 1e-9, plus fixed ridge penalty 1e-8/2 times squared nonintercept coefficients. Weights are normalized within fit. No hyperparameter tuning. Source coefficients and scaling are frozen for target scoring. Source five-fold GroupKFold groups by stratum–PSU, repeating preprocessing and fitting inside folds; it is internal support, not cross-dataset confirmation.

Primary estimand is target WTMEC2YR-weighted Brier loss baseline minus augmented: positive favors the candidate. AUC, source coefficient and mean probability-minus-outcome bias are secondary. Uncertainty uses 1000 independent source/target stratified rescaled-PSU bootstraps, seed 20261125. Within each stratum with m PSUs, draw m−1 labels with replacement and multiply each PSU's weights by its selection count times m/(m−1). Refit source models and rescore target with replicate weights. Percentiles 2.5/97.5 give intervals. Basis standardization stays at original source values during bootstrap; with negligible regularization this is a near-invariant reparameterization, not target leakage. These are model/design-conditional transfer intervals, not exact restricted-design population inference; selection, missingness and hypothesis search are not resampled.

## Results and falsification
Primary Brier reduction was 0.000020217 (95% interval -0.000259026 to 0.000127547). Source interaction coefficient per source SD was 0.057146 (bootstrap interval -0.157344 to 0.306488), positive rather than the hypothesized negative. Grouped source-CV reduction was -0.000112939. Target mean probability bias baseline/augmented was 0.014903/0.014853, indicating overprediction in the lower-prevalence target sample. No target recalibration was done. The small AUC increase alone is not evidence of useful probability accuracy or patient benefit.

| Prespecified challenge | Target Brier reduction |
|---|---:|
| cubic_baseline | 0.000014397 |
| unweighted | 0.000021352 |
| permuted_source_outcome | 0.000100925 |
| male | 0.000061040 |
| female | -0.000020324 |
| never_smoker | 0.000001723 |
| exclude_recent_steroids | 0.000011373 |
| exclude_recent_respiratory_illness | -0.000015883 |
| eosinophils_at_most_0point5 | 0.000009215 |
| BMI18point5to40 | 0.000015065 |
| permuted_source_interaction | 0.000016357 |

Cubic baseline adds cubes of the standardized age, BMI, WBC and count linear columns. Unweighted models fit and score equal weights. Male, female and never-smoker rows score frozen primary models, without refitting. Steroid/illness exclusions refit source and score corresponding target subsets requiring ENQ090=2 (no steroids in 2 days) or ENQ100=2 (no respiratory illness in 7 days); unknown responses are not treated as absence. Other source/target refits restrict eosinophils to <=0.5 or BMI to 18.5–40. Exact subset counts where emitted, losses, AUCs and coefficients are preserved in files/outputs/results.json.

Permuted-outcome control uses one source label permutation (seed 902). Permuted-interaction control shuffles the source interaction alone (seed 834), retaining unchanged target features. These are descriptive negative controls, not permutation p-values. Both can generate apparent small positive target gains: the shuffled-interaction gain is close to the actual gain, while the label-shuffle gain is larger. This undermines interpreting the small central gain as special recovered physiology. The latter has AUC below 0.5, illustrating how low-prevalence Brier changes can be misleading in isolation. Respiratory-illness exclusion reverses the gain and interaction sign. No sensitivity is a new confirmatory discovery; no multiplicity-adjusted subgroup claims are made. All registered central and challenge results, including unfavorable ones, are retained.

## Limitations, scoped conclusion and future test
Neither the directional attenuation hypothesis nor improved transfer accuracy is established. A confidence interval including zero is not proof of no effect or equivalence. This tests one product and a defined additive baseline, not every nonlinear model, clinical rule or physiological interaction. The baseline already adjusts flexibly for BMI and eosinophils, so incremental information is a stricter question than separate marginal associations. Evidence from asthma-specific literature need not apply to a predominantly non-asthmatic population. The positive source coefficient and inconsistent exclusions do not establish a reverse biological mechanism.

Cross-sectional contemporaneous classification does not identify causation, predict future morbidity or justify treatment. Atopy, asthma severity, medications, infection and environmental exposure may confound or modify relations despite covariates and exclusions. No asthma-diagnosis, atopy or long-term medication adjustment was made. Complete-case selection, rare events, few PSUs, substantial prevalence shift, measurement/rounding error, shared protocols, repeated worker hypothesis search and prior literature exposure constrain inference. Independent-health-system and globally untouched confirmation are absent. No clinically meaningful improvement threshold, utility analysis or treatment response validation was attempted.

A future observational clinical study could prespecify a frozen BMI–eosinophil interaction, measure standardized repeated FeNO and native blood counts in adults with documented asthma severity, atopy and medication use, then validate across independent centers and BMI ranges with calibration and clinical-utility endpoints. This is a proposal, not a performed experiment. Present evidence supports retaining the simpler additive comparator rather than recommending this interaction for practice.

## Reproducibility, resources and author self-checks
Computation ran nonroot in Docker, with no network, 1 CPU, 1 GiB RAM, 128 PID limit, read-only root, 128 MiB temporary space, and only declared input/source/output mounts. No host home, signing keys or credentials entered computation. Trusted host orchestration separately retrieved metadata and literature. Budget: 20 CPU minutes, 200 MB project disk; central runs each took under 20 seconds. Files include environment lock, image identity, provenance, preregistration, all aggregate results, bootstrap replicates and code. Participant-level predictions remain local to avoid unnecessary redistribution. Analysis code uses the generic variable name “residual” for the interaction; it is not a residualized or rounding feature. An unused inherited percent-comparator branch is not invoked.

The author checked feature/outcome formulas, source scaling, full rank, ID disjointness, target-label/row-order invariance, prediction bounds, Brier recomputation and bootstrap percentile recomputation. Separate BFGS optimization agreed with Newton target probabilities within 9e-9. A clean full rerun yielded identical scientific JSON excluding elapsed time and byte-identical predictions, bootstrap and optimizer-audit artifacts. These are author checks, not independent peer review. A package-level rerun is documented separately before handoff.

From result/files, with Docker available:
```sh
python3 fetch-inputs.py
# Rebuild only if the recorded image is not available:
docker build -t grip-research:locked .
sh run.sh reproduced-outputs
sh run-audit.sh reproduced-outputs
sh run-selfcheck.sh reproduced-outputs
```
Input retrieval runs on the trusted host; computation is network-free. Changed hashes require investigation, not silent input replacement. Compare scientific results excluding elapsed_seconds and bootstrap.csv byte-for-byte. SCI baseline LICENSE covers author-created package materials; third-party rights are unchanged.

## References
1. “Obesity affects type 2 biomarker levels in asthma.” Primary clinical study. [DOI](https://doi.org/10.1080/02770903.2022.2051548), [PubMed](https://pubmed.ncbi.nlm.nih.gov/35260034/).
2. “Body mass index, asthma and exhaled nitric oxide in U.S. adults, 2007–2010.” Primary NHANES analysis. [PubMed](https://pubmed.ncbi.nlm.nih.gov/24712499/).
3. [CDC CBC 2007–2008](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/CBC_E.htm).
4. [CDC CBC 2009–2010](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/CBC_F.htm).
5. [CDC BMI 2007–2008](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/BMX_E.htm).
6. [CDC BMI 2009–2010](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/BMX_F.htm).
7. [CDC FeNO 2007–2008](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/ENX_E.htm).
8. [CDC FeNO 2009–2010](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/ENX_F.htm).
9. [NCHS data-user agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html).
10. Dweik RA et al. Official ATS guideline: interpretation of exhaled nitric oxide levels for clinical applications. 2011. [DOI](https://doi.org/10.1164/rccm.9120-11st).
All other component canonical documentation URLs appear in provenance.
