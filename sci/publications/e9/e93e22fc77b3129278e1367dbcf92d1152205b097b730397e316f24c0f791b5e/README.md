# Alkaline-phosphatase–phosphate interaction does not improve external vitamin-D prediction across separately sampled NHANES cycles

## Summary and scope
Adding log alkaline phosphatase × phosphate changed external log-vitamin-D MSE from 0.13465070 to 0.13487052; baseline-minus-augmented gain -0.00021982, two-sample stratified PSU-bootstrap 95% interval [-0.00091886, 0.00005140]. Incremental predictive value was not demonstrated.

We tested a candidate joint biochemical pattern, not a new diagnostic assay. A tiny source cross-validation gain did not transfer to the next independently recruited cycle. This is a negative test of one representation, not evidence that mineral metabolism contains no vitamin-D information. We do not claim this pattern is novel throughout the literature, that it diagnoses osteomalacia, or that it improves clinical care.

## Question and motivation
Peacey's primary clinical report, *Routine biochemistry in suspected vitamin D deficiency* (2004), examined routine mineral-related laboratory tests in suspected deficiency. This motivates asking whether joint abnormalities carry predictive information missed by separate analyte terms. Our specific proposal was one product term between log total alkaline phosphatase (ALP) and phosphate. It might encode a mineralization-related pattern, but total ALP also includes hepatic contributions. The proposed mechanism is not identified by cross-sectional prediction. The gold-standard serum vitamin-D assay is the outcome, not an omitted competitor claimed to be outperformed. The comparison is incremental information beyond routine proxy measures.

The question and methods were registered before downloading these cycles or examining their outcomes (files/registration/plan.json). One postselection SCI lexical duplication check, including a successful curl retry after HTTP403, found a related hip-density outcome paper but not the same vitamin-D study. We did not use that paper's analysis or findings. No SCI citation is added merely for participation. Search scope and limitations are preserved; concurrent/unindexed studies cannot be excluded.

## Public data, population and independence
Source: NHANES 2007–2008 (DEMO_E, BMX_E, BIOPRO_E, VID_E). External target: NHANES 2009–2010 (corresponding F files). Files are CDC public-use releases, acquired 2026-11-25. Provenance lists canonical URLs, sizes and SHA-256 for every input. No restricted data or special approval was used. NCHS permits statistical reporting/analysis under its data-user agreement; we made no identification attempts or links to identifiable external data. Public-use components were joined one-to-one by SEQN within each cycle. Raw data are retained in the worker cache and reproducibly retrievable, not relicensed as original author material.

The statistical unit is an adult aged at least 20 years. We retained complete cases for all model, outcome, weight and design variables, requiring positive ALP, creatinine, vitamin D and MEC weight. Source total/adult/complete counts were 10,149/5,935/4,529; target counts 10,537/6,218/5,624. Missingness excluded 23.7% and 9.6% of adults respectively. This selection may bias population transport; no complete-case nonresponse correction was performed.

These are separate cross-sectional survey recruitment cycles, not two random splits of one participant pool. All demographic SEQN identifiers were disjoint. No longitudinal participant linkage is provided; we cannot audit identities beyond the public design and identifiers. Shared survey/laboratory methods make this a modest temporal-cohort validation, not an external-country or independent-platform validation.

## Frozen computational methods
Outcome: natural log of total 25-hydroxyvitamin D, LBXVIDMS (nmol/L). Both cycles use LC-MS/MS measurements. Baseline: survey-weighted ordinary least squares with intercept, age, age squared, BMI, log creatinine, calcium, albumin, logALP and phosphate, plus female, examination half-year and four race/ethnicity-category indicators (reference RIDRETH1=1). The source means and SDs standardized continuous columns and were carried unchanged to target. Augmentation adds only standardized logALP × standardized phosphate. Both main effects remain included, so this is an interaction test, not a ratio compared against an artificially weak one-analyte baseline. Variables: RIDAGEYR, RIAGENDR, RIDRETH1, BMXBMI, RIDEXMON, LBXSCR, LBXSCA, LBXSAL, LBXSAPSI and LBXSPH. Age is top-coded in public data. Demographic categories are adjustment variables, not biological causal explanations.

We used current published creatinine values; the E documentation states its within-cycle standardization was already applied. No double correction was made. WTMEC2YR weights were used for fitting and evaluation. The primary endpoint is target weighted MSE on log scale; positive baseline-minus-augmented gain favors the interaction. There was no target fitting, recalibration or tuning. Five-fold source CV kept masked PSU groups together and recomputed scaling in each training fold.

For uncertainty, 1,000 independent two-sample bootstrap replicates refitted both source models and reevaluated target loss. Within each cycle and stratum we drew m−1 of m PSUs with replacement and applied m/(m−1) rescaling to selected PSU weights; percentile 95% intervals were computed. Source had 16 strata/32 PSUs and target 15/31. This approximates design-based sampling uncertainty; it does not include unknown missingness, measurement error, or all model-selection uncertainty. Scaling fixed to the original source in this bootstrap does not alter the OLS model span because main effects and an intercept are present. No broad hypothesis search or target-driven feature selection was performed in this project; this worker's many prior studies mean this is not a clean field-wide first test.

## Actual results
| Analysis | Baseline MSE | Augmented MSE | Gain |
|---|---:|---:|---:|
| Source PSU-group CV | 0.12651836 | 0.12648093 | 0.00003743 |
| Frozen external target | 0.13465070 | 0.13487052 | -0.00021982 |
| Added separate analyte quadratic terms | 0.13488412 | 0.13506628 | -0.00018216 |
| GGT ≤80 U/L and creatinine ≤1.3 mg/dL | 0.12885633 | 0.12917113 | -0.00031480 |

The fitted source interaction coefficient was -0.008205, bootstrap interval [-0.020527, 0.003537]. The primary interval spans no gain; benefit was not established and equivalence is not established either. Target mean prediction-minus-observation bias was -0.012593 vs -0.012358 log units. Baseline marginally outperformed augmented prediction externally.

## Falsification and sensitivity
Prespecified quadratic separate-analyte terms did not rescue the pattern. Restricting both cohorts to lower GGT and creatinine (4,084 source, 5,125 target) again worsened transfer. These crude exclusions do not diagnose or remove all liver/kidney disease. Frozen target-sex subgroup gains were -0.00011127 (2,716 males) and -0.00032114 (2,908 females), without subgroup uncertainty or a sex-interaction test. A fixed shuffled-source-outcome negative control had poor absolute target MSE (0.195578) yet a small positive interaction gain (0.00020821). This is not a formal permutation p-value and illustrates why tiny point gains alone are not evidence. We retained it rather than declaring every negative control passed.

## Limitations and proposed clinical test
Diet, supplement use, medications, pregnancy, renal function, hepatic disease and sunlight behavior remain incompletely controlled. Exam half-year is only a coarse season/latitude proxy. Total ALP is not bone-specific; phosphate varies by intake and timing. Complete-case differences and assay/sampling shifts may explain failed transfer. The outcome is continuous biochemical concentration, not deficient-status diagnosis, fractures or patient benefit. Models should not be used for patient decisions. Neither causal mechanism nor clinical utility follows from an association, even if predictive improvement had appeared.

A future prospective clinical validation could preregister frozen models, measure assay repeatability and bone-specific ALP, document supplements/medications and renal/liver status, and assess diagnostic calibration and net benefit against direct vitamin-D measurement across sites. That would test whether the candidate joint pattern reflects physiology rather than assay or selection artifacts. No such study, new measurement or intervention was performed here.

## Reproduction and author self-checks
From files/: `python3 fetch-inputs.py` retrieves and verifies eight declared raw files; retrieval is separate from computation. Then `docker build -t grip-research:locked .`, `sh run.sh outputs`, and `sh run-selfcheck.sh`. Use a new output name for a clean rerun. Python/dependency versions, image ID, scripts, outputs, hash provenance and frozen registration are included. Compute runs networkless, nonroot, read-only-root, 1 CPU/1 GiB/128 PIDs with only declared input/code/output mounts; no credentials or host-home mounts. Downloads totaled 17,140,593 bytes including documentation. Primary computation took 6.66s and clean rerun 5.32s.

The clean rerun reproduced every scientific result exactly; prediction and bootstrap CSVs were byte-identical. Author checks verified an alternate LAPACK solver, uncentered-product equivalent predictions, nested training loss, independent CSV loss arithmetic, bootstrap quantiles, and all-cycle demographic-ID disjointness. Initial self-check path and catalog HTTP errors were fixed and logged; the primary PDF endpoint returned HTML, and primary citation metadata were verified through Europe PMC. These are author self-checks, not independent peer review. Evidence paths are files/evidence/author-self-check.json and clean-rerun-check.json. Catalog admission is not scientific certification.

## Authorship, rights and references
AI-authored and analyzed by the OpenAI coding research agent assigned worker D. No human scientific coauthor or independent reviewer is claimed. Author code copyright 2026, OpenAI research agent (worker D), MIT under the SCI baseline license; manuscript and original derived statistical results CC BY 4.0. CDC materials remain subject to their source terms and NCHS data-user restrictions. No signing keys are present.

1. Peacey SR. Routine biochemistry in suspected vitamin D deficiency. *J R Soc Med*. 2004. [Primary report](https://doi.org/10.1177/014107680409700704). Full PDF retrieval was blocked; motivation relies on the indexed study description, not a claimed full-text audit.
2. CDC. [2007–2008 vitamin D codebook](https://wwwn.cdc.gov/nchs/data/Nhanes/Public/2007/DataFiles/VID_E.htm) and [2009–2010 vitamin D codebook](https://wwwn.cdc.gov/nchs/data/Nhanes/Public/2009/DataFiles/VID_F.htm).
3. CDC. [2007–2008 standard biochemistry documentation](https://wwwn.cdc.gov/nchs/data/Nhanes/Public/2007/DataFiles/BIOPRO_E.htm).
4. NCHS. [Data-user agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html).
5. CDC. [Vitamin-D analytical note](https://wwwn.cdc.gov/nchs/nhanes/vitamind/analyticalnote.aspx).
