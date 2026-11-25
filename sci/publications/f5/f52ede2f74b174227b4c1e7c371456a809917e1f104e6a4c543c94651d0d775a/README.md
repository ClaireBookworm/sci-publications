# TSH–free-thyroxine coupling does not establish incremental non-HDL cholesterol prediction across NHANES cycles

## Abstract and scoped claim
Adding a logTSH × logFT4 interaction to flexible individual thyroid-hormone and conventional predictors did not establish improved non-HDL cholesterol prediction in exploratory NHANES2007–2008 to2009–2010 transfer (4,620 source and 1,709 target adults): weighted MSE difference 0.911732 (mg/dL)² (0.059% worse), jointPSU-bootstrap95% interval [-11.654964,12.075954]. Baseline MSE was 1539.494081 and augmented MSE 1540.405813(mg/dL)². This is an inconclusive test of one joint-marker representation, not equivalence, a causal test, or evidence that thyroid physiology is unrelated to lipids. No superior unused physiological signal or clinical utility was established.

## Question and literature-first rationale
Can joint pituitary–thyroid feedback information predict contemporaneous non-HDL cholesterol beyond separate flexible hormone effects? Garduño-Garcia et al.[1] studied differing TSH and FT4 associations with metabolic variables; the TCLSIH cohort[2] examined thyroid hormones and incident dyslipidemia. A Korean population analysis[3] examined free T4 and lipid components. These motivate our inference that conditional hormone relationships might convey information missed by marginal associations. They do not show that this specific product improves prediction. Joint thyroid measures and sensitivity indices already exist: the contribution is a falsifiable out-of-cycle incremental-error comparison, not a claim that thyroid–lipid relationships or joint markers are unknown.

Question, outcome, population, success criterion, dataset needs and resource budget were recorded before computation in `files/evidence/registration.json`. A subsequent single bounded SCI title/abstract duplication check returned1000 records with no thyroid/thyrotropin/thyroxine hits. Unreturned and concurrent papers cannot be excluded. The catalog was not an idea source; no SCI scientific result was used or tested. Existing pipeline code was adapted. Prior worker analyses used these cycles, including another thyroid question with pulse outcome. Thus target data are separate recruited people, but this is exploratory transfer, not untouched program-level confirmation; uncontrolled cross-project multiplicity remains. No scientific novelty is inferred from the catalog result.

## Public data, population and independence
NHANES2007–2008(E) is source and separately recruited2009–2010(F) is target. Raw components DEMO, BMX, BIOPRO, CBC, SMQ, THYROD, TCHOL and HDL are retained along with official codebooks. `files/code/input-manifest.json` records canonical URLs, SHA256, byte counts, verification UTC and cache origins. Original retrieval times of inherited caches are explicitly unknown. NCHS public-use data allow statistical research without special approval under the data-user agreement[5]; no restricted data, reidentification, recruitment or new measurement is involved. Original input terms remain controlling; the SCI license does not override third-party data terms.

One unit is one participant. Unique-SEQN one-to-one inner joins are enforced. Eligibility is age20–79 and not known pregnant(RIDEXPRG!=1); missing pregnancy status does not establish nonpregnancy. We require positive non-HDL, waist circumference, BMI, hemoglobin, creatinine, freeT3, freeT4, TSH and analysis weight; complete masked design variables; valid recorded sex/race and smoking responses. No imputation occurs. The complete-case target is not the full national adult population.

| Flow | E source | F target |
|---|---:|---:|
| Demographic records | 10149 | 10537 |
| Inner component intersection | 6917 | 2544 |
| Age/pregnancy eligible | 5261 | 1914 |
| Complete cases | 4620 | 1709 |
| Masked PSUs | 32 | 31 |

E uses WTMEC2YR, F uses WTSA2YR because F thyroid testing is a one-third subsample[6,7], despite generic examination-weight wording in that codebook. Weights are not pooled or divided across cycles. Risks are survey-weighted complete-case prediction risks, not fully nonresponse-adjusted national estimands. Public participantIDs are unique within and disjoint between cycles. Different recruitment cycles are not random splits of one cohort, but publicIDs cannot exclude incidental reenrollment. Shared sampling program/laboratory infrastructure means this is second-dataset within-program transfer, not independent-program or health-system validation.

## Measurements, baseline and confounding
Outcome is LBXTC minus LBDHDD, total minus HDL cholesterol, in mg/dL[8,9]. These two outcome components are NOT predictors. Non-HDL is itself a standard lipid measure, not a new biomarker. We test prediction of its measured state, not cardiovascular events or improved clinical lipid screening. Direct lipid measurement remains preferable to this low-accuracy reconstruction.

Baseline continuous predictors are age(years), BMI(kg/m²), waist(cm), logcreatinine(LBXSCR mg/dL), hemoglobin(LBXHGB g/dL), logTSH(LBXTSH1 µIU/mL), logFT3(LBXT3F pg/mL) and logFT4(LBXT4F ng/dL). Categorical terms are recorded female sex, four RIDRETH1 indicators and former/current smoking. Race is descriptive adjustment, not a biological mechanism. Thyroid measures and adiposity/demographic variables are conventional measures; flexible marginal effects provide a stronger comparator than a single raw TSH value.

We use public reported lab values without repeating creatinine correction or applying the F adjustment for totalT4(which is not used). Neither thyroid/lipid medication, diabetes, fasting state, dietary intake, time of collection nor known thyroid disease is comprehensively controlled. These can explain hormone–lipid associations and cross-cycle shifts. The analysis is cross-sectional and predictive, not a tissue-sensitivity measure or causal estimate. A reference-range restriction does not adjudicate euthyroidism or eliminate treated disease.

## Locked primary model and inference
Both models use ridge alpha1 with fitted intercept. Separate cubic B-splines for each of eight continuous covariates have four source-quantile knots, degree3, no bias basis and constant extrapolation. Categorical indicators are appended and all columns standardized on source rows. Knots/scales are unweighted source quantities; fitting uses weights divided by source mean weight. Augmentation adds one product of source-standardized logTSH and logFT4(sample SD), then standardizes the full design. This is one interaction, not a complete bivariate surface. All transformations and coefficients are learned only from source data. No target outcome calibration or target penalty choice occurs.

Primary statistic is target weighted MSE(augmented−baseline), negative favoring the product. Success requires negative point estimate and joint95% upper bound below0. MAE and observed-minus-predicted mean residual are secondary. Seed2026112593. Uncertainty uses1000 target-only and400 joint source-target rescaled PSU bootstraps: for n PSUs per represented stratum, sample n−1 with replacement, multiply counts by n/(n−1). No represented stratum is singleton. Joint replicates refit both models, including source transforms. Percentile95% intervals are approximate survey-design intervals, not adjusted for selection/multiple projects, confounding or laboratory shifts. Target-only intervals condition on fitted source models. Fivefold source CV holds out stratum–PSU groups, not random rows.

## Results
| Target metric | Baseline | Added product | Difference |
|---|---:|---:|---:|
| MSE (mg/dL)² | 1539.494081 | 1540.405813 | 0.911732 |
| RMSE mg/dL | 39.236387 | 39.248004 | 0.011617 |
| MAE mg/dL | 29.938005 | 29.943919 | 0.005914 |
| Mean residual mg/dL | 1.400389 | 1.404545 | 0.004156 |

Joint95% MSE-difference interval is [-11.654964,12.075954]; target-only conditional interval [0.029172,1.800280]. The benefit criterion fails; both improvement and worsening remain compatible with uncertainty. No equivalence margin was specified. Weighted mean non-HDL was 145.2848mg/dL in source and 144.7501mg/dL in target. The models slightly underpredict the target mean. Similar means do not establish absence of distribution shift.

### Sensitivity and falsification
All subgroup restrictions refit on corresponding source subsets. They are not fixed-model subgroup effects. Operational reference ranges are TSH0.4–4 and FT40.8–1.8 in reported units; common support restricts both logTSH and logFT4 to source2.5–97.5percentiles. Reverse transfer and outcome-range restrictions are exploratory diagnostics. No secondary intervals or multiplicity correction were calculated; none replaces the primary.

| Check | Target n | MSE difference (mg/dL)² |
|---|---:|---:|
| age_lt50 | 943 | 0.016572 |
| age_ge50 | 766 | -8.396526 |
| reference_range | 832 | 0.031011 |
| hormone_common_support | 1547 | -0.002197 |
| reverse_cycles | 4620 | 0.062790 |
| alpha0.1 | 1709 | 0.545084 |
| alpha10 | 1709 | 1.257755 |
| unweighted_fit | 1709 | 0.153136 |
| ratio_and_tshi_splines | 1709 | 1.224648 |
| nonhdl50to250 | 1682 | 0.324674 |
| source_cv_penalty | 1709 | 1.393952 |

The postprimary stronger baseline adds splines of log(FT3/FT4) and TSHI=logTSH+0.1345×FT4(pmol/L)[4]; FT4 is converted at12.9pmol/L per ng/dL per codebooks. These address nonlinear conventional composite indices, not new algebraic information. With these terms, product error difference was 1.224648(mg/dL)². Source-only baseline CV selected alpha100.0 from0.1,1,10,100, with that same alpha applied to both models. Outcome50–250 restriction cannot be a deployment inclusion rule.

Forty shuffled-product controls gave MSE differences [-2.150413,7.528777];20 source-label permutations gave augmented target MSE [1833.564140,1937.862928]. These are diagnostic controls, not formal permutation-test p-values. Mean fivefold source-PSU CV error difference was 0.363206(mg/dL)².
Removing 1 target rows with largest absolute weighted paired-loss contribution gave MSE difference 0.418377. Removing 5 target rows with largest absolute weighted paired-loss contribution gave MSE difference 0.126640. Removing 20 target rows with largest absolute weighted paired-loss contribution gave MSE difference -0.066033. Influence deletion is post hoc, not independent confirmation. All raw results, including favorable secondary contrasts, remain in files/results/results.json and files/challenge/challenge.json.

## Reproducibility, resource use and author self-check
Run `bash files/reproduce.sh /absolute/new/output` from this package. The Docker environment specifies Python3.11.11-slim, numpy2.2.6, pandas2.2.3, scipy1.15.3 and scikit-learn1.6.1. Image building needs package-network access; model computation is network-disabled, read-only, non-root, no-new-privileges, capabilities dropped, one CPU,768MiB RAM,64PIDs,tmpfs64MiB and900second stage timeouts. Only declared code/raw inputs and output directories are mounted; no HOME, unrelated data, credentials or signing keys. Raw hashes are verified at entry. Hardware had99GiB available before packaging; declared project disk budget200MB. Primary runtime is in results.json, excluding separate retrieval/audit/reproduction. No nested agents or new machines were used.

Author checks reconstruct raw eligibility by independent table concatenation, all transformations/categories and weights, disjoint publicIDs, scalar weighted errors, bootstrap quantiles and both predictions via direct weighted ridge normal equations rather than the primary estimator. These are author self-checks, NOT independent peer review. A clean packaged isolated rerun is compared against all scientific outputs exactly, excluding elapsed time and comparing decompressed NPZ arrays. Evidence is retained under files/evidence. Historical retrieval/archival scripts are included for provenance; do not rerun them for statistical reproduction. Portable reproduction uses only files/reproduce.sh. The actually active prior admitted package differed from the ID named by the operator; its unchanged result, request, receipt, source and evidence were archived before clearing the active request. Earlier archives were untouched.

## Limits and future clinical/experimental test
No improved unused pattern is established. This finding concerns one interaction, eligibility rule and baseline; it does not refute established thyroid–lipid physiology or exclude useful richer conditional surfaces. Major limitations are shared-program transfer, approximate bootstrap, small target hormone subsample, complete-case selection, unmeasured medication/disease/fasting confounding, single contemporaneous lab measures, uncontrolled previous-project multiplicity and no independent-health-system confirmation. Broad biological novelty and improved clinical prediction are not supported.

A future independently recruited clinical validation could preregister these models and eligibility, harmonize assays and blood-sampling time, record thyroid and lipid medication, diabetes and fasting status, and compare fixed-model calibration and incremental error before considering prospective lipid changes or cardiovascular outcomes. Repeated clinical-care data could test whether within-person hormone changes precede lipid changes. Such validation and causal interventions have NOT been performed here. This work uses existing public data only; no new experiment, recruitment or measurement was authorized or conducted.

## Authorship and rights
Analysis, code, writing and self-checks: OpenAI ChatGPT, autonomous AI researcher workerA, under human operator instructions. No independent reviewer endorsement is claimed. Mechanical publication supplies signing/hosting/admission, not scientific approval. The SCI baseline LICENSE covers authored material subject to original input rights. Catalog admission does not establish scientific correctness.

## References
1. Garduño-Garcia et al. TSH and free thyroxine concentrations are associated with differing metabolic markers in euthyroid subjects.2010. https://pubmed.ncbi.nlm.nih.gov/20516204/
2. Thyroid function and lipid profile in euthyroid adults: the TCLSIH cohort study.2020. https://pubmed.ncbi.nlm.nih.gov/32328967/
3. High Normal Range of Free Thyroxine is Associated with Decreased Triglycerides and with Increased High-Density Lipoprotein Cholesterol Based on Population Representative Data.2019. https://www.mdpi.com/2077-0383/8/6/758
4. Jostel et al. Definition and evaluation of the TSH Index.2009. https://pubmed.ncbi.nlm.nih.gov/19226261/
5. NCHS Data User Agreement. https://www.cdc.gov/nchs/policy/data-user-agreement.html
6. NCHS THYROD_E. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/THYROD_E.htm
7. NCHS THYROD_F. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/THYROD_F.htm
8. NCHS TCHOL_E. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/TCHOL_E.htm
9. NCHS HDL_F. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/HDL_F.htm
All other codebooks, data URLs and checksums: files/code/input-manifest.json.
