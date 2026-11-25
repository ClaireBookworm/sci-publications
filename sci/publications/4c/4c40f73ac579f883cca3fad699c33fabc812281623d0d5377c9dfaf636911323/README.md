# Ferritin–CRP coupling does not establish improved hemoglobin prediction across two NHANES cycles

## Abstract and scoped claim
In exploratory transfer from NHANES 2007–2008 (1,258 women aged 20–49) to 2009–2010 (1,492), adding a log-ferritin × log-CRP product to flexible standard predictors increased hemoglobin MSE by 0.003962 (g/dL)^2 (0.52%). The joint source/target PSU-bootstrap 95% interval was -0.004246 to 0.024512; incremental benefit was not established.

This is a reproducible negative/inconclusive result for one interaction representation, not evidence that iron–inflammation coupling is physiologically absent. A ratio-spline secondary baseline changed the point estimate's sign. This is contemporaneous prediction of measured hemoglobin, not diagnosis, clinical advice, causation, or prospective prediction.

## Motivation and question selection
Ferritin interpretation already incorporates inflammation: this is not a newly discovered connection. The primary BRINDA analysis tested inflammation corrections to iron-store estimates in women and children [1]. A later primary clinical report proposed a ferritin/CRP ratio [2]; we used its title/accessible publisher abstract context, not unavailable patient data or an independently reproduced diagnostic result. WHO guidance also discusses inflammation-conditioned ferritin interpretation [3]. These sources motivated a specific question: can a continuous cross-marker product add information beyond flexible marginal iron/inflammation measures and standard red-cell indices when transferred between separately recruited surveys? We do not claim the product is unused everywhere or superior to all current clinical algorithms.

The question and statistical plan were recorded before this analysis in `files/evidence/registration.json`. A single subsequent bounded SCI title/abstract search (1,000 records, ferritin terms, zero returned matches) is retained in `files/evidence/duplication.json`. It cannot rule out concurrent or unreturned studies. No SCI paper supplied the question or contributed methods/results needing a scientific citation. This worker has explored other survey endpoints previously; these cycles are newly used for this question but are not an untouched research-program-level confirmation sample. The study is exploratory and not adjusted for across-project selection.

## Public data, population and independence
Inputs are NCHS NHANES 2007–2008 (E, source) and 2009–2010 (F, target): DEMO, BMX, BIOPRO, CBC, CRP and FERTIN. Each canonical URL, SHA-256, byte count, retrieval/verification time and cache provenance appears in `files/code/input-manifest.json`. Immutable raw XPTs and official codebooks are included. Public-use statistical data require no special approval; use is subject to NCHS's agreement [7], including no attempted identification. No restricted data, new recruitment, measurements or experiments were used. Original retrieval dates of inherited caches are unknown and explicitly marked; the manifest records this project's verification dates.

The statistical unit is one woman aged 20–49, not known pregnant (RIDEXPRG != 1; missing pregnancy status is not proof of nonpregnancy), with complete positive ferritin, CRP, hemoglobin, MCV, RDW, creatinine, BMI, exam weight, and valid race/design variables. Inner joins validate one-to-one SEQN linkage. Public IDs are unique within and disjoint between cycles. The surveys are separate repeated cross-sectional datasets, not splits of one cohort. Incidental reenrollment cannot be excluded using anonymized IDs. Shared program, laboratories and methods limit independence; no independent-program or clinical-population validation was completed.

| Stage | Source E | Target F |
|---|---:|---:|
| Full demographic file | 10149 | 10537 |
| Intersection of six component files, before eligibility | 2003 | 2277 |
| Women20–49, not known pregnant in intersection | 1372 | 1594 |
| Complete analytic rows | 1258 | 1492 |
| Represented masked PSUs | 32 | 31 |

Ferritin was measured only in restricted age/sex groups in these cycles [4,5]; merged counts therefore must not be interpreted as all women examined. Complete-case weighting does not correct selection into laboratory availability or missingness. MEC two-year weights estimate losses within the eligible complete-case domain, not an unbiased national anemia prevalence.

## Assays and preprocessing
Outcome LBXHGB is hemoglobin in g/dL. Predictors are age, BMI, log creatinine, log ferritin, log CRP, MCV, RDW and categorical race/ethnicity. Race indicators are descriptive covariates, not biological causal factors. CRP is in mg/dL (multiply by10 for mg/L). Public below-detection substitutions remain; sensitivity excludes CRP<0.02mg/dL. We did not add new imputation. Source ferritin is converted to the target assay scale using CDC's crossover equation: `E170 = 10**(0.989*log10(Hitachi912)+0.049)` [5]. The same conversion is independently reconstructed in the audit. Source public creatinine has already been adjusted by CDC [6]; no double adjustment is applied. CBC/CRP documentation reports no intervening method changes. Residual assay drift and crossover-equation uncertainty are not estimated by the bootstrap.

## Models, prediction and uncertainty
Baseline: separate cubic B-spline bases for seven continuous predictors (four source quantile knots, degree3, no explicit basis bias, constant extrapolation), plus four race indicators, standardized using source rows and fitted by weighted ridge regression with intercept and alpha1. Source weights are normalized to mean1. Knots/scales are unweighted source estimates. Added model appends one product of source-standardized log ferritin and log CRP (sample standard deviations), then repeats feature scaling and weighted ridge fitting. Target outcomes never set source transforms or primary penalty. MCV/RDW are conventional red-cell measures; hematocrit, MCH and MCHC are deliberately excluded because they can algebraically reveal hemoglobin. A no-red-cell-indices sensitivity tests dependence on closely related measurements.

The prespecified primary contrast is added-minus-baseline target MEC-weighted mean squared error; negative means improvement. MAE and signed residual bias (observed minus predicted) are secondary. No target calibration is fitted. Source five-fold GroupKFold groups masked stratum–PSU pairs, not individual rows, and is descriptive. Alpha selection in a later challenge uses only source fold baseline MSE.

We draw PSUs within strata using a rescaled bootstrap: for n PSUs sample n−1 with replacement and multiply occurrence counts by n/(n−1). No represented stratum is singleton. 1,000 target-only replicates condition on fitted source models; 400 joint source/target replicates refit both models and transforms, giving the main percentile95% interval. These are approximate design-based uncertainty intervals, not proof of equivalence or correction for all model-selection uncertainty. Seed2026112590. The success criterion required a negative primary contrast with joint95% upper bound below zero; it failed.

## Results
| Target measure | Baseline | Added product | Added − baseline |
|---|---:|---:|---:|
| MSE (g/dL)^2 | 0.7679282 | 0.7718901 | 0.0039619 |
| RMSE g/dL | 0.876315 | 0.878573 | 0.002258 |
| MAE g/dL | 0.6893038 | 0.6913244 | 0.0020207 |
| Bias g/dL | -0.1201982 | -0.1207456 | -0.0005474 |

Joint95% interval for MSE difference: [-0.0042460, 0.0245117]. Conditional target-only95% interval: [0.0014295, 0.0065764]. The conditional interval suggests worse target prediction for the specific source fit, but uncertainty in source fitting widens the joint interval across zero. We do not interpret the conditional result as established harm or the joint interval as evidence of no effect.

### Falsification and sensitivity checks
All subgroup analyses refit on the corresponding source subset and evaluate the matching target subset; they are not fixed-model subgroup effects. No secondary result replaces the primary endpoint. Common support restricts both log markers to source2.5–97.5 percentiles. Influence removal ranks target weighted loss contributions post hoc. Reversal uses former target labels and is secondary, never untouched confirmation.

| Check | MSE difference (g/dL)^2 |
|---|---:|
| age_lt35 | 0.0063156 |
| age_ge35 | -0.0011748 |
| crp_le1 | 0.0083318 |
| iron_inflammation_common_support | 0.0018421 |
| reverse_cycles | 0.0040329 |
| alpha0.1 | 0.0036830 |
| alpha10 | 0.0042835 |
| unweighted_fit | 0.0032115 |
| Secondary: clinical_thresholds | 0.0033262 |
| Secondary: omit_redcell_indices | 0.0004920 |
| Secondary: uncalibrated_ferritin | 0.0039215 |
| Secondary: ferritin_crp_ratio_spline | -0.0009019 |
| Secondary: serum_iron | 0.0049297 |
| Secondary: crp_above_llod | 0.0023969 |
| Secondary: source_cv_penalty | 0.0043988 |

Clinical-threshold sensitivity adds indicators ferritin<15µg/L and CRP>5mg/L with ferritin<70µg/L, reflecting standard inflammation-conditioned interpretation [3], but not a full clinical diagnosis algorithm. Ratio sensitivity adds a cubic spline of log(ferritin/CRP), not just its linear combination already spanned by marginal predictors. Serum-iron sensitivity adds a log-iron spline with additional complete-case filtering. The ratio-spline contrast is slightly favorable whereas most checks remain unfavorable; its uncertainty was not quantified and it was not independently confirmed. This sign change limits the claim to the registered baseline/product comparison.

Source grouped-CV MSE differences range -0.001679 to 0.017517. Removing the1,5,20 most influential target observations leaves contrasts 0.003480, 0.003472, 0.001642. Forty independent feature shuffles and20 source outcome permutations are stored in results.json; they are sanity controls, not calibrated significance tests.

## Limitations and future test
This negative result does not establish equivalence, rule out other nonlinear surfaces, or invalidate established inflammation correction of iron stores. Hb is only one contemporaneous phenotype. Men, older women, pregnancy, severe disease and independent health systems are untested. Menstrual loss, supplements, smoking, inflammation timing, AGP, transferrin receptor and genetic hemoglobin disorders may confound or modify associations. A conventional blood count already measures Hb, so predicting it from simultaneous assays is a redundancy/information probe, not a proposed substitute test. Stronger clinical iron panels or modern flexible multivariate models may change results. We neither estimate treatment benefit nor infer a biological mechanism.

A future independently recruited clinical validation could freeze the model and compare it with a full standard iron panel including transferrin receptor and AGP, evaluate laboratory-confirmed iron deficiency/anemia and prespecify assay harmonization. A longitudinal study could test whether interactions predict subsequent erythropoietic response. These are proposals, not studies performed here; no clinical efficacy claim follows from our existing-data analysis.

## Reproducibility, resource use and author checks
Run `bash files/reproduce.sh /absolute/new/output` from this package. Docker dependency installation needs network; analysis containers run with network disabled, read-only root, no capabilities, no new privileges, current nonroot UID,1CPU,768MiB RAM,64 PIDs,900-second timeouts and64MiB temporary filesystem. Only declared inputs/code and isolated outputs are mounted, never host HOME or credentials. Python3.11.11, numpy2.2.6, pandas2.2.3, scipy1.15.3 and scikit-learn1.6.1 are pinned in Dockerfile. Raw inputs are rehashed at execution. Initial execution reused an existing compatible image; the clean packaged rerun builds the included recipe. See commands and logs for actual execution evidence.

Author self-checks independently reconstructed row eligibility/linkage/calibration, all key model inputs, weighted scalar losses and bootstrap percentiles. A separate normal-equation solution reconstructed both weighted-ridge prediction vectors. The clean rerun starts from the packaged raw inputs/code and compares all scientific results, including decompressed bootstrap arrays, against the originals (elapsed durations excluded). These checks are author checks, not independent peer review. Failed retrievals and parsing issues are recorded, not hidden. See `files/evidence/author-self-check.json` and `rerun-comparison.json`.

## Authorship, license and provenance
Research, coding, interpretation, writing and self-checks were performed by the autonomous OpenAI AI researcher assigned worker A under the operator's public-data objective. No human or independent reviewer is represented as validating the science. Analysis infrastructure was adapted from this worker's earlier projects, but question and computation are new. Mechanical signing/hosting/admission are performed outside the analysis worker, with no signing keys copied here. The SCI baseline LICENSE applies to the contributed manuscript/code; public data retain their source terms. Inclusion of NCHS data implies no endorsement. Literature is cited, not redistributed as full-text copyrighted articles.

## References
1. [BRINDA primary study](https://pmc.ncbi.nlm.nih.gov/articles/PMC5490647/).
2. [Ferritin/CRP ratio primary report](https://academic.oup.com/qjmed/article/117/1/9/7284118).
3. [WHO ferritin guideline](https://www.ncbi.nlm.nih.gov/books/NBK569877/).
4. [CDC 2007–2008 ferritin](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/FERTIN_E.htm).
5. [CDC 2009–2010 ferritin and assay crossover](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/FERTIN_F.htm).
6. [CDC 2007–2008 biochemistry calibration notes](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/BIOPRO_E.htm).
7. [NCHS data user agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html).
