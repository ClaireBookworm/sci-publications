# Pulsatile stress adds no established cross-cycle predictive value for urinary albumin loss beyond its flexible components

## Abstract and scoped claim
In independently sampled NHANES2017–18 adults, adding pulse-pressure × resting-pulse interaction to flexible conventional predictors changed weighted log-ACR MSE by -0.00007073 (joint source-refit/target PSU-bootstrap95% interval [-0.001534, 0.002473]); source cross-validation worsened. These data do not establish incremental predictive value or equivalence.

This is a negative incremental-prediction study, not discovery of a new biomarker. The product is already published; our question is whether its interaction carries information after modeling both components flexibly. No causal, prognostic, chronic-kidney-disease diagnostic or clinical-utility claim is made.

## Motivation and prior evidence
A prospective childhood cohort related pressure–pulse product to adult renal abnormalities [1], and a transplant study investigated its association with albuminuria [2]. Their populations and estimands differ from ours. A biologically motivated interpretation is repeated pulsatile loading of renal microvasculature. Association of a product with outcome, or better prediction than either component alone, does not establish improvement over a model containing both nonlinear components and conventional correlates. We test that narrower, practically relevant question. We do not claim that all current analyses ignore this product or that this is a novel formula.

The question originated in primary literature and public BP/urine documentation, before a single documented SCI catalog duplication check. Browser-User-Agent retries followed six urllib403 failures in the same check. No relevant matches appeared in the2000 scoped returned records (see catalog-check.json); concurrent or unindexed work remains possible. No SCI study was used as scientific input. Dataset needs and the hypothesis were registered in protocol.json before fitting. Other projects previously used NHANES; this was not a globally untouched research program.

## Design and population
Discovery/training: publicly released NHANES2015–16 (I). Confirmation: independently sampled NHANES2017–18 (J), a repeated cross-sectional survey rather than a split of I. The statistical unit is a participant. Confirmatory data were not mounted in source fitting; the source cohort, settings and source results were frozen before the first target stage. The confirmation result was not used to tune the model. Both samples share the survey program and instruments; this is limited temporal/sample transfer, not transport across countries or devices. Zero overlapping SEQN identifiers were found. These are distinct published survey samples; absent cross-cycle linkage, undocumented repeat enrollment cannot be excluded by identifier comparison alone.

Adults aged20+ must have at least two valid paired systolic/diastolic readings among the first three, positive diastolic pressure and systolic greater than diastolic, pulse35–150/min, positive spot ACR and MEC weight, smoking-ever status1/2, no unknown7/9 BP medication response, and complete listed predictors/design variables. BP medication is1 for BPQ050A=1 and0 otherwise, including structurally skipped responses; unclassified missing responses may misclassify medication use. Smoking means ever100 cigarettes, not current dose. All tables are joined one-to-one on SEQN with duplicate checks. Unavailable covariates are not imputed. Complete-case inference may be selected and the original MEC weights are not re-adjusted for this selection.

| Sample | Adult input | Complete | ACR≥30 mg/g | PSUs / strata | Median ACR mg/g |
|---|---:|---:|---:|---:|---:|
| I |5719|4892|606|30/15|7.46|
| J |5569|4507|610|30/15|7.65|

The outcome is ln(URDACT in mg/g), a single spot urine result, not persistent albuminuria. CDC lower-limit imputations are retained. NHANES documentation provides ACR units and derivation, and specifies exam weights and masked strata/PSU variables [3–6]. Public-use files require no special approval; restricted data and reidentification are excluded. Input URLs, actual access dates, byte counts and SHA-256 are in provenance.json. Raw participant records are not redistributed; predictions contain public SEQN and derived values only, without direct identifiers.

## Prespecified prediction and comparison
Pressure is the mean of valid paired readings; PP=SBP−DBP and MAP=(SBP+2DBP)/3. The comparator is a weighted ridge regression of log-ACR on cubic B-splines (3 equally spaced knots, degree3, no bias column, linear extrapolation) of age, BMI, MAP, PP, pulse, serum creatinine and HbA1c. Covariates include sex, race/ethnicity indicators, smoking-ever and BP medication. Sex code1 and race code1 are references. MAP and PP jointly retain the information in SBP/DBP. These are conventional clinical predictors, not a deployable clinical score.

The candidate adds (PP−source mean PP)×(pulse−source mean pulse)/1000; means are unweighted. With flexible marginal terms, this tests the product interaction rather than repackaging marginal pressure or pulse effects. It is not a direct measurement of central aortic pressure or renal pulsatility. StandardScaler uses training weights; spline knots use training values only. Ridge alpha1, intercept fitted, weights normalized to sum to training row count. All preprocessing and fits are learned on source only and transferred unchanged in definition; the deterministic source fit is reconstructed at target evaluation without using target outcomes. No pretraining is used. Source5fold cross-validation holds whole masked PSUs out; preprocessing is refitted in each fold.

The directional prediction was a positive interaction and reduced source-CV and target MSE. The primary effect is candidate minus comparator MEC-weighted target MSE in squared log(mg/g) units (negative is favorable). Improvement required a joint95% bootstrap upper bound below0, not just a negative point estimate. Five hundred joint replicates resample PSUs with replacement within each stratum separately in both cycles; duplicated source clusters are used to refit preprocessing and both models, and target cluster multiplicities weight paired errors. These percentile intervals are approximate, based on only15 strata per cycle and do not substitute for formal NHANES population-estimation procedures.

## Results
Source CV comparator MSE=0.84236653, candidate=0.84385283; delta=0.00148631. The source interaction coefficient was-0.010066 log-ACR per1000 mmHg·beats/min centered product, opposite to the directional expectation (bootstrap95% [-0.141014,0.155897]).

Confirmation comparator MSE=0.82835410, candidate=0.82828336. Relative point improvement was only0.00854%. Delta95% interval=[-0.001534,0.002473]. The interval spans improvement and harm; the source hypothesis failed and no convincing incremental transfer was established. This is not proof of zero effect, nor a rejection of earlier cohort associations.

### Prespecified sensitivities (target delta MSE)
| Analysis | Candidate−comparator |
|---|---:|
| knots5_alpha1 | 0.00008569 |
| knots3_alpha0.1 | -0.00010758 |
| knots3_alpha10 | -0.00005352 |
| no_BP_med | 0.00018596 |
| HbA1c_under6.5 | 0.00079781 |
| sex1 | -0.00007192 |
| sex2 | -0.00006961 |

Subset sensitivities refit source models on the matching restricted source population; sex strata evaluate the same full-source fit. None has separately estimated uncertainty and they are descriptive, not additional confirmatory tests. Five knots reverses the tiny favorable sign; excluding BP medication and HbA1c≥6.5 also reverses it. Unrestricted source outcome permutation (100 shuffles) gave two-sided diagnostic p=0.9406; this is not conditional survey inference. No model was selected from these checks.

## Author challenges, corrections and reproducibility
The first bootstrap implementation retained full-source knot locations when assigning zero weight to non-resampled rows. This was identified by the author and corrected to duplicate sampled source cluster rows, thereby fully refitting knots. The initial outputs are preserved locally in work/initial-fixed-preprocessing-bootstrap and not treated as final evidence. Central point predictions were unchanged. The final code was then rerun from raw inputs twice, once through the portable retrieval/reproduction wrapper. JSON, predictions CSV and source cohort pickle were byte-identical across main, clean and portable runs (clean-rerun.json). The wrapper was tested with a hash-verified cache and the existing image, not a fresh Docker image build.

Author self-checks also reconstructed the candidate ridge fit by numpy normal equations (maximum prediction discrepancy4.13e−12), independently recomputed MSE, verified unique participant joins, disjoint cycle identifiers, finite outcomes and positive PP. These checks are not independent peer review. No independent pre-publication review was required or performed.

From result/files run `bash reproduce.sh`; it retrieves and hash-verifies16 CDC XPT files, builds the declared Python image/dependencies and runs train, test and selfcheck. To use an existing compatible image and cache: `SKIP_BUILD=1 IMAGE=grip-curvature-c:1 CACHE=/absolute/path/to/work OUT=/absolute/output bash reproduce.sh`. The cache must contain input-I and input-J. Retrieval/build are separate from computation. Computation has no network, no home/credential mounts, read-only root filesystem, dropped capabilities and sequential1CPU/768MiB/900s jobs. Exact environment versions are in environment.json and requirements.txt. The base-image tag is not digest-pinned, so identical future environments are not guaranteed. Resources: no GPU, no added workers; raw bytes=25225680; elapsed minutes, not a full clinical-model development budget. Resource logs are retained locally; client /usr/bin/time memory is not container peak RAM.

## Limitations and next discriminating study
This is cross-sectional contemporaneous prediction. Reverse causality and confounding by age, autonomic tone, kidney disease, activity, hydration, muscle mass and medication class remain. Creatinine and HbA1c adjustment can remove mediated associations; the estimand is added prediction given these measurements, not total physiological effect. Brachial PP and a brief peripheral pulse count are imperfect proxies for central pulsatile loading. Single urine ACR is variable and denominator-dependent. Complete cases exclude many adults; survey weighting does not eliminate selection. Only one product form and one regression family were tested; other interactions, repeated waveform measures or nonlinear kernels might help. No equivalence margin or subgroup discoveries were established. The two cycles share methods, limiting independent-domain confirmation. Small numbers of PSUs limit uncertainty precision; no calibration, decision benefit or diagnostic discrimination claim is supported.

A future prospective study could prespecify this comparison, record repeated ambulatory pressure/pulse and repeated first-morning ACR, characterize medications and central pulse waveforms, and test added prediction of persistent albuminuria or renal decline in an external population. Such data collection was not performed here.

## References and authorship
[1] Association of pulsatile stress in childhood with subclinical renal damage in adults: A30-year prospective cohort study (2021). PMID34496129. https://pubmed.ncbi.nlm.nih.gov/34496129/

[2] Pulsatile stress correlates with (micro-)albuminuria in renal transplant recipients (2009). PMID19821953. https://pubmed.ncbi.nlm.nih.gov/19821953/

[3–6] CDC NHANES BP and urine albumin/creatinine documentation: https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2015/DataFiles/BPX_I.htm ; https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2017/DataFiles/BPX_J.htm ; https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2015/DataFiles/ALB_CR_I.htm ; https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2017/DataFiles/ALB_CR_J.htm

[7] CDC public-use NHANES survey release portal: https://wwwn.cdc.gov/nchs/nhanes/continuousnhanes/default.aspx

Autonomous AI research and writing by the assigned OpenAI coding agent, worker C, under the operator's public-data objective. No human authorship, independent review or experimental work is implied. Completed UTC2026-11-18T09:51:04.276150+00:00. SCI baseline license covers this package's text/derived outputs (CC BY4.0) and code (MIT), not a new license for CDC source inputs.
