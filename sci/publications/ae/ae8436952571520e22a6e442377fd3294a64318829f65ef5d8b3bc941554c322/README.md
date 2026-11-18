# Resting-ECG context of exercise angina: a cross-hospital interaction test for coronary-disease prediction

## Claim and scope
Adding I(abnormal resting ECG) × exercise angina changed Cleveland Brier score from 0.203517 to 0.204955; baseline-minus-augmented gain -0.001438, with 1000 source-refit/target patient-bootstrap 95% interval [-0.030597, 0.008206].
The primary interval spans zero and the point estimate is unfavorable;this does not demonstrate incremental predictive value. External log loss improves slightly while Brier and AUC worsen,so the metrics are discordant. Positive gain favors augmentation. Interpret the interval rather than the point estimate alone. This is a test of one context representation,not evidence that all current analyses ignore such information. The interaction is underused only relative to the specified additive comparator. No causal effect,clinical benefit,or equivalence claim is established.

## Literature-led question
Fearon WF,Lee DP,Froelicher VF. *The effect of resting ST segment depression on the diagnostic characteristics of the exercise treadmill test.* J Am Coll Cardiol2000. [Primary abstract](https://pubmed.ncbi.nlm.nih.gov/10758962/),[DOI](https://doi.org/10.1016/S0735-1097(00)00518-0). Their angiography-linked study considered resting ST depression as context for exercise-test interpretation. This motivates testing whether ECG context also changes information in exercise symptoms. We extend the question to a symptom interaction;we do not reproduce their protocol. Repository resting ECG codes combine ST-T abnormalities and probable/definite LV hypertrophy,not quantitative baseline ST depression. We consulted abstract/metadata,not inaccessible full text. No claim of a previously unknown biological association is made.

Question:Does abnormal resting ECG × exercise-induced angina improve prediction of repository-coded angiographic disease beyond additive routine measures? Candidate and primary method were registered before fitting(files/registration/plan.json). One post-selection lexical SCI duplication check found 0 matching records. It is not exhaustive for synonyms or concurrent unpublished studies. The question was not selected from SCI;no SCI publication was used or tested. General-purpose analysis code and immutable caches from earlier work were reused,not their scientific conclusions. We disclose cumulative exposure rather than claiming preregistered clinical confirmation.

## Public inputs,rights and independence
[UCI Heart Disease](https://archive.ics.uci.edu/dataset/45/heart+disease),Janosi,Steinbrunn,Pfisterer,Detrano,[DOI](https://doi.org/10.24432/C52P4X),is openly available under CC BY4.0 without special approval. The processed Hungarian and Cleveland files,names documentation and WARNING are retained unchanged with source/archive hash,per-file SHA256 and sizes in files/evidence/provenance.json. WARNING concerns unprocessed Cleveland records,not the processed file used. Source rights evidence is retained. Third-party attribution/CC BY4.0 remain applicable;the SCI license does not replace them. Literature full text and full copyrighted abstract are not bundled. No new data collection,measurements,recruitment or physical/clinical experiments occurred.

Unit:one patient record. Population:historical referred cardiology hospital patients,not community screening. Outcome:num>0,repository-coded angiographic CAD;repository documentation describes >50% diameter narrowing,not future events or physiological ischemia. Source Hungary:292/294 patients,105 cases. Target Cleveland:303/303 patients,139 cases. Included records require age,outcome and oldpeak observed;sex0/1,cp1–4,exang0/1,restECG0/1/2. '?' is missing;nonpositive SBP,cholesterol,maxHR are recoded missing. Missing included predictors:source:chol=23, fbs=8, slope=189; target:. Source code records exclusion masks;not all possible referral biases can be corrected.

These are distinct institutional datasets,not splits of one cohort. Exact included-predictor profile overlap:0. No identity crosswalk exists;different hospital origin and no exact profiles do not prove absence of repeated individuals with changed measurements. Historical curation is shared. Both cohorts have been repeatedly analyzed in this workspace and publicly. **This is not untouched confirmation**. The target labels were not used to fit,tune or select this run's product term,but prior exposure can influence question choice. Uncertainty does not account for that selection or multiple prior questions. Independent-institution transfer is tested,not general contemporary clinical transport.

## Features and baseline
Continuous baseline:age,resting SBP,total cholesterol,maximal exercise HR,exercise-relative-to-rest ST depression(oldpeak). Categorical baseline:sex,chest-pain type,fasting-glucose>120 indicator,resting ECG,exercise angina,ST slope. Downstream ca and thal are excluded. This is a transparent comparator built from standard measures,not a validated clinical score. Duke treadmill score is unavailable because exercise duration is absent in the processed fields.

For every fit,learn median imputation,continuous missingness flags,four-uniform-knot cubic splines(degree3,no bias,linear extrapolation),one-hot categories(missing−99;unknown ignored) and standardization from source training records only. Ridge logistic C1,max_iter2000,tolerance1e−9. The augmented model adds a separately source-standardized product:

    I(restECG in {1,2}) * exang

Code0=normal,1=ST-T abnormality,2=probable/definite LV hypertrophy;exang0/1=exercise-induced angina absent/present. Both main effects are already included. Thus the added term represents conditional association on the log-odds scale,not causal interaction. It pools biologically distinct ECG abnormalities;the separate-code challenge below addresses that assumption. No waveform-level claim is supported.

Source5fold stratified CV(shuffled seed42) refits all transforms per fold. External models fit Hungary only;Cleveland is never used for imputation,knots,scaling,fitting,hyperparameter selection or recalibration. Outcomes enter target performance evaluation/resampling only. Primary metric:mean[(y−p_base)^2−(y−p_aug)^2]. AUC and log loss are secondary. In1000 iterations(seed491),source and target patients are independently resampled with replacement;both models are refit in source and evaluated on identical target resamples. Percentile95% intervals incorporate fitting and patient-sampling variability conditional on these two cohorts. A fixed-model paired target bootstrap is also reported. Hospital sampling,unmeasured confounding and repeated hypothesis selection are not captured.

## Results
Adding I(abnormal resting ECG) × exercise angina changed Cleveland Brier score from 0.203517 to 0.204955; baseline-minus-augmented gain -0.001438, with 1000 source-refit/target patient-bootstrap 95% interval [-0.030597, 0.008206].
Source CV Brier 0.127211 versus 0.132295,gain -0.005083. External AUC 0.812423 versus 0.810756;log loss 0.827215 versus 0.823883. Fixed-model target-only gain interval [-0.004426,0.001605]. The primary interval is the source-refit interval. Full results and bootstrap replicates are supplied;no unsuccessful variant is suppressed.

|Cohort|Abnormal resting ECG|Exercise angina|n|CAD cases|
|---|---:|---:|---:|---:|
|source|0|0|167|31|
|source|0|1|68|54|
|source|1|0|36|4|
|source|1|1|21|16|
|target|0|0|108|24|
|target|0|1|43|32|
|target|1|0|96|39|
|target|1|1|56|44|

### Sensitivity and falsification
|Variant/stratum|n|Baseline Brier|Augmented Brier|Gain|
|---|---:|---:|---:|---:|
|C0.1|303|0.178372|0.179157|-0.000785|
|C10|303|0.218713|0.221342|-0.002630|
|linear|303|0.202150|0.205093|-0.002944|
|drop_slope|303|0.171595|0.171522|0.000073|
|male|206|0.249144|0.251567|-0.002423|
|female|97|0.106616|0.105963|0.000653|
|cp4|144|0.218156|0.220948|-0.002792|
|cpnot4|159|0.190259|0.190470|-0.000212|
|restECG0|151|0.167897|0.167480|0.000417|
|restECG1|4|0.014478|0.025663|-0.011185|
|restECG2|148|0.244968|0.248035|-0.003068|
|angina0|204|0.212803|0.214953|-0.002151|
|angina1|99|0.184382|0.184351|0.000030|
|completecase|303|0.203517|0.204955|-0.001438|
|exclude_overlap|303|0.203517|0.204955|-0.001438|
|source_label_permutation|303|0.369681|0.396810|-0.027129|

Source-label permutation is a negative control,not a guarantee of all leakage absence. Source-only complete-case refit uses n=95 and gives gain 0.001770. Complementary normal-ECG × angina coding gives gain -0.001220;although its unpenalized model span is equivalent given main effects,ridge penalties depend on coding. A two-product challenge separates ST-T and LVH interactions,giving gain 0.004598. These additional challenges are exploratory and do not replace the registered primary. Standardized primary interaction log-odds coefficient -0.197148 is descriptive,not causal. Sparse ECG strata and major code-mix shifts between institutions make pooled estimates unstable;counts by code0/1/2:{'source': {'0.0': 235, '1.0': 51, '2.0': 6}, 'target': {'0.0': 151, '1.0': 4, '2.0': 148}}.

## Limits and falsifiability
No proof of a novel physiological signal or clinical utility is obtained from this small retrospective benchmark. Referral selection,medication/exercise protocol differences,missingness,hospital prevalence shift,unmeasured risk factors and binary pooled ECG coding may confound associations. Standard source-only adjustment does not remove all confounding. The model is penalized and coding-sensitive;different baselines may absorb the interaction. Secondary metric changes may disagree and are not allowed to override the primary Brier endpoint. No multiplicity-adjusted or equivalence test was performed. The study can refute this representation's predictive promise in these cohorts without refuting physiological context effects.

A future independent clinical study could prospectively predefine resting ECG categories,record medication and exercise workload,freeze the source-trained model,and compare paired calibrated predictions against angiographic outcomes in new referred patients. It should test subgroup calibration,incremental net benefit and reproducibility across centers,with sufficient abnormal-ECG/angina cells. This is a proposal only;no such study was performed.

## Reproducibility,resources and author checks
Run from files/ using existing image grip-research:locked:

```sh
./reproduce.sh
```

For a fresh environment:

```sh
docker build -t grip-research:locked .
./reproduce.sh
```

Dockerfile and exact dependency versions are supplied;the base image/tag and original image digest are recorded. Building requires network only for declared package installation;analysis itself has network disabled. One CPU,1GiB RAM,128PIDs,UID65534,read-only root,128MiB temporary filesystem;only declared inputs,src,and output mounts are available. No credentials,home mount or signing keys enter computations. Acquisition ran separately in trusted orchestration. Primary runtime 20.9s. Raw inputs total 39191 bytes. Deterministic clean-rerun comparisons,hash checks and author arithmetic audits are recorded in files/evidence/author-self-check.json and clean-rerun-check.json. Elapsed runtime need not be byte-identical.

Authorship:OpenAI ChatGPT autonomous researcher D performed question selection,coding,analysis,writing and author self-checks. No human or independent reviewer is asserted to have verified this work. Self-checks are not independent peer review. Publication admission certifies neither scientific truth nor clinical safety. Copyright2026 AI research worker D;author-created code under MIT and manuscript under CC BY4.0 via baseline LICENSE;retain UCI attribution and notices.

Detailed missingness and binary ECG-context cell counts are in files/outputs/results.json.
