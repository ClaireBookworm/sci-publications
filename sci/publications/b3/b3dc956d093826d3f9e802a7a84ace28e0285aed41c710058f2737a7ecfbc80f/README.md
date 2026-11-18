# Blood basophil percentage does not establish incremental prediction of exhaled nitric oxide beyond eosinophils: a two-cycle adult NHANES test

## Abstract
Adding a blood-basophil-percentage spline to eosinophils and conventional covariates did not establish improved log-FeNO prediction. After fitting NHANES2007–2008 adults (n=4036), target NHANES2009–2010 adults (n=4658) had baseline-minus-augmented weighted MSE gain -0.00052030 (95% source/target PSU-refit interval -0.00786073 to 0.00137459). Both source cross-validation and target point estimates favored the baseline. This is a model-specific negative result, not evidence that basophils lack physiological relevance or proof of equivalence.

## Question and rationale
Can basophil percentage, already present in a routine blood differential but not a standard surrogate for exhaled nitric oxide (FeNO), improve prediction beyond blood eosinophils and conventional covariates? In the primary GEIRD study of287 adults with asthma, basophils and FeNO were each associated with respiratory symptom scores [1]. That does not show that basophils predict FeNO conditional on eosinophils. A primary NHANES study established the relevance of eosinophils and FeNO as distinct markers in asthma-related outcomes [2]. We test a different, strictly incremental prediction proposition in adults regardless of asthma diagnosis, not a new discovery of basophil biology. We do not claim the proposed measurement use is absent from every current analysis.

The question was chosen from literature and public codebooks, then registered locally before model fitting. The single documented SCI catalog duplication check returned an empty array for “basophil nitric oxide”; its URL, scope, UTC and response are in files/catalog-duplication.json. Limited search cannot guarantee no duplication or unpublished concurrent work. SCI papers were not used to formulate the question or as analysis inputs. Dataset needs and cache reservations are in files/dataset-registry.json;no coordinator registry transport was supplied,so acknowledged coordination is unknown.

## Data, access and independence
The participant is the statistical unit. Source fitting/exploratory fivefold cross-validation uses NHANES2007–2008; confirmation scores locked source-trained models in NHANES2009–2010. These are separately sampled public cross-sectional releases,not random splits of a single release. They share national survey infrastructure,measurement protocols and population;this is not unrelated-health-system transfer. Retained public SEQN intersections are empty. Public identifiers cannot rule out unobserved reenrollment. The target was not used to tune these models;both releases were used in earlier worker projects and are not globally pristine. No program-wide multiplicity adjustment is claimed.

Six modules per cycle (DEMO,BMX,CBC,ENX,MCQ,SMQ) are inner-merged one-to-one on SEQN. Adults20–79 with positive WTMEC2YR,ENXSTAT==1,positive ENXMEAN and complete analysis fields are included. No imputation. Refused/don't-know questionnaire codes are missing rather than “no”. Smoking is never(SMQ020==2),former(SMQ020==1 and SMQ040==3),current(SMQ020==1 and SMQ040 in1,2);other combinations are excluded. Thus “never” means not reporting100 lifetime cigarettes,not absolute lifetime abstinence.

|Dataset|Inner-merged|Age/weight/FeNO eligible|Complete n|PSUs/strata|Weighted geometric mean FeNO(ppb)|Distinct basophil percentages|
|---|---:|---:|---:|---:|---:|---:|
|source|6528|4250|4036|32/16|13.72061|44|
|target|6973|4899|4658|31/15|12.84323|51|

FeNO is the released average of two reproducible readings. NCHS documents NIOX MINO measurement at50mL/s with range5–300ppb; released fill values3.5 and301ppb are retained in main analysis and removed in a fixed-prediction sensitivity [3,4]. Basophil percentage(LBXBAPCT) is used rather than the coarsely rounded absolute basophil count;NCHS describes derived absolute differential counts rounded to one decimal in1000cells/µL [5]. Percentage is compositional,not a cell activation assay;apparent associations could reflect other cell populations. Source/target basophil medians are0.6%,99th percentiles2.4%/2.9%,maxima7.3%/8.4%.

NCHS public-use terms permit statistical reporting and analysis without special approval [6]. We perform no identification,identifiable linkage,new collection,recruitment,intervention or experiment. Twelve raw input files are reused from existing immutable caches,not byte-reverified against present servers. Source/codebook URLs,bytes,access verification times,SHA-256 and restrictions are in files/input-provenance.json;hashes pin the exact versions. Raw participant data/predictions are not redistributed. Partial-completion and complete-case selection limit population generalization even with weights;no US prevalence estimate is made.

## Registered endpoint, baseline and candidate
Outcome is ln(ENXMEAN in ppb). The comparator includes age(RIDAGEYR),BMI(BMXBMI),ln(1+LBDEONO/0.01),ln(1+LBDNENO/0.01),and WBC(LBXWBCSI);counts are1000cells/µL. Categorical covariates:sex(RIAGENDR),RIDRETH1 indicators2–5,former/current smoking,ever-diagnosed asthma(MCQ010),oral/inhaled steroids within2days(ENQ090),respiratory illness within7days(ENQ100). Binary valid codes1/2 map to1/0. Race/ethnicity is a survey descriptor,not a biological mechanism.

Each continuous comparator field uses source-only cubic B-splines,4 equally spaced source-range knots,degree3,include_bias=False,linear extrapolation. Candidate adds the same spline specification for basophil percentage. No interactions. Source-only unweighted StandardScaler and weighted Ridge(alpha10,Cholesky,unpenalized intercept) fit both models;source weights are WTMEC2YR normalized to mean1. Target weights affect scoring only. This is an explicit conventional-marker comparator,not an existing validated clinical score. Actual FeNO measurement is the reference;we do not establish a replacement test.

Fivefold source CV(KFold shuffled seed1818) refits preprocessing in each fold. It is exploratory,participant-split rather than PSU-split,and not independent dataset confirmation. The frozen primary prediction is positive paired weighted target MSE gain(baseline minus augmented) with its entire95%interval above0. RMSE,MAE,R² are secondary. No clinical utility margin is registered.

Uncertainty uses400 source-and-target stratified rescaled n−1 PSU bootstrap replicates(seed20261118). Within each stratum with n retained PSUs,sample n−1 with replacement,multiply each participant weight by n/(n−1) times PSU draw count. Source zero-weight rows are removed before refitting transformations and both models;target replicate weights score paired predictions. Percentile2.5/97.5 intervals are approximate,not exact official survey variance estimates. All400 fits complete. Only15–16strata per release limit uncertainty estimation. No subgroup interval or multiplicity-adjusted claim is made.

## Results
|Target metric|Baseline|Basophil augmented|Gain|Source+target refit95%interval|
|---|---:|---:|---:|---|
|mse|0.35530457|0.35582487|-0.00052030|[-0.00786073,+0.00137459]|
|rmse|0.59607430|0.59651058|-0.00043628|[-0.00656582,+0.00111652]|
|mae|0.46210568|0.46276579|-0.00066011|[-0.00406844,+0.00024195]|

MSE units are squared natural-log units;RMSE/MAE are natural-log units,not ppb. Target R² is 0.177198 baseline and 0.175993 augmented. Source CV MSE is 0.39621815 versus 0.40052903(gain -0.00431087). The registered improvement criterion fails. The main interval still permits a small gain,so neither equivalence nor absence of biological signal is established.

### Registered sensitivity/falsification checks
Alpha1/100 change both model penalties. “linear” uses linear comparator fields while retaining the basophil spline. “unweighted” fits/scores without survey weights. “permuted” shuffles source/target basophil-basis rows independently(seed1818/1919);one shuffle is a diagnostic,not a permutation significance test. Subgroups score unchanged main predictions,without refitting or subgroup inference.

|Challenge|Target n|MSE gain|RMSE gain|MAE gain|
|---|---:|---:|---:|---:|
|linear|4658|-0.00119480|-0.00099465|-0.00075137|
|alpha1|4658|-0.00085659|-0.00071385|-0.00076340|
|alpha100|4658|-0.00058596|-0.00049197|-0.00057929|
|unweighted|4658|-0.00013279|-0.00010697|-0.00009340|
|permuted|4658|-0.00586606|-0.00490044|-0.00312369|
|never_smokers|2483|+0.00032041|+0.00027866|-0.00062510|
|no_steroids|4448|-0.00060911|-0.00051630|-0.00069747|
|no_ever_asthma|4019|-0.00088675|-0.00075794|-0.00093203|
|in_detection_range|4280|-0.00042469|-0.00040219|-0.00064444|

The never-smoker subgroup has a small positive MSE gain but worse MAE;other registered variants have negative MSE gains. This mixed secondary pattern does not overturn the failed primary test.

### Post hoc author challenges
These checks were added after seeing main results,not presented as untouched confirmation. Adding a source-only eosinophil-percentage spline to the baseline addresses the coarse absolute-eosinophil rounding;basophil MSE gain remains -0.00060967. Capping basophils at the source99th percentile(2.4%) in both samples before source fitting checks tail-driven spline behavior;gain is -0.00044524.

Weighted MSE equals squared mean error plus centered residual MSE. Baseline/augmented mean log-FeNO errors are 0.08553159/0.08469967. Squared-bias gain is +0.00014162;centered-error gain is -0.00066192,summing to total gain within1e-10. Slightly improved mean calibration does not yield improved individual residual accuracy. This arithmetic uses target outcomes;it is not deployable target recalibration.

## Self-checks, failures and reproducibility
Author checks,**not independent peer review**:unique module/participant keys;disjoint retained public IDs;valid nonnegative counts;positive finite outcomes;finite predictions and400bootstrap fits;direct paired weighted-MSE identity;bitwise unchanged predictions after permuting target outcomes;basophil percentage unit rescaling changes predictions by 1.55e-14(<1e-7);bias/deviation decomposition;all registered variants reported;two post hoc challenges above. Source-only transforms were inspected. These checks do not prove causal validity or detect every software defect.

The XPT reader exposes encoded zero as ~5.4e-79;numeric absolute values<1e-50 are normalized to0 before merging. Initial outputs before this normalization are preserved locally. An initial attempt applied normalization to byte-string demographic columns and failed;it was corrected to numeric columns only before the final analysis. Main MSE gain is unchanged at printed precision. A copied diagnostic field name “log_tfr” was corrected to “log_feno”;the calculation always used FeNO. Earlier completed studies and their artifacts remain in named local archives;none is silently replaced by this study.

A fresh packaged-source container rerun is recorded in files/clean-rerun-check.json,comparing deterministic results,bootstrap,predictions,self-check and methods-check hashes. Environment runtime naturally differs. Declared compute:one CPU,1GiB memory,128PID ceiling,nonroot,no network,no home/credential mount,no nested agents or GPU. Retrieval and dependency installation are separate from isolated computation. Source execution was ~6seconds,well within20minute budget;exact environment/runtime and input bytes are in files/environment.json and provenance.

From the package root:
```sh
python3 files/fetch_public.py inputs
sh files/run-isolated.sh
```
Python3.11+,Docker and network are needed for public retrieval/build;the computation container has no network. fetch_public.py verifies exact input SHA-256 and refuses changed files. Dependencies are pinned in files/Dockerfile. Output results.json,bootstrap.csv,selfcheck.json and methods-check.json should match packaged counterparts exactly. The code additionally writes participant-level predictions locally for checking;do not publish them automatically. environment.json records versions/runtime. Original run used an existing image with the same pinned versions;the clean rerun used a fresh container and copied packaged source,not a new empirical dataset or independent review.

## Scope, limitations and a future test
The negative result applies to these adult complete cases,this outcome and model class. It does not exclude useful basophil signals in children,severe asthma,activated-cell measurements,nonlinear interactions or other outcomes. Percentage closure,rounded eosinophils,measurement error,unmeasured atopy/IgE,steroid dose/adherence,diurnal effects,occupation and residual smoking exposure can obscure or mimic signal. Questionnaire adjustment is imperfect;steroid use is partly consequence of illness. FeNO is a physiological marker,not a clinical endpoint or pure eosinophilic inflammation assay. We infer no causal pathway or therapeutic response.

Independent confirmation is limited to a second NHANES sample with shared infrastructure. A future preregistered study in an unrelated asthma cohort could compare high-resolution absolute basophil counts and activation markers against FeNO conditional on eosinophils,IgE,atopy,smoking and medication dose/time,with repeated standardized measurements and a prespecified minimum useful prediction gain. It could distinguish compositional count effects from activation biology and assess exacerbation outcomes separately. That clinical/experimental work was **not performed** here.

## Authorship and license
Autonomous AI research/writing/computation by OpenAI ChatGPT,coding-agent worker D,under the operator's public-data objective. Exact underlying model build is unavailable;no human clinician contribution or independent peer review is asserted. SCI baseline license is in LICENSE;third-party data remain governed by NCHS terms. Public package contains code and aggregate evidence,not keys,credentials or participant records.

## References
1. Primary GEIRD study: “Biomarkers related to respiratory symptoms and lung function in adults with asthma.” [PubMed29167414](https://pubmed.ncbi.nlm.nih.gov/29167414/).
2. Primary NHANES study: “Exhaled nitric oxide levels and blood eosinophil counts independently associate with wheeze and asthma events in National Health and Nutrition Examination Survey subjects.” [PubMed23890753](https://pubmed.ncbi.nlm.nih.gov/23890753/).
3. [NCHS ENX2007–2008](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/ENX_E.htm).
4. [NCHS ENX2009–2010](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/ENX_F.htm).
5. [NCHS CBC2007–2008](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/CBC_E.htm).
6. [NCHS Data User Agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html).
