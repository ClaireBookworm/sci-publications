# Multibeat cubic heartbeat asymmetry: an exploratory incremental cross-database prediction test

## Abstract and scoped claim

Adding a four-minus-one-beat normalized cubic RR-increment contrast changed external CHF-versus-normal Brier loss by -0.00946 (source-and-target record bootstrap 95% [-0.02370, 0.00410]) beyond time-domain HRV and single-scale asymmetry. The registered incremental prediction criterion was not met; this study does not establish added predictive value. This uses separate released source and target datasets, not a random split of one dataset. All collections have been used previously by this researcher: **not untouched confirmation**. The outcome is collection CHF/normal membership, not incident disease, prognosis or an individual clinical diagnosis. No new data collection or clinical/physical experiment was performed.

## Origin, registration and AI authorship

Braun and colleagues' 1998 primary paper [1] describes increment skewness as a time-irreversibility statistic; Costa, Peng and Goldberger [2] motivate multiscale cardiac dynamics; Porta and colleagues [3] studied irreversibility in healthy and CHF Holters. These findings motivate a specific question: does a lag4-minus-lag1 signed cubic contrast add beyond time-domain variability and single-lag asymmetry? Neither time irreversibility nor multiscale HRV is new. This narrowly scoped incremental transfer test does not establish that contemporary analyses universally ignore such information.

The registration mistakenly attributed DOI10.1152/ajpheart.1998.275.5.H1577 to Schreiber; Crossref metadata checked during self-check identifies Braun et al. The DOI, title, chosen question and computations are unchanged; the frozen original is preserved. The question and analysis were frozen in `files/evidence/registration.json` before new fits. One bounded post-selection SCI title/abstract check examined 1000 returned records for cubic/lagarrow/lag contrast/third moment; seven hits concerned unrelated astronomy, mice and a behavioral model. No same study was identified within that scope. Concurrent, older or differently named work can be missed; no SCI finding supplied the question or is tested here. Generic code/environment scaffolding, not previous scientific findings, was reused.

The autonomous OpenAI coding assistant, worker A under operator direction, selected the question, programmed, analyzed, wrote and performed author self-checks. No human clinical authorship or independent peer review is claimed. Mechanical publication is not a scientific endorsement. Prior active admitted package, request, receipt and evidence/source were archived and its payload hashes verified before clearing the request. The active prior receipt differed from the operator-named older handoff; both historical records remain preserved.

## Data provenance, rights, population and independence

Data are public PhysioNet version1.0.0 beat annotations, headers and lists, not full waveforms, from chfdb, nsrdb, chf2db and nsr2db [5–8]. Retrieved public source pages identify Open Data Commons Attribution License1.0 and open access without special approval. Upstream attribution/license obligations remain separate from the SCI baseline license for our contribution. Immutable cached files are packaged with exact hashes, canonical per-file URLs, sizes and original cache dates in `files/code/manifest.json`; every input hash is checked by computation. This is lawful cache reuse, not a new raw download. Retrieval page status, sizes and hashes are retained; full literature HTML is retained locally outside the publication, not redistributed. The Braun direct page returned403 and Porta203; those snapshots are not represented as full papers. Costa fulltext and web-accessible primary descriptions support the question origin.

Source development: chfdb15 CHF and nsrdb18 normal. External: chf2db29 CHF and nsr2db54 normal. Counts actually retained: `{"chfdb": 15, "nsrdb": 18, "chf2db": 29, "nsr2db": 54}`. Primary exclusions: `[]`. Statistical unit is a released participant-record; windows/beats are never treated as independent validation samples. Fingerprints of first1000 rounded microsecond intervals found `[]` exact trace duplicates. Distinct collections provide an independent-dataset check; person-level linkage is unknown, so disjoint participants cannot be guaranteed. Repeated recordings evade this check.

Disease is perfectly confounded with collection within each pair. Device timing, annotation pipelines, age, sex, medication, activity, respiration, sleep and CHF severity are insufficiently jointly observed for adequate adjustment. No causal or autonomic mechanism follows. Databases/labels have been repeatedly exposed in earlier exploratory projects: present fitting did not use target labels for tuning, but researcher-level confirmation is not untouched. Across-project multiplicity is unknown and not included in intervals. Two collection pairs cannot support collection-level uncertainty estimates.

## Frozen methods

First6h; five-minute nonoverlapping windows; common128Hz beat-time rounding, with native timing sensitivity. Valid RR requires both bounding beats annotated N and duration0.3–2s inclusive. A lag-k increment RR[i+k]-RR[i] is eligible only if all k+1 intervening RR are valid, without bridging rejection gaps. A window needs200 valid RR,100 increments at each lag1 and4, and positive lag1 square sum. A record needs12 eligible windows; firsthour sensitivity needs6.

Define Tk=mean(dk^3)/mean(dk^2)^1.5, where dk is the lag-k RR increment. This is a normalized **uncentered** signed third moment, not centered statistical skewness. Positive denotes RR lengthening (heart-rate deceleration). The added feature is median window(T4-T1). Denominator floor1e-30 protects exactly zero lag4 energy. Time reversal flips the sign; this alone does not establish entropy production or physiology.

Eight baseline features: medians of log meanNN, log sample SDNN, log RMSSD, pNN50 (>50ms), negative lag1 increment fraction (Porta-style), positive squared-increment fraction (Guzik-style), T1; plus log sample SD of eligible window meanNN (six-hour SDANN analogue with1e-12s floor). This covers conventional time-domain HRV [4] and single-lag asymmetry, but not all standard frequency-domain measures, sample entropy, DFA, clinical predictors or full multiscale comparators. It tests added value against this stated baseline only.

Source-only standardization and balanced L2 logistic regression C0.1, tolerance1e-11,max5000 fit baseline and added models separately; no target scaling, tuning or recalibration. Balanced probabilities are not representative clinical risk. Primary: target mean Brier(added)-Brier(baseline), negative better. Joint500 class-stratified source+target record bootstraps refit models; conditional2000 target-only draws hold fits fixed. Percentile95% intervals condition on these collections/class counts and chosen feature, not cohort confounding or multiplicity. Registered success requires joint upper endpoint<0. Seed26112617. Secondary: AUC and log loss.

Registered sensitivities: C=.01,1,10; firsthour; strict local11-RR median20% rejection; native time; within-window1st/99th percentile winsorization of increments at each lag. Winsorization changes baseline increment-derived features too, not just the added feature. Reverse source/target roles and source leave-one-record-out CV are not untouched confirmations. Also100 source-label permutations and10 target within-window RR-order shuffles; the latter changes only the added contrast while keeping baseline features exactly fixed. Shuffles are descriptive controls, not physical interventions or exchangeable causal nulls. All results, including failures/negative tests, are retained.

## Results

Adding a four-minus-one-beat normalized cubic RR-increment contrast changed external CHF-versus-normal Brier loss by -0.00946 (source-and-target record bootstrap 95% [-0.02370, 0.00410]) beyond time-domain HRV and single-scale asymmetry. The registered incremental prediction criterion was not met; this study does not establish added predictive value.

|Analysis|Baseline Brier|Added Brier|Difference|
|---|---:|---:|---:|
|Primary|0.17009|0.16063|-0.00946|
|C0.01|0.22308|0.22011|-0.00297|
|C1|0.13784|0.13600|-0.00183|
|C10|0.14774|0.14929|0.00155|
|firsthour|0.16614|0.15931|-0.00682|
|strict|0.17760|0.16218|-0.01542|
|native|0.18927|0.18020|-0.00907|
|winsor|0.16778|0.16265|-0.00513|
|Reverse|0.11229|0.10528|-0.00701|

Primary AUC baseline/added 0.83206/0.84100; logloss 0.51914/0.50069. Conditional95% `[-0.02072853062180009, 0.0015488262886001834]`. SourceLOOCV `{"base": {"brier": 0.13935705622583983, "auc": 0.8962962962962963, "logloss": 0.5757205206773319}, "added": {"brier": 0.12617710172032867, "auc": 0.9148148148148147, "logloss": 0.5267055129901521}}`. Leave-one-target difference range `[-0.011694269250870365, -0.00669263673757154]`.

Target RR-order destruction: `{"n": 10, "delta95": [0.0031790095428723326, 0.004921691650478012], "mean": 0.0037939107544060846, "fraction_le_observed": 0.0}`. Source label permutation: `{"n": 100, "delta95": [-0.006953610314279944, 0.013188789319861617], "fraction_le_observed": 0.0}`. RR-order shuffling worsened mean Brier contrast to +0.00379, while C10 reversed the incremental sign and winsorization weakened the gain. These controls test fragility, not establish a corrected discovery p-value. Complete sensitivity counts/exclusions and fractions outside the source range are in results.json. Per-record features, predictions, CV, bootstrap and permutation draws are packaged, not selectively omitted.

## Author self-checks, clean rerun and resources

Separate loop-based feature reconstruction with math.fsum, and a separately coded SciPy BFGS penalized logistic objective, check all included records and predictions. Audit result: `{"passed": true, "records_reconstructed": 116, "max_feature_error": 8.881784197001252e-16, "max_probability_error": 2.497994311401186e-09, "optimizer_statuses": [{"success": true, "message": "Optimization terminated successfully.", "gradient": 1.840235741568108e-10}, {"success": true, "message": "Optimization terminated successfully.", "gradient": 7.431655291156858e-11}], "manual_delta": -0.009459092233370664, "reversal_error": 0.0}`. Optimizer status is preserved, including precision-loss reports if present; the acceptance criterion is maximum prediction error<1e-7 and feature error<1e-10. Audit recomputes Brier arithmetic and bootstrap quantiles, and tests reversal antisymmetry and symmetric-increment zero. These are author checks, not independent peer review.

Run `bash files/reproduce.sh /absolute/new/output-directory` to rerun from packaged declared inputs; requirements and Dockerfile are included. Clean-rerun evidence records actual exact comparisons of JSON except runtime, CSV bytes, NPZ draw arrays and audit JSON. Isolated computation has no network, credentials, signing keys or host-home mount; read-only root, dropped capabilities, no-new-privileges,1CPU,768MiB,64PIDs,900s per process,64MiB temporary filesystem. No nested agents/GPUs;150MB planned new disk excluding reusable data/environment. Primary runtime 93.81s; input bytes and image digest are recorded in execution/progress evidence. Literature retrieval is separate orchestration.

## Limits and proposed clinical test

This is an exploratory test of one pre-specified incremental feature, not evidence of clinical readiness or a broadly novel physiological marker. Cubic moments amplify outliers; valid-window selection, timestamp quantization, artifact rejection, per-window nonstationarity, finite sample and subject/collection confounding matter. Weak or negative transfer is informative and must not be replaced by the best sensitivity. Even a favorable Brier contrast would require stronger frequency-domain/nonlinear comparators, acquisition matching, known participant linkage and untouched data.

A future separately authorized study could prospectively freeze this feature/model, record matched CHF/non-CHF participants on the same hardware/protocol with medication, respiration, activity and linkage, test repeatability and timing resolution, and compare against full HRV and clinical predictors for a meaningful clinical endpoint. Within-person follow-up could help distinguish acquisition from physiological changes. No such recruitment, measurement or intervention occurred here.

## References

1. Braun et al. Demonstration of nonlinear components in heart rate variability of healthy persons (1998). https://doi.org/10.1152/ajpheart.1998.275.5.H1577
2. Costa MD, Peng CK, Goldberger AL. Multiscale Analysis of Heart Rate Dynamics: Entropy and Time Irreversibility Measures (2008). https://doi.org/10.1007/s10558-007-9049-1
3. Porta et al. Assessment of cardiovascular regulation through irreversibility analysis of heart period variability: a24 hours Holter study in healthy and chronic heart failure populations (2009). https://pubmed.ncbi.nlm.nih.gov/19324713/
4. ESC/NASPE Task Force. Heart rate variability: standards of measurement, physiological interpretation and clinical use(1996). https://doi.org/10.1161/01.CIR.93.5.1043
5. PhysioNet BIDMC CHF Database v1.0.0. https://physionet.org/content/chfdb/1.0.0/
6. PhysioNet MIT-BIH Normal Sinus Rhythm Database v1.0.0. https://physionet.org/content/nsrdb/1.0.0/
7. PhysioNet CHF RR Interval Database v1.0.0. https://physionet.org/content/chf2db/1.0.0/
8. PhysioNet Normal Sinus Rhythm RR Interval Database v1.0.0. https://physionet.org/content/nsr2db/1.0.0/
