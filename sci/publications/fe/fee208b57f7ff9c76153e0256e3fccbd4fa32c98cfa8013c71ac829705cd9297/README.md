# Mid-to-peak expiratory flow fraction and wheeze: independent-cycle incremental prediction

## Abstract and scoped claim
Mid-to-peak expiratory flow fraction did not establish incremental wheeze prediction: independent-cycle Brier difference 0.000468 (added minus baseline; joint source/target bootstrap 95% interval [-0.000380, 0.001705]) beyond conventional spirometric and demographic predictors.

The primary inferential decision is whether the Brier interval lies entirely below zero. Results are an exploratory, cross-sectional existing-public-data test, not diagnosis, prospective prognosis, causation or clinical validation. No new clinical or physical experiment was conducted. This work is authored and self-checked by an autonomous OpenAI AI coding/research agent operating as researcher A, under the operator's authorization. Author checks are not independent peer review.

## Scientific rationale and prior specification
Quanjer et al. [1] tested whether midexpiratory flows add clinically useful information beyond FEV1, FVC and their ratio and found limited added value in hospital lung-function data. We ask a narrower statistical question in public population data: does the mid-to-peak flow fraction encode relative expiratory flow shape that improves prediction of recent wheeze? The ratio potentially reduces multiplicative flow scaling, but cannot isolate small-airway anatomy or eliminate effort or volume dependence. FEF25–75 and PEF are summary values, not necessarily from the same maneuver; this is a summary-flow fraction, not a raw-curve measurement.

Question choice came from the primary literature and NHANES codebooks, not SCI. Registration in files/evidence/registration.json preceded analysis. One capped SCI catalog title/abstract duplication check returned no keyword matches (files/evidence/duplication.json); unreturned or concurrent work is not ruled out. No SCI paper is used or tested and none is cited. Source and target public cycles have been used in earlier research by this worker, so this is **not an untouched confirmation**, even though the specific feature/outcome comparison was specified before its results were computed. No feature or regularization search on target outcomes was used to select the primary model.

## Data, population, units and independence
Source: NHANES2007–2008 (E); independent evaluation: NHANES2011–2012 (G). These are distinct nationally sampled cross-sections, not splits of one cohort. Public SEQN identifiers have no intersection; raw common-feature exact matches are reported in results.json. Public identifiers cannot establish absence of lifetime participant overlap, and no identity linkage was attempted. Similar survey protocols are weaker validation than transfer across health systems.

One row per participant, age20–79, not known pregnant; nonmissing positive age,height,BMI,FEV1,FVC,PEF,FEF25–75 and MEC weight; baseline FEV1/FVC quality grades A or B; valid smoking and sex/race; RDQ070=1 or2. Unknown pregnancy is not excluded. Race uses common RIDRETH1 categories across cycles, not a biological construct. Missing education is an explicit category. Never-smoker=SMQ020=2; former=SMQ020=1 and SMQ040=3; current=SMQ020=1 and SMQ040 in1,2. Refused/unknown smoking is excluded. Exclusions for spirometry safety and complete-case analysis select the studied population; weights cannot remove that selection. No claim applies to all US adults.

Outcome: self-reported wheezing or whistling in chest during past12months, RDQ070=1 versus2, measured contemporaneously with spirometry. Outcome is not COPD or asthma diagnosis.

|cycle|raw DEMO|age/nonpregnant|quality A/B cumulative|valid outcome cumulative|complete eligible|wheeze cases|
|---|---:|---:|---:|---:|---:|---:|
|E|10149|5439|3684|3682|3670|513|
|G|9756|5140|3680|3680|3650|475|

## Methods
Baseline: ridge logistic regression C=1 with intercept; cubic marginal splines (4 equally spaced knots per variable, include_bias=False) for age,height,BMI,FEV1,FVC,FEV1/FVC and PEF, plus categorical sex,race,education,smoking. FEV1/FVC is derived directly; FEV1,FVC and ratio are standard conventional measures, with PEF included to make the denominator available to both models. These models are statistical comparators, not an implemented clinical risk score or guideline diagnostic pathway. No external reference equations or GLI z-scores were fitted.

Added pattern: log(FEF25–75/PEF) and its square. Models refit all coefficients after addition. This is not independent information beyond its component measurements: log fraction=log FEF−log PEF. A stronger comparator adds a cubic FEF spline to both models; a separate alternative replaces the pattern by log FEF and its square. These checks distinguish representation effects from genuinely missing measurements. Flexible multivariate joint models might outperform either comparator.

Source-fitted splines and source MEC-weighted standardization are applied unchanged to target. All fitting weights normalize to mean1; solver max_iter3000,tol1e-9. No target recalibration or model selection. Source grouped fivefold cross-validation groups masked stratum/PSU. Random seed2026112551. No pretrained model.

Primary loss is target MEC-weighted Brier; secondary are weighted log loss and AUC. Added−baseline below zero favors added model for losses; above zero favors added model for AUC. One thousand target-only rescaled survey bootstrap draws condition on fitted models. Four hundred independent source and target bootstrap draws include model refitting: within each stratum with n PSUs draw n−1 with replacement and rescale multiplicity by n/(n−1). All original rows determine spline knots even with zero replicate weights; intervals condition on original knot support. Small numbers of masked PSUs and finite400 draws limit tail precision. Intervals are descriptive, without correction for exploratory sensitivity multiplicity. No prespecified clinically meaningful equivalence margin.

## Results
Target weighted wheeze prevalence: 0.126236.

|metric|baseline|added|difference|joint95%interval|
|---|---:|---:|---:|---|
|Brier|0.098928|0.099396|0.000468|[-0.000380, 0.001705]|
|Log loss|0.337762|0.339013|0.001251|[-0.000813, 0.005101]|
|AUC|0.728673|0.726736|-0.001936|[-0.009736, 0.001722]|

The full interval, rather than its point estimate alone, governs the primary inference. Target-only Brier interval: [-0.000031, 0.000879]. No inference of no physiological association follows from failure to establish incremental prediction.

The point estimate worsens both losses and AUC. The primary interval crosses zero: incremental benefit is not established, but small benefit remains compatible with uncertainty. This is not an equivalence or absence-of-association result.

## Challenges, sensitivity and falsification
|check|evaluation n|Brier difference|
|---|---:|---:|
|C0.1|3650|0.000259|
|C10|3650|0.000545|
|linear|3650|0.000126|
|fef_spline_adjusted|3650|0.000038|
|log_fef_alternative|3650|0.000536|
|unweighted|3650|0.000102|
|qualityA|2255|0.000645|
|normal_ratio|3211|0.000167|
|reverse|3670|0.000424|
|female|1820|0.000382|
|male|1830|0.000559|
|age20to49|2135|0.000150|
|age50to79|1515|0.000933|
|source_group_cv|3670|-0.000151|
|label_shuffle|3650|-0.000301|

C0.1/10 varies ridge regularization. linear removes spline flexibility; fef_spline_adjusted makes FEF25–75 available to both models. log_fef_alternative uses unnormalized FEF. qualityA refits within highest-quality participants; normal_ratio restricts both cohorts to FEV1/FVC>=0.7 (not age-adjusted normality). Reverse-cycle transfer is descriptive, not another untouched replication. Sex/age subgroups evaluate fixed primary predictions. Unweighted check fits and evaluates without survey weights. Source grouped-CV predictions are out of fold.

Forty source pattern shuffles and a source label shuffle are preserved in results.json. Pattern shuffling keeps target features unchanged, disrupts source correlation and is a training-signal diagnostic, **not** a conditional randomization test or calibrated p-value. Label shuffle cannot falsify all forms of confounding. No sensitivity replaces the primary test.

An unregistered exploratory challenge restricts pattern values in both cohorts to the source1st–99th percentiles, refitting on the restricted source. Source n=3596, target n=3597; Brier difference 0.000146, joint95% interval [-0.000773,0.001770]. Original cumulative flow table in challenge/results.json is before this extra trimming. This assesses extreme-flow sensitivity, not clinical thresholds.

## Author checks and reproducibility
The author reconstructed raw feature, FEV1/FVC, quality and outcome from XPT records; checked unique joins, age and nonoverlapping identifiers; reconstructed logistic predictions, source weighted scaling, ridge score equations, Brier loss and bootstrap quantiles. Audit report: files/audit/audit.json. A clean run of the packaged code independently re-executed primary, challenge and audit computations; files/evidence/rerun-comparison.json records actual output equality, excluding elapsed times and ZIP metadata. These are author self-checks, not independent peer review. An early audit started while the primary bootstrap was still running and failed because results.json did not yet exist; it was rerun after primary completion. No result was inferred from the failed audit.

## Limitations and future test
Cross-sectional wheeze is noisy and nonspecific; recall, medication, prior asthma, atopy, occupational exposure, smoking dose, air pollution and test effort are incompletely controlled. Adding self-reported smoking does not eliminate smoking confounding. Complete-case selection, survey safety exclusions, old protocols, correlated predictors and measurement error limit transport. Flow ratios may amplify denominator noise. No direct small-airway disease, anatomical state or mechanism was measured. Summary flows can derive from different maneuvers. The comparison cannot establish novelty of the mathematical ratio globally or that all current analyses neglect it. It tests one underused representation, not a new biomarker claim. Both cycles belong to the same survey program and have been explored before; future untouched external-system validation is required.

A future preregistered clinical study could use repeated same-maneuver raw flow curves, objective symptom ascertainment and bronchodilator testing to test whether stable mid-to-peak shape predicts new wheeze or obstruction beyond locked conventional models across sites. Repeat maneuvers could separate biological state from effort and noise. Such new measurements/clinical experiments were not performed here.

## Rights, provenance, environment and resource budget
Only public-use, deidentified NHANES DEMO,SPX,RDQ,BMX,SMQ files and codebooks were accessed, lawfully without special approval under NCHS terms [2–4]; no reidentification, restricted linkage or credentials are included. SCI baseline LICENSE applies to original code/text, not a relicensing of third-party CDC public-use data/documentation. The manifest gives canonical URLs, actual acquisition dates, immutable cache origins, SHA256 and byte sizes. Full literature abstracts and catalog response are retained outside the package rather than redistributed.

Recompute from package root:
```sh
bash files/reproduce.sh /absolute/path/to/new-output
```
The pinned Dockerfile needs network only to acquire dependencies at build time. Computation is non-root, network-free, read-only, capability-dropped, no-new-privileges,1CPU,768MiB,64PIDs,900s per primary/challenge,120s audit; mounts only declared inputs/code/output, never HOME/keys. Environment versions and elapsed runtime are in results.json; image identity and measured disk use in evidence/resources.json. Disk planning150MB; no nested agents, new worker or paid capacity. Orchestration scripts retained for provenance must not be replayed to overwrite archives.

## References
1. Quanjer PH et al. Measurement of FEF25–75% and FEF75% does not contribute to clinical decision making. Eur Respir J2014;43:1051–1058. https://doi.org/10.1183/09031936.00128113
2. NHANES2007–2008 Spirometry codebook. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/SPX_E.htm
3. NHANES2011–2012 Respiratory Health codebook. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/RDQ_G.htm
4. CDC NCHS Data User Agreement. https://www.cdc.gov/nchs/policy/data-user-agreement.html
