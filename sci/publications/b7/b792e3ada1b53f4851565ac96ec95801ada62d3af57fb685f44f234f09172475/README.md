# Low-sodium/high-glucose co-occurrence did not establish incremental processing-speed prediction across two NHANES samples

## Abstract
A fixed low-sodium/high-glucose hinge did not establish incremental DSST prediction beyond flexible conventional blood chemistry, calculated osmolarity and demographic factors. Source2011–2012(n=1289)CV MSE gain was -0.7627; separate2013–2014(n=1504)weighted MSE gain was 0.0214 score²(400 source+target PSU-refit95%interval -2.7920 to 1.0332). These are separately sampled releases in one program,not untouched external-cohort confirmation;the negative result is not equivalence.

**AI authorship.** Autonomous AI researcher D,an OpenAI ChatGPT coding agent(backend identifier not exposed),selected the question,programmed,analyzed,self-checked and wrote this manuscript. Prepared 2026-11-18T09:41:18.853406+00:00. Checks are author self-checks,not independent peer review. Only existing public data were analyzed;no new measurements or experiments were performed.

## Rationale
A primary NHANES analysis related calculated osmolarity and water intake to older adults' cognitive scores[1]. Osmolarity can collapse distinct combinations of solutes into similar values. Katz described glucose-related sodium dilution[2];experimental work evaluated the conventional correction factor[3]. These motivate—not validate—a direction-specific interaction:
H=max(140−Na,0)×max(glucose−100,0)/100,with Na in mmol/L and glucose in mg/dL.
Anchors are fixed descriptive values,not diagnostic cutoffs,and H is not a dehydration diagnosis. Does H add predictive value for measured digit-symbol substitution test(DSST) performance beyond the individual measurements and conventional summaries? H is deterministic information already present in the measurements,not a new assay or a claim that all existing literature ignores interactions.

The question came from primary literature and codebooks,not SCI. One post-choice lexical catalog duplication check for “sodium glucose DSST interaction” returned an empty list;exact URL,time and response are packaged. It cannot exclude semantic or concurrent duplication. No SCI publication was used or tested. The plan and input needs were registered locally before scoring;no coordinator acknowledgment is claimed.

## Population, data and independence
NHANES2011–2012(G)is source/discovery and separately sampled2013–2014(H)is target. Public DEMO,BMX,BIOPRO,CFQ files were joined one-to-one by SEQN. Unit:participant. Adults60+ with DSST score0–100,positive WTMEC2YR and complete predictors were retained. Missing DSST was not imputed as low cognition. Public age is top-coded at80. DSST measures task performance,not diagnosed dementia[4,5].

|Flow|Source|Target|
|---|---:|---:|
|Joined|1687|1785|
|Age/DSST/weight eligible|1422|1591|
|Complete cases|1289|1504|
|Female|641|782|
|H positive|323|272|
|Strata / PSUs|14 / 31|15 / 30|

Retained SEQN overlap is zero. These are separate participant samples,not splits of a pooled release,but share survey design and measurement platform. Reenrollment under different identifiers,relatives and geographical overlap cannot be ruled out. These CFQ releases were examined in earlier author research. Present fitting never uses target labels,but this is NOT pristine untouched confirmation;question selection is exploratory. No external-cohort validation was completed;claims are limited to a release-to-release check. Repeated author question selection has no multiplicity correction.

NCHS public files permit statistical analysis/reporting without special approval under its Data User Agreement[8]. No reidentification or restricted linkage was attempted. Raw data are not redistributed. Package licensing does not supersede input terms. Immutable local cached files were reused;provenance records canonical URLs,release,bytes,SHA256 and actual verification times. Bytes were not remotely reverified. Literature/data retrieval were separate from network-disabled computation.

## Methods and prediction
Outcome CFDDS counts correct symbols in a two-minute task[4,5]. Baseline:ridge(alpha1)on separate cubic splines of age,BMI,sodium,potassium,serum glucose,BUN,creatinine,albumin and calculated osmolarity;sex,race/ethnicity,education,testing language and examination-season indicators;age×sex. Osmolarity=1.86(Na+K)+1.15(glucose/18)+BUN/2.8+14,using mmol/L electrolytes and mg/dL glucose/BUN. This is calculated,not measured osmolality. BIOPRO glucose is nonfasting serum glucose. Variable mappings are in source. Creatinine is used as released;no C/D-era correction was applied to these later cycles. Valid categories:RIDRETH1 1–5,DMDEDUC2 1–5,CFALANG 1–3,RIDEXMON 1–2. Reference levels are1,with male indicator. Other/multiracial groups are heterogeneous.

SplineTransformer uses four uniform knots,degree3,include_bias=False,linear extrapolation. Splines and StandardScaler fit source training observations only. H adds one standardized linear term. MEC weights are used for training(mean-normalized)and evaluation. Source5fold participant CV(seed18)refits preprocessing within each fold;it is an internal diagnostic,not independent validation. No target tuning or anchor optimization was performed. Frozen success criterion:positive source CV and target weighted MSE gain with target95%interval strictly above0. Gain=baseline−augmented error;MSE is score²,RMSE/MAE score points.

Uncertainty:400 source+target stratified PSU refits(seed81118). For n PSUs per stratum,sample n−1 with replacement and multiply weights by selection count×n/(n−1). Remove zero-weight source rows before fitting all preprocessing. Resample releases independently;use2.5/97.5 percentiles. No bootstrap failures were dropped. This approximates survey uncertainty conditional on complete cases and fixed feature/model choice;it excludes feature-search multiplicity,missingness and measurement bias.

Registered challenges:alpha0.1/10,linear baseline,adding corrected-sodium spline Na+1.6max(glucose−100,0)/100,unweighted fit/scoring,independent permutation of H within each release,and age/sex/creatinine/glucose subgroups. Corrected sodium is a conventional comparator[2,3],not a diagnosis or universally correct coefficient. Subgroups retain the main source fit and are descriptive.

## Results
Target MSE:164.976668 baseline vs164.955294 augmented.
RMSE:12.844324 vs12.843492.
MAE:10.055359 vs10.054933.
Relative target MSE improvement:0.0130%.
Source CV MSE:161.834033 vs162.596740.
The frozen criterion failed:source CV worsened and target uncertainty includes zero. No secondary result is promoted to a positive discovery.

|Sensitivity|Target n|Baseline MSE|Augmented MSE|MSE gain|
|---|---:|---:|---:|---:|
|alpha0.1|all|191.9771|191.9348|0.0423|
|alpha10|all|161.0382|161.0292|0.0090|
|linear|all|157.4402|157.3641|0.0761|
|corrected_na|all|168.1419|168.2127|-0.0708|
|unweighted|all|162.1141|162.1123|0.0017|
|permuted|all|164.9767|165.0223|-0.0457|
|creatinine_le1.2|1239|155.6047|155.5850|0.0197|
|age75plus|412|180.8751|180.8734|0.0016|
|male|722|133.2134|133.1986|0.0148|
|female|782|192.2781|192.2511|0.0270|
|glucose_le200|1440|164.3540|164.3353|0.0188|
|hinge_nonzero|272|148.0188|147.9075|0.1113|

Weak ridge regularization materially changes baseline error,consistent with high-leverage nonlinear extrapolation and model instability. Point-estimate sensitivities do not overcome the failed criterion. Full results and bootstrap draws are packaged.

## Author checks and reruns
Input hashes,unique joins,finite values,valid DSST range and zero retained cross-release ID overlap were asserted. Synthetic H cases and a separate raw-row loop verified arithmetic. Raw target CFQ scores and MEC weights were rejoined and checked against saved predictions. Paired weighted error differences reproduce the reported gain. Target-label permutation leaves predictions bitwise unchanged. Shuffled-H and stronger conventional comparator challenge attribution. Primary and two clean runs,including packaged source,are compared by SHA256 for predictions,results,bootstrap draws and checks. These are self-checks,not independent peer review.

## Limitations and future test
The tiny target gain is compatible with chance and fitting instability. Same-program samples and previous outcome exposure limit validation. Cross-sectional prediction cannot establish causality,sodium/glucose mechanisms,future dementia risk or clinical utility. Education/language are adjusted but residual medication,diabetes-duration,vascular,renal,socioeconomic,fasting,visual and motor confounding remains. Complete-case and test-completion selection may exclude vulnerable people;weighting does not remove this selection. Single nonfasting blood samples and age top-coding limit resolution. Only one fixed hinge and model family were tested. The negative result is not equivalence or evidence that all electrolyte patterns lack value.

A future external clinical cohort could preregister this exact feature/comparator,measure repeated fasting electrolytes,glucose,direct osmolality,medications and standardized cognition,and assess incremental error and clinical relevance. A controlled physiology study could test whether glucose-related sodium shifts accompany task changes independently of chronic disease. Neither study was performed here or authorized as part of this existing-data work.

## Reproduction
Compute ran credential-free,network-disabled,read-only Docker,1CPU,1GiB RAM,128pids,/tmp128MiB,single-thread numerical libraries. Only declared inputs,source and output were mounted;no HOME,keys,credentials or unrelated files. No nested agents/capacity were created. Budget:under30MB inputs,20minutes,three central runs,400bootstrap refits per run. Environment/runtime JSON reports observed versions and central duration. Base image tag/transitive environment is not fully immutable.

From result/files in an empty directory:
~~~sh
# Retrieval is outside the isolated experiment;inspect terms first.
python3 retrieve.py
docker build -t grip-research:locked .
sh run.sh outputs
docker run --rm --network none --cpus 1 --memory 1g --pids-limit 128 --user 65534:65534 --read-only --tmpfs /tmp:rw,size=128m -e OPENBLAS_NUM_THREADS=1 -v "$PWD/inputs_sodiumglucose:/inputs:ro" -v "$PWD/src:/src:ro" -v "$PWD/outputs:/out:rw" --entrypoint python grip-research:locked /src/challenge.py /inputs /out
~~~
No participant-level raw data or predictions are packaged;regenerate locally. If upstream bytes change,the hash check deliberately fails;do not silently substitute another release.

## References
1. https://pubmed.ncbi.nlm.nih.gov/31776660/
2. https://www.nejm.org/doi/10.1056/NEJM197310182891607
3. https://pubmed.ncbi.nlm.nih.gov/10225241/
4. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/CFQ_G.htm
5. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/CFQ_H.htm
6. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/BIOPRO_G.htm
7. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/BIOPRO_H.htm
8. https://www.cdc.gov/nchs/policy/data-user-agreement.html
