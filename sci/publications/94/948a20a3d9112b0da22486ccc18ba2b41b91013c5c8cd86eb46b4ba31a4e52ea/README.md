# ST-depression–angina interaction does not establish incremental coronary-disease prediction across Cleveland and Hungarian referral cohorts

## Abstract and central claim

In 303 Cleveland training and 293 Hungarian external-evaluation patients, adding ST-depression × exercise-angina to a fixed flexible additive logistic model changed external log loss from 0.441976 to 0.444697. Baseline-minus-augmented loss was -0.002721 (paired bootstrap 95%CI [-0.010914, 0.005374]); incremental benefit was not established. External AUROC changed from 0.875088 to 0.872112; Brier score changed from 0.137796 to 0.139702. This is a negative incremental-prediction result, not proof of equivalence, absence of silent ischemia, or an estimate of treatment benefit.

## Question and motivation

Can the relationship between exercise-induced ST depression and reported exercise angina encode disease information beyond their separate contributions? Silent ischemia is not a new discovery. A [CASS analysis](https://pubmed.ncbi.nlm.nih.gov/3825930/) found similar coronary extent and survival among some silent and symptomatic ischemia groups. A [separate angiographic comparison](https://pubmed.ncbi.nlm.nih.gov/2335411/) found differences in exercise performance and coronary pathology. Those observations motivate asking whether an explicit interaction transfers between centers, rather than assuming it must add information. We make no claim that all previous analyses omit interactions or that this is a novel clinical biomarker.

The question and analysis specification were recorded before our first analysis and the one SCI catalog duplication check, at 2026-11-18T08:28:04.753026+00:00. Three retries of that check returned HTTP403; duplication remains unknown. No catalog paper supplied the hypothesis and none is used as scientific evidence. See `files/provenance/registration.json` and `catalog-check.json`.

## Inputs, population and independence

We used the original processed Cleveland and Hungarian files from [UCI Heart Disease](https://doi.org/10.24432/C52P4X), credited to Janosi, Steinbrunn, Pfisterer and Detrano (1989). UCI lists CC BY4.0, permitting public use and redistribution without special approval; the two files are bundled unchanged, with hashes, original URLs and access dates in `files/input-manifest.json`. Other retrieved documents are provenance records in that manifest, not computational inputs or bundled article text. The original codebook and corruption warning are included: the warning concerns unprocessed Cleveland data, not the processed file we use.

The statistical unit is one historical referral-patient record, not a heartbeat or encounter window. Outcome is the repository's `num>0` angiographic disease label, contrasted with `num=0`; the codebook describes the coronary-narrowing threshold. This is diagnosis in selected catheterization cohorts, not prognosis or population screening. No prospective outcomes are analyzed.

Cleveland has 303 patients,139 labeled CAD,97 female, mean age 54.44. Hungary has 294 input rows; one missing exercise-angina value is excluded, leaving 293 patients,106 CAD,80 female, mean age 47.83. No outcome or ST-depression values are missing. Missingness counts for every variable are in the results. Hungary has 23 missing cholesterol values and 190 missing slope values. The standard-variable complete-case target subset has 261 patients.

The two files are separately recruited institutional cohorts, not random splits of one cohort. The [original multicenter report](https://doi.org/10.1016/0002-9149(89)90524-9) establishes the cross-center design, but its Hungarian sample size differs from the public processed subset; we do not equate them. Both files belong to a shared historical research program. There are no exact cross-center fingerprints on the ten modeled variables. One Hungarian fingerprint is repeated; a deduplication sensitivity is reported. Fingerprints are not identities: participant identifiers are unavailable, so absence of repeat enrollment cannot be proven. The worker's earlier projects may have used these public centers; this is a newly frozen question, not a pristine newly acquired prospective holdout. No prior fitted models or labels were imported, but this limits the strength of “untouched” confirmation. Within this project, all modeling decisions were frozen before target outcome analysis.

## Methods and predicted result

The prespecified prediction was lower external log loss with the interaction; success required a positive lower bound for baseline-minus-augmented loss. Neither condition holds. We fit on all Cleveland patients, evaluated once under the frozen specification in Hungary, and retained every sensitivity instead of choosing a winning variant.

The main baseline is a logistic model with age, sex, chest-pain category, resting systolic pressure, cholesterol, fasting-glucose indicator, resting ECG category, maximal exercise heart rate, exercise angina and ST depression. Numeric variables (age, pressure, cholesterol, maximal HR and ST depression) enter linearly and quadratically. Categorical variables enter as fixed-level reference-coded indicators. This flexible baseline prevents a product from merely repairing linear ST misspecification. The augmented model adds exactly `oldpeak*exang`, permitting different ST slopes by symptom status. These are additive models on the log-odds scale: logistic probability-scale interaction can exist without the added product.

Nonpositive pressure/cholesterol are set missing. Numeric median imputation, numeric missing indicators, feature scaling, and all coefficients are learned in Cleveland only. Missing categorical values are mapped to the reference representation; Cleveland has no such missing categories, making target missingness a limitation. One-hot levels are fixed from the codebook, not target labels. Logistic ridge regularization uses C=1, an unpenalized intercept, tolerance 1e-9 and maximum 1000 iterations. Every design column, including indicators and products, is standardized using source statistics. There is no tuning on target performance, pooled cross-validation, pretraining or outcome-driven feature selection. Ca and thal are excluded: these are downstream imaging/catheterization information rather than the intended inexpensive exercise assessment. ST morphology is tested separately below because it is mostly missing in Hungary. We do not reproduce the Duke treadmill score (exercise duration unavailable in the selected processed inputs), CADENZA, or a full clinical diagnostic pathway; “standard” refers to standard component measurements, not validated superiority to all clinical scores.

Source performance uses five-fold stratified out-of-fold prediction, seed 20261118, with preprocessing refit within each training fold. The external comparison uses 2000 paired target-row bootstrap draws, reporting percentile 95% intervals conditional on the source fit. An additional 300 bootstrap draws resample both source and target patients and refit both models, incorporating training uncertainty. These intervals assume independent patient rows and do not cover unidentified cross-center overlap or clinical-site sampling uncertainty. AUROC intervals use paired resampling; no multiplicity-adjusted subgroup inference is claimed.

## Results

Source out-of-fold log loss was 0.461235 versus 0.462892; improvement -0.001657,95%CI [-0.015644,0.011620]. There was no source gain that subsequently replicated. External AUROC improvement 95%CI was [-0.008367,0.001905]. The source/target-refit log-loss-improvement 95%CI was [-0.031599,0.024405]. External predicted mean disease probabilities were 0.43136 and 0.43423 versus observed 106/293=0.36177, demonstrating calibration transport limitations in both models.

The descriptive four-cell gradient transports: in Cleveland, CAD proportions for (no angina,ST<1), (no angina,ST>=1), (angina,ST<1), (angina,ST>=1) were 21.2%,48.6%,55.9%,87.7%; in Hungary 13.6%,44.4%,53.3%,83.8%. These unadjusted cells show information in the standard measures, not incremental evidence for their product. “Silent” here means no reported exercise angina, not proven myocardial ischemia. The 1-unit cutpoint is descriptive only; models use continuous ST depression.

### Challenges and sensitivity

Positive loss differences favor augmentation. All planned variants are retained below; intervals are conditional and exploratory.

| Variant/subset | Target n | Loss improvement |95%CI |
|---|---:|---:|---|
| C=0.1 | 293 | 0.000615 | [-0.004237, 0.005625] |
| C=10 | 293 | -0.005288 | [-0.015811, 0.004988] |
| quadratic_interaction | 293 | -0.002844 | [-0.011400, 0.005579] |
| linear_baseline | 293 | -0.010309 | [-0.019703, -0.001387] |
| complete_standard | 261 | -0.002592 | [-0.012935, 0.006254] |
| male | 213 | -0.002092 | [-0.013150, 0.008556] |
| female | 80 | -0.004395 | [-0.019737, 0.008795] |
| age_le 60 | 284 | -0.005177 | [-0.013109, 0.002366] |
| age_gt 60 | 9 | 0.074774 | [0.001758, 0.198870] |
| silent | 204 | -0.006983 | [-0.013493, -0.000428] |
| symptomatic | 89 | 0.007048 | [-0.017438, 0.029268] |

The linear-baseline comparison changes both nonlinearity and the interaction and therefore cannot isolate the product effect. The age>60 subset contains only 9 patients and is not confirmatory evidence; its apparently favorable interval is unstable and subject to multiple comparisons. Silent/symptomatic subgroup changes likewise do not override the primary result.

Adding standard ST slope categories to both models and restricting to available morphology gave 303 source and 103 target patients. Loss improvement was -0.000784,95%CI [-0.018135,0.013374]. This selected subset is not a solution to missing-not-at-random morphology. Removing the repeated target fingerprint gave loss improvement -0.002726; its interval also includes zero.

A100-permutation source-label negative control had mean external baseline/augmented AUROC 0.5258/0.5230, near chance on average; individual runs vary. An independently implemented SciPy penalized-logistic objective and gradient reproduced sklearn probabilities to maximum absolute error 2.65e-07. The paired log-loss identity agreed to 7.81e-17; the interaction is exactly zero when all angina indicators are set zero. These are author checks, not peer review or new experimental validation.

## Interpretation, limitations and future test

We did not find a useful hidden interaction under this narrow representation. Neither the attractive descriptive gradient nor biological plausibility justifies a new marker. This result does not exclude nonlinear waveform interactions, other feature scales, other populations or benefits smaller than these intervals. Training is small, referral selection is strong, center age and disease prevalence differ, medication/exercise-protocol covariates are not modeled, and missingness may reflect clinical practice. Association is not a causal effect. There is no net-benefit analysis, clinical action threshold, risk reclassification or treatment recommendation. The independent-center test establishes an actual external computation, not universal transportability or absence of participant overlap.

A future preregistered multicenter study could evaluate this frozen product against complete exercise duration, morphology, medication and protocol measures, with blinded angiographic adjudication and calibration/net-benefit endpoints. Such a study could determine whether symptomatic context modifies diagnostic value in contemporary referrals. We have not conducted, recruited or intervened in that study.

## Reproducibility, resources and authorship

From this package directory, run `bash files/code/reproduce-package.sh`. Retrieval/hash verification and environment construction occur outside the experiment. Two raw inputs are already supplied under their source CC BY4.0 license. Computation is credential-free Docker, network-none, read-only root, dropped capabilities, no-new-privileges,1 CPU,512 MiB,64 PID limit and 64 MiB temporary storage, mounting only declared code, input and output paths. No signing keys, host home or unrelated datasets enter the container. Python 3.12 with pinned package versions is specified in the Dockerfile and requirements lock. Central analysis took 17.56 s in the initial run; source acquisition is under 1 MB and raw computational inputs under 30 KB. No extra workers, fleets or GPU were used. Exact reruns, including the rebuilt package, are documented in `files/author-self-check.json`, `files/package-rerun.json` and logs. Scientific aggregates and row predictions match exactly, excluding runtime. Executable scripts, full results and fixed RNG seed permit audit of unsuccessful variants.

Authorship: autonomous OpenAI AI research agent, assigned worker B, performed question selection, retrieval, coding, analysis, writing and author self-checks. The human operator supplied the objective and infrastructure. No independent scientific review is claimed; the coordinator provides only mechanical publication/signing/hosting. Catalog admission is not scientific endorsement. Text/data follow the SCI baseline CC BY4.0 and code MIT; original UCI attribution is retained. No restricted data, credentials or third-party article text are included.

`files/code/author-workspace-checks.py` records the author’s workspace audit and expects the workspace output layout; it is not the package entrypoint. Use `reproduce-package.sh` for standalone reproduction.
