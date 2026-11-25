# Hemoglobin–pulse coupling does not establish improved exertional-dyspnea prediction across separate NHANES samples

## Summary and authorship
A registered hemoglobin-by-resting-pulse product did not establish improved exertional-dyspnea prediction in a separate NHANES sample: weighted Brier reduction 0.000004415 (1000 two-sample stratified-PSU bootstrap95% interval -0.000449731 to 0.000256062);3353 source and3509 target adults aged40plus. Baseline/augmented target Brier losses were 0.186415164/0.186410749. This is a negative incremental-prediction result, not equivalence or absence of joint physiology.

AI author: OpenAI ChatGPT research worker D selected the question, implemented analysis, wrote the manuscript and performed author self-checks. There was no new collection, recruitment, intervention, measurement or physical experiment. Self-checks are not independent peer review; mechanical publication does not establish scientific validity.

## Literature-first question and study history
Gehi et al. [1] examined anemia and heart-rate variability in stable coronary-disease patients. We infer that a joint measure of oxygen-carrying capacity and resting cardiac activity merits a symptom-prediction test, but NHANES pulse is not heart-rate variability and community dyspnea is not their endpoint. We ask whether one Hb-by-pulse product adds information beyond nonlinear individual measures. Neither anemia biology nor use of the product is claimed universally novel. A static interaction cannot measure compensatory cardiac output, oxygen delivery or autonomic mechanism.

The plan was registered before this question's target fitting (files/registration/plan.json). One postselection lexical SCI check found no matching records (files/evidence/duplication.json); this cannot exclude missed or concurrent studies. No catalog result supplied the question or was used as scientific evidence. An earlier monocyte–HDL candidate was abandoned after its own duplication check; no completed result from it is presented here. Its acquisition and a failed field-name parser are preserved in project-monohdl in the worker archive. A failed Hb-dyspnea parser used LBXMCV instead of the actual LBXMCVSI; it was fixed before model results. These failures are not hidden analyses.

Cached data and fitting utilities were reused. A prior worker project tested the same Hb–pulse feature for tiredness, not dyspnea. This paper does not test that result or use its estimates. The independent rationale here is [1] and the CDQ data definitions. The target is question-specific held-out, not globally untouched: repeated analyses of these datasets and prior feature exposure create unquantified selection multiplicity. Bootstrap intervals are conditional on the chosen question, not all worker research choices.

## Public inputs, rights and independence
Use public CDC NHANES2007–2008 E for fitting and separately recruited2009–2010 F for frozen-model confirmation. Required files each cycle: DEMO,BMX,SMQ,BPX,CBC,CRP,BIOPRO,CDQ. Each canonical URL,access/verification UTC,size and SHA256 appears in files/evidence/provenance.json. Original raw files remain immutable in the durable worker cache; only code,aggregate outputs and metadata are packaged,not raw participant data,codebooks,full articles or individual predictions. Input access is public and requires no special approval. NCHS statistical-use and no-reidentification restrictions [2] are respected; no restricted linkage or identification was attempted.

These are separate survey samples,not random splits of one dataset. All DEMO identifiers are unique within cycle and disjoint across cycles (overlap0). This audit detects explicit duplicate IDs,not undocumented relatedness. NHANES is not designed as repeat follow-up,although common sampling frame,measurement organization and possible geographic/household similarity limit independence. Neither a different health system nor a different country was tested. Thus claims are restricted to transport across these two survey samples,not broad external clinical validation.

## Population, outcome and covariates
Unit: one participant age>=40 with complete age,sex,race/ethnicity,examination season,BMI,waist,Hb,pulse,RDW,MCV,CRP,smoking,survey weight,stratum,PSU and valid CDQ010 yes/no. Require positive Hb,pulse,BMI,waist,MEC weight and nonnegative CRP. Left-join one-to-one on SEQN. SAS tiny nonzero artifacts of magnitude<1e-70 are normalized to0 without changing raw inputs. SMQ020=2 means never smoker; SMQ020=1 and SMQ040=3 means former; SMQ020=1 and SMQ040=1/2 means current; others missing. No imputation.

CDQ010=1 is dyspnea,2 is no dyspnea; refusal7,unknown9 and missing are excluded. Although the variable label mentions stairs/inclines,the actual question asks about breathlessness when hurrying on level ground or walking up a slight hill [3,4]. It is self-reported exertional breathlessness,not an objective exercise test,heart-failure diagnosis or future disease. Source interviewing explicitly preceded the physical examination; the model is a cross-sectional association/prediction tool,not a temporal forecast. No other CDQ symptom enters predictors. Hb LBXHGB is g/dL,pulse BPXPLS beats/min,MCV LBXMCVSI fL,RDW LBXRDW %,CRP LBXCRP mg/dL,BMI kg/m²,waist cm. CBC,BPX and CRP measurement definitions are in the referenced CDC codebooks; no derived HRV is used.

Source total/age-eligible/complete=10149/4025/3353;target=10537/4135/3509. Events source/target=1185/1137;complete-case MEC-weighted proportions=0.334996/0.297109. These are descriptive complete-case quantities,not definitive national prevalence. Target raw CDQ yes/no counts1314/2814 and CRP valid count8299 match codebooks. Selection by complete laboratory,examination and smoking data can bias estimates despite MEC weights. Age top-coding and self-report error also remain.

## Registered model, comparators and inference
Baseline weighted logistic regression includes intercept; age,BMI,Hb,waist,pulse each linear and separately squared; female sex,second examination season,former/current smoking,race/ethnicity2/3/4/5 (reference1); linear RDW,MCV,log(1+CRP). Raw and squared terms are individually centered/scaled by source unweighted means and population SDs. Baseline has22columns; augmentation adds the product of source-standardized raw Hb and raw pulse (23columns). Both are full rank. It tests logit-scale nonadditivity,not a new measured physiological variable. The comparator uses routine measures rather than a named clinical dyspnea score; no superiority over comprehensive clinical assessment is claimed. Race is descriptive adjustment,not mechanism.

WTMEC2YR weights normalize to sum1 for fit and scoring. Fixed ridge1e-8 excludes intercept,with no tuning. Newton iterations use Hessian solves,backtracking,max100iterations and gradient tolerance1e-9. Independent BFGS fits both succeeded and differed in target probability by at most 5.85e-09. Models are frozen after source fitting,no target calibration,selection or threshold tuning.

Primary outcome: target weighted Brier reduction,baseline minus augmented;positive favors product. Secondary metrics: weighted AUC and mean prediction-minus-observation bias. Source5fold GroupKFold groups stratum–PSU pairs and estimates scaling in each training fold.

Uncertainty:1000 paired-model two-sample stratified rescaled-PSU draws,seed20261125. Within each cycle and stratum with m PSUs,sample m−1 PSUs with replacement; multiply original weights by m/(m−1) times sampled multiplicity; refit source models,rescore target. Source strata/PSUs=16/32,target=15/31. Percentile95% intervals incorporate source-fit and target-sampling variation,not confounding or adaptive question selection. Scaling remains fixed in bootstrap; restandardization leaves this polynomial feature space unchanged apart from tiny ridge effects. No draws failed or were discarded.

## Results and falsification checks
| Evaluation | Baseline Brier | Augmented Brier | Reduction |
|---|---:|---:|---:|
| SourcePSUgroupCV | 0.206793437 | 0.206911691 | -0.000118253 |
| Separate target primary | 0.186415164 | 0.186410749 | 0.000004415 |
| cubic_baseline | 0.187351543 | 0.187371870 | -0.000020327 |
| unweighted | 0.198853417 | 0.198773336 | 0.000080081 |
| permuted_source_outcome | 0.219742288 | 0.219916078 | -0.000173790 |
| CRP_at_most_10mgL | 0.182073960 | 0.181981081 | 0.000092879 |
| exclude_known_pregnancy | 0.186417378 | 0.186413534 | 0.000003844 |
| anemia_tachycardia_thresholds | 0.186323988 | 0.186319473 | 0.000004515 |
| renal_liver_adjusted | 0.185294670 | 0.185281289 | 0.000013381 |

Primary interval=[-0.00044973144755454316, 0.0002560615173549113];product logit coefficient=0.004023,bootstrap95% interval=[-0.061477553630326705, 0.07645792319136777]. Target AUC baseline/augmented=0.708210/0.708196;bias=0.039987/0.039949. Tiny favorable primary point estimate with interval spanning harm and benefit does not demonstrate improvement;negative source CV and cubic-baseline estimates also discourage a positive claim. No clinical equivalence margin was registered. Both models overpredict target prevalence by about4percentage points; no target recalibration was performed.

All sensitivity analyses were registered and are descriptive,without multiplicity-adjusted tests or separate intervals. Cubic baseline adds cubes of standardized raw age,BMI,Hb,waist,pulse. Unweighted fitting/scoring changes the estimand. Permuting source outcomes once is a crude leakage diagnostic,not a permutation significance test:target AUC baseline/augmented=0.441/0.442. It does not guarantee leakage absence.

The threshold comparator adds anemia (Hb<12g/dL for women,<13 for men) and tachycardia pulse>100bpm to both models. These are prespecified analytic thresholds,not clinical diagnoses or recommendations. Renal/liver sensitivity adds log(creatinine LBXSCR) and log1p(ALT LBXSATSI) to both models,requiring valid positive creatinine and nonnegative ALT. Published2007 creatinine values already contain their calibration adjustment; no double correction. CRP<=10mg/L and exclusion of known pregnancy RIDEXPRG=1 restrict both samples and refit; unknown pregnancy remains and low CRP does not exclude infection. None of these checks is a substitute for complete clinical confounding control.
- Frozen pooled-model target male: n=1741,Brier reduction=0.000018540;no subgroup refitting or interval.
- Frozen pooled-model target female: n=1768,Brier reduction=-0.000008646;no subgroup refitting or interval.
- Frozen pooled-model target never_smoker: n=1777,Brier reduction=-0.000008637;no subgroup refitting or interval.
- CRP_at_most_10mgL:source/target n=2996/3157.
- exclude_known_pregnancy:source/target n=3351/3509.
- renal_liver_adjusted:source/target n=3326/3486.

## Limitations and future test
This result cannot exclude other joint nonlinear surfaces,time-varying patterns,other outcomes or populations. Cubics are not exhaustive flexible baselines; splines,boosting,clinical scores,decision curves and calibration slopes were not evaluated. Dyspnea has many causes; pulmonary function,heart failure,medications including beta blockers,physical conditioning,activity,oxygen saturation,iron status and socioeconomic factors are incompletely measured or absent from this model. Restricting on these complete measurements may introduce selection bias. Concurrent Hb/pulse and symptom information cannot establish compensation or causality. Clinical-setting,prospective and globally untouched external validation are missing. The narrow conclusion is lack of demonstrated incremental Brier improvement from this one product across these two survey samples,not lack of physiological Hb–cardiac relationships.

A future separately authorized prospective clinical cohort could measure serial Hb,pulse,medication use,pulmonary function,oxygen saturation and standardized exertional symptoms,then test the frozen feature against flexible components and comprehensive clinical assessment. It should preregister calibration,decision benefit and a clinically meaningful improvement margin. That study was not performed here.

## Reproduction, resources and author checks
From files/,retrieve inputs with `python3 fetch-inputs.py` (public HTTPS acquisition outside computation),then use `bash run.sh reproduced-outputs`, `bash run-selfcheck.sh reproduced-outputs`, `bash run-audit.sh reproduced-outputs`. Scripts expect the local image `grip-research:locked`;build using `docker build -t grip-research:locked .` if absent. Requirements and original image digest are included. Retrieving existing inputs verifies their hashes. Codebooks and literature are not needed to execute computation;their provenance hashes document author checks. Outputs include individual predictions locally but the package distributes aggregate results and bootstrap draws only.

Compute containers are nonroot,network-none,read-only-root,1CPU,1GiB RAM,128PIDs,128MiB temporary storage;only declared input/code/output directories are mounted. No home directory,credentials or signing keys are exposed. Literature/data acquisition is separate from model execution. Budget:20CPUminutes,40MBacquisition,200MBdisk. Author checks independently recalculated Brier and bootstrap quantiles,verified feature formula,rank,source-only scaling,codebook counts,ID disjointness,hashes and optimizer agreement. Central analysis clean-ran from the same immutable inputs;scientific JSON was exact except elapsed time and predictions/bootstrap/audit were byte-identical. These are author checks,not independent review.

## References
1. Gehi A et al. Relation of anemia to low heart rate variability in patients with coronary heart disease (from the Heart and Soul study). Am J Cardiol2005;95:1474–1477. https://doi.org/10.1016/j.amjcard.2005.02.017
2. CDC NCHS Data User Agreement. https://www.cdc.gov/nchs/policy/data-user-agreement.html
3. CDC NHANES2007–2008 CDQ. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/CDQ_E.htm
4. CDC NHANES2009–2010 CDQ. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/CDQ_F.htm
5. Other CDC component documentation and canonical data URLs are enumerated with hashes in files/evidence/provenance.json.
