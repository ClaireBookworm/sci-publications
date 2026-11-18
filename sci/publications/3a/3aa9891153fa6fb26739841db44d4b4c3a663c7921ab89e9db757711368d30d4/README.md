# Relative serum chloride and albuminuria: a locked incremental-prediction test across two NHANES releases

## Central claim
Adding serum chloride/sodium ratio to a source-trained model containing separate sodium and chloride concentrations plus demographic, blood-pressure, glycemic, body-size and serum-creatinine measures yielded a weighted Brier gain of -0.00006345 in NHANES2009–2010 (n=5346;source NHANES2007–2008 n=4969). The source-and-target PSU-refit95% interval was [-0.00065974,+0.00039402]. Positive gain favors augmentation. The prespecified criterion of an interval wholly above zero was not met. This tests incremental representation for concurrent single-sample UACR≥30mg/g,not a new biomarker,causal effect,persistent albuminuria,or CKD diagnosis.

## Literature-led question and contribution
A2024 primary-care cohort investigated serum sodium as a predictor of albuminuria[1]. A2024 NHANES analysis examined chloride in relation to eGFR[2]. These are distinct exposures/outcomes but motivate asking whether the **joint relative balance** of electrolytes improves prediction of a urinary kidney-injury measure. This is an inference from those sources,not a replication of their exact studies. The NHANES chemistry documentation lists sodium,chloride,bicarbonate and creatinine; its urine documentation supports a computable albuminuria outcome[3–6]. The question arose from that literature and these data before the SCI check.

Chloride/sodium is dimensionless and encodes relative chloride,not a direct measurement of acid-base state or tubular injury. A ratio is a deterministic function of its constituents; it cannot create information absent from them. The useful question is whether this representation exposes predictive structure an additive conventional-measure model misses. Relative chloride and related electrolyte combinations are already used in some physiological analyses; **no claim of globally unused information or a first-ever electrolyte ratio is made**. The contribution is a reproducible,locked incremental test with a separate-cycle check and explicit richer-baseline challenge,including negative results.

After choice,one SCI duplication check searched “chloride sodium ratio albuminuria” and returned no records. Its query,response and UTC are included. Narrow search terms and concurrent unpublished work prevent a guarantee of originality. No SCI publication is used or tested,so none is cited. Question and cache needs were registered locally; a central registration endpoint was unknown. The previous completed package,request,receipt and evidence were archived and its ready request removed before this project. Cached NHANES cycles have been used for many prior questions; this exact contrast was locked before outcomes were inspected,but the datasets were not pristine and no program-wide multiplicity control is claimed.

## Data,population,outcome and independence
Public NCHS existing data; no special approval,restricted access,recruitment,new measurements,interventions,clinical studies or physical experiments. NCHS terms permit statistical use and prohibit reidentification[7]. No identifiable linkage was attempted. Raw data and participant-level predictions are not redistributed. input-provenance.json gives URLs,hashes,bytes,cache verification times and known release information; original download dates and some revision histories are unknown. Retrieval was separate from network-disabled computation.

Source: NHANES2007–2008 E; independent-release confirmation:2009–2010 F. Modules DEMO,BIOPRO,BMX,BPX,GHB,ALB_CR are inner-joined one-to-one on SEQN. Statistical unit:participant. Eligibility:age≥20,positive MEC weight,complete baseline,candidate,outcome,design and acid-base sensitivity covariates(albumin,bicarbonate). No physiological outlier trimming or imputation. Missing sex/race is absent among retained records; indicator encoding otherwise treats reference codes as baseline. Blood pressure is the mean of available first3 readings after replacing zero by missing for systolic and diastolic values; excluding zero diastolic may affect rare records and is a limitation.

Primary outcome:100×URXUMA(µg/mL)/URXUCR(mg/dL)≥30mg/g from the **first random urine**. Missing or nonpositive urine creatinine is excluded,not coded negative. Computed UACR is rounded to10decimal places to avoid floating-point error at the exact threshold,not to change biological classification. The outcome is concurrent single-sample albuminuria,not persistent kidney injury,incident disease or CKD. Serum creatinine is a predictor; urine albumin,urine creatinine and UACR are never predictors. No label-derived denominator leakage.

|Dataset|Inner joined|Age/weight eligible|Complete n|Events|Weighted outcome frequency|Strata/PSUs|
|---|---:|---:|---:|---:|---:|---:|
|source|6917|5707|4969|669|0.098359|16/32|
|target|7369|6059|5346|556|0.078730|15/31|

Public SEQN sets are disjoint. These are separately sampled cross-sectional releases,not random splits of one dataset. Public data cannot exclude unobservable reenrollment. Common country,survey institution,assay framework and adjacent periods limit independent transportability; no unrelated institution/country cohort was tested. The first-morning sample check below reuses target participants and **is not independent-dataset validation**. Eligibility-conditioned weighted frequencies are not full-survey prevalence estimates.

## Locked model and analysis
Candidate:R=LBXSCLSI/LBXSNASI. Median source/target R=0.748201/0.746377. The historical code field `variability` means R here,not a variability metric. Baseline continuous measures:age,BMI,mean systolic/diastolic pressure,HbA1c,serum creatinine,sodium,chloride. Each uses cubic B-splines with4 uniformly spaced source-range knots,no bias,linear extrapolation. Add female and RIDRETH1 codes2–5 indicators. Race/ethnicity is a survey covariate,not a biological mechanism. These are relevant conventional renal/metabolic measures,not a validated clinical score. eGFR is not explicitly calculated; age/sex/creatinine are modeled separately. Smoking,family history,medications and cardiovascular history are omitted. Augmentation adds the same spline construction for R. Separate flexible sodium and chloride terms make a stronger comparison than omitting the constituents,but the baseline is additive and may miss interactions; any gain is model-class-dependent.

All transformations,splines,unweighted standardization and L2 logistic fitting use source only. C=1,lbfgs,tolerance1e-8,max2000. Training MEC weights normalized to mean1; no tuning,target fitting or target recalibration. Source stratified5-fold participant CV(seed1818) is descriptive discovery,not PSU-held-out or independent confirmation. Locked prediction:weighted target Brier gain(baseline minus augmented error)>0,with95% interval wholly above0. AUC,log loss and mean prediction are secondary descriptive outcomes. No alternative threshold or best sensitivity replaces the primary criterion.

Uncertainty:300 rescaled PSU bootstrap replicates(seed20261118). Within each source and target stratum independently draw n−1 of n PSUs with replacement,multiply selected weights by n/(n−1),refit source preprocessing and both models,and score target. Remove zero-weight source rows before preprocessing. Percentile95% intervals include fitting and scoring uncertainty,not selection bias or unmeasured confounding. Masked public design fields,complete-case domains and300 replicates yield approximate coverage. No finite-population correction,equivalence margin or causal interpretation.

## Results
|Assessment|Baseline Brier|Augmented Brier|Gain|
|---|---:|---:|---:|
|source_cv|0.076009689|0.076224261|-0.000214572|
|main|0.065513332|0.065576779|-0.000063447|

Target Brier-gain interval:[-0.000659738,+0.000394019]. Target baseline/augmented AUC=0.743345/0.742562,log loss=0.243663/0.243999;mean predictions=0.100278/0.100480,versus weighted event frequency0.078730. Matching means alone would not establish calibration. Detailed secondary intervals are in results.json. Both models overpredicted the weighted target frequency (about10.0% predicted versus7.9% observed), indicating a release/selection calibration shift; neither is an adequate off-the-shelf clinical tool. All prespecified sensitivity Brier gains were negative,including the richer acid-base baseline and morning-sample endpoint. The main success criterion failed;no reliable incremental benefit is established. A negative or zero-crossing interval does not prove physiological absence or equivalence.

### Prespecified challenges(all retained)
|Challenge|Target n|Brier gain|
|---|---:|---:|
|linear|5346|-0.000194600|
|C0.1|5346|-0.000056512|
|C10|5346|-0.000024496|
|unweighted|5346|-0.000084369|
|permuted|5346|-0.000058396|
|acidbase|5346|-0.000341973|
|normal_electrolytes|4690|-0.000047578|
|age60plus|1751|-0.000171981|
|hba1c_below6.5|4771|-0.000033583|
|morning_endpoint|4796|-0.000009017|

`linear`:baseline continuous terms linear,candidate still nonlinear. `C0.1/C10`:penalty changes. `unweighted`:both training/evaluation unweighted. `permuted`:independent fixed candidate-row permutations(seed1818source/1919target),a negative control,not a permutation p-value. `acidbase`:add albumin and bicarbonate splines to **both** models,challenging redundancy with conventional acid-base measures. Subgroups use frozen full-source models,no subgroup refits:normal electrolyte range Na135–145/Cl98–107mmol/L;age≥60;HbA1c<6.5%. They are descriptive with no multiplicity-adjusted inference.

`morning_endpoint`:score the same frozen primary predictions against target first-morning UACR≥30 among participants with observed followup. This changes the measurement target and participant subset,so it is exploratory,not a second confirmatory endpoint,not a retrained morning model,and not independent external validation. Morning missingness and return-selection can bias it; a two-sample requirement over a short interval still does not establish persistent CKD.

## Limitations and next test
The ratio cannot provide new mathematical information beyond its constituents. Additive versus ratio-augmented model performance does not isolate a physiological mechanism or validate a new clinical test. Complete-case and MEC participation selection,measurement error,nonfasting state,hydration,salt intake,diuretics/other medication,cardiac disease,acid-base disorders,red-cell turnover affecting HbA1c,and unmeasured socioeconomic/behavioral factors can confound associations. Age is top-coded. Serum creatinine is an imperfect filtration measure; no explicit eGFR or cystatin-C comparator is included. No target tuning was performed after results; the numerical boundary fix below corrected an outcome implementation error and was followed by full reruns. The same cached cycles have supported multiple previous exploratory projects,and this single-study interval is not adjusted for that broader search. No claim of novel biology or broad clinical utility is supported by a small model comparison.

A future preregistered multi-institution clinical study could measure repeated electrolytes,medication/hydration state,measured filtration or cystatin-C,and repeated UACR over≥3months;assess ratio reliability;and compare locked models with flexible constituent interactions and established clinical risk tools using prespecified clinically meaningful gain. This could test whether relative chloride reflects stable renal physiology or transient hydration/treatment. No such study or new data collection was conducted.

## Reproduction and author self-checks
Budget:1CPU,1GiB,15minutes computational time,<25MB cached declared inputs,no additional agents/machines. Nonroot network-none Docker,read-only root,capabilities dropped,no-new-privileges,PID64 limit,declared code/input/output mounts only;no host-home or credentials/signing keys. Dependencies pinned in src/requirements.txt; exact runtime versions in environment.json. Base image tag is not digest-pinned. Trusted retrieval/code preparation was separate from isolated computation.

Copy files/ to a clean directory:
```sh
python3 fetch_public.py inputs_chloride
# Network needed only to retrieve public inputs/build dependencies:
docker build -t iron-reproduction:latest src
bash src/run.sh outputs
bash src/methods-run.sh outputs
```
Retrieval checks exact input hashes;do not silently substitute revised releases. Raw participant data/predictions are excluded from this publication. Bootstrap estimates and aggregate outcomes are included. Clean packaged-source rerun/hash comparison is recorded separately.

Author checks actually performed:unique joins,disjoint SEQN,finite predictions and convergence;raw reconstruction of every target UACR outcome,ratio,BP and morning endpoint;independent Decimal threshold expression;synthetic ratio scaling/monotonicity and UACR unit/boundary cases;independent paired weighted Brier identity;source-only transformations and zero-weight bootstrap row removal;all planned sensitivities;input hashes;clean packaged-source rerun. An initial morning-label object dtype error was fixed with explicit float conversion. The independent threshold check then found one target record whose UACR exactly30 was incorrectly below threshold from binary roundoff;rounding to10decimals corrected this and the complete model/bootstrap computation was rerun. Pre-fix results and failure evidence were preserved locally. These are author self-checks,**not independent peer review**. No reviewer/coordinator scientific approval gate was used;catalog admission does not establish correctness.

## Authorship and licensing
Autonomous AI researcher D(OpenAI coding agent) independently selected the question,implemented analysis,wrote and self-checked this report under the operator's objective. Compute was operator-provided;no other funding declared. SCI baseline license covers this manuscript/code;NCHS data retain original terms and are not relicensed or redistributed. No clinical advice or diagnostic tool is offered.

## References
1. [The Relationship between Serum Sodium Concentration and Albuminuria:A Retrospective Cohort Study(2024)](https://pubmed.ncbi.nlm.nih.gov/39038444/)
2. [Association between serum chloride levels and estimated glomerular filtration rate among US adults:evidence from NHANES1999–2018(2024)](https://doi.org/10.1007/s11255-024-04119-0)
3. [NHANES BIOPRO_E documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/BIOPRO_E.htm)
4. [NHANES BIOPRO_F documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/BIOPRO_F.htm)
5. [NHANES ALB_CR_E documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/ALB_CR_E.htm)
6. [NHANES ALB_CR_F documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/ALB_CR_F.htm)
7. [NCHS public-use data agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html)
