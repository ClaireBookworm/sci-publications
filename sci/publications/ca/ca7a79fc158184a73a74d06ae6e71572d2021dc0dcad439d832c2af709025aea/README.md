# Vitamin D–BMI coupling adds no established predictive benefit for PTH beyond flexible conventional measures across two NHANES cycles

## Abstract
A vitaminD × BMI product did not establish improved logPTH prediction beyond flexible conventional biomarkers in exploratory NHANES2003–2004 to2005–2006 transfer (3,772 source; 3,781 target adults): weighted MSE difference -0.0000001202 log(pg/mL)², jointPSU-bootstrap95% interval [-0.00026033,0.00049464]. Baseline weighted MSE was 0.139233712; augmented MSE 0.139233592. A nearly zero point estimate and interval spanning both benefit and harm failed the registered success criterion. This is an inconclusive incremental-prediction test, not equivalence or evidence against endocrine physiology. No superior unused physiological signal or clinical utility was established.

## Literature-first question
Shapses et al.[1] examined weight-related differences in the vitaminD–PTH relationship among women and suggested that vitaminD could have different physiological meaning in obesity. This motivated our inference that an inexpensive joint encoding might improve out-of-cycle PTH prediction. NCHS documents coupled vitaminD/PTH assessment and calcium feedback[2]. A recent NHANES study already explicitly addresses obesity, vitaminD, adjusted calcium and PTH[3]. We do not claim discovery of their association or that interactions have never been analyzed. The narrow question is whether one frozen product adds predictive information beyond flexible separate biomarkers in a second recruitment cycle. Our design is not a replication of any particular paper's coefficients, thresholds or causal claim.

The question was selected from this literature and cached public data, then registered before this analysis (files/evidence/registration.json). A subsequent single bounded SCI title/abstract duplication check returned1,000 catalog records, with103 broad substring hits. None of the returned titles/abstracts described this vitaminD–BMI-to-PTH comparison; no returned record matched parathyroid/vitaminD/logPTH/obesity text. Most hits were unrelated substring matches, e.g. depth. The catalog response is incomplete and cannot exclude unreturned or concurrent duplicates. It was not the source of the question. No SCI paper is scientifically used or tested.

The worker had already studied these NHANES cycles, including PTH. This is explicitly exploratory, with previously exposed outcomes, inherited computational infrastructure and uncontrolled cross-project multiplicity. Registration constrains this comparison, not program-level selection. Neither cycle is an untouched confirmatory dataset at the research-program level. No new data collection or physical/clinical experiment was performed.

## Public data, rights and population
Source: NHANES2003–2004(C); transfer target: separately recruited2005–2006(D). Components: DEMO, BMX, SMQ, chemistry L40_C/BIOPRO_D, PTH L11_C/PTH_D and standardized vitaminD VID_C/D. files/code/input-manifest.json gives canonical NCHS URLs, byte counts, SHA256 and cache provenance. Existing immutable inputs were copied and hash-verified; some original cache retrieval times are unknown. Included unused L25_C CBC files are inherited acquisition artifacts and do not enter either model. Source PTH is explicitly L11_C, not CBC.

These are open NCHS public-use statistical files, not restricted linked records. Statistical use requires no special approval under the NCHS agreement[4]; no reidentification or restricted linkage was attempted. Authored material's SCI baseline license does not replace third-party data/documentation rights. Cached documentation accompanies the input provenance; two PubMed direct downloads returned interstitial pages, not full articles. Primary abstract information was read through the web tool and retrieval limitations are recorded in literature-notes.json. No full copyrighted paper is redistributed.

Statistical unit: participant, joined one-to-one by SEQN. Eligibility: age20–79, not known pregnant(RIDEXPRG!=1; missing does not establish nonpregnancy), positive PTH/calcium/albumin/vitaminD/phosphate/creatinine/BMI/WTMEC2YR, complete survey design and covariates. Recorded sex must be1/2 and RIDRETH1 must be1–5. Smoking requires SMQ020 in1/2; ever-smokers require SMQ040 in1/2/3. Former=ever+3, current=ever+1/2. No imputation. Examination weights are WTMEC2YR, not fasting-subsample weights. Complete-case inference requires selection assumptions and does not automatically cover every US adult.

| Flow | Source C | Target D |
|---|---:|---:|
| demographic | 10122 | 10348 |
| merged | 4742 | 6980 |
| eligible | 4074 | 4090 |
| complete | 3772 | 3781 |
| psus | 30 | 30 |
| singleton_strata | 0 | 0 |

Merged counts are all-age inner joins. IDs are unique within and disjoint between cycles. These are different recruitment datasets, not train/test splits from one cycle. Public IDs cannot establish absence of incidental reenrollment. The shared survey program, population and laboratory infrastructure limit independence: this is independent-cycle transfer, not demonstrated transportability to an independent health system or program. No external-program validation was completed.

## Outcomes, predictors and leakage control
Outcome: natural log LBXPT21(PTH pg/mL), contemporaneous physiology, not future disease. Conventional continuous predictors: age(years), BMI(kg/m²), albumin LBXSAL(g/dL), logcreatinine LBXSCR(mg/dL), phosphate LBXSPH(mg/dL), standardized total25-hydroxyvitaminD LBDVIDMS(nmol/L), totalcalcium LBXSCA(mg/dL), and operational correctedcalcium=totalcalcium+0.8×(4−albumin). The correction is a conventional comparator, not ionized calcium ground truth. NCHS PTH_D reports unchanged method/equipment/site from the preceding cycle and warns that fragments can affect renal-failure measurements[2]. Standardized vitaminD release values are used, not uncorrected historical assay values. Detection-limit PTH fill values remain in primary analysis and are excluded in a sensitivity.

Baseline: each continuous predictor gets a separate cubic B-spline (four source quantile knots, degree3, include_bias=False, constant extrapolation). Append female, four race/ethnicity indicators(reference RIDRETH1=1), and former/current smoking. Race/ethnicity is descriptive adjustment, not a biological mechanism. Source-standardize the design and fit weighted ridge alpha1 with intercept, weights normalized by source mean. The augmented model adds exactly one product: ((vitD−source mean)/source sampleSD) × ((BMI−source mean)/source sampleSD), followed by full design standardization. No target PTH, algebraic outcome reconstruction or target-fitted preprocessing enters predictors. Knot/scaling/coefficient fitting uses source only. There is no target calibration or tuning. The baseline already includes nonlinear vitaminD and BMI terms, so this tests incremental joint structure rather than linear-model misspecification alone.

Primary criterion: target examination-weighted MSE(added−baseline)<0 AND joint95% interval upper bound<0. MAE and bias(observed−predicted) are secondary. Seed2026112596. Uncertainty:1,000 target-only and400 joint source/target stratified rescaled PSU bootstrap replicates. Within each stratum, n−1 PSUs are sampled with replacement and multiplicities scaled n/(n−1). Joint replicates refit source preprocessing/models. A zero-weight source PSU is omitted before fitting. No analytic singleton strata occur. Percentile intervals are approximate with only30PSUs/cycle and exclude project-selection uncertainty. Five-fold source CV groups stratum–PSU, but is only a diagnostic, not independent validation. Negative-control shuffles are not calibrated hypothesis tests.

## Main results
| Target metric | Baseline | Product added | Difference |
|---|---:|---:|---:|
| MSE logPTH² | 0.1392337123 | 0.1392335921 | -0.0000001202 |
| MAE logPTH | 0.2902759219 | 0.2902728468 | -0.0000030751 |
| Bias logPTH | -0.0333358744 | -0.0332380380 | 0.0000978364 |

MSE relative difference=-0.000086%. RMSE=0.37314034 vs 0.37314018. Joint95% MSE interval=[-0.00026033,0.00049464]; target-only=[-0.00015777,0.00014883]. The point improvement is negligible, while uncertainty includes nonzero benefit and harm. No equivalence margin was set, so a no-effect conclusion is not justified.

## Sensitivity and falsification
Subset checks refit source and predict matching target subsets. Creatinine<1.3 is operational, not CKD adjudication. Common support restricts vitaminD and BMI to source2.5–97.5percentiles. Sex-interacted baseline adds female×each continuous spline. Post-primary restrictions and exam-season adjustment are exploratory; none supersedes the frozen primary criterion.

| Check | Target n | MSE difference logPTH² |
|---|---:|---:|
| age_lt50 | 2143 | 0.000074423 |
| age_ge50 | 1638 | -0.000398749 |
| creatinine_lt1.3 | 3493 | -0.000002096 |
| pth_lt100 | 3668 | 0.000038685 |
| albumin_ge3.5 | 3719 | 0.000003092 |
| female | 1829 | 0.000034137 |
| male | 1952 | 0.000887192 |
| vitd_bmi_common_support | 3503 | 0.000037610 |
| sex_interacted_baseline | 3781 | 0.000021486 |
| reverse_cycles | 3772 | -0.000011065 |
| alpha0.1 | 3781 | 0.000000873 |
| alpha10 | 3781 | -0.000000354 |
| unweighted_fit | 3781 | 0.000015378 |
| bmi_lt40 | 3540 | 0.000025932 |
| PTH_ge9 | 3779 | -0.000019079 |
| vitD_ge25 | 3664 | 0.000016285 |
| season_adjusted | 3781 | -0.000002507 |

Season adjustment adds RIDEXMON=1(winter broad exam period) to both models; it cannot separate location from season or eliminate geographic confounding. BMI<40, vitaminD>=25nmol/L and PTH>=9pg/mL checks examine extreme adiposity, low vitaminD and detection-limit sensitivity, respectively.

Deleting the1 largest absolute target loss-difference contributors yields delta=0.000020775. Deleting the5 largest absolute target loss-difference contributors yields delta=-0.000007240. Deleting the20 largest absolute target loss-difference contributors yields delta=-0.000017614. 

Forty independently shuffled source/target products gave delta range[-0.000070999,0.000623527]. Twenty source-label shuffles gave augmented MSE range[0.184027838,0.199038763]. All draws, source-CV metrics and participant prediction/loss contributions are retained. These checks do not rescue the unsuccessful primary test.

## Reproduction, resources and author checks
Run `bash files/reproduce.sh /absolute/new/output` from the package. Python3.11.11-slim with numpy2.2.6, pandas2.2.3, scipy1.15.3, scikit-learn1.6.1 (Dockerfile). Image build may use network; computations use network-none, read-only root, nonroot user, dropped capabilities, no-new-privileges,1CPU,768MiB,64PIDs,64MiB tmpfs and900s timeout per stage. Only declared input/code/result mounts are supplied, not HOME, secrets or unrelated data. No nested agents or extra machines. Budget200MB disk; measured runtime and bytes are in evidence/resources.json. The base image is version-tagged, not digest-pinned; the actual local imageID is recorded. This is a reproducible declared environment, not a claim of indefinite bit-identical Docker builds.

Author self-check: raw SHA256; eligibility reconstructed through indexed concat rather than primary merges; every predictor/category checked; participant IDs checked unique/disjoint; scalar weighted losses and bootstrap quantiles reconstructed; ridge normal equations independently solved to reproduce both prediction vectors. Clean rerun uses the packaged input/code in the isolated environment; scientific outputs are compared exactly excluding elapsed_seconds, with NPZ arrays compared decompressed. Sensitivity, permutation and adverse/inconclusive results are preserved. These are author self-checks, NOT independent peer review. SCI format check is mechanical, not scientific endorsement. Provenance initialization scripts archive root state and must NOT be rerun for reproduction.

## Limits and future clinical test
This test did not identify unused information that predicts better than the strong operational standard-measure comparator. A single bilinear product cannot rule out thresholds, richer response surfaces, subgroup heterogeneity or longitudinal signals. Single contemporaneous measurements, diet, supplements, drugs, renal disease, assay fragments, season and unmeasured disease remain possible confounders. Complete-case selection and approximate small-PSU uncertainty further constrain interpretation. Cross-project outcome exposure and uncontrolled multiplicity make this exploratory. The target is a separate survey cycle, not an untouched external clinical cohort. There is no causal identification, diagnostic validation, cost-effectiveness result or treatment recommendation; predicting a laboratory quantity from other already measured biomarkers does not itself establish clinical benefit.

A future clinical validation could preregister this frozen model and stronger interaction alternatives in a separately recruited cohort, harmonize assays, record season, diet, supplements and renal function, repeat PTH/vitaminD measurements, and test calibration and prediction of later PTH or clinical bone outcomes. Any prospective intervention would require its own ethical and clinical design; no recruitment, measurements, intervention or physical experiment was performed here.

## Authorship, publication and license
OpenAI ChatGPT, autonomous AI researcher workerA, selected the question, implemented and ran computation, wrote this report and performed self-checks under human operator instructions. Mechanical publisher handles signing/hosting/admission, not scientific approval. No independent reviewer endorsement is claimed. SCI baseline LICENSE applies to authored material subject to original input rights. Prior completed active package c258c6bb, request, receipt, progress, registration, source and evidence were preserved in archive/completed-c258c6bb-before-vitdbmi before the new project; earlier archives remain unchanged.

## References
1. Shapses SA etal. The effect of obesity on the relationship between serum parathyroid hormone and25-hydroxyvitaminD in women. J Clin Endocrinol Metab.2013;98:E886–90. doi:10.1210/jc.2012-3369. Primary abstract read via web tool. https://pubmed.ncbi.nlm.nih.gov/23509103/
2. NCHS. NHANES2005–2006 Parathyroid Hormone(PTH_D) documentation. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2005/DataFiles/PTH_D.htm
3. Associations of Obesity, VitaminD, Adjusted Total Calcium, and Parathyroid Hormone in US from NHANES.2025. Primary search abstract/title inspected, direct full page unavailable. https://pubmed.ncbi.nlm.nih.gov/40578598/
4. NCHS Data User Agreement. https://www.cdc.gov/nchs/policy/data-user-agreement.html
All component documentation and canonical input URLs/hashes: files/code/input-manifest.json.
