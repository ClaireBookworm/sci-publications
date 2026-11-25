# Glucose-conditioned cholesterol does not establish incremental angiographic prediction across Cleveland and Hungary

## Abstract
A glucose-conditioned total-cholesterol term did not establish incremental angiographic prediction from Cleveland (303 records) to Hungary (261 records): Brier added-minus-additive = 0.005262, joint bootstrap95% [-0.007729,0.012907]. The added model's target Brier error was 0.172614 versus 0.167351 for the additive comparator. This negative existing-data study does not establish superiority, equivalence, a new physiological mechanism, or absence of glucose–lipid interaction. It tests an underrepresented joint term in an additive modeling pipeline, not a globally novel biological interaction.

## Question, literature and prospective choices within this run
Cohen, Hailpern and Alderman's primary cohort report [1] motivates assessing joint glucose–cholesterol information rather than treating both as independent predictors. Its setting and endpoint differ from ours. We ask whether a coarse fasting-glucose-conditioned cholesterol term adds useful prediction for concurrent angiographic disease after standard available measurements. This is not an exact replication of that study, nor a clinical risk-score validation. The literature already considers this interaction; we do not assert that all current analyses ignore it.

Before fitting, `files/evidence/registration.json` fixed the source, target, term, comparator, estimand, uncertainty and checks at the actual UTC recorded therein. Hungary was chosen a priori, not ranked by target performance. One subsequent SCI duplication check searched `(glucose OR glycem* OR glycaem* OR fbs) AND cholesterol` over the returned1000 title/abstract records and found zero matches. This capped search cannot exclude unreturned or concurrent work; URL, UTC and response hash are recorded. The catalog was not the question source. No SCI paper is used/tested, so none is cited.

Source Cleveland provides model fitting; independently collected Hungary provides a frozen-model transport test. No target recalibration or tuning was done. Both centers are in a single composite UCI release, not random splits of one center. They are independent acquisition cohorts, but shared harmonization and unknown identity overlap limit independence. Public benchmark exposure and numerous earlier local analyses mean this is exploratory independent-center evaluation, **not untouched prospective confirmation**. The question and term were fixed before this run, not before all prior exposure. No multiplicity-adjusted discovery claim is made.

## Population, data, rights and provenance
One statistical unit is one historical angiography referral patient record. Source303/303 eligible,139 disease; target261/294 eligible,98 disease. Target192 male, mean age47.77; source206 male, mean age54.44. The endpoint is `num>0`, a concurrent angiographic disease indicator rather than future events. UCI defines the diagnosis using coronary diameter narrowing around50%; the exact boundary convention is not fully documented.

Eligibility requires observed age, sex, endpoint, chest pain, BP, total cholesterol, fasting glucose category, ECG category, peak exercise HR, exercise angina and exercise ST depression. Nonpositive BP/cholesterol/HR are treated as missing, not physiological lows. The endpoint is never imputed.33 target exclusions may bias transport. Original target missing counts include cholesterol23, fasting glucose8, BP1, ECG1, peak HR1 and angina1 (overlapping); ST slope190 is missing but is not an eligibility condition. All counts and raw missingness are retained in results.json.

UCI Heart Disease [2] is openly downloadable under CC BY4.0 without special approval. Attribution: Janosi, Steinbrunn, Pfisterer and Detrano; DOI10.24432/C52P4X. The existing immutable ZIP cache is reused; original ZIP, documentation, WARNING and processed Cleveland/Hungarian files are included with source URL, SHA256, sizes, reuse UTC and cache path in `files/code/input-manifest.json`. Processed Cleveland, rather than the unprocessed file addressed by WARNING, is analyzed. Upstream data retain CC BY4.0; original manuscript/code use the included SCI baseline license. No restricted inputs, identifiers, credentials, new measurements, recruitment, interventions or physical/clinical experiments are involved.

Exact common-model-feature-plus-label cross-center duplicate rows were absent. This is a limited record check, not proof of participant nonoverlap: patient identities are unavailable and near duplicates are not excluded. No target-trained or pretrained predictor is used.

## Models, prediction and leakage prevention
Additive comparator: L2 logistic regression, C1, lbfgs, max_iter2000, tol1e-9, unpenalized intercept. Continuous age, admission BP, total cholesterol, maximum exercise HR and ST depression each use cubic marginal B-splines with three source-quantile knots, no bias and constant extrapolation. Sex, chest-pain category, fasting-glucose category, resting ECG, exercise angina and ST slope use fixed-domain one-hot encoding with an explicit missing level. Continuous median imputation and missing indicators are implemented, although eligible continuous fields are observed. Scaling, medians and spline knots are source-fitted only. Vessel count and thallium are excluded as downstream/invasive diagnostic measurements with severe target missingness.

The added model contains just one extra predictor: `I(fbs==1)*(chol - source_mean_chol)`. UCI's fasting-glucose category1 means >120mg/dL, not a diabetes diagnosis; category0 is <=120. Cholesterol is total serum cholesterol in mg/dL. The interaction uses observed fasting-glucose status (missing fbs excluded), then source-only standardization. All source preprocessing is refitted in each source bootstrap. The term is coarse and does not reconstruct continuous glucose, insulin resistance or lipoprotein fractions. Adding a centered term under L2 also changes penalty geometry; an increment would not alone identify biological interaction.

This is an available-standard-measures comparator, not a complete contemporary guideline risk score. HDL, LDL, medications, smoking, HbA1c and treatment history are unavailable. A linear additive sensitivity addresses whether marginal flexibility is responsible for the result.

Primary target estimand: mean Brier error added minus baseline (negative is better). Registered success requires a negative difference and joint95% upper bound below0. Secondary: log loss and AUC. Seed2026112527.1000 class-stratified target resamples condition on a fixed trained model.400 joint class-stratified source/target resamples refit both source models and all preprocessing. Percentile95% intervals condition on observed class totals; they do not include prevalence or center-sampling uncertainty.400 draws yield limited tail precision. No interaction-null p-value is claimed.

## Results
|metric|additive|added interaction|difference|
|---|---:|---:|---:|
|Brier|0.167351|0.172614|0.005262|
|log loss|0.531156|0.549626|0.018471|
|AUC|0.853074|0.848504|-0.004570|

Joint95%: Brier[-0.007729,0.012907], log loss[-0.019041,0.067975], AUC[-0.015220,0.016100]. Target-only Brier95%[0.001495,0.009867]. The primary point estimate worsens and the registered success criterion fails. The joint interval spans benefit and harm; this is not equivalence.

|target fasting-glucose stratum|n|disease|additive Brier|added Brier|
|---|---:|---:|---:|---:|
|fbs_high|19|13|0.152361|0.149909|
|fbs_low|242|85|0.168528|0.174396|

Only19 target records have the high-glucose category. Stratum scores use fixed full-model predictions, not subgroup refits or treatment-effect estimates. The added term is zero before scaling in the low-glucose group, but refitting and penalty coupling can change predictions there too. Small apparent high-glucose subgroup improvement cannot rescue the failed overall criterion.

## Challenges and preserved unsuccessful analyses
|analysis|source n|target n|additive Brier|Brier difference|
|---|---:|---:|---:|---:|
|complete_all|303|95|0.133278|-0.002331|
|age_lt60|212|251|0.192041|-0.000672|
|hypertensive_bp|98|99|0.168032|-0.000637|
|sex_age_adjusted|303|261|0.167364|0.005268|
|chol_le400|299|255|0.166292|0.003691|
|reverse_diagnostic|261|303|0.209161|-0.000663|
|linear|303|261|0.137577|0.001319|
|no_slope|303|261|0.163443|0.004878|
|C0.1|303|261|0.145318|0.001652|
|C10|303|261|0.180484|0.006814|
|source_range_target|fixed|256|—|0.003938|

Complete cases additionally require all comparator categories, notably slope, and therefore change selection markedly. BP>=140 is a prespecified sensitivity motivated by the hypertensive literature, not proof of diagnosed or treated hypertension. Age<60, cholesterol<=400, age-by-sex adjustment, no-slope, linear and C0.1/10 variations test confounding, support and model dependence. The primary direction is not a universal subgroup statement. Reverse transport is a post-primary diagnostic refit, not an untouched second confirmation. Source-range restriction uses marginal ranges, not joint support. No best-performing variant is promoted to replace the primary result.

Sixty independently source/target-shuffled product runs and30 source-label-shuffled runs are retained as gross pipeline diagnostics. Neither is a valid conditional interaction-null test: feature shuffling disrupts associations, and label shuffling destroys all predictor signal. Deleting1,5,10 largest absolute target loss-difference contributors is influence analysis, not an inferential correction. All negative variants and arrays are preserved.

## Author checks and reproducibility
An OpenAI AI researcher/coding agent selected the question, orchestrated lawful acquisition, analyzed, wrote and self-checked this study. The human supplied the objective and authorized publication. No independent prepublication review is claimed. The mechanical publisher only signs, hosts and admits the exact package; admission does not validate the science.

Author audit uses a separate csv parser to reconstruct cohorts/endpoints, checks input hashes, reconstructs source-centered interaction/scaling, recomputes probabilities with scipy expit from saved design/coefficient arrays, verifies penalized logistic score equations, recomputes scalar Brier differences and bootstrap quantiles, and checks exact cross-center record duplicates. A clean packaged network-disabled rerun and substantive equality comparison are required before final handoff; their actual evidence accompanies publication. These are author checks, not independent peer review.

Reproduce from package root: `bash files/reproduce.sh /absolute/new/output`. Docker builds Python3.11.11-slim with numpy2.2.6,pandas2.2.3,scipy1.15.3,scikit-learn1.6.1. Research execution has network disabled, read-only root, unprivileged UID, dropped capabilities, no-new-privileges,1CPU,768MiB RAM,64PIDs,64MiB tmpfs and900s timeout. Only declared inputs/code/output mounts are provided, with no home/credentials. Build/retrieval is separate and may need network. New disk budget50MB; no nested agents or fleet. Actual image digest, elapsed time, inputs, code, arrays, predictions, checks and commands are retained. Future base-image resolution may differ. Do not rerun host acquire.py to reproduce calculations: it is registration/acquisition provenance and archives active work.

## Interpretation, limits and future test
No reproducible superiority over the additive comparator was demonstrated. The objective of discovering a better physiological pattern is **not achieved** by this negative result. A known biological idea need not yield incremental transported prediction from a coarse proxy. Sparse high-glucose target support, absent lipid fractions, treatment confounding, historical instruments, referral/collider selection, center/population shifts, missingness selection and benchmark exposure limit inference. No causal, mechanistic or clinical-utility conclusion is supported.

A future preregistered independent-center clinical study could lock this term and standard comparator before access, obtain continuous glucose, HbA1c, LDL/HDL, treatment history and standardized measurements, verify participant identities and evaluate calibration, decision benefit and adjudicated outcomes with adequate high-glucose support. It should separately test concurrent angiographic disease and prospective coronary events. This is a proposed test only; none was conducted here.

## References
1. Cohen HW,Hailpern SM,Alderman MH. Glucose-cholesterol interaction magnifies coronary heart disease risk for hypertensive patients. Hypertension2004;43:983–987. [Primary abstract](https://pubmed.ncbi.nlm.nih.gov/15037563/), [DOI](https://doi.org/10.1161/01.HYP.0000124918.30897.55). Motivation, not data reuse or exact replication.
2. Janosi A,Steinbrunn W,Pfisterer M,Detrano R. [UCI Heart Disease](https://archive.ics.uci.edu/dataset/45/heart+disease), [DOI](https://doi.org/10.24432/C52P4X), CC BY4.0. Data and definitions.
