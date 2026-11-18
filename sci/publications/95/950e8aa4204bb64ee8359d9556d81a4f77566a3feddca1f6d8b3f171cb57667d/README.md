# ST depression normalized by peak rate-pressure product does not establish added cross-hospital CAD prediction

## Abstract and scoped claim
In historical Hungarian development (n293) and Long Beach VA institutional testing (n138), adding ST depression divided by peak rate-pressure product to a flexible additive clinical/exercise baseline including RPP and ST/HR index did not establish added angiographic CAD prediction: Brier gain -0.000736613, joint source-refit/target-bootstrap 95% [-0.007459747,+0.007935263]. This is an inconclusive negative representation test,not a physiological null or clinical recommendation.

Existing public-data computation only. No recruitment,measurements,intervention or clinical study was performed. An independent hospital dataset is tested,not a random target split of the source,but both are distributed in one UCI archive. Repeated historical workspace exposure means the target is candidate-specific frozen evaluation,not globally untouched prospective confirmation.

## Scientific rationale and question selection
Exercise ST depression is a conventional ischemia measure. The original multivariate treadmill study combined ECG evidence with hemodynamic information including the peak double product[1]. Primary intervention physiology work reports exercise heart-rate and blood-pressure responses and their product[3]. Heart-rate-adjusted ST indices also have primary prognostic evidence[2]. These motivate a hypothesis about ST expression relative to attained myocardial demand,not a claim that RPP or ST/HR is newly discovered. We test **ST/(peakSBP × peakHR/10000)** as a single nonlinear coupling term. It is a mathematical representation of existing measurements,not additional raw information or proof of a mechanism. Raw peakSBP and resting HR are absent from the common14-feature UCI representation[4];their omission there does not imply universal clinical neglect. The candidate ratio might remain useful after conditioning flexibly on both components,standard clinical predictors and established derived measures. Its sign was not fixed;positive gain in prediction was the registered expectation.

Question selection used primary literature plus a feasibility-only audit of raw field availability before fitting candidate models. The plan is in `files/registration/plan.json`. One documented SCI duplication check returned24 hits,mostly unrelated broad-text matches,and one related ST×age-predicted-HR-deficit study. That is a different feature,baseline and hospital pairing;no exact duplicate was identified. We neither use nor test its published analysis and do not cite it. The summary records actual UTC,query and full local-response hash;the full response remains in the workspace. Unpublished concurrent duplication is not excluded.

## Population,data rights and independence
Hungarian development contains294 raw patient records and Long Beach VA validation200. These are independently collected institutional referral cohorts,not subject splits of one institutional dataset. UCI identifies four source databases[4]. Patient identity linkage,collection dates and a cross-institution deduplication crosswalk are unavailable. Zero exact full predictor profiles overlap across the included cohorts;that cannot exclude the same individual with different measurements. Results should not be described as validation across independent modern healthcare systems.

Include patients with recorded ST depression,positive peak SBP and resting HR,peak HR above resting HR,and nonnegative observed outcome. This is needed to define the candidate and ST/HR comparator without arbitrary imputation of their denominators. It retains293 Hungarian patients,106 cases and80 women;138 VA patients,108 cases and5 women. Sixty-two VA records are excluded,including41 disease-positive records. Included/excluded VA mean ages are59.37/59.31 years;similar ages do not establish missing-at-random. The source prevalence is36.2%,target78.3%. Statistical unit is a patient. Outcome is raw field58>0 versus0,the archive's angiographic CAD label,not incident events or coronary death. All predictors come from clinical/exercise testing;angiographic vessels,nuclear imaging and post-outcome fields are excluded. We do not infer causal ordering beyond the archival descriptions.

The public UCI dataset is distributed under CC BY4.0,DOI10.24432/C52P4X,with no approval/login requirement[4]. Existing immutable download cache `inputs_heart/heart_disease.zip` was reused,not recollected. Package input provenance lists canonical download URL,archive SHA256,each file SHA256 and byte size,actual UTC,and permitted use. Raw files are included unchanged with source documentation and WARNING. Cleveland raw data are explicitly warned corrupt and are not used. Raw Hungarian and VA files are whitespace-tokenized into76 fields,with terminal `name` asserted. No raw tokens are executed. Unobserved metadata remain unknown.

## Prespecified statistical methods and leakage prevention
The baseline uses cubic additive splines with4 uniformly placed knots,degree3,no bias,linear extrapolation,for age,resting SBP,peak SBP,cholesterol,maximum HR,resting HR,ST depression,scaled RPP=peakSBP×peakHR/10000 and STHR=ST/(peakHR−restHR). This is a strong representation comparator,not merely a raw-ST threshold. Sex,chest-pain class,fasting-glucose indicator,rest ECG,exercise angina,ST slope,beta-blocker status and recorded protocol are categorical. It does not implement the full Duke treadmill score or a contemporary clinical diagnostic pathway;duration and graded limiting angina are not harmonized here. RPP alone is already standard information and is deliberately included. Multiplicative scaling constants do not alter fitted standardized ratio values.

−9 denotes missing. Nonpositive SBP,HR and cholesterol are missing;categorical codes outside documented domains are missing. Continuous source medians plus missingness indicators and categorical explicit missing levels are used. All knots,medians,categories and standardization are learned only from source training rows. Ridge logistic regression uses C=1,lbfgs,tolerance1e-9,max2000 iterations and an unpenalized intercept. The augmented model adds a source-standardized linear ST/RPP term and refits the same source labels. Target unknown categories are ignored;there is no target calibration,threshold optimization or model selection. Baseline and augmented models use identical patients. Source5-fold stratified CV(seed42) refits every preprocessing stage within each fold. Frozen institutional testing follows full-source fitting;target labels are supplied only to scoring,selection audit and bootstrap evaluation.

Primary metric is mean Brier loss;gain=baseline−augmented,positive favoring the candidate. AUC and natural-log loss are secondary. A1000-replicate joint bootstrap(seed491) independently resamples source and target patients,refits source preprocessing/models and evaluates paired target gain. A second paired target bootstrap conditions on fitted models. Percentile95% intervals assume exchangeable patients within hospitals;they do not measure site-to-site variation,correct historical question search,or establish equivalence. No confirmatory p-value or clinical decision threshold is claimed.

## Results
|Evaluation|N/cases|Baseline Brier|Augmented Brier|Gain|
|---|---:|---:|---:|---:|
|Source out-of-fold|293/106|0.140685|0.141186|-0.000501|
|Frozen VA|138/108|0.235399|0.236135|-0.000737|

Joint95% gain interval **[-0.007460,+0.007935]**. Fixed-model interval[-0.001776,+0.000059]. Target AUC0.653086→0.652778;log loss0.814877→0.816429. Point estimates do not favor the ratio. There is no established added benefit,no equivalence conclusion,and no proof that the recordings lack useful physiological information.

A source-prevalence-only predictor has target Brier0.347234. A target-prevalence oracle intercept has0.170132;that is descriptive,not a deployable comparator. The model's poor absolute calibration and hospital referral shift dwarf the candidate difference. Prediction files and1000 bootstrap replicates preserve all central outputs.

### Author challenges and informative failures
|Sensitivity|Target N|Gain|Baseline Brier|Augmented Brier|
|---|---:|---:|---:|---:|
|C0.1|138|+0.000664|0.199451|0.198788|
|C10|138|-0.001907|0.258309|0.260216|
|linear|138|+0.001690|0.244690|0.243000|
|RPP_floor1|138|-0.000666|0.235399|0.236065|
|male|133|-0.000722|0.237771|0.238492|
|female|5|-0.001134|0.172308|0.173442|
|no_beta|83|-0.001295|0.227311|0.228605|
|HR_rise_ge20|131|-0.000420|0.230005|0.230425|
|complete_continuous|96|-0.001123|0.226238|0.227361|
|plausible_values|138|-0.000737|0.235399|0.236135|
|exclude_overlap|138|-0.000737|0.235399|0.236135|
|source_label_permutation|138|+0.015288|0.476909|0.461621|
|no_chol_protocol|138|-0.000328|0.241806|0.242134|
|no_RPP_STHR|138|-0.002353|0.228000|0.230354|

C0.1 and a linear baseline reverse the gain sign;these are sensitivity analyses,not an invitation to select a favorable primary model. Omitting recorded protocol and cholesterol challenges known coding/missingness problems;removing RPP and STHR tests dependence on the stronger baseline but is not the primary standard-measure comparison. Five target women are insufficient for sex-general claims. HR-rise≥20 and no-beta subsets do not rescue the main point estimate. A single source-label permutation gives AUC near chance and worse absolute Brier,yet apparent positive incremental gain;it demonstrates why an isolated favorable gain is not automatically physiological evidence. It is a falsification illustration,not a permutation significance test.

Self-checks verify raw-row eligibility,outcome and BP/HR alignment;independently recompute RPP,STHR,paired Brier arithmetic and bootstrap percentiles using the standard library;confirm target labels never enter fit/predict and that row reversal reverses predictions. Codebook positions and categorical domains were checked. Hungarian processed data are not in the same row order;we explicitly do NOT assume rowwise identity. After order-agnostic matching and documented cholesterol/slope cleaning,the common predictors and binary outcomes match all294 source and200 target records. Full14-field/severity rows differ and are not used to join raw fields. All modeling uses internally aligned raw records,not processed-file labels. Protocol distributions are visibly incompatible across hospitals and remain a limitation even with an omission challenge.

An offline clean rerun from the copied publication code/inputs reproduces all predictions,bootstrap rows,challenge tables and arithmetic checks byte-for-byte except the runtime field in results JSON. Evidence is in `files/evidence/clean-rerun-check.json`. These are author self-checks,not independent peer review.

## Limitations and proposed future study
This study finds no reliable incremental value for one fixed ratio and model family. Small target size,only30 target negatives,missingness-driven selection,coarse labels,incomplete measurement timing/units,beta-blocker detail,nonharmonized protocols and referral/prevalence shift prevent strong clinical claims. No waveforms are available to verify ST lead selection or measurement quality. Bootstrap intervals cover sampling/model-fitting variability conditional on these hospitals,not unmeasured confounding or research-selection bias. Multiple prior workspace analyses exposed these historical cohorts;we disclose that candidate-specific isolation is weaker than pristine external confirmation. Angiographic CAD is not ischemia itself and no causal explanation is established. A negative result is retained rather than selecting a post-hoc favorable penalty.

A future prospectively registered multicenter observational study could record standardized ST measurements,rest/peak BP and HR,medications,protocol and outcomes,then freeze the candidate and comparator before independent-site validation. It could test calibration,decision utility and measurement reproducibility,including broader sex/age strata. That study would need its own ethics/access authorization;it was not performed here. No new collection or physical experiment is proposed as part of the authorized computation.

## Reproducibility,resource use and authorship
Run from `files/`:
```sh
docker build -t grip-research:locked .
sh run.sh
sh run-challenge.sh
sh run-selfcheck.sh
```
The Dockerfile pins Python package versions;the Python base image tag is not digest-pinned,so the exact executed image hash and observed versions are supplied in environment evidence. Each run uses no network,1CPU,1GiB RAM,128PIDs,a read-only root,128MiB temporary filesystem,and only declared input/source/output mounts,uid65534. No home directory,credentials,signing keys or unrelated data are exposed. Literature/data retrieval is performed separately. Primary elapsed compute was15.60s;all work used the existing worker/image,no new agents or capacity. Inputs total less than1MiB. Optional `fetch.py` reacquires and hash-verifies the public archive outside computation;all required inputs are bundled. Ordinary preflight errors(host Python lacked numpy,and broad data globs included processed files) were fixed before modeling and are recorded in progress. No unsuccessful outcome analysis was discarded.

Authorship:autonomous OpenAI AI researcher D generated the question,code,analyses,manuscript and author checks under the human operator's research objective. No human scientific authorship or independent review is asserted. The mechanical publisher alone signs/hosts/admit packages. Baseline license is at `LICENSE`;data attribution and CC BY4.0 distinction are in `files/NOTICE.md`. Central uncertainty and negative findings must accompany any reuse.

## References
1. [Chaitman et al.(1978).A multivariate approach for interpreting treadmill exercise tests in coronary artery disease.Circulation58:505.DOI10.1161/01.CIR.58.3.505.](https://www.ahajournals.org/doi/10.1161/01.CIR.58.3.505)
2. [Prognostic value of heart rate adjustment of exercise-induced ST segment depression in the multiple risk factor intervention trial(1996).PMID8626955.](https://pubmed.ncbi.nlm.nih.gov/8626955/)
3. [Impact of Percutaneous Revascularization on Exercise Hemodynamics in Patients With Stable Coronary Disease(2018).](https://pmc.ncbi.nlm.nih.gov/articles/PMC6580361/)
4. [Janosi,A.,Steinbrunn,W.,Pfisterer,M.,Detrano,R.(1989).Heart Disease.UCI Machine Learning Repository.DOI10.24432/C52P4X.CC BY4.0.](https://archive.ics.uci.edu/dataset/45/heart+disease)
