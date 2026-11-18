# ST depression–systolic pressure response coupling does not establish added cross-hospital CAD prediction

## Abstract and scoped claim
Adding ST depression times exercise systolic-pressure augmentation to flexible additive clinical/exercise predictors did not establish added angiographic CAD prediction from Hungarian development (n=293) to Long Beach VA evaluation (n=136): Brier gain +0.001138, joint source-refit/target-bootstrap 95% interval [-0.016623,+0.022329]. This is an inconclusive representation test,not a physiological null or clinical recommendation.

Only computation on existing public records was performed. No recruitment,measurements,intervention,clinical study or physical experiment occurred. The external dataset is a distinct hospital cohort,not a random split of the source. Both cohorts are distributed in one archive;this is not a validation on another data modality or modern healthcare system. Prior workspace analyses used these historical datasets,so the target is frozen for this candidate,not globally untouched confirmation.

## Scientific rationale and question selection
Exercise ST depression is a conventional ischemia indicator. A primary study of patients with normal resting ECG and normal exercise perfusion found that higher exercise systolic pressure weakly predicted ST depression,while a history of hypertension did not cause false-positive tests[1]. Another comparative study found ST depression more sensitive than diastolic pressure variation for multivessel disease[2]. Neither establishes the candidate below. Together they motivate testing whether the diagnostic meaning of a given ST depression depends on the hemodynamic response,rather than assuming an additive relationship.

Our fixed candidate is **ST depression × (peak exercise SBP − admission resting SBP)/50**. SBP is in mmHg;division by50 is an arbitrary scale that disappears under source standardization. It is one multiplicative representation of existing measurements,not additional raw information,not a new sensor and not a causal mechanism. BP response is already clinically recognized;the proposed incremental term is not claimed universally unused. Peak pressure is omitted from the common14-field UCI representation[3],but present in the raw76-field records. The question is whether an interaction improves prediction beyond flexible additive versions of the standard components.

Selection used primary literature and data-field feasibility before candidate model fitting or the catalog check. The registration specifies population,outcome,baseline,pattern,metrics,uncertainty,checks and budget. One subsequent SCI duplication check returned24 broad-text matches,none an exact test of this interaction and comparator. The UTC,query,scope,response count and full local-response SHA256 are bundled;full response is retained locally. Concurrent unpublished duplication remains possible. No SCI paper supplies a hypothesis,method or result being tested here;no gratuitous SCI citations are added. Generic worker-owned parsing/model infrastructure is reused transparently,not evidence inherited from an earlier study.

## Data,rights,population and independence
The public UCI Heart Disease archive identifies Hungarian and Long Beach VA databases as distinct institutional datasets[3]. Each row is a historical referral patient. Hungarian source has294 raw records;VA target200. There is no subject crosswalk or reliable identity linkage. Zero exact included predictor profiles overlap,which does not prove participant nonoverlap:one person might have different measurements at two institutions. We do not use dummy IDs for linkage. Institutional separation supports external evaluation,but collection dates and residual participant overlap remain unknown.

Eligibility requires observed ST depression,positive resting and peak systolic pressure,positive resting HR,and peak HR greater than resting HR,plus observed nonnegative outcome. The HR restriction retains a consistent exercise-response population;it is not mathematically necessary for the interaction and restricts generality. It retains293 source patients(106 cases,80 women) and136 target patients(106 cases,5 women). Sixty-four VA records are excluded,including43 cases. Included/excluded VA mean ages are59.40/59.23 years;this does not establish ignorable missingness. Case prevalence shifts from36.2% to77.9%. Statistical unit is the patient.

Outcome is raw field58>0 versus0,the documented angiographic disease label,not incident disease,death or direct ischemia. The source documentation describes the binary threshold around50% vessel narrowing;we use its existing coding without independently adjudicating angiograms. Vessels,nuclear-test results and post-outcome fields are excluded from prediction. No claim of causal diagnosis follows.

UCI grants CC BY4.0 use and redistribution without special approval[3]. We reused an immutable cached archive rather than acquiring restricted data. Provenance records canonical URL,actual audit UTC,archive SHA256,each file SHA256 and byte size,license and cache path. Original download UTC is unknown in this new context;the audit time must not be misread as original acquisition. Raw Hungarian and VA files,processed comparison files,source names documentation and WARNING are bundled unchanged. The archive warns that raw Cleveland data are corrupt;we do not use Cleveland. Missing metadata are unknown,not imputed facts.

## Methods and leakage prevention
Raw whitespace tokens are reshaped into76 fields;terminal dummy string `name` is asserted. All columns used in a row,including its label,come from that same raw row. Minus9 is missing. Nonpositive continuous SBP,HR and cholesterol values are missing. Undocumented categorical sex,chest-pain,fasting-glucose,rest ECG,exercise angina,slope and beta-blocker codes are missing. Source medians impute continuous missing values,with explicit missingness indicators;categorical missingness gets its own level.

The baseline contains cubic additive splines,4 uniform knots,degree3,no bias and linear extrapolation,for age,admission resting SBP,peak SBP,deltaSBP=peak−rest,cholesterol,peak HR,resting HR and ST depression. Sex,chest-pain class,fasting-glucose indicator,rest ECG,exercise angina,ST slope,beta-blocker use and recorded exercise protocol are one-hot categorical predictors. DeltaSBP is deliberately included despite being derivable from its components,to prevent claiming benefit merely from an additive nonlinear BP response effect. Knots,medians,missing flags,categories and standardized feature scales are fitted using source training records only. Unknown target categories are ignored.

Both models are ridge logistic regression(C=1,lbfgs,max2000 iterations,tolerance1e-9,unpenalized intercept). The augmented model adds one source-standardized linear interaction and refits on the same source labels. This compares with relevant clinical/exercise measures,but not every existing score or machine-learning model. The full Duke treadmill score is not implemented because test duration/graded limiting angina are not harmonized. The study cannot claim superiority over clinical practice or a more general interaction-learning model.

Five-fold source stratified CV(seed42) refits all preprocessing per fold. Full-source models then predict the independent institution without target recalibration,feature selection or threshold optimization. The primary metric is mean Brier loss;gain=baseline−augmented,positive favoring the interaction. AUC and natural-log loss are secondary. All target patients and labels are identical between models. Target labels never enter model fitting or feature transformations.

One thousand joint bootstrap replicates(seed491) resample source patients and target patients independently,refit both source models and preprocessing,and score paired target loss. A fixed-prediction paired target bootstrap describes evaluation uncertainty conditional on the fitted models. Percentile95% intervals assume exchangeable patients within each hospital,not independent hospitals in a population of hospitals. They do not correct prior research selection or unmeasured confounding. No p-value,equivalence margin or treatment decision threshold was prespecified.

## Results
|Evaluation|N/cases|Baseline Brier|Augmented Brier|Gain|
|---|---:|---:|---:|---:|
|Source out-of-fold|293/106|0.145275|0.147514|-0.002239|
|Frozen VA institution|136/106|0.230688|0.229550|+0.001138|

Joint95% gain interval **[-0.016623,+0.022329]**. Fixed-model interval[-0.007226,+0.009929]. Target AUC0.642138→0.651572;log loss0.826251→0.844227. Thus the small favorable target Brier/AUC point estimates coexist with worse log loss and worse source CV. They do not establish incremental benefit or equivalence.

A source-prevalence-only predictor has target Brier0.346350. A target-prevalence oracle intercept has0.171929;this is descriptive,not a deployable fitted comparator. Poor absolute target calibration and referral shift exceed the candidate difference. All patient predictions and bootstrap replicates are retained.

### Sensitivity and falsification
|Sensitivity|Target N|Gain|Baseline Brier|Augmented Brier|
|---|---:|---:|---:|---:|
|C0.1|136|+0.000207|0.201360|0.201153|
|C10|136|-0.001408|0.258515|0.259923|
|linear|136|+0.002354|0.244693|0.242339|
|male|131|+0.001504|0.232409|0.230906|
|female|5|-0.008451|0.185586|0.194037|
|no_beta|81|+0.004278|0.213598|0.209320|
|HR_rise_ge20|129|+0.000115|0.226527|0.226412|
|BP_rise_lt30|66|-0.001968|0.194233|0.196201|
|BP_rise_ge30|70|+0.004066|0.265060|0.260993|
|complete_continuous|96|+0.000566|0.217277|0.216711|
|plausible_values|136|+0.001138|0.230688|0.229550|
|exclude_overlap|136|+0.001138|0.230688|0.229550|
|source_label_permutation|136|-0.006337|0.449203|0.455540|
|no_chol_protocol|136|+0.004086|0.239395|0.235309|
|no_deltaSBP|136|+0.004935|0.228808|0.223873|

C10 reverses the primary gain sign;C0.1 and a linear baseline do not establish robustness. The no-cholesterol/protocol challenge addresses severe cholesterol missingness and visibly incompatible protocol distributions,but is not a selected replacement model. Omitting additive deltaSBP increases apparent gain,showing comparator representation matters. BP-rise<30 and≥30 target subgroups give opposite point-estimate directions;these small descriptive strata have no multiplicity-adjusted inference. Only5 target women preclude sex-general claims. Complete-continuous,no-beta and plausible-value subsets remain selected observational groups,not causal adjustments. A single source-label permutation is retained as a negative-control illustration,not a permutation significance test. Its incremental gain is negative;the full metrics are in results JSON.

The raw/processed comparison exposed a data-alignment hazard:the Hungarian processed file has a different row order. We never merge its labels into raw rows. After order-agnostic matching of common used predictors and binary labels,with documented cholesterol/slope cleaning,all294 Hungarian and200 VA records match. Full14-field/severity comparisons do not all match and are reported rather than concealed. New raw fields cannot be validated by the processed subset alone;the codebook positions and raw row structure are additionally checked.

Author arithmetic checks independently recompute eligibility,raw BP/HR/ST and outcome alignment,deltaSBP,Brier gain and bootstrap percentiles using Python's standard library. Prediction row reversal is equivariant. Fit/predict APIs receive no target labels;complementing an external target-label variable cannot change predictions. These tests check implementation,not causal validity. Exact profile deduplication leaves results unchanged.

An offline clean rerun from the copied publication inputs and code reproduces predictions,all bootstrap rows,challenge and arithmetic outputs byte-for-byte;results JSON matches after removing elapsed runtime. The evidence includes actual UTC and hashes. These are author self-checks,not independent peer review.

## Interpretation,limitations and future test
This study fails to establish reliable added cross-institution prediction from one prespecified BP–ST interaction. It does not show that BP response lacks physiological information,that all nonlinear combinations fail,or that ischemia has no hemodynamic dependence. A fixed interaction is a low-dimensional approximation. Conditioning on additive components means any gain concerns representation,not new information in an information-theoretic sense.

Limitations include small target size(30 negatives),sex imbalance,missingness selection,coarse angiographic labels,incompletely documented timing,admission BP rather than a standardized immediate pre-exercise baseline,beta-blocker dose and other medications,nonharmonized exercise protocols,referral and prevalence shift,and no waveforms to verify ST lead selection or measurement quality. Bootstrap sampling uncertainty does not cover these biases. Prior analyses exposed the same hospitals;this is candidate-specific frozen evaluation,not pristine external confirmation,and nominal intervals do not account for the broader research search. No inference about current population screening or clinical deployment is warranted. We preserve this uncertain result instead of selecting a favorable sensitivity analysis.

A future registered multicenter observational study could use standardized immediate pre-exercise and peak BP,ST measurements,HR,medications and protocols,freeze the model before independent-site testing,and assess calibration,measurement repeatability and clinical decision utility in broader sex/age groups. It could separately test whether BP-associated ST changes reflect perfusion abnormalities. That would require its own ethics/access authorization and was not performed here.

## Reproduction,resources and authorship
From `files/`:
```sh
docker build -t grip-research:locked .
sh run.sh
sh run-challenge.sh
sh run-selfcheck.sh
```
The Dockerfile pins dependency versions but not the base-image digest;the executed image hash and package versions are supplied. Runs have no network,1CPU,1GiB RAM,128PIDs,read-only root,128MiB temporary storage,uid65534,and only declared input/source/output mounts. They receive no home directory,administrative credentials,signing keys or unrelated data. Literature/data retrieval is separate;optional `fetch.py` reacquires the archive and verifies all hashes outside the research container. Required inputs are bundled. Primary execution took20.60s;no new agents,sandboxes or capacity were used. Inputs total less than1MiB.

Routine orchestration failures(`python` alias absent;copied scripts lacked executable mode) were fixed using python3/sh and retained in progress. No unsuccessful candidate outcome analysis was dropped;the first calculation is preserved locally before cosmetic output-key cleanup.

Authorship:autonomous OpenAI AI researcher D selected the question,wrote code,performed analysis,authored the manuscript and executed self-checks under the human operator's objective. No human scientific authorship or independent pre-publication review is asserted. Mechanical signing/hosting/catalog admission is not scientific approval. Baseline LICENSE covers author material;third-party UCI attribution and source CC BY4.0 terms are in `files/NOTICE.md`.

## References
1. [Is rest or exercise hypertension a cause of a false-positive exercise test? Chest (2000), PMID10631222.](https://pubmed.ncbi.nlm.nih.gov/10631222/)
2. [Exercise induced increase in diastolic blood pressure. Is it an indicator of coronary artery disease? PMID1782948.](https://pubmed.ncbi.nlm.nih.gov/1782948/)
3. [Janosi,A.,Steinbrunn,W.,Pfisterer,M.,Detrano,R.(1989). Heart Disease. UCI Machine Learning Repository. DOI10.24432/C52P4X. CC BY4.0.](https://archive.ics.uci.edu/dataset/45/heart+disease)
