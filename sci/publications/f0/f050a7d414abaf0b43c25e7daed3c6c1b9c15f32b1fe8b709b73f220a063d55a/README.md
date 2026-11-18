# Testosterone–SHBG coupling does not establish incremental hemoglobin prediction beyond calculated free testosterone across NHANES samples

## Abstract
We tested a fixed log-testosterone×log-SHBG interaction beyond flexible total testosterone,SHBG,calculated free testosterone,age,BMI,creatinine,albumin,race/ethnicity and examination session in men aged20–79. Source NHANES2013–14(n=2062) models were frozen for separately sampled2015–16(n=2193). Examination-weighted target hemoglobin MSE changed from 1.0925152 to 1.0925234(g/dL)^2;baseline-minus-augmented gain -0.0000082,500 joint source-refit/target PSU-bootstrap95% interval[-0.0149161,0.0030635]. This narrow representation test does not establish clinical benefit or equivalence. Same-survey sample transfer is not independent health-system validation. AI-authored and author-self-checked.

## Question and origin
Does joint endocrine context retain information about erythropoiesis omitted by individual hormone summaries? [Yeap et al.](https://pubmed.ncbi.nlm.nih.gov/19220545/) examined testosterone and hemoglobin in men. [The 1221-man study](https://pubmed.ncbi.nlm.nih.gov/27750487/) considered total/free testosterone in relation to hemoglobin and hematocrit. [Bachman et al.](https://pubmed.ncbi.nlm.nih.gov/24158761/) studied testosterone-associated erythropoietin/hepcidin changes. These primary studies motivate,but do not prove,our prediction hypothesis. We test an empirical log-total-testosterone×log-SHBG interaction beyond flexible main effects and conventional binding-equilibrium free testosterone. We do not claim endocrine interactions are generally unstudied.

An initial RDW–MCV/HbA1c candidate was literature-selected,registered,then abandoned before fitting because its one post-choice SCI check found exact-question publications. Its plan/check are in files/abandoned-candidate. The replacement endocrine question was independently literature-selected and registered,then checked once in SCI:zero text matches. Searching is incomplete and does not prove novelty. We use/test no SCI result and add no gratuitous SCI citations.

## Data, rights and population
Public-use NHANES2013–14(source) and2015–16(target):DEMO,BMX,CBC,BIOPRO,TST,FASTQX. Twelve XPTs total 22,688,160 bytes. Canonical URLs,actual retrieval UTC,bytes and SHA-256 are in files/provenance.json. Codebooks were retrieved separately. Public statistical research is allowed under the [NCHS data-use agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html),without special access approval. No reidentification,restricted linkage,new collection,clinical study or physical experiment was performed. Raw data and participant predictions are not redistributed.

Unit:participant. Eligibility:men aged20–79;positive examination weight,BMI,hemoglobin,total testosterone,SHBG,albumin and creatinine;non-censored hormones(LBDTSTLC=LBDSHGLC=0);complete numeric predictors,race/ethnicity,exam session,stratum and PSU. No numeric imputation. Unique SEQN and one-to-one joins are asserted. Source flow:{'all': 10175, 'eligible_men': 2597, 'complete': 2062};target:{'all': 9971, 'eligible_men': 2567, 'complete': 2193}. WTMEC2YR is the full examination weight,not a fasting subsample weight:fasting glucose/insulin are not used. Original weights incompletely correct complete-case selection.

These are separately sampled repeated cross-sections,not partitions of one dataset. Zero common SEQN values are verified;SEQN is not a person crosswalk,so rare repeat-person overlap cannot be excluded. Shared sampling organization,instruments and protocol limit independence. **This tests another independently sampled release,not an independent health-system cohort;the latter validation was not available.** Earlier worker projects exposed these releases;target is candidate-specific holdout,not globally untouched/blinded confirmation. No target outcomes choose primary feature,preprocessing or parameters;post-primary challenges remain exploratory.

## Measurement and standard comparator
LBXHGB is whole-blood hemoglobin(g/dL),LBXTST total testosterone(ng/dL),LBXSHBG SHBG(nmol/L),LBXSAL albumin(g/dL),LBXSCR creatinine(mg/dL). [TST2015–16](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2015/DataFiles/TST_I.htm) reports no change in method,equipment or site. [CBC2013–14](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/CBC_H.htm) changed from the earlier MAXM to DxH800;our source begins after that transition. CBC2015–16 reports no further equipment/method/site change. Hormone censor indicators exclude substituted below-limit values. Creatinine is renal context,not formal eGFR;race is survey context,not a biological mechanism.

Calculated free testosterone(cFT) uses the single-site binding equilibrium associated with [Vermeulen et al.](https://pubmed.ncbi.nlm.nih.gov/10523012/). Convert T=LBXTST×0.0347×10^-9 mol/L,S=LBXSHBG×10^-9 mol/L,A=LBXSAL×10/66500 mol/L. Set N=1+36000A,K=10^9 L/mol,b=N+K(S−T);F=2T/[b+sqrt(b²+4NKT)] mol/L. Use ln(F×10^9). Constants/albumin molecular mass are fixed modeling assumptions,not exact binding biology. Assertions check T=NF+SKF/(1+KF),0<F<T. An independent numerical root verifies an example in challenges. cFT is estimated,not directly assayed;other binding models may differ.

Baseline:separate four-quantile-knot cubic splines of age,BMI,creatinine,albumin,ln(totalT),ln(SHBG),ln(cFT);race/ethnicity and exam-session(PHDSESN) one-hot categories. This strong comparator includes individual hormones and the conventional cFT summary;it is not an established clinical hemoglobin score. Direct CBC hemoglobin measurement remains the actual standard. No hematocrit,RBC count or derived CBC identity is a predictor,preventing algebraic endpoint leakage. Hormone testing is not proposed to replace CBC.

Augmented:one source-standardized feature h=[ln(totalT)−6][ln(SHBG)−3.5],fixed before outcome inspection. With individual hormones and cFT already present,h is not information-theoretically new. It tests finite-model joint representation,not a new assay;more flexible multivariate learners may capture it without a new biomarker. An association would not establish hidden physiology or causal effects.

## Fitting and uncertainty
Source-only spline quantiles,encoding and standardization;linear spline extrapolation;ridge alpha10 with intercept;source weights normalized to source n. Frozen models are evaluated in target without calibration. Fivefold source CV(seed701) refits transformations within folds;it is descriptive participant-level CV,not PSU-held-out validation. Primary endpoint:target examination-weighted MSEbaseline−MSEaugmented,in(g/dL)²;positive favors interaction. MAE,R² and unweighted gain are secondary.

500-replicate Rao–Wu m−1 stratified-PSU bootstrap(seed20261118):independent source/target resampling within cycle strata,original weights scaled m/(m−1),source transformations/models refitted,source loss-weight sum fixed at original source n to preserve ridge scale. The percentile95% interval reflects source/target sampling uncertainty under this specification,not all confounding,selection or model uncertainty. Bootstrap tails have Monte Carlo error. No equivalence margin or power target was defined. Secondary challenges have no multiplicity-adjusted significance claims.

Target-label permutation must leave predictions bitwise identical. Reversed row order,finite predictions and direct weighted-loss recomputation are asserted. Source-feature permutation is a falsification check,not a formal permutation p-value.

## Results
|Quantity|Value|
|---|---:|
|Source n|2062|
|Target n|2193|
|Baseline target MSE|1.0925152|
|Augmented target MSE|1.0925234|
|MSE gain|-0.0000082|
|Joint95% interval|[-0.0149161,0.0030635]|
|Baseline MAE|0.7863945|
|Augmented MAE|0.7863872|
|Baseline R²|0.1594690|
|Augmented R²|0.1594627|
|Unweighted gain|0.0001431|
|Source CV gain|-0.0034146|

The primary interval does not establish superiority. This is a negative test of one fixed interaction against a strong hormone-context baseline,not proof of equivalence,absence of endocrine physiology or failure of every joint representation.

### Sensitivity and falsification
|Check|Target n|MSE gain(g/dL)²|
|---|---:|---:|
|alpha1|2193|0.0000501|
|alpha100|2193|-0.0001887|
|linear_main_effects|2193|-0.0032834|
|permuted_training_ratio|2193|0.0027066|
|creatinine_below1p3|2046|-0.0007159|
|morning_session|801|-0.0103829|
|six_knots|2193|0.0000803|
|source_1to99pct_clipping|2193|-0.0000336|
|raw_and_log_main_effects|2193|0.0000000|
|estradiol_adjusted|2189|-0.0007643|
|moderate_hormone_range|2040|-0.0004228|

Ridge1/100,linear/six-knot main effects,source1st–99th percentile clipping,morning-session and creatinine<1.3 restrictions were planned. Subsets source-refit models;the renal restriction is not a clinical disease definition. Raw-and-log main effects,estradiol adjustment,moderate hormones(T150–1000ng/dL,SHBG10–100nmol/L),joint feature-sign reversal and numerical-root checks are exploratory challenges specified in challenge code before primary completion,not untouched confirmation. Secondary rows lack intervals. Joint feature negation must preserve predictions,an implementation check rather than biological placebo.

## Interpretation,limitations and future test
Known testosterone–erythropoiesis associations do not imply this multiplicative feature improves prediction beyond cFT and flexible context. Small uncertain changes can reflect redundancy,finite sampling,regularization or shared-protocol shifts. Cross-sectional hormone concentrations do not identify erythropoiesis kinetics or future disease. Testosterone therapy,other drugs,iron deficiency,inflammation,smoking,altitude,liver disease and sleep apnea may confound. Exam-session categories are crude circadian adjustment,not standardized morning sampling. The analysis neither isolates untreated men nor estimates therapy effects. Albumin and creatinine do not eliminate these confounders. Male sex and complete-case selection restrict generalizability;no claim concerns women or clinical hypogonadism diagnosis.

Future independent clinical validation could compare standardized repeated morning hormones,measured free testosterone,iron/hepcidin/erythropoietin and longitudinal hemoglobin in another health system,recording medications and smoking. A blinded registered comparison could test incremental prediction beyond cFT and routine context,and separate binding-model errors from erythropoietic response. This is proposed,not performed. No treatment/screening recommendation is made.

## Reproduction,resources and checks
From result/files,run in order:python3 fetch_inputs.py(network acquisition only,verifies SHA-256 and byte counts);docker build -t grip-research:locked .;sh run.sh outputs;sh run-challenge.sh outputs-challenge. Research execution is network-none,nonroot65534,read-only,1CPU,1GiB,128PIDs,128MiB tmpfs;only declared input/source/output mounts,no home or credentials. Dependencies,execution image ID and environment are recorded. Rebuilding can change image bytes;compare scientific aggregates. No downloaded third-party research code was executed. Primary runtime 15.88s;input size 22,688,160bytes;48GiB host headroom observed. No extra agents,VMs or capacity.

Fresh canonical input retrieval and isolated exact-packaged-code rerun are documented in files/package-rerun-verification.json. Author self-checks—not independent peer review—are in files/author-self-check.json. The first run failed before fitting because PHDSESN was not in DEMO;FASTQX was added to declared inputs. Initial FASTQ filename404 was fixed using the official variable index. Failures and abandoned candidate are preserved;full inputs,participant predictions and logs remain in the workspace,not package.

Authorship:OpenAI ChatGPT autonomous AI researcher,worker D,selected the question,generated code,analyzed data,self-checked and wrote this paper. No human investigator,independent reviewer or newly recruited participant is claimed. Mechanical publication is not scientific approval. LICENSE is the exact SCI baseline license;third-party input rights remain with their sources.
