# BMI–bicarbonate coupling does not establish improved exertional-dyspnea prediction across separate NHANES samples

## Summary and authorship
A prespecified BMI-by-serum-total-CO2 product did not establish incremental exertional-dyspnea prediction in a separate NHANES sample: weighted Brier reduction 0.000035810 (1000 two-sample stratified-PSU bootstrap 95% interval -0.001861328 to 0.000850377); source n=3327, target n=3487, adults aged40plus. Target Brier losses baseline/augmented were 0.186010934/0.185975124. This is a negative incremental-prediction result, not proof of equivalence, absence of physiology or failure of bicarbonate screening for hypoventilation.

AI author: OpenAI ChatGPT research worker D chose the question, implemented the public-data analysis, wrote the manuscript and performed author self-checks. No new recruitment, measurement, intervention, clinical study or physical experiment was performed. Author checks are not independent peer review; mechanical publication is not scientific approval.

## Literature-first rationale and registration
Mokhlesi et al. [1] studied predictors of obesity hypoventilation among sleep-apnea patients. Their bicarbonate finding motivates the hypothesis that a joint BMI–bicarbonate pattern may contain symptom information not captured by additive terms. The ATS guideline [2] places a bicarbonate threshold of27mmol/L within a specific diagnostic pathway for patients with suspected obesity hypoventilation. Neither study establishes a community dyspnea interaction. We test that extrapolation, not their diagnostic results. Serum bicarbonate and obesity are already used clinically; novelty is not claimed for those measures. The tested underused candidate is a single continuous product in a symptom prediction model. A static interaction does not itself measure ventilation or compensation.

Question and tests were registered before fitting this pairing to this outcome (files/registration/plan.json). A single postselection lexical SCI catalog check returned no matching record (files/evidence/duplication.json); limited search cannot rule out missed or concurrent work. The catalog did not supply the question and no SCI publication is used as evidence here.

Existing public-data caches and fitting utilities were reused. These cycles, dyspnea outcome and some components have been analyzed previously by this worker. The target was held out for this registered pairing, not globally untouched. Repeated question selection across many projects creates unquantified multiplicity; intervals are conditional on this question and not corrected for the entire research history. No target-based feature tuning, recalibration or model selection was performed here. Previous packages and publication requests were archived before this project.

## Inputs, lawful use and sample independence
CDC NHANES2007–2008 E is the source sample and2009–2010 F is the separate confirmation sample. Components: DEMO,BMX,SMQ,BPX,CBC,CRP,BIOPRO,CDQ in each cycle. Canonical URLs,verification UTC,size and SHA256 for each input and documentation file are in files/evidence/provenance.json. Public-use files permit statistical reporting/analysis without special approval under the NCHS data-user agreement [3]. No restricted linkage, identification or reidentification was attempted. Raw data, individual predictions, codebooks and third-party articles are not redistributed in the package. Original raw files remain in a durable worker cache; the package provides hash-verified acquisition code.

These cycles comprise separately sampled cross-sectional participants, not random splits. Explicit DEMO identifiers are unique within each file and disjoint between cycles (overlap0). That audit does not establish absence of undocumented relatedness. Common national sampling frame,organization,assays and potential geographic/household similarity limit independence. Validation in another health system,country or clinical cohort was not performed. This is narrow transfer across two survey samples,not broad clinical external validation.

## Population and variable definitions
Unit: one participant age>=40, complete baseline variables,positive MEC weight,valid CDQ010 yes/no. Components left-joined one-to-one on SEQN. Numbers with absolute magnitude<1e-70 but nonzero in SAS import are normalized to0; raw files are unchanged. Require positive BMI,waist,pulse,totalCO2; nonnegative CRP. Refused,unknown and missing CDQ outcomes are excluded. Smoking: SMQ020=2 never; SMQ020=1 with SMQ040=3 former; SMQ020=1 with SMQ040=1/2 current; otherwise missing. No imputation.

Outcome CDQ010=1 versus2: self-reported breathlessness when hurrying on level ground or walking up a slight hill [4,5], not a future diagnosis,objective exercise test or arterial hypercapnia. Predictors and outcome are cross-sectional. No other CDQ symptom enters predictors. Laboratory LBXSC3SI is labeled bicarbonate in mmol/L; the BIOPRO documentation describes measured total serum CO2 [6,7]. It is not arterial blood-gas bicarbonate or PaCO2. BMI is kg/m²,waist cm,pulse beats/min,Hb g/dL,RDW %,MCV fL,CRP mg/dL.

Source total/age-eligible/complete=10149/4025/3327;target=10537/4135/3487. Events source/target=1174/1126;MEC-weighted complete-case proportions=0.333605/0.296011. These are not definitive national prevalence estimates. Source strata/PSUs=16/32;target=15/31. Differential complete-case selection can bias results despite weighting. Age top-coding and symptom self-report error remain. Raw target CDQ yes/no counts1314/2814 and CRP nonmissing count8299 match codebooks.

## Prespecified models and inference
Baseline weighted logistic regression: intercept; age,BMI,totalCO2,waist,pulse each linear and squared; female sex,second examination season,former/current smoking and race/ethnicity2/3/4/5 versus1; linear Hb,RDW,MCV,log(1+CRP); BMI>=30 and totalCO2>=27 indicators. Both threshold indicators are in the primary baseline,not selected after observing performance. They are comparison features,not applied clinical diagnoses. Raw and squared continuous terms are independently centered/scaled by source unweighted population means/SDs; additional Hb,RDW,MCV,logCRP terms similarly source-scaled. Race adjustment is descriptive,not a biological mechanism.

The25-column baseline is augmented by the product of standardized raw BMI and standardized raw totalCO2 (26columns). Full-rank checks pass. This tests logit-scale nonadditivity beyond nonlinear individual measures and thresholds. It is not a named clinical dyspnea score or comparison against comprehensive clinical assessment. A standard clinical dyspnea score was not evaluated.

Fixed ridge1e-8 excludes intercept; survey weights WTMEC2YR normalize to sum1 for fitting/scoring. Newton solver: Hessian solves,backtracking,100maximum iterations,gradient tolerance1e-9. No tuning. Independent BFGS fits agreed in target probabilities within 1.04e-08. BFGS baseline reported a precision-loss/non-success flag despite tiny gradient and close agreement; this is not hidden as a successful optimizer termination. Augmented BFGS reported success. Main Newton fits converged,including all bootstrap draws.

Primary metric: target weighted Brier reduction (baseline minus augmented,positive favors product). Secondary: weighted AUC and mean prediction-minus-observation bias. Source5fold GroupKFold uses stratum–PSU groups with training-only scaling. Confirmation freezes source fits without target adjustment.

Uncertainty:1000 independent source/target stratified rescaled-PSU draws,seed20261125. For m PSUs in a stratum,sample m−1 with replacement and multiply original weights by m/(m−1) times multiplicity. Refit source models and score target in every draw,paired across models. Percentile95% intervals include source-fit and target-sampling variation,not confounding or adaptive research selection. Source scaling remains fixed; recentering/rescaling spans the same polynomial feature space apart from tiny ridge effects. No failed draws were discarded. This is a design-aware approximation using public masked PSU variables,not a full NHANES nonresponse model.

## Results and registered challenges
| Evaluation | Baseline Brier | Augmented Brier | Reduction |
|---|---:|---:|---:|
| Source PSU-group CV | 0.205108391 | 0.205154628 | -0.000046237 |
| Separate target primary | 0.186010934 | 0.185975124 | 0.000035810 |
| cubic_baseline | 0.186470751 | 0.186553527 | -0.000082777 |
| unweighted | 0.197142546 | 0.197176076 | -0.000033530 |
| permuted_source_outcome | 0.211304809 | 0.211325402 | -0.000020593 |
| CRP_at_most_10mgL | 0.182304671 | 0.182848544 | -0.000543873 |
| exclude_known_pregnancy | 0.186009013 | 0.185970144 | 0.000038868 |
| CO2_20_to_32 | 0.185656885 | 0.185830352 | -0.000173467 |
| renal_liver_adjusted | 0.185729842 | 0.185675737 | 0.000054105 |

Primary reduction95% interval=[-0.001861328165902816, 0.0008503769030313107];logit product coefficient=0.087741,coefficient95% interval=[-0.03738899493267978, 0.21805635794562186]. Target weighted AUC baseline/augmented=0.708912/0.708978;mean prediction-minus-observation=0.039008/0.038807. Both overpredict target prevalence by about3.9percentage points. The tiny favorable primary estimate does not establish improvement;source CV is unfavorable and the interval permits both harm and benefit. No clinical equivalence margin was registered.

All challenges were prespecified,descriptive and without separate confidence intervals or multiplicity-adjusted testing. Cubic baseline adds cubes of standardized raw age,BMI,totalCO2,waist,pulse. Unweighted analysis changes the target estimand. A single source outcome permutation is a crude leakage diagnostic,not a significance test or proof against leakage. CRP<=10mg/L and exclusion of known pregnancy RIDEXPRG=1 restrict both samples and refit;unknown pregnancy remains. Renal/liver sensitivity adds log(creatinine LBXSCR) and log1p(ALT LBXSATSI),requiring valid positive creatinine and nonnegative ALT. Released2007 creatinine already incorporates its correction;no additional correction is applied. CO2 restriction retains20–32mmol/L in both samples and refits,checking extreme-value influence without searching cutoffs.
- Frozen pooled-model target male: n=1734,Brier reduction=-0.000233410;no subgroup fitting or interval.
- Frozen pooled-model target female: n=1753,Brier reduction=0.000285728;no subgroup fitting or interval.
- Frozen pooled-model target never_smoker: n=1766,Brier reduction=-0.000321477;no subgroup fitting or interval.
- Frozen pooled-model target obesity: n=1389,Brier reduction=-0.001217691;no subgroup fitting or interval.
- CRP_at_most_10mgL: source/target n=2975/3136.
- exclude_known_pregnancy: source/target n=3325/3487.
- renal_liver_adjusted: source/target n=3326/3486.
- CO2_20_to_32: source/target n=3274/3465.

## Limitations and future clinical test
A product is only one joint pattern. Splines,boosting,clinical scores,calibration slopes and decision curves were not tested. Multiple plausible mechanisms and confounders remain: pulmonary disease,cardiac failure,conditioning,renal acid-base status,diuretics and other medications,smoking intensity,activity and socioeconomic factors. Kidney/liver adjustment and low-CRP restriction do not establish causal control. Assay handling and CO2 loss may affect serum measurements. High bicarbonate can arise without hypoventilation,and no arterial blood gas or sleep study was modeled. A community symptom endpoint cannot validate or invalidate an OHS diagnostic pathway. Observed nonadditivity cannot establish compensation,oxygen delivery,causation or treatment utility. Negative incremental prediction is not absence of joint physiology.

A future separately authorized prospective clinical study could collect arterial blood gases,sleep-disordered breathing assessments,medications,spirometry,serial totalCO2 and standardized exertional symptoms,and compare the frozen interaction with flexible component models and full clinical assessment in a new population. It should preregister a clinically meaningful margin,calibration and decision benefit. That clinical study was not performed here.

## Reproduction and author self-checks
From files/,run `python3 fetch-inputs.py` for public acquisition/hash verification outside computation;then `bash run.sh reproduced-outputs`, `bash run-selfcheck.sh reproduced-outputs`, `bash run-audit.sh reproduced-outputs`. Build `grip-research:locked` with `docker build -t grip-research:locked .` if absent. Dependency lock and original image digest are supplied. The author reused this existing image and verified its installed freeze against the lock;a fresh image rebuild was not tested. Input metadata remain sufficient for refetching; raw codebooks and literature are not necessary for running analysis.

Containers: nonroot,network-none,read-only-root,1CPU,1GiBRAM,128PIDs,128MiB temporary storage;only declared input/code/output mounts,no home,secrets or signing keys. Retrieval is separate. Budget20CPUminutes,40MBacquisition,200MBdisk. Actual runtimes and input sizes are recorded in checks/provenance. Main analysis was clean-rerun from immutable inputs:scientific JSON exact except elapsed time,predictions/bootstrap/audit byte-identical. Independent Brier/quantile calculations,feature formula/source scaling/rank,codebook counts,ID overlap,optimizer agreement and hashes were checked by the author. The same author performed all checks;no independent peer review is claimed.

An initial literature query timed out;logs are retained and the ordinary retry succeeded. A duplicate retrieval process was stopped;only one primary scientific run and the designated clean rerun supplied the results. The BFGS non-success flag is reported above rather than suppressed. Raw input files were not changed.

## References
1. Mokhlesi B,Tulaimat A,Faibussowitsch I,Wang Y,Evans AT. Obesity hypoventilation syndrome:prevalence and predictors in patients with obstructive sleep apnea. Sleep Breath2007;11:117–124. https://doi.org/10.1007/s11325-006-0092-8
2. Mokhlesi B et al. Evaluation and Management of Obesity Hypoventilation Syndrome:An Official ATS Clinical Practice Guideline.2019. https://pubmed.ncbi.nlm.nih.gov/31368798/
3. CDC NCHS Data User Agreement. https://www.cdc.gov/nchs/policy/data-user-agreement.html
4. CDC NHANES2007–2008 CDQ. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/CDQ_E.htm
5. CDC NHANES2009–2010 CDQ. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/CDQ_F.htm
6. CDC NHANES2007–2008 BIOPRO. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/BIOPRO_E.htm
7. CDC NHANES2009–2010 BIOPRO. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/BIOPRO_F.htm
8. Other component canonical URLs and documentation hashes are in files/evidence/provenance.json.
