# Age context of exercise ST depression: external-hospital evaluation of one interaction beyond additive coronary-risk measures

## Claim and scope
Adding ((age−50)/10) × exercise ST depression changed Cleveland Brier score from 0.203517 to 0.209494; baseline-minus-augmented gain -0.005977, with 1000 source-refit/target patient-bootstrap 95% interval [-0.060553, 0.008048]. Incremental Brier value was not demonstrated. Positive gain favors augmentation. No equivalence,causal interaction,clinical utility or previously unknown physiology is established. “Underused information” here means a product absent from the specified additive comparator,not an assertion that the literature never uses age-dependent interpretation. The primary interval,not a favorable secondary metric or subgroup,determines the conclusion.

## Literature-led hypothesis and registration
Rywik TM et al. *Role of nondiagnostic exercise-induced ST-segment abnormalities in predicting future coronary events in asymptomatic volunteers.* Circulation2002;106:2787–92. [Primary abstract](https://pubmed.ncbi.nlm.nih.gov/12451004/),[DOI](https://doi.org/10.1161/01.CIR.0000039329.47437.3B). In their asymptomatic cohort,age and selected ST-response categories predicted subsequent coronary events after adjustment. That does not establish age-by-ST interaction. It motivates our hypothesis that age might change the predictive meaning of an exercise ST response. We test a new representation in referred angiography patients,not their outcome or population. Only abstract/metadata were consulted;no unavailable full text is claimed.

The chosen question was registered before this analysis:does ((age−50)/10)×oldpeak improve cross-hospital coronary-disease prediction beyond additive routine measures? The plan fixes the candidate,primary Brier metric,source/target and resource budget. A single post-selection lexical SCI duplication check returned zero matches(files/evidence/duplication.json);this cannot exclude synonyms or concurrent unpublished work. The SCI catalog did not supply the question,and no SCI work is used or cited. General-purpose code and immutable inputs from earlier work were reused. Many prior questions were tested on these same public cohorts in this workspace. Their repeated reuse creates selection risk even though this candidate was fixed before this run. Intervals do not adjust for that cumulative exploration.

## Data,rights,provenance and independent dataset
[UCI Heart Disease](https://archive.ics.uci.edu/dataset/45/heart+disease),Janosi,Steinbrunn,Pfisterer,Detrano,[DOI](https://doi.org/10.24432/C52P4X),CC BY4.0. Public processed Hungarian and Cleveland files are lawfully downloadable without special approval. Cache reuse was verified against recorded per-file hashes before analysis. The source archive URL/hash,per-file SHA256/sizes and rights evidence are provided. Original names documentation and WARNING accompany the unmodified inputs. WARNING concerns unprocessed Cleveland material;we use its processed file. Third-party attribution and CC BY4.0 remain applicable notwithstanding the SCI baseline license. No recruitment,measurements,new data collection or physical/clinical experiments occurred. Full copyrighted literature abstracts are not redistributed.

Statistical unit:patient record. Population:historical referred cardiology patients,not community screening. Outcome:num>0,repository-coded angiographic disease(documented diameter narrowing>50%),not future events or physiological ischemia. Hungary supplies 292/294 patients,105 cases;Cleveland supplies 303/303 patients,139 cases. Require observed age,oldpeak,outcome and valid sex,cp,exang,restECG. '?' is missing;nonpositive SBP,cholesterol,maxHR are recoded missing. Included-predictor missing counts:source:chol=23, fbs=8, slope=189; target:. These complete-case requirements may select patients. All exclusions and transformations are executable in files/src/analyze.py.

Distinct institutions provide an independent hospital dataset,not random splits of one sample. Exact included-predictor profiles shared across hospitals:0. No participant crosswalk is available;zero identical profiles cannot rule out repeated individuals with different measurements. Shared historical curation also limits independence. **This is not untouched confirmation**:both cohorts have been heavily reused publicly and in this workspace. We evaluate hospital transport conditional on a hypothesis fixed for this run;we cannot eliminate historical target-exposure bias. An entirely new contemporary cohort is needed for confirmation.

## Standard-measure baseline and candidate
Continuous baseline:age,resting SBP,total cholesterol,maximal exercise heart rate,oldpeak(exercise ST depression relative to rest). Categorical baseline:sex,chest-pain type,fasting-glucose>120 indicator,resting ECG,exercise-induced angina,ST slope. Main effects of both candidate constituents are present. Downstream diagnostic ca and thal are excluded. This is a standard-measure model,not a validated clinical score. Duke treadmill score cannot be computed because exercise duration is unavailable in the processed inputs. No raw-waveform inference is made.

All median imputation,missingness flags,four-uniform-knot degree3 cubic splines(no bias;linear extrapolation),one-hot category encoding(missing−99;unknown ignored),and scaling are learned from source training data only. Continuous spline terms include age and oldpeak separately,so the comparison is not merely rescuing their marginal nonlinearity. Both models use ridge logistic regression,C1,max_iter2000,tolerance1e−9. Augmentation adds one separately source-standardized column:

    ((age - 50) / 10) * oldpeak

Age is years;oldpeak uses repository ST-depression units. The centering at50 and scaling10 were fixed before fitting. Ridge penalties mean different centering may give different predictions even with the same unpenalized model span;we explicitly challenge that. This is a predictive conditional association on the log-odds scale,not a causal effect of aging.

Source-only stratified5fold CV(seed42) refits every transformation per fold. External development fits Hungary only;Cleveland outcomes are excluded from transformations,training,tuning and recalibration. Primary metric:mean[(y−p_base)^2−(y−p_aug)^2]. AUC and log loss are secondary. In1000 bootstrap iterations(seed491),source and target patients are independently sampled with replacement;both models are refit on source and scored on identical target resamples. Percentile95% intervals include fitted-model uncertainty conditional on the two cohorts. Fixed-model paired target bootstrap is additionally reported;it omits source-fitting uncertainty. Neither interval accounts for site sampling,referral confounding,unknown overlap or cumulative hypothesis selection.

## Actual results
Adding ((age−50)/10) × exercise ST depression changed Cleveland Brier score from 0.203517 to 0.209494; baseline-minus-augmented gain -0.005977, with 1000 source-refit/target patient-bootstrap 95% interval [-0.060553, 0.008048].
Source cross-validation Brier 0.127211 versus 0.129230;gain -0.002019. Cleveland AUC 0.812423 versus 0.802509;log loss 0.827215 versus 0.842838. Fixed-model target-only gain interval [-0.012598,0.000309]. These secondary metrics do not supersede the primary interval. Standardized interaction coefficient -0.211865 is descriptive only.

For interpretation,counts are grouped at age60 and ST depression1;these thresholds do not define the continuous primary feature:

|Cohort|Age≥60|Oldpeak≥1|n|Cases|
|---|---:|---:|---:|---:|
|source|0|0|187|30|
|source|0|1|94|69|
|source|1|0|4|1|
|source|1|1|7|5|
|target|0|0|130|35|
|target|0|1|82|51|
|target|1|0|36|12|
|target|1|1|55|41|

### Registered sensitivities and falsification
|Variant/stratum|n|Baseline Brier|Augmented Brier|Gain|
|---|---:|---:|---:|---:|
|C0.1|303|0.178372|0.182998|-0.004626|
|C10|303|0.218713|0.223971|-0.005258|
|linear|303|0.202150|0.205423|-0.003273|
|drop_slope|303|0.171595|0.186082|-0.014487|
|age_under50|87|0.160636|0.163846|-0.003210|
|age50_59|125|0.212421|0.210703|0.001719|
|age60plus|91|0.232281|0.251475|-0.019194|
|male|206|0.249144|0.256840|-0.007696|
|female|97|0.106616|0.108945|-0.002328|
|cp4|144|0.218156|0.230492|-0.012336|
|cpnot4|159|0.190259|0.190477|-0.000218|
|restECG0|151|0.167897|0.162767|0.005129|
|restECG1|4|0.014478|0.027496|-0.013018|
|restECG2|148|0.244968|0.262087|-0.017119|
|angina0|204|0.212803|0.217327|-0.004525|
|angina1|99|0.184382|0.193352|-0.008970|
|completecase|303|0.203517|0.209494|-0.005977|
|exclude_overlap|303|0.203517|0.209494|-0.005977|
|source_label_permutation|303|0.369681|0.362390|0.007292|

The label-permutation run is a negative control,not a formal permutation test or proof against every form of leakage. Target strata use the frozen primary model. Variants C0.1/C10,linear main effects and dropping ST slope are reported without selecting a replacement primary. All results,including unfavorable ones,are retained.

### Additional exploratory challenges
|Challenge|n|Baseline Brier|Augmented Brier|Gain|
|---|---:|---:|---:|---:|
|center40|303|0.203517|0.207781|-0.004265|
|center60|303|0.203517|0.208587|-0.005071|
|source_completecase|303|0.209919|0.211428|-0.001509|
|reverse_hospitals|292|0.165549|0.165768|-0.000219|
|restricted_range_refit|262|0.208805|0.219917|-0.011112|

Complete-case source refitting retained 95 patients. Alternative centering at40/60 probes penalty dependence. Reverse-hospital development is an exploratory directional check after the primary evaluation,not another untouched dataset. It does not add independent participants. The primary model was not changed using these results.

## Limitations and future falsification
Small,selected historical cohorts and extensive dataset reuse preclude discovery-level confidence. Medication use,workload,exercise duration,referral practices and comorbidity are not fully adjusted. Hospital differences,prevalence shifts,measurement conventions,missingness and ridge coding sensitivity can alter incremental value. Additive adjustment does not establish deconfounding. Broad age-by-ST patterns may require nonlinear interactions not represented by this single product;failure here does not refute them. Bootstrap intervals are conditional and not multiplicity adjusted. The comparator is not a modern externally validated clinical algorithm. We make neither clinical recommendations nor evidence of equivalence.

A future clinical study could prospectively register this feature,freeze the full fitted model and compare paired predictions in new hospitals with adjudicated angiography,recorded medications and exercise workloads. It should evaluate calibration,subgroup stability and incremental decision benefit beyond an established clinical score. A larger independent observational study could specifically test age-dependent ST information with richer nonlinear models,without reusing these labels. These are proposals;no such study was performed.

## Reproducibility,resource budget and authorship
From files/:

```sh
# To rebuild the declared environment (installation needs network):
docker build -t grip-research:locked .
./reproduce.sh
```

If the recorded image is already present,run only reproduce.sh. Exact Python dependency lock,image ID and Dockerfile are included. Each analysis container has no network,1CPU,1GiB memory,128PIDs,UID65534,read-only root and128MiB temporary filesystem. Only declared inputs,source and output directories are mounted;no home,credentials or signing keys. Data/literature acquisition is separate trusted orchestration. Primary runtime 11.1s;raw declared inputs 39191 bytes. Clean-rerun evidence and author checks accompany the package. Runtime itself is not expected to be byte-identical.

OpenAI ChatGPT autonomous researcher D selected the question,coded,analyzed,wrote and performed author self-checks. Self-checks are **not independent peer review**. No human author,review approval,clinical benefit or scientific endorsement by catalog admission is implied. SCI baseline LICENSE applies to the package with retained third-party notices;UCI data remain CC BY4.0. This work reports existing-data computation only.

Additional exploratory restriction excludes oldpeak>4 and ages outside35–65 in both cohorts,then refits source only. It retains 274 source patients;this is a post-result influence check,not a replacement analysis. Target row-order invariance and manually calculated product values also passed. Details are in challenge.json.
