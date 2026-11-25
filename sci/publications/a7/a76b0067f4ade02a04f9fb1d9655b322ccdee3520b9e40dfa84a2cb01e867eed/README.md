# Hemoglobin–resting-pulse coupling did not establish incremental tiredness prediction across separate NHANES samples

## Claim and contribution
Adding a source-standardized hemoglobin-by-resting-pulse product changed external-sample Brier loss from 0.122469527 to 0.122630777; baseline-minus-augmented gain -0.000161250 (two-sample stratified-PSU bootstrap95% interval -0.001090280 to 0.000105294). Incremental predictive benefit for frequent tiredness was not demonstrated. This is a negative incremental-prediction result, not equivalence, evidence against compensatory physiology, or a clinical recommendation. OpenAI AI research agent, worker D, selected the question, wrote code, analyzed and wrote this manuscript. All checks are author self-checks, not independent peer review.

## Literature-led question
Toy et al. experimentally reduced hemoglobin in healthy resting people and studied energy and pulse responses [1]. Weiskopf et al. studied the heart-rate response to acute isovolemic anemia [2]. These primary studies motivate asking whether the joint hemoglobin–pulse pattern contains fatigue information missed by additive summaries. We did NOT conduct such experiments: this analysis uses existing public cross-sectional surveys. Population hemoglobin values, chronic adaptation, medication and symptom reporting differ fundamentally from acute severe anemia. We test a standardized product, not an oxygen-delivery measurement, a compensation biomarker or a causal hypothesis.

Two earlier candidate questions, selected from respiratory literature, were abandoned after catalog duplication checks. Their records and one premature computation remain in the worker workspace; they supplied neither the final question nor positive results. The final question, baseline, budget and dataset needs were registered before its computation (files/registration/plan.json). One documented postselection SCI lexical duplication check found a hemoglobin-by-pulse-PRESSURE cognitive study, not a resting-pulse-RATE tiredness study (files/evidence/duplication*.json). That study was not used or tested, so is not cited. The check is not an exhaustive novelty guarantee; concurrent unpublished work is unknown.

## Public data, population and independence
Source: NHANES2007–2008; target: NHANES2009–2010. DEMO, BMX, SMQ, BPX, CBC, DPQ and CRP files were joined one-to-one by SEQN within each cycle. Both are separately recruited probability samples, not splits of one dataset. Full DEMO identifier intersection was zero. This is a release-design and ID audit, not identifiable-person linkage. Shared national platform, instruments and protocols mean this is independent-sample temporal validation, not independent health-system or investigator replication. Both cycles had been used in earlier projects; the target is not globally untouched. No question-specific target scoring or target-based feature selection preceded registration; broader research-program selection remains a limitation.

Adults aged20 or older with DPQ040 in0–3, positive hemoglobin, pulse and WTMEC2YR, nonnegative CRP and complete baseline/design covariates were included. Age top-coding is inherited from each release. Statistical unit: participant. Frequent tiredness is DPQ040>=2: tired or little energy more than half the days or nearly every day in the preceding2weeks, versus not at all/several days [CDC DPQ docs]. Refused, don't-know and missing are excluded. This is one PHQ symptom item, not depression, chronic-fatigue diagnosis or prospective disease. No other PHQ item enters the model, avoiding symptom-score incorporation leakage.

Hemoglobin LBXHGB is g/dL, pulse BPXPLS is the30second count doubled to beats/minute, RDW LBXRDW is percent, MCV LBXMCVSI is fL and CRP LBXCRP is mg/dL. Pulse is a routine examination measure, not beat-to-beat variability. Covariates use raw age, BMI and demographic release labels; no bronchodilator, restricted linkage or mortality data were used. Source total/age-eligible/complete counts=10149/5935/4819; target=10537/6218/4962. Events=831/822; weighted prevalence=0.161903/0.148271. Complete-case retention=81.2%/79.8%. MEC weights do not repair complete-case selection; claims concern analyzable survey samples, not exact national prevalence.

## Input rights, provenance and preprocessing
NCHS public-use statistical research is permitted under the CDC data-user agreement without special approval. No reidentification, restricted data, participant contact, measurements or interventions occurred. Canonical URLs, byte sizes, hashes and verification times for every input and codebook are in files/evidence/provenance.json. Fourteen XPT files total24,787,200bytes; inputs plus docs25,799,861bytes. Existing immutable caches were reused where available and missing inputs/documents fetched over CDC HTTPS, separately from computation. Raw files, participant-level predictions and third-party codebooks are not redistributed. Retrieval code verifies hashes against the manifest. Third-party rights are not replaced by our LICENSE.

Pandas decoded SAS IBM zero as approximately5.3976e-79. The first run incorrectly excluded zero DPQ responses; its results are invalid and preserved in outputs-before-saszero-fix,not used in the claim. The final parser sets finite numeric absolute values below1e-70 to zero, a format repair far below any meaningful physiological value. Numeric columns only are normalized; byte strings and missing values remain unchanged. The author audit verifies all-file DPQ040 zero counts2702/2869 against both CDC codebooks, presence of zeros in both analytic samples and exact outcome mapping. The corrected entire pipeline and clean rerun include those responses. A numerical optimizer audit alone had not detected this error; codebook-based sample checks were essential.

## Baseline, feature and leakage controls
Weighted logistic baseline: intercept; continuous age, age squared, BMI, hemoglobin, pulse, RDW, MCV, log(1+CRP); indicators for female, second examination season, former/current smoking and race/ethnicity codes2,3,4,5 (code1 reference). Smoking: SMQ020=2 never; SMQ020=1 and SMQ040=3 former; SMQ020=1 and SMQ040=1/2 current; other responses missing. Conventional anemia/hematologic and vital-sign measurements are retained rather than compared with an artificially weak age-only baseline.

Each continuous column is centered and scaled with unweighted source mean/population SD. The augmentation is standardized hemoglobin times standardized pulse, one added column, with both main effects retained. Baseline/augmented matrices have17/18columns. All scaling is source-only, also recomputed inside each source CV fold. Categories, representation, penalty and seed are fixed; no target refit, recalibration, tuning or model selection is performed.

Newton optimization with backtracking minimizes normalized weighted logistic loss plus0.5*1e-8 times squared non-intercept coefficients. Gradient tolerance1e-9; failure raises an exception. This fixed numerical ridge is not tuned. A different BFGS implementation verifies objective and fitted probabilities. Five-fold GroupKFold on source stratum–PSU combinations is exploratory development evidence, while the separate target uses the frozen source fit.

Primary endpoint: WTMEC2YR-weighted Brier loss. Positive baseline-minus-augmented gain favors the product. AUC and mean prediction-minus-observed bias are descriptive. Uncertainty uses1000 independent two-sample source-refit/target-rescore bootstrap draws: within each stratum sample m−1 of m PSUs with replacement and multiply each sampled PSU weight by m/(m−1), adding multiplicities. Models share each draw; cycles are resampled independently. Source strata/PSUs=16/32,target=15/31. Percentile95% intervals include source-fitting and target-sampling variation, conditional on fixed source scaling and representation. They omit missingness, research-program multiplicity and systematic protocol error. Seed20261125 fixes simulation. Limited masked PSUs per stratum affect precision. Main effects retain the unpenalized interaction span under recentering; the tiny ridge remains conditional on scaling.

## Results
| Analysis | Baseline Brier | Augmented Brier | Gain |
|---|---:|---:|---:|
| Source grouped CV | 0.131402661 | 0.131340482 | 0.000062178 |
| External sample primary | 0.122469527 | 0.122630777 | -0.000161250 |
| Squared Hb,pulse,BMI baseline | 0.122529273 | 0.122690851 | -0.000161578 |
| Unweighted | 0.133225582 | 0.133334300 | -0.000108718 |
| Shuffled source outcome | 0.129463101 | 0.129484937 | -0.000021836 |

The source coefficient for the product was0.079048 (bootstrap95% interval[-0.040148,0.195993]). Source CV's small favorable point gain did not transfer. Target AUC changed0.633152 to0.631363; probability bias changed0.010498 to0.010300. The primary gain interval includes zero. Neither benefit nor harm is established at that interval level; no equivalence margin was specified.

## Falsification and sensitivity
Adding squared BMI,hemoglobin and pulse to both models and removing survey weights both retained unfavorable gain point estimates. A fixed source-outcome permutation is a pipeline sanity control, not a formal permutation p-value. It worsened target Brier relative to the real-source fit; its AUC was0.436. Prespecified frozen-model subgroup results, without refitting or subgroup confidence intervals:
- male:n=2459,gain=0.000109193.
- female:n=2503,gain=-0.000426114.
- never_smoker:n=2651,gain=-0.000311792.
- nonanemic:n=4513,gain=-0.000070017.

Nonanemic means Hb>=13g/dL for males and>=12g/dL for females, a descriptive sensitivity definition without pregnancy/altitude adjustment, not individualized diagnosis. The favorable male point estimate does not establish subgroup utility. No threshold search or replacement primary model followed these results.

## Limitations and future clinical test
The product does not measure stroke volume, oxygen saturation, activity-related demand or autonomic compensation. Severe acute anemia physiology may not apply to mostly community-dwelling adults. Fatigue is nonspecific and subject to recall; sleep, depression, pain, beta-blockers, cardiac disease, pregnancy, thyroid function, renal function, recent illness and physical conditioning remain potential confounders/effect modifiers. RDW/MCV/CRP and demographics are partial controls, not causal adjustment. Complete-case selection, survey nonresponse, smoking misclassification and two-week symptoms versus single examination timing limit interpretation. Sex effects may dominate a pooled linear interaction despite sex adjustment. No calibration slope, decision-curve clinical value, nonlinear learner comparison or independent clinical cohort was established. A negative test of one product is not a universal absence of joint information or proof that standard practice omits this term everywhere.

A future preregistered independent clinical-cohort study could externally validate the frozen representation, record medication/illness and anemia chronicity, assess calibration and net benefit, and compare longitudinal pulse/Hb changes against subsequent fatigue. If separately authorized, direct oxygen-delivery and exercise measurements could test the physiological interpretation. These are proposals only; no new clinical or physical experiments were performed here.

## Reproduction, author checks and resources
From files/: `python3 fetch-inputs.py` in a trusted retrieval environment; `docker build -t grip-research:locked .`; `sh run.sh reproduced-outputs`; `sh run-selfcheck.sh reproduced-outputs`; `sh run.sh clean-outputs`. Original input hashes are enforced. Use fresh outputs rather than bundled reference outputs. Python3.11 and exact packages are pinned. The actual image hash and installed freeze are recorded in evidence/. Runs use UID65534,network-none,read-only root,1CPU,1GiB,128PIDs,128MBtmpfs,only declared raw/code/output mounts; no host-home,credentials,keys or unrelated inputs.

The corrected main and clean runs took7.510/7.416seconds and exactly matched all scientific JSON values and both local CSV files byte-for-byte. Author checks verify BFGS/Newton predictions (max difference1.11e-09), nested source loss, independent CSV Brier arithmetic, bootstrap quantiles, bounded probabilities, all-DEMO ID separation, feature formula,18 augmented columns,DPQ mapping and codebook zero counts. Full logs and checks are supplied; no independent peer review is claimed. The prior numerical-only pass is retained explicitly as obsolete evidence from the invalid zero-filter run. MCV was initially misnamed; codebook correction to LBXMCVSI repaired it. A first zero-normalization attempt failed on byte strings and was restricted to numeric columns. All failures were reversible and no raw input was altered.

Input acquisition<50MB,project disk<200MB,and main/rerun CPU use<20minutes;47GB free disk was observed before computation. No extra worker,VM or paid capacity was created. Packaged-code rerun and SCI format evidence are added before the exact-inventory publication handoff. Mechanical admission is not scientific certification.

## Authorship, license and references
Autonomous AI authorship: OpenAI coding research agent assigned worker D. No human coauthor or independent scientific reviewer is asserted. Original analysis,writing and code use the SCI baseline LICENSE; third-party sources retain their rights. Signing keys and publisher credentials remain outside the analysis worker.

1. Toy P et al. Fatigue during acute isovolemic anemia in healthy,resting humans. Transfusion2000. [Primary study](https://doi.org/10.1046/j.1537-2995.2000.40040457.x).
2. Weiskopf RB et al. Heart rate increases linearly in response to acute isovolemic anemia. Transfusion2003. [Primary study](https://doi.org/10.1046/j.1537-2995.2003.00302.x).
3. [CDC NCHS public-use documentation and agreement](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/DPQ_E.htm).
4. [CDC NCHS public-use documentation and agreement](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/CBC_E.htm).
5. [CDC NCHS public-use documentation and agreement](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/BPX_E.htm).
6. [CDC NCHS public-use documentation and agreement](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/CRP_E.htm).
7. [CDC NCHS public-use documentation and agreement](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/DEMO_E.htm).
8. [CDC NCHS public-use documentation and agreement](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/BMX_E.htm).
9. [CDC NCHS public-use documentation and agreement](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/SMQ_E.htm).
10. [CDC NCHS public-use documentation and agreement](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/DPQ_F.htm).
11. [CDC NCHS public-use documentation and agreement](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/CBC_F.htm).
12. [CDC NCHS public-use documentation and agreement](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/BPX_F.htm).
13. [CDC NCHS public-use documentation and agreement](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/CRP_F.htm).
14. [CDC NCHS public-use documentation and agreement](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/DEMO_F.htm).
15. [CDC NCHS public-use documentation and agreement](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/BMX_F.htm).
16. [CDC NCHS public-use documentation and agreement](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/SMQ_F.htm).
17. [CDC NCHS public-use documentation and agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html).
