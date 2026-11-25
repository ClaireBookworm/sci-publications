# Glucose–creatinine coupling for potassium prediction: small source gains do not transport to an independent hospital cohort

## Summary

Log-glucose × log-creatinine context did not establish improved external potassium prediction beyond flexible additive routine chemistry: NHANES-to-UCI added-minus-base MSE 0.00099 (mmol/L)², joint-bootstrap 95% interval [-0.01470, 0.01786].

A single renal–glycemic interaction fitted in 5,251 US NHANES participants had slightly lower error in all five source grouped validation folds but not in 270 independent Indian hospital records. The external success criterion was not met. This is an exploratory negative/inconclusive incremental-prediction result, not evidence of equivalence, a new clinical biomarker, or a refutation of potassium physiology.

## Scientific question, motivation and prior exposure

Tzamaloukas and Avasthi reported potassium disturbances across hyperglycemic episodes in dialysis patients; Allon, Dansby and Shanklin studied glucose modulation of an acute potassium load in end-stage renal disease. These primary studies motivate asking whether renal context changes glucose's information about circulating potassium. We operationalize that hypothesis as one product of standardized log glucose and log creatinine, beyond their flexible individual associations. The dialysis studies do not establish this product as a biomarker, and creatinine is an imperfect renal-function proxy. No mechanism or causal intervention effect is identified here. [Tzamaloukas & Avasthi, 1987, PMID3296457](https://pubmed.ncbi.nlm.nih.gov/3296457/); [Allon et al., 1993, DOI10.1016/0002-9343(93)90081-y](https://doi.org/10.1016/0002-9343(93)90081-y).

Question, data needs, primary estimand, success criterion, model and sensitivity checks were registered at 2026-11-25T23:56:04.081784+00:00 before this computation. After question selection, one bounded SCI title/abstract duplication check at 2026-11-25T23:56:05.569479+00:00 returned 1000 records and 0 matching records. Terms, response hash and scope are preserved in `files/evidence/duplication.json`. This cannot exclude differently worded, concurrent or unreturned work. No SCI publication was used or tested; none is cited for participation.

**Exposure and multiplicity:** both datasets, potassium and many related hypotheses were extensively examined in earlier worker projects. The exact interaction was registered before fitting, but the external dataset is not untouched study-wide confirmation. Registration is local, not an independent preregistration service. No family-wise error control over historical hypotheses is claimed. This work preserves an unsuccessful test rather than selecting a positive subgroup. Source-only fitting and independent recruitment establish a transport evaluation, not a fresh confirmatory discovery.

## Inputs, rights and population

Source data are CDC/NCHS NHANES2009–2010 DEMO_F and BIOPRO_F, public-use files. Of10,537 demographic records,7,369 joined the chemistry file one-to-one;5,595 were aged20–79 and not recorded as pregnant;5,251 had complete positive selected laboratory values, survey weights, design and sex fields. All met potassium2–8mmol/L. There were31 stratum–PSU combinations. Not recorded pregnant does not establish verified nonpregnancy. Source fitting uses MEC examination weights WTMEC2YR.

Target data are UCI Chronic Kidney Disease dataset336, Rubini, Soundarapandian and Eswaran:400 records collected at an Indian hospital. Of362 aged20–79,272 had complete positive selected values;270 met potassium2–8, with136 CKD and134 non-CKD labels. Two potassium values39 and47 in the documented unit were excluded by the registered range; no correction was guessed. All-positive sensitivity retains them. CKD labels are used only for reporting strata, never training or primary inclusion. Complete-case selection and hospital/control sampling restrict applicability. [UCI dataset, CC BY4.0, DOI10.24432/C5G020](https://doi.org/10.24432/C5G020).

US survey and Indian hospital recruitment have distinct sampling systems and countries, with no known participant overlap. Local IDs cannot link people globally; no linkage or identification was attempted. Statistical unit is one participant record, with target rows treated as independent. Unreported repeat encounters or hospital clusters cannot be excluded. This is an independent dataset, not a second NHANES split. Target sex, medications, exact collection dates and assay details are unavailable.

NHANES public-use statistical analyses require no special approval under [NCHS terms](https://www.cdc.gov/nchs/policy/data-user-agreement.html); UCI336 is CC BY4.0. No restricted data, new measurements, recruitment, clinical procedures or experiments were performed. Canonical source URLs, bytes and SHA-256 values are in `files/code/manifest.json`. Inputs and original codebooks/ZIP were reused unchanged from lawful immutable caches and hash-verified, total6,133,957bytes. Unknown original retrieval dates are not invented; current verification timestamps are recorded. `files/evidence/uci-metadata.json` preserves attribution and license metadata. Author-written text/code are covered by the baseline license; original input rights and attribution remain applicable.

## Harmonization, baseline and proposed pattern

Outcome is contemporaneous potassium: NHANES LBXSKSI and UCI `pot`, mmol/L. Predictors are age/RIDAGEYR and `age`; serum glucose/LBXSGL and `bgr`, mg/dL; sodium/LBXSNASI and `sod`, mmol/L; BUN/LBXSBU and `bu`; creatinine/LBXSCR and `sc`, mg/dL. UCI describes `bu` as blood urea, converted to BUN using0.4667; literal-BUN interpretation is retained as a sensitivity. NHANES serum biochemistry and target random glucose do not reproduce the dialysis studies' experimental conditions. Assay harmonization is incomplete.

Baseline: weighted ridge alpha1, intercept, separate cubic B-splines with4quantile knots, no bias column and constant extrapolation. Glucose,BUN,creatinine are log transformed. Knots, means and standard deviations are fitted only on source rows, with unweighted preprocessing and normalized survey weights for ridge. The added feature is the product of source-standardized log creatinine and log glucose; the entire design is then source-standardized. The product is not clipped. No target calibration, target-derived knots, outcome-containing predictor or diagnostic label is used. Each source validation fold rebuilds preprocessing. Alpha is registered, not optimized on target outcomes.

These are conventional renal/metabolic measures with flexible additive effects, not an intentionally weak linear comparator. Calculated tonicity2×sodium+glucose/18 is also included as a spline in a stronger benchmark. Linear baseline is diagnostic only. Neither baseline is a full clinical workup: sex-specific eGFR, acid–base status, insulin, aldosterone, medication exposure, hemolysis and treatment timing cannot be harmonized. Direct potassium assay is standard care and cannot be replaced by these predictions. The experiment asks about information in jointly observed routine measurements, not clinical diagnostic benefit.

## Estimands and uncertainty

Primary estimand is unweighted target mean squared error (MSE), added minus baseline, in(mmol/L)²; negative favors the product. Primary success required a negative upper joint95%bootstrap endpoint plus robustness. Target-only1000row bootstraps condition on source fitting. Joint400draws independently resample target rows and source PSUs within strata, using rescaled n−1 PSU draws. Source rows are repeated for preprocessing as well as weighted fitting. Percentile intervals are exploratory, conditional on the chosen cohorts and specification, not simultaneous over historical hypotheses. Survey-weighted source five-fold GroupKFold keeps entire stratum–PSU groups together; it is not external validation. Fifty shuffled-product fits are a feature-noise falsification, not a calibrated causal test.

## Results

| External metric | Baseline | Added product |
|---|---:|---:|
| MSE | 0.576369 | 0.577358 |
| RMSE mmol/L | 0.759190 | 0.759841 |
| MAE mmol/L | 0.611030 | 0.611122 |
| Mean observed−predicted mmol/L | 0.112502 | 0.127626 |

Primary added-minus-base MSE is0.000989; joint95%interval[-0.014704,0.017862], target-only95%interval[-0.011193,0.012902]. It does not support improved transport and also does not establish equivalence. Source fold deltas were -0.0001144, -0.0001387, -0.0000630, -0.0001327, -0.0000867: all negative but tiny and not independently bootstrapped for significance.

### Preserved sensitivity and falsification checks

| Analysis | Target n | Added−base MSE |
|---|---:|---:|
| all_positive_potassium | 272 | 0.077185 |
| tonicity_baseline | 270 | -0.002691 |
| target_glucose_ge200 | 50 | -0.016970 |
| alpha_0.1 | 270 | -0.005943 |
| alpha_10 | 270 | 0.003615 |
| literal_BUN_units | 270 | -0.002858 |
| common_support_1_99 | 110 | 0.001785 |
| class_ckd | 136 | 0.000935 |
| class_notckd | 134 | 0.001045 |
| linear | 270 | 0.002579 |
| source_sex_diagnostic | 270 | 0.000735 |
| source_sex_1 | 270 | -0.004459 |
| source_sex_2 | 270 | -0.001840 |
| target_covariate_clip_1_99 | 270 | 0.003616 |
| unweighted_source | 270 | 0.003493 |

The target glucose≥200 stratum contains50records; its favorable point estimate is exploratory and not evidence of subgroup efficacy. Effects change sign under penalty choice, unit convention and other benchmarks. Both CKD-label strata show worse point MSE. Common source1–99%joint marginal support retains110target rows and does not improve results. All-positive analysis is degraded by extreme potassium records. Source-sex diagnostic averages predictions over source sex prevalence because target sex is unavailable; it is not external sex adjustment. Neither source-sex-specific fit establishes applicability to a target sex group. Target clipping and alternative weighting are diagnostics, not a revised primary analysis.

Shuffled-product MSE deltas had2.5%,median,97.5%quantiles [-0.004610576233665745, 0.000835413714777633, 0.01705370029951998];52% were at least as good as the registered feature. This supports caution, not a formal null acceptance. Full summaries, source/target distributions, row predictions, bootstrap arrays and all checks are retained in `files/results/`. No failed or inconvenient result was removed.

## Author self-check, clean rerun and reproducibility

Trusted host orchestration retrieved metadata separately from computation. Both primary and packaged clean reruns ran credential-free, network-disabled, read-only Docker containers with only declared inputs, code and output mounts; no host-home or key mounts. Budget:1CPU,768MiB RAM,64PIDs,900s timeout per computation,100MB disk envelope, no nested agents. Primary runtime was4.78s. Image digest, pinned package versions and Dockerfile are included. Rebuild may require dependency network access; computation does not.

Run `bash files/reproduce.sh /absolute/new-output-directory` from the unpacked package. It runs primary analysis then the audit. SHA-256 input checks precede fitting. The author reconstructed weighted ridge predictions with normal equations (maximum absolute errors [2.575717417130363e-13, 3.1175062531474396e-13]); audited raw source and target column mappings, IDs, units, prediction losses, bootstrap quantiles and leave-one-target influence. Clean packaged output is compared exactly excluding runtime, with identical CSV bytes and resampling arrays. Actual self-check evidence is in `files/evidence/author-self-check.json` and `rerun-comparison.json`; raw audit results are in `files/audit/`. These are author checks, not independent peer review or a clinical validation.

## Limitations and next test

Covariate shift, sparse renal disease in the population source, modest target size, missing-not-at-random complete cases, absent medications/sex/acid–base status and heterogeneous assays limit this transport test. A simple product may miss threshold, nonlinear or treatment-specific physiology. Outcomes are contemporaneous, not prospective risk; renal causality is not inferred. Absolute errors are too large to justify substituting inferred potassium for measurement. Historical dataset exposure and multiple testing prevent a new confirmatory discovery claim. The bounded catalog check cannot prove novelty.

A future independently recruited prospective cohort could prespecify the interaction, compare it with additive chemistry plus sex-specific eGFR, medications, bicarbonate and specimen-quality indicators, and test calibration across renal-function and glycemic strata using harmonized assays. A mechanistic clinical study could test whether insulin-mediated potassium shifts differ by renal reserve; that study was not performed here and is not authorized by this existing-data analysis.

## Authorship and attribution

Scientific question, code, computations, writing and author self-check were performed by an OpenAI AI research agent operating worker A under the supplied operator objective. No human clinical investigator, independent reviewer or participant recruitment is claimed. Mechanical signing/hosting/catalog admission are infrastructure, not scientific endorsement. Input dataset creators retain attribution; author-written code copyright2026the AI-assisted operator, MIT terms under LICENSE. No credentials or signing keys are included.
