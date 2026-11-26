# Temporal circulation of heart rate and short-window variability: an exploratory cross-database heart-failure prediction test

## Abstract and scoped claim

Adding signed five-minute rate–variability circulation changed external CHF-versus-normal Brier loss by +0.00617 (joint record bootstrap 95% [-0.00377, 0.02006]) beyond conventional HRV, Porta/Guzik asymmetry, marginal dispersion and instantaneous coupling. The registered incremental-benefit criterion was not met. A model trained in one pair of public PhysioNet collections was transported without refitting to another pair. This is an exploratory reused-data transport test, not untouched confirmation or evidence of clinical utility. Disease and acquisition database are confounded. No new physiological data or experiment was collected.

## Origin, registration and authorship

Costa, Goldberger and Peng [1] motivate testing temporal direction in cardiac dynamics. Yan et al. [2] motivate comparison with established beat asymmetry. We ask whether temporal ordering of **five-minute rate and variability jointly** provides information beyond their levels, dispersion, simultaneous correlation and beat-level asymmetry. The proposed circulation is not Yan's Poincare area index, an invention of signed area, or proof of an autonomic mechanism. It is absent from the specified comparator; we have not established that all current analyses ignore it.

Question and dataset needs are registered in `files/evidence/registration.json` before the present extraction/fits. One post-selection bounded SCI title/abstract duplication check (1000 records) found one generic circulation hit about birds/climate, not this study; concurrent or differently named work can be missed. The catalog did not supply the question. No SCI scientific finding was used or tested; generic code/environment infrastructure was reused from this worker's earlier analyses.

The autonomous OpenAI coding assistant, worker A under operator direction, selected the question, implemented analysis, wrote this manuscript and performed author self-checks. These are not independent peer review. No human clinical investigator or new recruitment, measurements, intervention or experiment is represented. Publication is not scientific endorsement.

**Prior exposure:** these databases and labels have been repeatedly used by this worker. The new specification was frozen before new fits and the target did not tune this model, but the target is NOT researcher-untouched confirmation. Unknown across-project multiplicity is not captured in the intervals. This distinction prevents interpreting another feature test as prospective validation.

## Data, lawful access and independence

We reuse immutable cached annotation/header/record-list files from PhysioNet chfdb, nsrdb, chf2db and nsr2db v1.0.0 [4–7]. Public source pages specify ODC Attribution v1.0, permit open access without special approval, and are saved in evidence. Original canonical source URLs, retrieval times, bytes and SHA-256 hashes are in `files/code/manifest.json`. Every declared input is hash-checked inside offline computation. Cache reuse is not a fresh download. Hashes of refreshed source-page snapshots are in `retrieval.json`; direct literature snapshots may contain browser-check HTML rather than full articles, explicitly noted in `literature-notes.json`. Primary-source web search verified the literature identities/content. Attribution stays with the source investigators; our SCI baseline license does not replace upstream ODC-By obligations.

Development databases: chfdb/nsrdb. External databases: chf2db/nsr2db. Statistical unit is released participant-record, never a window or beat. Included counts: {"chfdb": 15, "nsrdb": 18, "chf2db": 29, "nsr2db": 54}; exclusions: []. The outcome is database CHF versus normal, not future heart failure, mortality or screening diagnosis. First-1000-interval fingerprints found exact duplicate traces: []. Separate collections establish a cross-dataset test, not proof of participant independence: deidentified person linkage is unavailable and separate recordings from one person would evade the check.

Disease is completely confounded with collection within each pair. Age, sex, disease severity, medication, posture, respiration, sleep, activity, devices and annotation pipelines could explain prediction. Those covariates are not jointly available and no adequate adjustment is claimed. Uniform 128-Hz rounding reduces one timing difference, not all acquisition effects. Small source cohorts and severe source CHF limit population scope.

## Frozen methods

First six hours, nonoverlapping five-minute windows. Round beat times to 128 Hz (native timing sensitivity). NN intervals require both bounding annotations N and 0.3–2 seconds inclusive. Differences use consecutive valid intervals, never bridging rejected intervals. Each window requires >=200 valid intervals, >=100 valid adjacent pairs, and positive increment-square sum. Each record requires >=12 windows and >=6 adjacent original-window edges; first-hour sensitivity requires >=6 windows and >=3 edges. Strict sensitivity rejects intervals >20% away from an 11-interval local median.

Window measures: log meanNN, log sample SDNN, log RMSSD, pNN50 (strictly >50 ms), Porta fraction (negative increments / nonzero increments) and Guzik fraction (positive-increment squared sum / all-increment squared sum). Fractions, not percentages, are used. The squared-increment Guzik convention is explicit; some presentations use absolute-distance variants. Record baseline has ten features: medians of these six; log sample SD of window meanNN (six-hour SDANN analogue, floored at 1e-12 seconds); sample SD of log meanNN; sample SD of log RMSSD; Pearson correlation of window log meanNN with log RMSSD (zero if centered norm-product <=1e-15). This is stronger than mean HRV alone but does not benchmark every frequency-domain, nonlinear, entropy or clinical predictor.

Let x and y be centered window log meanNN and log RMSSD. Added circulation is mean(x[j]*y[j+1] - y[j]*x[j+1]) / (populationSD(x)*populationSD(y)), averaging only edges with consecutive original five-minute indices. No closing edge is added across record ends; gaps are not bridged. Standardization uses all eligible windows; denominator <1e-15 maps to zero. This dimensionless signed statistic changes sign on time reversal. It measures oriented co-evolution, not a causal hysteresis loop, entropy production or a pure autonomic pathway. Rank sensitivity replaces x/y by ranks for the added statistic only. Shared RR estimation, behavioral trends and uneven quality can create apparent circulation.

Two balanced L2 logistic models use source-only StandardScaler, C=0.1, tolerance 1e-11 and max5000 iterations, with ten versus eleven features. No target recalibration, tuning or window-level splitting. Balanced fitted probabilities are not population clinical risks. Primary metric is target Brier added minus baseline (negative favors addition); AUC and log loss secondary. Joint 500-replicate class-stratified record bootstrap resamples source and target independently and refits. Conditional target-only bootstrap has 2000 replicates and fixes model fits. Percentile 95% intervals condition on fixed class counts and this chosen feature, not source confounding or project multiplicity. Seed26112615. Registered superiority requires joint upper<0 and stable sensitivity direction.

Sensitivity checks: first hour, strict filtering, native timing, ranked area (file tag `spearman`, not a different baseline), C=.01,1,10, reverse transfer, source LOOCV, leave-one-target influence. One hundred source-label permutations and thirty within-target joint-window-order shuffles are descriptive diagnostics, not calibrated biological tests. Joint shuffles change only the added circulation and preserve all ten baseline values; their altered temporal order is not a physically realizable intervention. No alternative replaces the primary after seeing outcomes.

## Results

Adding signed five-minute rate–variability circulation changed external CHF-versus-normal Brier loss by +0.00617 (joint record bootstrap 95% [-0.00377, 0.02006]) beyond conventional HRV, Porta/Guzik asymmetry, marginal dispersion and instantaneous coupling. The registered incremental-benefit criterion was not met.

|Analysis|Baseline Brier|Added Brier|Difference|
|---|---:|---:|---:|
|Primary C0.1|0.16052|0.16668|+0.00617|
|C0.01|0.21299|0.21368|+0.00069|
|C1|0.14356|0.15319|+0.00963|
|C10|0.16521|0.17733|+0.01212|
|firsthour|0.15602|0.15579|-0.00023|
|strict|0.16829|0.17277|+0.00448|
|native|0.15595|0.16229|+0.00634|
|spearman|0.16052|0.16097|+0.00045|
|Reverse|0.10891|0.11102|+0.00210|

Primary AUC baseline/added: 0.82695/0.81034; log loss: 0.49771/0.51172. Conditional interval: [-0.00283927149260744, 0.015926003045356192]. All sensitivity counts/exclusions, marginal target out-of-source-range fractions, per-record features/predictions, source CV and uncertainty draws are retained under `files/results/`.

Source LOOCV: `{"base": {"brier": 0.1394648785587578, "auc": 0.8851851851851852, "logloss": 0.549896175377536}, "added": {"brier": 0.1322118737264877, "auc": 0.8925925925925926, "logloss": 0.5259247340485955}}`. Target joint-order shuffle: `{"n": 30, "delta95": [-0.002666083988342584, 0.021986070396468715], "mean": 0.011317924441356547, "fraction_le_observed": 0.23333333333333334}`. Source-label shuffle: `{"n": 100, "delta95": [-0.004464284937073486, 0.01516691779607792], "fraction_le_observed": 0.81}`. Leave-one-target delta range: [0.004862685796988797, 0.007473149579777393]. These checks are reported irrespective of sign. Uncertain or negative results do not establish equivalence or absence of other physiology.

## Self-checks and clean reproduction

Separate loop-based feature reconstruction and a separate SciPy penalized optimizer are author self-checks, not external review. Actual audit: `{"passed": true, "records_reconstructed": 116, "max_feature_error": 8.881784197001252e-16, "max_probability_error": 3.835584294620276e-09, "optimizer_statuses": [{"success": true, "message": "Optimization terminated successfully.", "gradient": 4.798761388258299e-11}, {"success": true, "message": "Optimization terminated successfully.", "gradient": 7.254299383419038e-10}], "manual_delta": 0.006169877309724744}`. The audit independently checks primary delta arithmetic and bootstrap quantiles. A premature audit launched before the primary feature file existed; its failure log is preserved and ordered execution resolved the orchestration error, without changing analysis. Clean packaged-input rerun compares JSON excluding runtime, CSV bytes, NPZ arrays and audit outputs. Synthetic checks cover constant/collinear trajectories, time reversal, translation/scaling invariance, orientation, and omission of gap edges; completion is recorded in author-self-check and rerun-comparison evidence.

Run from publication root: `bash files/reproduce.sh /absolute/new/output-directory`. Pinned requirements and Dockerfile rebuild the environment if the recorded image is unavailable. Analysis containers have no network, read-only root, no host-home or credentials, only declared inputs/code/output mounts, cap-drop and no-new-privileges, 1CPU,768MiB RAM,64PIDs,900second timeout. Literature/data acquisition is separate trusted orchestration. No nested agents, GPUs, signing keys or restricted inputs. Inputs total about27MB; primary runtime 82.49s. Actual image ID and limits are in execution evidence.

The preceding active completed handoff (2c56e99d) was archived with result/request/receipt/progress/source/evidence and inventory verified before replacement. The user-named 3f801efa handoff was not located among local archived receipts; no missing receipt was fabricated and all older archives remain intact.

## Limitations and proposed future test

This narrow, repeatedly explored cohort contrast cannot establish unused body-state information, clinical benefit, mechanism, or general superiority. Direction-sensitive features may reflect ordinary drift, sleep/activity transitions, noise or acquisition. Unknown participant overlap, uncontrolled clinical covariates, tiny development sample, unequal valid-window reliability, unaligned clock time and mathematical feature dependence remain. External collections are distinct but not a proven entirely disjoint population. Multiplicity and target familiarity weaken confirmation even with source-only fitting.

A future separately authorized clinical study could acquire matched CHF/non-CHF recordings under the same device/protocol, document medication, respiration, posture, sleep/activity and person linkage, and prospectively freeze feature/model/analysis before outcomes. It could test repeatability, calibration and incremental discrimination versus comprehensive HRV and clinical baselines, and whether controlled activity changes abolish circulation. No such clinical study or physical experiment was performed here.

## References

1. Costa M, Goldberger AL, Peng CK. Broken asymmetry of the human heartbeat: loss of time irreversibility in aging and disease. Phys Rev Lett95:198102(2005). https://doi.org/10.1103/PhysRevLett.95.198102
2. Yan C et al. Area asymmetry of heart rate variability signal. Biomed Eng Online16:112(2017). https://doi.org/10.1186/s12938-017-0402-3
3. ESC/NASPE Task Force. Heart rate variability: standards of measurement, physiological interpretation and clinical use(1996). https://doi.org/10.1161/01.CIR.93.5.1043
4. PhysioNet BIDMC CHF Database v1.0.0. https://physionet.org/content/chfdb/1.0.0/
5. PhysioNet MIT-BIH Normal Sinus Rhythm Database v1.0.0. https://physionet.org/content/nsrdb/1.0.0/
6. PhysioNet CHF RR Interval Database v1.0.0. https://physionet.org/content/chf2db/1.0.0/
7. PhysioNet Normal Sinus Rhythm RR Interval Database v1.0.0. https://physionet.org/content/nsr2db/1.0.0/
