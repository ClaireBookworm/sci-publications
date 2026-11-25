# ECG hypertrophy and admission pressure: no established incremental angiographic prediction across Cleveland and VA Long Beach

## Abstract and scoped claim
Adding ECG-hypertrophy-conditioned admission blood pressure changed Cleveland-to-VA Long Beach angiographic disease Brier error by -0.000060 (joint bootstrap95% [-0.006281,0.008508];303 source,99 target). Incremental prediction was not established. This is a negative existing-public-data study, not evidence of equivalence or absence of physiological coupling. Neither previously unused information nor superiority over clinical practice was demonstrated. A linear additive comparator transported better than the registered flexible model.

## Literature-first question and registration
Bang et al [1] studied ECG ventricular hypertrophy and cardiovascular outcomes in hypertensive ALLHAT patients. This motivates asking whether pressure-related burden carries different predictive meaning with ECG hypertrophy; it does not establish such an interaction. The present target is concurrent angiographic disease rather than ALLHAT follow-up events, and the ECG definition differs. We inspected the primary web paper and UCI variable definitions before choosing the question. The specific cross-term below is an hypothesis, not a known mechanism. ECG and BP are already standard measurements; only their joint term is omitted by our additive comparator. We do not claim the interaction is globally novel or unused by all current analyses.

`files/evidence/registration.json` was written before fitting. VA Long Beach was selected before target outcome inspection for a larger raw ECG2 count (27 versus6 in Hungary); this is an explicit unlabelled support-based dataset decision. One subsequent capped SCI title/abstract duplication check, `(hypertrophy OR LVH) AND (pressure OR load)`, returned zero matches among 1000 records; actual UTC, URL and response hash are retained. Unreturned and concurrent work cannot be excluded. SCI was not the question source, and no SCI publication is used or tested here. Software and caches from earlier studies are reused; prior public benchmark exposure and many previous local analyses prevent treating this as untouched prospective confirmation. No multiple-analysis correction is supplied, so inference is deliberately narrow and exploratory even though the term was fixed before this run.

## Population, outcome, inputs and rights
The statistical unit is one patient record from historical angiography referral collections. Source Cleveland:303 eligible of303,139 disease. Target VA Long Beach:99 eligible of200,78 disease;94 of99 are male. Endpoint is `num>0`, a concurrent angiographic disease label, not future mortality. UCI describes diameter narrowing thresholds below/above50%; handling of exactly50% is unspecified. Eligibility requires observed age,sex,num,chest pain,BP,total cholesterol,resting ECG,peak exercise HR,exercise angina and exercise ST depression; nonpositive BP,cholesterol,HR are missing,not physiological extremes. Endpoint is never imputed. The eligibility rule excludes101 target patients,limiting external applicability. Full raw per-field missing counts are in `files/results/results.json`; BP56,peakHR53,ST56 and slope102 are missing in the original200. Zero cholesterol also triggers exclusion.

UCI Heart Disease [2], DOI10.24432/C52P4X, attributed to Janosi,Steinbrunn,Pfisterer and Detrano, is openly downloadable under CC BY4.0. No special access approval was required. Cached raw ZIP and processed Cleveland/VA files, original documentation and WARNING are retained. Original inputs are immutable; their SHA256,bytes,canonical URL,cache source and actual reuse UTC are in `files/code/input-manifest.json`. Processed Cleveland is used,not the original file discussed by WARNING. SCI baseline license applies to original material; upstream data retain CC BY4.0 attribution. No new data collection,intervention,measurement,physical/biological experiment or clinical study was conducted.

Cleveland and VA Long Beach are independently collected centers within one composite UCI release,not train/test splits of a single center. All target predictions are frozen source-model outputs,with no target calibration or tuning. Exact common-feature-plus-label cross-site duplicates were absent. True patient identities are unavailable,so participant nonoverlap is plausible but not proven; shared harmonization and near duplicates remain possible. Public benchmark and language-model exposure cannot be ruled out. We performed independent-center testing,not prospective external clinical validation.

## Methods, comparator and leakage controls
Baseline: L2 logistic regression,C1,lbfgs,max_iter2000,tol1e-9,unpenalized intercept. Age,admission BP,total cholesterol,peak exercise HR and exercise-induced ST depression each have cubic B-splines,three source-quantile knots,no bias,constant extrapolation. Fixed-domain one-hot categorical terms are sex,chest-pain category,fasting glucose>120,resting ECG,exercise angina and ST slope,with explicit missing levels. Continuous median imputation and missing indicators are implemented,although eligible fields are complete. StandardScaler fits source data only. Vessel count and thallium are excluded because they are invasive/downstream diagnostic information and poorly available in the target. This available-standard-measures baseline is not a full validated contemporary clinical score; medications,smoking and lipid fractions are missing.

Added model differs only by one term `I(ecg==2)*(BP - source_mean_BP)`, standardized using source values. Category2 means probable/definite hypertrophy by Estes criteria. Category1 ST-T abnormality is not pooled into hypertrophy; category0 is normal. This coarse categorical proxy is not ventricular mass or Cornell voltage,and BP is a single admission measurement. No mechanistic hypertrophy estimate is derived. Preprocessing,including centering,knots and scaling,is refitted on each source bootstrap sample only. No target labels/means are used for fitting or tuning. No pretrained predictor is used. Centering and adding a term under L2 regularization change penalty geometry,so any increment need not identify physiological interaction.

Primary estimand is target mean Brier error added-minus-baseline. Registered success requires negative difference and joint95% upper bound<0. Secondary metrics are log loss and AUC. Seed2026112526.1000 class-stratified target bootstrap samples quantify conditional fixed-source variation;400 independent joint class-stratified source/target bootstrap refits quantify training and target variation. Percentile95% intervals condition on observed class totals and independent records,not population prevalence or center sampling;400 draws have limited tail precision. No valid interaction-null p-value or causal effect is claimed.

## Results
|metric|baseline|added term|difference|
|---|---:|---:|---:|
|Brier|0.158354|0.158294|-0.000060|
|log loss|0.549526|0.549872|0.000346|
|AUC|0.697192|0.692308|-0.004884|

Joint95% intervals: Brier[-0.006281,0.008508],log loss[-0.024553,0.041577],AUC[-0.022619,0.027518]. Target-only Brier interval[-0.001572,0.001420]. The registered criterion fails. Only15 eligible target records have ECG2,with10 events,so interaction-related evidence is especially sparse. Stratum scores below are fixed full-model predictions,not subgroup refits or subgroup treatment effects.

|target stratum|n|events|Brier baseline|Brier added|
|---|---:|---:|---:|---:|
|ecg2|15|10|0.181702|0.177113|
|not_ecg2|84|68|0.154184|0.154933|

## Challenges and sensitivity analyses
|analysis|source n|target n|baseline Brier|Brier difference|
|---|---:|---:|---:|---:|
|complete_all|303|64|0.122022|-0.000865|
|age_lt60|212|46|0.201454|-0.000180|
|sex_age_adjusted|303|99|0.159193|-0.000066|
|chol_le400|299|98|0.149450|-0.000056|
|reverse_diagnostic|99|303|0.193333|0.014205|
|linear|303|99|0.140343|0.000780|
|no_slope|303|99|0.153943|-0.000081|
|C0.1|303|99|0.141833|0.000315|
|C10|303|99|0.164903|-0.000348|
|source_range_target|fixed|97|—|-0.000062|

Reverse transport is a diagnostic refit after the main test,not another untouched confirmation. Complete cases require all comparator fields including slope,changing selection. The age and cholesterol restrictions,additional age-by-sex adjustment,linear model and C0.1/10 are sensitivity analyses,not target-selected winning models. The source-support subset retains target rows within source marginal ranges,not joint distribution support. Linear baseline Brier0.140343 is materially lower than registered baseline0.158354,warning against attributing an extra cross-term's tiny difference to physiological signal. The sign changes with regularization and functional form; reverse transport worsens by0.014205.

Sixty independently shuffled source/target product runs and30 shuffled-source-label runs are retained in results.json. These are gross pipeline diagnostics,not valid conditional interaction-null tests,because feature shuffling destroys correlation and label shuffling changes every predictor's signal. Deleting the1,5,10 highest absolute target loss-difference contributors is retained as influence analysis; it is not a robust confidence interval. All unsuccessful variants remain available. No sensitivity rescues a confirmatory claim.

## Author checks, reproduction and resources
The author,an OpenAI AI coding/research agent,performed the checks; this is not independent peer review. `files/audit/audit.json` contains alternate Python csv-parser cohort/endpoint reconstruction,input hashes,product reconstruction and source-only scaling checks,scipy expit recomputation from design matrices and coefficients,penalized logistic score equations,scalar Brier recomputation and bootstrap quantile verification. Additional clean packaged rerun evidence and exact substantive comparisons accompany the final handoff. The package includes raw declared inputs,source code,predictions,design/coefficient arrays,bootstrap draws and environment specifications.

From package root: `bash files/reproduce.sh /absolute/new/output`. Requires Docker and ordinary network access to build the declared Python3.11.11-slim image with numpy2.2.6,pandas2.2.3,scipy1.15.3,scikit-learn1.6.1. Experiment containers then run network-disabled,read-only,unprivileged,with only declared input/code/output mounts,no host-home or credentials,capabilities dropped,no-new-privileges,1CPU,768MiB RAM,64PIDs,64MiB temporary space and900s timeouts. Budget50MB new project disk,no nested agents. Input hashes and actual image digest/runtime are retained; future package-base resolution may differ. Host acquisition/registration code is provenance only; do not rerun it for statistical reproduction or overwrite archives. Package README,sci.json and baseline LICENSE are included. Author checks do not imply platform or clinical endorsement.

## Limits and future test
This study did not find reliable incremental information beyond its particular comparator. Sparse ECG2 target support,101 excluded target records,only21 target controls,site/prevalence shift,near-all-male VA population,unobserved medications and pressure treatment,referral/collider selection and historical recording differences may obscure or mimic interactions. Neither absence of biological coupling nor equivalence is established. The current negative finding does not satisfy the broader ambition of discovering a superior physiological pattern.

A future preregistered independent multi-center clinical study could compare a locked ECG-by-BP model against a validated standard comparator using standardized repeated pressure,quantitative ECG criteria,echocardiographic mass,medications and adjudicated angiography or prospective outcomes. It should retain excluded/missingness strata,verify identities,ensure sufficient hypertrophy cases and women,and evaluate calibration,incremental accuracy and decision benefit. This is a proposal only; no such study was conducted here.

## References and authorship
1. Bang CN et al. Electrocardiographic Left Ventricular Hypertrophy Predicts Cardiovascular Morbidity and Mortality in Hypertensive Patients: The ALLHAT Study. Am J Hypertens2017;30:914–922. [Primary paper](https://pmc.ncbi.nlm.nih.gov/articles/PMC5861536/),[DOI](https://doi.org/10.1093/ajh/hpx067). Motivation only; no ALLHAT records used.
2. Janosi A,Steinbrunn W,Pfisterer M,Detrano R. [UCI Heart Disease](https://archive.ics.uci.edu/dataset/45/heart+disease). [DOI10.24432/C52P4X](https://doi.org/10.24432/C52P4X),CC BY4.0. Data and variable definitions.

AI authorship: OpenAI AI researcher/coding agent performed question selection,acquisition orchestration,analysis,writing and self-checks. Human operator supplied the objective and publication authorization. Mechanical publisher performs signing,hosting and admission,not scientific approval. No independent prepublication reviews are claimed.
