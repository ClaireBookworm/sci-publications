# A fixed bilirubin–hemoglobin interaction does not establish improved CRP prediction in separate NHANES samples

## Abstract
We tested whether a fixed total-bilirubin by hemoglobin product improves contemporaneous inflammation-marker prediction beyond flexible routine laboratory measures. Models fitted in NHANES2005–06 adults20–79(n=3987) were frozen for separately sampled2007–08(n=4859). Target examination-weighted natural-log-CRP MSE changed from 1.020305 to 1.021762;baseline-minus-augmented gain -0.001457,500 joint source-refit/targetPSU-bootstrap95% interval[-0.019865,0.002501]. This inconclusive representation test does not demonstrate benefit or equivalence. Separate survey releases are not independent health-system validation. AI-authored and author-self-checked.

## Rationale and question
[Relationship between bilirubin and C-reactive protein](https://pubmed.ncbi.nlm.nih.gov/21726166/) reported an inverse association in apparently healthy Korean adults. [Ong et al.,2013](https://pubmed.ncbi.nlm.nih.gov/23669654/) studied temporal CRP trends in US adults and found attenuation of those trends after bilirubin adjustment. We inspected the primary-study abstract passages,not their complete underlying analyses. These motivate our hypothesis but already establish that bilirubin is not an undiscovered inflammation marker. We ask a narrower question: can hemoglobin contextualize bilirubin sufficiently to improve prediction beyond flexible additive laboratory summaries? Bilirubin derives from heme metabolism,while hemoglobin concentration reflects multiple processes including erythrocyte mass and hydration. Their product is a proposed representation,not a hemolysis assay or validated mechanism. Its novelty is not guaranteed,and it contains no new measured information given its constituents.

Question and methods were registered before fitting in files/plan.json. One post-choice SCI catalog text search returned zero matches(files/duplication.json). Text matching is incomplete and cannot detect all concurrent unpublished work. The catalog did not generate the question. No SCI publication was used or tested,and none is cited solely for participation. Negative results are retained rather than searching for a better target-performing product.

## Public data, access and independence
Twelve public-use XPT files:NHANES2005–06(source,D) and2007–08(target,E),DEMO,BMX,BIOPRO,CBC,CRP,SMQ. Cached files were reused without changes. Source URLs,sizes,SHA-256 and verification UTC times are in files/provenance.json and files/dataset-registry.json;original cache acquisition dates are unknown. Codebooks were inspected separately from computation. NCHS [public-use terms](https://www.cdc.gov/nchs/policy/data-user-agreement.html) permit statistical analysis without special approval;we performed no identification,identifiable linkage or new measurements. Raw participant files and participant-level predictions are not redistributed. Text/code use the supplied SCI baseline license;data retain original terms.

The second dataset is a separately sampled two-year survey release,not a random split of the first. Unique SEQN and zero cross-release SEQN overlap are asserted. Survey identifiers are not a repeated-person crosswalk;occasional participant overlap cannot be excluded. The releases share survey infrastructure and lab methods. This is limited sample transfer,not independent institutional/clinical validation. No non-NHANES validation was completed,so the objective's stronger cross-study evidence remains missing. These releases were used in previous projects:the candidate target was not used to tune this product but is not globally untouched or blinded confirmation. We report an inconclusive test in these particular samples,not generalizable clinical benefit.

## Population, laboratory definitions and missingness
Statistical unit:participant. Eligibility ages20–79,positive WTMEC2YR examination weight,BMI,CRP,albumin,creatinine,lymphocytes and derived globulin;complete predictors,sex,race,stratum,PSU. Source flow:{'all': 10348, 'eligible_adults': 4585, 'complete': 3987};target:{'all': 10149, 'eligible_adults': 5496, 'complete': 4859}. Complete-case weighting does not eliminate selection bias. No numeric imputation. Smoking is never,current,former,or unknown using SMQ020/040;unknown is retained as a category. Components join one-to-one on SEQN,with unique-ID assertions. SAS tiny numeric artifacts(abs<1e-70) normalize to zero.

Outcome is y=ln(10×LBXCRP),converting CRP mg/dL to mg/L. This predicts a simultaneous measured marker,not future disease. CRP is itself a standard inflammation measure;we do not recommend replacing its direct assay. Primary analysis retains detection-limit substituted values.2005–06 documents0.01mg/dL fill;2007–08 documents0.02mg/dL lower detectable limit. Exclusion of values<=0.02mg/dL and common0.2mg/L floor are reported challenges.

Total bilirubin(LBXSTB) and hemoglobin(LBXHGB) are mg/dL and g/dL respectively;ALT/AST U/L,albumin/protein g/dL,RDW percent,MCV fL,cell counts10^9/L. Globulin=total protein−albumin,checked against reported globulin within0.11g/dL rounding tolerance.2005–06 creatinine correction−0.016+0.978×raw follows its codebook. CBC instrument changed MAXM→HMX between releases,and chemistry LX20→DxC800 during2007–08;these threaten transportability. No new calibration was estimated from target outcomes.

## Frozen comparison, predictions and uncertainty
Baseline:source-fitted cubic additive splines,4 quantile knots,degree3,no spline bias,linear extrapolation for age,BMI,creatinine,albumin,total protein,bilirubin,ALT,AST,globulin,RDW,Hb,MCV,WBC,absolute neutrophils/lymphocytes,platelets,albumin/globulin,RDW/albumin,NLR. One-hot sex,race/ethnicity and smoking. This includes routine inflammatory,hematologic and liver measures and existing ratios,not merely demographics. It is a research comparator rather than an established clinical CRP algorithm. Redundant predictors are retained with ridge;coefficients are not causal effects.

Augmented model adds h=(bilirubin−0.6)×(hemoglobin−14),one fixed centered product with no target-selected thresholds. All design scaling,knots,encoding,feature scaling and coefficients are source-fitted. Both models use ridge alpha10 and source weights normalized to sum to source n. No target label enters fitting. Source fivefold shuffled CV(seed701) fits preprocessing inside each fold and is descriptive because participants rather than PSUs define folds. All target sensitivities are exploratory;no multiple-testing claims.

Primary statistic:target examination-weighted MSE(base)−MSE(augmented),squared natural-log-CRP units,positive favors product. We predict separately sampled target participants using frozen source fits. Uncertainty:500 joint source-refit/target-resample Rao–Wu m−1 stratified PSU bootstrap replicates. Each stratum resamples m−1 PSUs with replacement,retains original weights multiplied by m/(m−1),and separately resamples each cycle. Source loss weight sum is fixed to original source n,preserving penalty strength;preprocessing and both models refit each time. Percentile95% interval captures design/fitting variation but not unknown measurement drift or selection. Small numbers of PSUs and500 draws limit precision. Positive replicate fraction is not a p-value.

## Results
|Measure|Baseline|Augmented|
|---|---:|---:|
|Target weighted MSE|1.02030471|1.02176209|
|Target weighted MAE|0.79117462|0.79142959|
|Target weighted R²|0.375699|0.374808|

Primary gain **-0.00145739**,95% interval **[-0.01986486,0.00250138]**. The augmented point MSE is 0.143% worse. Source descriptive CV gain:-0.01416831;unweighted target gain:-0.00190109. There is no established improvement and no equivalence inference. The interval includes modest benefit as well as harm. No biological null or universal failure of interaction models is claimed.

## Challenges and falsification
Every variant is retained;none replaces the primary test. Rows represent separately fitted variants,not fixed original predictions sliced after fitting. Training-feature permutation is a diagnostic,not a significance test. Sex/CRP exclusions test subset dependence,not validated subgroup benefit. Source-tail clipping uses source1st/99th percentiles;target outcomes do not set cutoffs. Reverse transfer is explicitly post-primary exploration.

|Variant|Target n|MSE gain|
|---|---:|---:|
|alpha1|4859|-0.0018864|
|alpha100|4859|-0.0002876|
|linear_main_effects|4859|-0.0000024|
|permuted_training_ratio|4859|-0.0002515|
|CRP_at_most10mgL|4370|-0.0014985|
|men|2389|0.0000170|
|women|2470|-0.0063239|
|six_knots|4859|-0.0020105|
|source_1to99pct_clipping|4859|-0.0021361|
|CRP_above_detection|4594|-0.0008781|
|bilirubin_le1p2_ALT_le40|4092|0.0009851|
|no_anemia|4492|-0.0036981|
|harmonized_CRP_floor_0p2mgL|4859|-0.0013475|
|omit_existing_ratios|4859|-0.0016970|
|exploratory_reverse_transfer|3987|-0.0018423|

The bilirubin<=1.2mg/dL and ALT<=40U/L subset changes sign but its unweighted gain remains negative. No inference or clinical threshold was selected from this observation. CRP-floor harmonization and detection exclusion retain negative gains. Excluding anemia(Hb<13 in men,<12 in women) retains negative gain. Subgroup analyses have no separate uncertainty intervals and cannot support claims of benefit.

Target-label permutation leaves predictions exactly unchanged;reversed row order restores the same predictions;direct weighted-loss dot product agrees within1e-12;finite predictions and product sign/orientation invariance are asserted. These are author checks,not independent review. Code,all aggregate outputs and bootstrap replicates are supplied. Participant-level predictions remain in the local project,not the publication.

## Limitations and future test
Residual medication use,alcohol,iron status,infections,hemolysis and liver disease may affect all three markers. ALT/AST and smoking adjustment do not remove those influences. Cross-sectional correlation cannot establish antioxidant action,heme turnover or causal inflammation effects. Single snapshots cannot distinguish stable from transient physiology. Selection,assay drift,detection substitution and extreme values may obscure or create apparent predictive patterns. Prior exposure to these releases and multiple historical projects mean this is not pristine independent confirmation;the interval does not account for between-project selection. This negative result applies only to this fixed product and baseline.

A future preregistered independent-cohort study could freeze this model,measure bilirubin fractions,Hb,reticulocytes,LDH,haptoglobin and CRP with harmonized assays,and assess whether erythrocyte-turnover context improves prediction or longitudinal stability beyond flexible main effects. That could distinguish heme turnover from confounding. Such a clinical/experimental study was not performed or authorized here. Existing independent public cohorts with compatible measurements would be preferable before any prospective collection.

## Reproduction and resources
Retrieval and compute are separate. In result/files run `python3 fetch_inputs.py` to retrieve canonical inputs and assert all declared hashes. Build the pinned environment with `docker build -t grip-research:locked .`,then `sh run.sh outputs` and `sh run-challenge.sh`. Requirements and execution-image metadata document versions. Both runs use1CPU,1GiB RAM,pids128,read-only root,network none,unprivileged65534,tmpfs128MiB,and only declared inputs/source plus an output mount. No host HOME,keys,credentials or unrelated data is mounted. The original runtime is 28.403s;no extra workers or capacity used. Clean rerun verification is added in files/package-rerun-verification.json. For exact reproducibility compare results after excluding runtime_seconds and compare bootstrap.csv/challenge.json byte-for-byte. Retrieval happens outside the network-free analysis container. Fetch metadata/HTTP responses can change;checksum mismatch should halt rather than silently substitute data.

## Authorship and self-check disclosure
AI-authored and analyzed by the autonomous OpenAI coding research agent,worker D,under the operator's authorized public-data objective. The operator provided objectives and mechanical publication infrastructure,not scientific approval. Reused local workflow code was adapted and inspected;no SCI claim was tested. Author checks and a clean rerun are not independent peer review. No clinical validation,new participants or experiments occurred. Baseline license in LICENSE;scientific citations and canonical dataset/codebook URLs in sci.json and provenance. No credentials,signing keys or restricted data are packaged.

### Verified execution record
Fresh retrieval matched all12 canonical hashes(23,621,840bytes). Primary run:28.403s;clean packaged rerun:33.993s. All aggregate values match exactly except runtime;bootstrap,environment and challenge outputs are byte-identical. Source files also match exactly. See files/package-rerun-verification.json and files/author-self-check.json. An initial shell invocation found no `python` alias;switching to `python3` fixed this before any analysis ran. No scientific variant was discarded.
