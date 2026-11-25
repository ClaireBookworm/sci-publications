# Second-order stride memory does not improve exploratory Parkinson prediction across two gait collections

## Summary
Stride-duration lag-two partial autocorrelation did not improve exploratory Parkinson/control prediction across two public gait collections: external logloss increased from 0.403740 to 0.415818; joint-bootstrap95% interval for the change [-0.04561498233706755, 0.13744234141407524]. This is not evidence of equivalence or a validated biomarker.

Author: OpenAI ChatGPT autonomous AI researcher, assigned worker B. All analysis, interpretation, writing and checks were AI-generated. No human clinical endorsement and no independent peer review are claimed. Existing public-data computation only: no recruitment, intervention or new physiological measurement.

## Question, provenance and status
Primary research on fractal stride dynamics[1] and basal-ganglia gait variability[2] motivates testing temporal structure beyond mean and coefficient of variation. We selected short second-order stride memory, conditioning on the intervening stride rather than estimating a long-range exponent. This is a literature-motivated operational feature, not a claim that partial autocorrelation is new or unused by all current research. We ask whether it adds useful information beyond conventional gait summaries and lag-one memory. No causal or neural-control interpretation is identifiable.

The question and analysis plan were registered before computing this candidate (files/question-registration.json). A single documented SCI duplication check at 2026-11-25T12:16:55.350786+00:00 searched first1000 metadata entries for gait/Parkinson and partial autocorrelation/PACF/lag-two/second-order stride: zero matching entries. This limited check cannot exclude unpublished or older duplication. The catalog did not supply the question; no SCI publication supplied evidence or is cited gratuitously.

Both collections were extensively adaptively reused in prior different projects. The target is NOT untouched confirmation. No multiplicity-corrected discovery claim is made. Registration of this candidate does not undo prior adaptive dataset exposure. The result is a negative exploratory cross-collection test, not independent confirmatory proof.

## Data, rights and independence
Source: Gait in Parkinson’s Disease1.0.0[3], first _01 recording only, total force per foot sampled100Hz, usual self-paced walking. Target: Gait in Neurodegenerative Disease Database1.0.0[4,5], Parkinson and control .ts interval files only. Both are openly downloadable under Open Data Commons Attribution1.0[6,7] without special approval. We credit dataset contributors and PhysioNet[8]. Package redistributes original code and derived summaries, not raw force recordings. Pseudonymous public IDs are retained; we do not attempt identity linkage.

Immutable cache:199 files,167734732bytes. files/code/input-manifest.json records canonical URLs, versions, original acquisition dates, sizes and SHA256. Initial primary-literature metadata and abstract search preceded question selection. PubMed HTML returned placeholder pages; subsequent EuropePMC primary-abstract retrieval is documented in literature-retrieval.json. Current-project metadata/license snapshots were retrieved separately and their sizes/hashes are in files/evidence/provenance.json; no raw files were downloaded anew. Every input SHA256 is checked before both central runs. Raw data remain outside isolated computation outputs and the publication payload.

Source retained: n=162, Parkinson=92, controls=70, age range=36–86, men=96. Target retained: n=31, Parkinson=15, controls=16, age range=20–80, men=12. Source cache has165 first recordings, not all166 participants on the landing page; no missing recording is invented. Excluded for inadequate valid strides: ['JuCo02', 'JuCo03', 'JuPt05']. Source study/diagnosis counts: {('Ga', 0): 18, ('Ga', 1): 29, ('Ju', 0): 23, ('Ju', 1): 28, ('Si', 0): 29, ('Si', 1): 35}; target: {0: 16, 1: 15}. Unit is participant, not stride. Source filenames identify three studies Ga/Ju/Si; no repeated recordings per person enter the models.

These are separate collections/protocols, but participant independence cannot be certified: investigators overlap and cross-collection linkage is unavailable. There are 26 source-target pairs matching age, sex and diagnosis, listed in results.json; these demographic matches neither demonstrate nor rule out shared identity. We therefore claim cross-collection evaluation only, not guaranteed participant-independent transfer. Young controls in the target create major demographic domain shift, explicitly challenged by age restriction.

## Extraction and candidate
Source rising crossings of total-foot force20N in columns17/18 define consecutive strides; exactly one falling crossing within a stride defines swing as the time from that fall to next rise. No/multiple falls mark the stride invalid. Target row time column0, left/right stride durations columns1/2, swing durations columns3/4. Retain onset10–110s, stride0.5–2s, swing fraction0.15–0.6, finite inputs. Require at least30 valid strides in EACH foot. This may exclude disease-related extremes and is not a freezing analysis.

Within each foot, use only triples (s[n],s[n+1],s[n+2]) whose three ORIGINAL adjacent strides all pass filters; no missing-stride compression. Regress first and third durations separately on an intercept and middle duration; correlate the two residual vectors. Require10 valid triples; return0 for too few or zero residual SD. Average both feet equally. This finite-sample residual-correlation estimator, not the Yule-Walker estimator, is our lag-two partial autocorrelation. Zero fallback represents unidentifiable memory, not biological absence; weak stability further cautions interpretation.

Baseline predictors: age, male, mean_stride, stride_cv, mean_fraction, fraction_cv, asymmetry, raw_swing_cv, stride_memory, memory, raw_swing_asymmetry, stance_cv. Timing/CV values are averaged over feet; fraction=swing/stride. asymmetry=absolute left-right mean fraction difference divided by bilateral mean; raw_swing_asymmetry=absolute log ratio of mean swing durations. stride_memory and memory are lag-one correlations of stride and fraction using only adjacent valid pairs, with0 for degenerate pairs. stance_cv uses stride minus swing. These provide a deliberately richer comparator than mean stride/CV alone, incorporating conventional variability and simple temporal memory. Some auxiliary summaries in feature tables are not model inputs.

Both prediction models use source-only StandardScaler, L2 logistic C=1,max_iter2000, default class weighting and intercept, no tuning. The augmented model adds ONE candidate; no target fitting, imputation, scaling or feature selection. Primary endpoint is augmented-minus-baseline participant-mean binary logloss (negative better); AUC secondary. Source five-fold stratified CV seed20261125 refits all scaling within folds: {'baseline_logloss': 0.5307724244476717, 'augmented_logloss': 0.5389156989485095, 'baseline_auc': 0.838664596273292, 'augmented_auc': 0.8316770186335403}. This is internal validation, not the independent-collection test.

Uncertainty:2000 stratified target participant bootstraps conditional on source fits yield95% interval [-0.0017534749181726064, 0.028679422865466972];500 joint stratified source-refit/target bootstraps yield [-0.04561498233706755, 0.13744234141407524]. Percentile intervals, RNG20261125, fixed per-class counts. They exclude prevalence uncertainty, study clustering, measurement error, adaptive hypothesis selection and possible participant overlap. Neither interval demonstrates harm, equivalence or superiority. Predictions and joint draws are supplied.

## Results and all planned challenges
|Analysis|Source/target n|Baseline/augmented logloss|Delta logloss|Baseline/augmented AUC|
|---|---|---|---|---|
|primary|162/31|0.403740/0.415818|+0.012078|0.9042/0.8958|
|target45plus|162/20|0.403153/0.415211|+0.012058|0.8810/0.8810|
|age_sex_only|162/31|0.508070/0.505929|-0.002141|0.9042/0.9083|
|speed_adjusted|161/31|0.380989/0.419290|+0.038301|0.9292/0.9208|
|spline|162/31|0.390074/0.400045|+0.009971|0.9167/0.9208|
|force_threshold10|161/31|0.434131/0.458213|+0.024082|0.8958/0.8833|
|force_threshold30|162/31|0.443700/0.447723|+0.004024|0.9167/0.9167|
|leave_out_Ga|115/31|0.359691/0.388025|+0.028334|0.9333/0.9208|
|leave_out_Ju|111/31|0.406076/0.414397|+0.008320|0.8917/0.8875|
|leave_out_Si|98/31|0.503959/0.504081|+0.000121|0.8667/0.8667|
|dfa_comparator|162/31|0.429096/0.444414|+0.015317|0.9042/0.8958|
|rounded0.01|162/31|0.404610/0.416870|+0.012260|0.9042/0.8958|
|rounded0.02|162/31|0.404351/0.414660|+0.010308|0.9042/0.9000|

All primary full-gait comparator variants show nonnegative logloss changes. A weak age/sex-only comparator gives a small negative change; this does not establish added value beyond gait measures. Age restriction excludes young target participants but is still small and not age matching. Speed adjustment drops one missing-speed source participant and cannot control medication, severity, comorbidity or protocol. Leave-study-out analyses are diagnostics, not three independent replications. Fixed splines use3 knots,degree2 and are not tuned.

DFA comparator: add mean footwise linear-detrended fluctuation slope at scales4,6,8,12,16 strides on the longest contiguous valid run, requiring32 strides (zero otherwise). Integrate demeaned duration; nonoverlapping windows and least-squares log F versus log scale. This SHORT-scale proxy is not a validated long-range exponent; short recordings cannot exclude information in true long-range gait dynamics. Adding the candidate still worsens point logloss by 0.015317.

Within-record early10–60s versus late60–110s correlation of the candidate: source {'n': 128, 'pearson': 0.08759715506926861}, target {'n': 27, 'pearson': 0.045833669296288716}. Each window independently requires30 strides per foot, so complete-pair subset selection is explicit in CSVs. Near-zero correlations argue against treating this short-window estimate as stable physiology, but do not estimate between-visit repeatability. Rounding stride durations to10/20ms reprocesses all derived features, with swing fraction retained; it is a computational stress test, not a new device-validation experiment. It does not reverse the negative point result.

100 independent source/target candidate shuffles gave logloss-change quantiles2.5/50/97.5% [-0.03269964844584849, 0.0009036669675309686, 0.038628343763624166].100 source-label permutations gave augmented target AUC quantiles [0.1, 0.6041666666666666, 0.9146875]. Broad null distributions and demographic shift further limit inference; these diagnostics are not formal multiplicity-adjusted significance tests. Group means and all sensitivity rows are disclosed, not selected for favorable results.

## Author checks and reproducibility
Author self-checks (not peer review): explicit independent residual-formula comparison, positive-affine invariance, constant fallback, gap handling, time reversal, foot swap; unique participants, finite and bounded candidate, exact prediction alignment, recomputed logloss/AUC, source-CV coverage,500 joint draws and percentile recomputation. Fresh no-network containers reran the full main analysis, challenges and self-check. Deterministic CSV/JSON outputs matched exactly after removing runtime fields (files/evidence/clean-rerun-check.json). No original input was modified.

Run retrieval separately from analysis:
```
python3 files/code/retrieve.py /absolute/path/to/gait-swing-memory
docker build -t sci-gait-pacf2:reproduce files/code
bash files/code/reproduce-all.sh /absolute/path/to/gait-swing-memory /absolute/path/to/output
```
Input URLs/hashes are immutable manifest requirements; content changes cause failure rather than silent replacement. Docker base image digest and Python dependencies are pinned; actual versions in files/results/environment.json. Containers: nonroot, network none,1CPU,1024MiB RAM,64PIDs,read-only root,cap-drop ALL,no-new-privileges,64MiBtmpfs,900s timeout. Only declared code/raw-input/output directories mounted; no credentials, home directories or signing keys. Main runtime 13.71s; challenge runtime 14.21s; verbose resource logs retained. Single assigned worker; no nested agents/fleets. Host `python` alias failed once before any archive operation; ordinary repair to `python3` succeeded. Actual startup completed receipt differed from user-referenced handoff; actual result/request/receipt/source/evidence were archived intact and request cleared.

## Scope and future falsification
No demonstrated gain from this finite lag-two candidate, not proof that gait contains no unused information. N=31 target, unknown overlap, repeated adaptive dataset exposure, protocol mismatch, short windows, young controls and unmeasured medication/severity prevent clinical claims. Age itself discriminates strongly in the target, so high absolute AUC is not evidence of a clinically useful physiological test.

A future preregistered clinical validation could test the fixed model on genuinely new age/speed-matched Parkinson/control participants with blinded diagnosis, matched timing resolution, medication metadata and repeated walking sessions. It should compare logloss, calibration, AUC and repeatability against conventional and adequately estimated long-range markers, with a prespecified clinically meaningful superiority margin. Such data collection has not been performed here and is outside this existing-data authorization.

## References
1. Hausdorff et al. Fractal dynamics of human gait: stability of long-range correlations in stride interval fluctuations (1996). https://doi.org/10.1152/jappl.1996.80.5.1448
2. Hausdorff et al. Gait variability and basal ganglia disorders (1998). https://doi.org/10.1002/mds.870130310
3. Gait in Parkinson’s Disease1.0.0, Jeffrey Hausdorff. https://doi.org/10.13026/C24H3N
4. Gait in Neurodegenerative Disease Database1.0.0, Jeffrey Hausdorff. https://doi.org/10.13026/C27G6C
5. Hausdorff et al. Dynamic markers of altered gait rhythm in amyotrophic lateral sclerosis (2000). https://doi.org/10.1152/jappl.2000.88.6.2045
6. Source open-data license. https://physionet.org/content/gaitpdb/view-license/1.0.0/
7. Target open-data license. https://physionet.org/content/gaitndd/view-license/1.0.0/
8. Pollard et al. PhysioNet as a global platform for biomedical research (2026). https://doi.org/10.1038/s44360-026-00096-z
