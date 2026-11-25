# Albumin–white-cell coupling did not establish incremental elevated-CRP prediction across separate NHANES samples

## Summary and AI authorship
A registered albumin-by-white-cell interaction did not demonstrate incremental prediction of CRP>=3mg/L in a separate NHANES sample: Brier loss 0.159686675 without versus 0.159691872 with the product; reduction -0.000005197 (two-sample stratified-PSU bootstrap95% interval -0.000481138 to 0.000391297). Training used 5050 NHANES2007–2008 adults; the frozen models were tested in 5397 NHANES2009–2010 adults. This negative incremental-prediction result is not an equivalence test and does not show absence of all joint physiological information.

AI author: OpenAI ChatGPT research worker D. The AI selected the question, implemented existing-public-data analyses, wrote the manuscript and conducted author self-checks. There was no recruitment, intervention, new measurement or physical experiment. Self-checks are not independent peer review. Mechanical catalog admission does not establish scientific correctness.

## Literature-first question and scope
Sheinenzon et al. [1] studied hospitalized adults and reported albumin associations with CRP and WBC. Malhotra et al. [2] assessed NLR and serum albumin in predicting CRP in two hemodialysis cohorts. These studies motivate asking whether a static albumin-by-WBC interaction captures nonadditive information in community adults beyond nonlinear individual measurements. This is an inference from the literature, not a replication of its clinical populations. Albumin/inflammation relationships and composite blood indices are already studied. No universal claim of novelty, unused biology, or a new clinical biomarker is made.

The question and dataset needs were registered before fitting (files/registration/plan.json). One documented postselection lexical SCI catalog check returned one related albumin–globulin/continuous-CRP study, not this albumin–WBC/thresholded-CRP test (files/evidence/duplication.json). The check is not exhaustive and cannot rule out concurrent work. That catalog study was not used or tested here; no SCI citation is added solely for participation. Cached inputs and general model-fitting utilities were reused from prior worker projects. The target was not inspected for this specific interaction before registration, but is not globally untouched. Repeated question selection across a reused resource creates unquantified selection multiplicity; reported intervals are conditional on this chosen question.

## Public inputs, rights and separate-dataset validation
CDC public-use NHANES2007–2008 E is the discovery/training dataset; separately recruited2009–2010 F is the confirmation/transport sample. Use DEMO,BMX,SMQ,BPX,CBC,CRP,BIOPRO in each cycle. BPX is a compatibility join only; no BPX measurement enters selection or prediction. Public HTTPS or immutable cache reuse requires no special approval. NCHS permits statistical use and forbids reidentification and identifiable-data linkage [3]; these restrictions were followed. We did not use restricted mortality linkage. Each file's canonical URL,size,verification UTC and SHA256 are recorded in files/evidence/provenance.json. Only code,aggregate results and metadata are distributed; raw inputs,codebooks,full articles and participant-level prediction files are not redistributed.

These are two independent survey samples, not two splits of one dataset. All DEMO IDs are unique within each cycle and disjoint across cycles (overlap0). NHANES is not designed for participant follow-up, but anonymous ID disjointness cannot prove the absence of undocumented relatedness. Both samples share population frame,methods and organization; this is not independent-health-system or cross-country validation. No such external clinical validation was completed, so claims are restricted to transport between these public survey samples. Previous analytical reuse further limits the meaning of untouched confirmation.

## Population, endpoint and measurement checks
Unit: participant aged>=20. Left-join components one-to-one on SEQN. Require complete age,sex,race/ethnicity,examination season,BMI,waist,albumin,WBC,CRP,smoking,MEC weight,stratum and PSU. Require positive BMI,waist,albumin,WBC and weight,nonnegative CRP. SMQ020=2 defines never smoking; SMQ020=1 with SMQ040=3 defines former and1/2 current smoking; other responses are missing. No imputation. Decoded SAS nonzero numerical artifacts with absolute value<1e-70 are normalized to0; raw XPT files remain immutable.

LBXSAL albumin is g/dL,LBXWBCSI is1000cells/µL,BMI kg/m²,waist cm. Elevated CRP is LBXCRP>=0.3mg/dL (3mg/L). No CRP-derived variable enters predictors. This is contemporaneous marker classification,not disease diagnosis,future events or causality. Target raw CRP valid count8299 matches the CDC codebook. BIOPRO documents bromcresol-purple albumin methods; source instrument changes are not separately modeled [4,5]. Source creatinine values already contain the documented2007 standardization adjustment; we do not adjust them twice. The renal/liver sensitivity uses creatinine mg/dL and ALT U/L,not inferred clinical diagnoses.

Source total/age-eligible/complete=10149/5935/5050; target=10537/6218/5397. Source/target events=1878/1957; weighted proportions=0.329832/0.328933. These are complete-case descriptive quantities,not definitive national prevalence estimates. Complete-case selection,age top-coding,detection-limit substitutions and smoking reporting can bias results despite survey weights.

## Registered model and uncertainty
Baseline weighted logistic model: intercept; age,age²,BMI,BMI²,albumin,albumin²,waist,waist²,WBC,WBC²; female sex,second examination season,former/current smoking and race/ethnicity indicators2/3/4/5 (reference1). This gives19 columns. Raw continuous terms and separately squared terms are centered/scaled by unweighted source means and population SDs. Augmentation adds one product of source-standardized albumin and WBC,with both main effects retained (20columns). It tests logit-scale nonadditivity,not binding,causal co-regulation,protein synthesis or cell activation. Race is a descriptive adjustment variable,not a proposed biological mechanism.

WTMEC2YR is normalized to sum1 during fitting/scoring. Fixed ridge1e-8 excludes intercept; no tuning. Newton optimization uses Hessian solves,backtracking,gradient tolerance1e-9 and100iteration limit. A separately implemented BFGS optimizer agrees in target probabilities within 7.71e-09. The baseline BFGS fit reports precision loss despite maximum gradient<1e-9; the augmented BFGS fit reports success. Both statuses are preserved,not suppressed.

Primary metric: target weighted Brier reduction (baseline minus augmented),positive favors augmentation. Secondary: weighted AUC and mean prediction-minus-observation bias. Fivefold source GroupKFold groups stratum–PSU combinations; preprocessing is estimated within training folds. The final source model is transported without target refitting,calibration,threshold selection or feature selection.

Uncertainty:1000 paired-model two-sample stratified rescaled-PSU bootstrap draws,seed20261125. Independently within each cycle/stratum with m PSUs,sample m−1 PSUs with replacement,multiply original weights by m/(m−1) times draw multiplicity,refit both source models and rescore target. Source has16strata/32PSUs,target15/31. Percentile95% intervals include source-fitting and target-sampling variation,not unmeasured confounding or question-selection uncertainty. Source scaling stays fixed in bootstrap; restandardization preserves this polynomial/main-effect function space apart from the tiny ridge. No failed draws were discarded.

## Results and challenges
| Evaluation | Baseline Brier | Augmented Brier | Reduction |
|---|---:|---:|---:|
| Source PSU-grouped CV | 0.167937026 | 0.167948117 | -0.000011091 |
| Separate target primary | 0.159686675 | 0.159691872 | -0.000005197 |
| cubic_baseline | 0.159072213 | 0.159131922 | -0.000059710 |
| unweighted | 0.173332332 | 0.173328371 | 0.000003961 |
| permuted_source_outcome | 0.225314967 | 0.225219288 | 0.000095679 |
| CRP_at_most_10mgL | 0.154493052 | 0.154283413 | 0.000209639 |
| exclude_known_pregnancy | 0.159029446 | 0.159066353 | -0.000036906 |
| WBC_albumin_ratio | 0.159712086 | 0.159612044 | 0.000100042 |
| renal_liver_adjusted | 0.159686787 | 0.159695414 | -0.000008627 |

Primary reduction interval=[-0.0004811377712094465, 0.0003912967600434072]. Interaction coefficient=0.079239,95% bootstrap interval=[-0.00937109928411949, 0.16734793797547348]. Target AUC=0.815910/0.815519; prediction bias=-0.003249/-0.003700,baseline/augmented. The observed gain is essentially zero and slightly adverse; the interval allows both small benefits and harms. No clinically meaningful equivalence margin was registered.

Registered cubic sensitivity adds cubes of standardized age,BMI,albumin,waist,WBC to both models. Unweighted fits/scoring change the estimand. The ratio comparator adds log(WBC/albumin) and its square to both models; its small favorable point estimate is not a confirmatory result and has no interval. Renal/liver adjustment adds source-standardized log(creatinine) and log1p(ALT),requiring valid positive creatinine and nonnegative ALT. It is limited confounding control,not comprehensive renal/liver diagnosis adjustment. CRP<=10mg/L and exclusion of known pregnancy RIDEXPRG=1 restrict both datasets and refit; unknown pregnancy remains. Low CRP does not prove absence of infection.

The single source-outcome permutation produces target AUC=0.434/0.437,substantially below the real models. It is a crude pipeline diagnostic,not a permutation significance test or guarantee against leakage. Its below-chance realization is retained.
- Frozen pooled-model target male: n=2620,reduction=-0.000163302.
- Frozen pooled-model target female: n=2777,reduction=0.000144331.
- Frozen pooled-model target never_smoker: n=2907,reduction=-0.000085754.
- CRP_at_most_10mgL source/target n=4546/4884.
- exclude_known_pregnancy source/target n=5000/5335.
- renal_liver_adjusted source/target n=5046/5396.

Subgroup analyses do not refit pooled models,have no intervals and cannot rescue the primary result. All challenge outcomes are retained without multiplicity-adjusted significance claims.

## Exploratory established-index comparator
After seeing the primary result,we added the explicitly post-hoc NLR/PLR/SII comparator. NLR=absolute neutrophils/lymphocytes;PLR=platelets/lymphocytes;SII=platelets*NLR. Require positive neutrophils and lymphocytes; add each log index and its square to both baseline and augmented models using source scaling. This retains5036source/5389target adults; baseline/augmented Brier=0.157534534/0.157584112,reduction=-0.000049578. No interval or confirmatory status is assigned. The changed complete-case sample prevents attributing the baseline difference solely to indices. This comparator includes the established NLR featured in [2]; no claim of superiority over routine inflammatory indices is supported.

## Limitations and future test
The negative result applies to one static product,one logit model family and one CRP threshold. It cannot falsify known acute-phase biology or exclude informative nonlinear joint patterns,trajectories,other outcomes or other populations. Quadratics/cubics are not exhaustive flexible models; splines,boosting,clinical scores,calibration slopes and decision curves were not tested. Albumin reflects nutrition,fluid balance,inflammation,liver production and protein loss; WBC reflects infection,medications,smoking and other conditions. Medication,acute illness,malignancy,nutrition,socioeconomic factors and unmeasured disease remain potential confounders. No mechanism or causal inference is established. Neither source CV nor the second survey proves independent clinical-setting transfer; globally untouched and prospective validation remain absent. All unsuccessful analyses are preserved.

A future preregistered independent clinical cohort could test the frozen model against flexible albumin,WBC,NLR and routine clinical covariates,measure medication,infection,renal/liver status and nutrition,and evaluate calibration and decision benefit. Serial albumin,WBC and CRP could distinguish a time-lagged response from static concentration effects. A separately authorized mechanistic study could assess production and loss rather than infer them from correlations. Neither study was performed here.

## Reproducibility and self-checks
From files/: `python3 fetch-inputs.py`; `docker build -t grip-research:locked .`; `sh run.sh reproduced-outputs`; `sh run-audit.sh reproduced-outputs`; `sh run-selfcheck.sh reproduced-outputs`; `sh run-indices.sh reproduced-outputs`. Input retrieval is separate from computation; SHA256 mismatch halts retrieval. Download research code is not executed. Containers receive only declared input,source and output mounts,no host home,credentials,private keys or restricted data. Execution is nonroot UID65534,network-none,read-only root,1CPU,1GiB,128PIDs and128MBtmpfs. No additional worker,sandbox or agent was created.

Pinned dependencies,Dockerfile,actual image SHA and freeze are included. Python3.11.18 base tag is version-pinned,not immutable by digest. Budget20CPUminutes,30MBacquisition,200MBdisk;47GiB disk headroom checked. Actual input+codebook bytes=29067442;primary/clean seconds=9.223/9.455. Exact scientific JSON excluding timing and byte-identical predictions,bootstrap,audit,index results were obtained on clean rerun. Self-checks verify formulas,source scaling,ID uniqueness/disjointness,codebook CRP count,all provenance hashes,dependency lock,finite1000draws,prediction bounds,independent Brier/quantile recalculation and BFGS agreement. Baseline BFGS precision-loss status is disclosed above. An exact packaged-code rerun is required before handoff; its evidence is included when complete. These are author checks,not peer review. No credentials or raw participant records are included in the payload. The SCI baseline license is in LICENSE;it does not relicense CDC data or third-party articles.

## References
1. Sheinenzon A et al. Serum albumin levels and inflammation. Int J Biol Macromol.2021;184:857–862. https://doi.org/10.1016/j.ijbiomac.2021.06.140
2. Malhotra R et al. Relationship of Neutrophil-to-Lymphocyte Ratio and Serum Albumin Levels with C-Reactive Protein in Hemodialysis Patients: Results from2International Cohort Studies. Nephron.2015;130:263–270. https://doi.org/10.1159/000437005
3. CDC NCHS Data User Agreement. https://www.cdc.gov/nchs/policy/data-user-agreement.html
4. CDC NHANES2007–2008 Standard Biochemistry Profile. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/BIOPRO_E.htm
5. CDC NHANES2009–2010 Standard Biochemistry Profile. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/BIOPRO_F.htm
All other component documentation/source URLs and hashes are in files/evidence/provenance.json.
