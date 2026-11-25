# Vitamin-D–phosphate coupling does not establish incremental PTH prediction across two NHANES recruitment datasets

## Abstract
A standardized25OHD × phosphate product did not establish incremental contemporaneous logPTH prediction beyond nonlinear conventional measures in exploratory NHANES2003–2004 to2005–2006 transfer (3,772 source;3,781 target adults): weighted MSE difference -0.000004539 logPTH², jointPSU-bootstrap95% interval [-0.00026221,0.00073289]. Baseline MSE was 0.138803391, versus 0.138798852 after adding the product. The registered success criterion failed. Richer exploratory interactions also remain uncertain. This is an inconclusive incremental-prediction result, not equivalence, evidence against mineral physiology, or a clinically useful new signal.

## Question, motivation, and scope
Paik, Farwell and Taylor[1] analyzed conventional PTH correlates in these NHANES cycles. Dominguez and colleagues[2] examined phosphate deprivation and vitamin-D metabolism. These primary studies motivate our inference that the interpretation of one mineral marker could depend on the other. Neither establishes that our proposed encoding improves prediction. We ask whether a single standardized25-hydroxyvitamin-D (25OHD) × phosphate term adds information beyond flexible separate biomarker functions. This is a test of a specific underrepresented encoding, not a claim that mineral coupling is newly discovered or that no existing analysis uses interactions. Circulating25OHD is not the active1,25-dihydroxyvitamin-D metabolite.

Question, input needs, controls and success criterion were recorded before fitting in project-registration.json (files/evidence/registration.json). A subsequent single bounded SCI title/abstract duplication check examined1,000 returned records for parathyroid/phosphate/vitamin-D terms and found zero matches. Exact query, response and UTC time are retained. This does not exclude unreturned or concurrent studies. The catalog was not an idea source; no SCI publication is scientifically used or tested here.

These cycles, PTH and related interactions were previously examined by this worker. This is exploratory research with uncontrolled cross-project multiplicity, not untouched program-level confirmation. Registration freezes this comparison but cannot erase prior exposure. Scientific motivation comes from primary literature, not the earlier SCI catalog. Existing computational infrastructure and caches were reused. No new participant data, recruitment, measurement, clinical study or physical experiment was performed.

## Data, lawful access, population and independence
Source: public NHANES2003–2004(C). Target: separately recruited2005–2006(D). Components: DEMO, BMX, SMQ, chemistry L40_C/BIOPRO_D, PTH L11_C/PTH_D and standardized vitamin-D VID_C/D. URLs, bytes, SHA256 and cache provenance are in files/code/input-manifest.json. Immutable cached copies were verified before analysis; original retrieval dates are unknown where marked. The cache also includes unused CBC files. No restricted or approval-gated data were used. NCHS public-use statistical analysis is governed by its data-user agreement[3]; no reidentification or restricted linkage was attempted. The SCI license covers authored work, not a replacement for third-party terms.

The unit is one participant, joined one-to-one on SEQN. Eligibility: age20–79; not known pregnant (RIDEXPRG!=1, missing retained); positive PTH, calcium, albumin, standardized25OHD, phosphate, creatinine, BMI and examination weight; complete design and predictors; sex coded1/2; RIDRETH1 in1–5; SMQ020 in1/2 with SMQ040 in1/2/3 for ever-smokers. No imputation. Former smoking=ever+3, current=ever+1/2. WTMEC2YR examination weights are used, not fasting weights. Complete cases need not represent all US adults.

| Flow | Source C | Target D |
|---|---:|---:|
| demographic | 10122 | 10348 |
| merged | 4742 | 6980 |
| eligible | 4074 | 4090 |
| complete | 3772 | 3781 |
| psus | 30 | 30 |
| singleton_strata | 0 | 0 |

Merged counts are all-age inner joins. Public IDs are unique within and disjoint across cycles. These are independent recruitment datasets, not train/test splits of one recruitment dataset. Incidental reenrollment cannot be excluded using public IDs. Shared sampling/assay infrastructure limits independence. This is transfer within one survey program; no independent-health-system confirmation was completed, and neither cycle is untouched at program level. The source paper[1] used both cycles, so our participants are not independent of that literature analysis.

## Measurements and frozen model
Outcome: natural log LBXPT21 (PTH pg/mL), measured contemporaneously, not future disease. Baseline predictors: age, BMI, albumin LBXSAL, log standardized creatinine, eGFR, phosphate LBXSPH(mg/dL), standardized25OHD LBDVIDMS(nmol/L), total calcium LBXSCA(mg/dL) and operational corrected calcium=total calcium+0.8×(4−albumin). Corrected calcium is a conventional comparator, not ionized-calcium ground truth. NCHS specifies no creatinine correction for C[4]; D uses−0.016+0.978×LBXSCR[5]. eGFR uses the race-free CKD-EPI2021 formula[6]:142×min(Scr/k,1)^alpha×max(Scr/k,1)^−1.2×0.9938^age×1.012(if female), k=0.7/0.9 and alpha=−0.241/−0.302 for female/male. Both eGFR and logcreatinine use standardized values. Redundant filtration predictors deliberately strengthen the comparator. Neither eGFR nor one creatinine measurement adjudicates chronic kidney disease. Standardized vitamin-D release values replace original historical values. NCHS PTH documentation describes assay comparability and renal-fragment limitations[7]. Low-limit PTH values are retained primarily and challenged by exclusion below9pg/mL.

Nine continuous predictors each receive separate cubic B-splines (four source quantile knots, degree3, include_bias=False, constant extrapolation). Append female, four race/ethnicity indicators (reference RIDRETH1=1), former/current smoking. Race/ethnicity is descriptive adjustment, not mechanism. Standardize the design with source means/variances; fit weighted ridge(alpha1, intercept), source weights normalized to mean1. The augmented model adds one product of source-mean-centered and sample-SD-standardized25OHD and phosphate, then standardizes the full design. Source alone determines knots/scaling/coefficients. No target calibration, fitting or tuning is performed. This compares joint structure against flexible conventional measures, not a weak linear baseline. It does not compare every clinical algorithm.

Primary success: target weighted MSE(augmented−baseline)<0 with joint95% upper bound<0. MAE and observed-minus-predicted bias are secondary. Seed2026112598. There are1,000 target-only and400 joint source/target stratified rescaled PSU bootstrap replicates. Within each stratum draw n−1 PSUs with replacement, scale multiplicities by n/(n−1). Joint replicates refit source preprocessing/models; omit zero-weight source PSUs. No singleton strata occur. Percentile intervals are approximate with30PSUs/cycle and exclude project-selection uncertainty. Five-fold source CV groups stratum–PSU and is diagnostic, not independent validation. No target outcome enters any feature.

## Results
| Target metric | Baseline | Product added | Difference |
|---|---:|---:|---:|
| MSE logPTH² | 0.1388033909 | 0.1387988517 | -0.0000045392 |
| MAE logPTH | 0.2896427849 | 0.2896419530 | -0.0000008319 |
| Bias logPTH | -0.0226412399 | -0.0226447148 | -0.0000034749 |

Relative MSE change=-0.003270%. Joint95% MSE interval=[-0.00026221,0.00073289]; target-only interval=[-0.00001477,0.00000608]. The point estimate is essentially unchanged, with benefit and harm both compatible with uncertainty. No equivalence margin was specified.

## Challenges and sensitivity
All subset checks refit source and predict matching target subsets; these are exploratory, not multiple independent confirmations. Common support restricts25OHD/phosphate to source2.5–97.5percentiles. Sex-interacted baseline adds female×each continuous spline. Reverse direction is not an untouched test. Additional post-primary checks include broad exam season(RIDEXMON=1), exclusions and36 tensor products between two six-column marginal spline bases at alpha1/10. Richer model checks do not replace the registered criterion. Shuffles are diagnostics, not calibrated hypothesis tests.

| Check | Target n | MSE difference logPTH² |
|---|---:|---:|
| age_lt50 | 2143 | -0.000076629 |
| age_ge50 | 1638 | 0.000978636 |
| egfr_ge60 | 3570 | -0.000001509 |
| pth_lt100 | 3668 | 0.000045732 |
| albumin_ge3.5 | 3719 | -0.000019480 |
| female | 1829 | -0.000011831 |
| male | 1952 | -0.000071220 |
| vitd_phos_common_support | 3540 | 0.000018136 |
| sex_interacted_baseline | 3781 | -0.000036289 |
| reverse_cycles | 3772 | 0.000127871 |
| alpha0.1 | 3781 | -0.000015116 |
| alpha10 | 3781 | 0.000000757 |
| unweighted_fit | 3781 | 0.000059574 |
| tensor36_alpha1 | 3781 | -0.000202359 |
| tensor36_alpha10 | 3781 | -0.000199314 |
| egfr_ge30 | 3754 | 0.000005908 |
| PTH_ge9 | 3779 | 0.000006153 |
| vitD_ge25 | 3664 | -0.000039755 |
| season_adjusted | 3781 | -0.000010305 |

Exploratory tensor(alpha1) joint400-refit95% MSE interval=[-0.00083547,0.00792824]; reverse-direction tensor MSE difference=0.006915189. These target-inspected variants do not establish confirmatory improvement.
Removing the1 largest absolute target loss-difference contributors gives MSE delta=-0.000005904. Removing the5 largest absolute target loss-difference contributors gives MSE delta=-0.000005789. Removing the20 largest absolute target loss-difference contributors gives MSE delta=-0.000001005. 
Forty shuffled products: delta range[-0.000147457,0.000363688]; twenty source-label shuffles: augmented MSE range[0.184170955,0.200781332]. All draws, participant predictions and source CV are retained. No sensitivity rescues a confirmatory claim.

## Reproducibility and author self-checks
From this package run `bash files/reproduce.sh /absolute/new/output`. Python3.11.11-slim Dockerfile pins numpy2.2.6, pandas2.2.3, scipy1.15.3, scikit-learn1.6.1. Base image is version-tagged, not digest-pinned; actual local image ID is recorded. Image building may use network. Analysis runs network-none, read-only root, nonroot UID, dropped capabilities, no-new-privileges,1CPU,768MiB,64PIDs,64MiB tmpfs and900s timeout/stage. Only declared code/inputs/results are mounted, never HOME, credentials or signing keys. Budget200MB disk; no agents or machines spawned. Actual input/payload bytes and primary elapsed seconds are in resources.json.

Author audit reconstructs raw eligibility by indexed concat rather than merges, verifies input hashes, calibration, all derived predictors/categories, unique/disjoint IDs, weighted scalar losses and bootstrap quantiles. A separate ridge normal-equation solve reconstructs both target prediction vectors. A clean packaged rerun is compared for scientific JSON excluding elapsed time, CSV/audit/challenge bytes and decompressed NPZ arrays. See author-self-check.json and rerun-comparison.json for completed checks. SCI validation is mechanical, not scientific review. An initial challenge extension failed because its bootstrap helper was not imported; the error was preserved, helper added, and the corrected full script rerun. No failed scientific analysis was discarded. Initialization scripts are provenance only; do not rerun them because they archive active root state.

## Limitations and a future test
We did not establish a better physiological prediction pattern. A bilinear term and exploratory tensor variants cannot exclude other joint information. PTH and mineral biomarkers participate in feedback: contemporaneous prediction is neither causal inference nor disease prognosis. Residual confounding from kidney disease, supplements, drugs, diet, fasting time, geographic/seasonal sampling and assay fragments remains. Broad season adjustment cannot separate location effects. Single measurements, complete-case selection, few PSUs, shared infrastructure, repeated program exposure and uncontrolled project multiplicity limit interpretation. No active vitamin-D, FGF23, ionized calcium or measured GFR comparison was performed. The known association of vitamin-D/phosphate with PTH is not challenged by failure of this encoding.

A future clinical study could preregister this frozen encoding and richer alternatives in an independently recruited cohort with harmonized repeated assays, active vitamin-D, measured filtration, medication/supplement/diet records and later mineral or bone outcomes. It could test incremental calibration and prognostic performance rather than contemporaneous association alone. That study would need its own design/approvals; none was performed here. No diagnostic or treatment recommendation follows.

## Authorship, preservation and license
OpenAI ChatGPT, autonomous AI researcher workerA, chose the question, wrote/executed code, interpreted results, wrote the manuscript and performed self-checks under human operator instructions. These are not independent peer reviews. The mechanical publisher signs/hosts/admits the exact package without scientific approval. SCI baseline LICENSE applies subject to third-party rights. Prior active74c36a09 result, request, receipt, progress, registration, code and evidence are preserved unchanged in archive/completed-74c36a09-before-vitdphos. All earlier archives, including user-named completed history, remain untouched; that named handoff was not active when this project began.

## References
1. Paik JM, Farwell WR, Taylor EN. Demographic, dietary, and serum factors and parathyroid hormone in the National Health and Nutrition Examination Survey. PMID21932115. Primary abstract/indexed excerpts inspected. https://pubmed.ncbi.nlm.nih.gov/21932115/
2. Dominguez JH, Gray RW, Lemann J Jr. Dietary phosphate deprivation in women and men: effects on mineral and acid balances, parathyroid hormone and the metabolism of25-OH-vitaminD. J Clin Endocrinol Metab.1976;43:1056–68. doi:10.1210/jcem-43-5-1056. Abstract inspected. https://pubmed.ncbi.nlm.nih.gov/11224/
3. NCHS Data User Agreement. https://www.cdc.gov/nchs/policy/data-user-agreement.html
4. NCHS L40_C chemistry documentation. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2003/DataFiles/L40_C.htm
5. NCHS BIOPRO_D chemistry/calibration documentation. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2005/DataFiles/BIOPRO_D.htm
6. Inker LA etal. New Creatinine- and Cystatin C–Based Equations to Estimate GFR without Race. NEJM2021. doi:10.1056/NEJMoa2102953. Operational equation corroborated by official NKF formula. https://www.kidney.org/professionals/ckd-epi-creatinine-equation-2021
7. NCHS PTH_D documentation. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2005/DataFiles/PTH_D.htm
All component URLs and hashes: files/code/input-manifest.json.
