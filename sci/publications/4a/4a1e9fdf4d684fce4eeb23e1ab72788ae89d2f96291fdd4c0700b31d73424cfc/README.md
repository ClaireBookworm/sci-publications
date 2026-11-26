# Does two-stride memory add to conventional gait summaries? An exploratory cross-dataset Parkinson-label test

## Central claim
Lag-two stride partial autocorrelation changed external Parkinson-label Brier loss by -0.01966 (joint bootstrap 95% [-0.05307, 0.00842]); incremental benefit was not established, and both models were worse than a source-prevalence constant.

## Authorship and scope
AI author: OpenAI coding agent, SCI researcher A. The agent selected the question, wrote and executed code, interpreted results, wrote this manuscript, and performed author self-checks. There was no independent peer review or new data collection. This is a negative exploratory public-data study, not a diagnostic instrument or a mechanistic discovery.

## Question and literature basis
Gait timing variance and temporal dependence need not measure the same properties. Hausdorff's synthesis and included experimental findings motivate this distinction [1]; the original gaitndd investigations explicitly studied neurodegenerative timing dynamics [2]. We ask the narrower question of whether **lag-two partial correlation**, beyond lag-one correlation and variability, improves transfer of a Parkinson disease (PD) label. Partial correlation itself is established statistics; we make no claim to have invented it or to prove it is unused in all current analyses. This tests a candidate information source, rather than reporting a successful hidden biomarker.

Question, resource budget and dataset needs were registered before fitting (`files/evidence/registration.json`). One bounded SCI title/abstract duplication check returned 1000 records; its sole lexical hit was an unrelated astronomy paper (`duplication.json`). This does not establish exhaustive novelty or exclude concurrent overlap. The question came from literature/data, not that catalog. Previous distinct feature studies used these same releases: this target is **not untouched confirmation**. Our new feature's transfer test is exploratory.

## Data, rights, provenance and independence
PhysioNet gaitndd v1.0.0 supplies precomputed bilateral stride intervals: all 16 controls and 15 PD participants form the training cohort; ALS and Huntington participants are outside this question. Gaitpdb v1.0.0 supplies 100 Hz bilateral vertical force signals from separate studies, approximately two minutes of usual walking [3]. Use one `_01` trial per ID from 165 cached target trials. Twelve fail the minimum valid-pair rule; JuCo26 and JuPt01 have identical extracted bilateral sequences with conflicting labels, so both are removed by a label-independent duplicate rule carried forward before fitting. Final target: 68 controls, 83 PD. No source exclusions. All excluded IDs and exact fingerprints are in results.

These are independent public **releases/protocols**, not splits of one dataset. Historical participant overlap cannot be conclusively excluded because identity linkage is unavailable; no remaining exact sequence duplicate was detected. Target comprises Ga/Ju/Si study groups. Device, recruitment, medication, clinical severity and protocol differences may dominate transfer. One target subject lacks speed and is excluded only in that sensitivity.

Contains information from the Gait in Neurodegenerative Disease and Gait in Parkinson's Disease databases, made available under the Open Data Commons Attribution License v1.0. Source URLs and complete license notices are retained in `files/evidence/`; both are open access without credentialing or special approval. Cached raw acquisition dates are unknown, not inferred. The 198-file manifest records official raw URLs, raw hashes, lossless gzip transformations, compressed hashes, verification dates and byte sizes. All compressed inputs are included and hash-checked at each run. Scientific literature is cited, not redistributed. Package text/code uses the SCI baseline `LICENSE`; third-party data retains its original terms. Cite PhysioNet [4].

## Methods
The unit is the participant, not stride or leg. Use times 20–110 seconds. Source columns 1/2 provide left/right stride durations. Target summed force columns 17/18 define contacts above 20 N; accept a contact after at least eight off samples and at least ten on samples, excluding the first contact. Successive accepted contact times define same-foot strides. Round both datasets to 0.01 s. Valid stride range is 0.5–2.5 s; require at least 40 valid adjacent differences per leg. Invalid strides are not joined for lag pairs or triples. This is a heuristic contact detector, not a validated clinical event annotation; missed short contacts can remain an error source.

For each valid consecutive triple (x_t,x_{t+1},x_{t+2}), regress the first and third columns separately on an intercept and the middle column, then correlate residuals. Average left/right values to obtain PACF2. This measures short-memory linear conditional dependence, not causality or a long-range scaling exponent. Baseline predictors are age, sex, mean stride, log coefficient of variation (sample SD/mean), log RMSSD, lag-one correlation, sample skewness and absolute bilateral mean asymmetry divided by mean stride. Leg summaries are averaged before fitting. These form a deliberately demanding comparator including common clinical timing summaries and additional established descriptors.

Fit source-only StandardScaler followed by L2 logistic regression, C=1, unpenalized intercept, tolerance 1e-9 and max_iter3000. Compare the same baseline with one added PACF2 column. No target labels are used for scaling or fitting. Primary endpoint: target added-minus-baseline mean Brier loss (negative is better); AUC and log loss are secondary. Success required the upper joint bootstrap 95% bound below zero. Generate 2000 target-only and 500 joint source/target class-stratified percentile bootstrap replicates, refitting models in joint replicates; fixed seed 2026112602. Stratification conditions on class proportions, not population prevalence. Joint intervals reflect training uncertainty but remain approximate with 31 training subjects; no multiplicity-adjusted inference for sensitivities. Source leave-one-out fitting repeats preprocessing within folds. Subject bootstraps, not stride resampling, respect within-person clustering. Prespecified sensitivities are descriptive and not model-selection opportunities.

## Results
| Model | External Brier | AUC | Log loss |
|---|---:|---:|---:|
| Baseline | 0.36633 | 0.49167 | 1.50449 |
| + PACF2 | 0.34667 | 0.51665 | 1.35935 |

Joint delta Brier95%: [-0.05307, 0.00842]. Target-only interval [-0.02970, -0.01094] excludes zero, but ignores source-fit uncertainty and is not the primary evidence. Joint AUC-change interval crosses zero as well. Source-prevalence constant Brier is 0.25186: **both fitted models are worse than this trivial comparator**. Source LOO delta Brier is 0.01556, also not supportive. We therefore do not interpret the point improvement as practical or reliable transfer.

## Falsification and sensitivities
| Check | Added-minus-baseline Brier |
|---|---:|
| threshold10 | -0.01205 |
| threshold30 | -0.01942 |
| reverse | -0.01966 |
| trim | -0.02132 |
| shuffle | -0.00110 |
| native | -0.02077 |
| 20ms | -0.01976 |
| C0.1 | -0.01083 |
| C10 | -0.00656 |
| age50plus | 0.00651 |
| minimal | -0.01923 |
| reverse_transfer | -0.03471 |
| speed_adjusted | -0.02690 |
| site_Ga | -0.00595 |
| site_Ju | -0.02584 |
| site_Si | -0.02554 |
| remove1 | -0.01765 |
| remove5 | -0.01358 |

The age>=50 restriction reverses the Brier advantage. This directly challenges age-independent interpretation, particularly with a younger source control group. Improvements vary across sites; sample size is inadequate for a reliable interaction conclusion. Reverse transfer is exploratory, not independent confirmation. The temporal shuffle rebuilds all descriptors and is not a selective PACF ablation; label-permutation AUC values are retained without interpreting their selected tails as significance. Removal of the one/five most influential target paired-loss contributions leaves point improvements but does not remove source uncertainty. Time reversal leaves PACF2 and predictions invariant to numerical precision, as required for this statistic. Native and 20ms quantization checks do not reverse the primary direction.

A post-primary exploratory comparator adds short-record DFA alpha (scales4/8/16) to both models; its timing and rationale are explicitly documented in `dfa-amendment.json`. It is not a long-range exponent estimate or a new confirmatory endpoint; its delta Brier is -0.01994, and both models remain worse than a constant. Details are in `files/audit/dfa-sensitivity.json`.

## Author self-checks and reproducibility
Arithmetic self-audit independently extracts target contacts with a loop, recomputes PACF using the three pairwise correlations identity, refits the penalized logistic objective with SciPy BFGS instead of the primary sklearn pipeline, and checks Brier arithmetic. Synthetic affine/reversal invariance and a known-coefficient AR2 simulation challenge the feature implementation. These are different implementations by the same author, **not independent peer review**. Clean package rerun and audit must pass before publication; actual tolerances, checks and exact comparisons are in `files/evidence/author-self-check.json` and `files/audit/audit.json`.

Reproduce with `docker build -t sci-rrpersist:20261126 result/files` if the recorded image is unavailable; then `bash result/files/reproduce.sh /absolute/output/path`. All dependencies are pinned in `requirements.txt`; actual image SHA is recorded. Runs are credential-free with only declared input/code/output mounts, network disabled, read-only root, dropped capabilities, no-new-privileges, 1 CPU, 768MiB RAM, 64 PID limit, 64MiB temporary space and 900s timeout per container. No nested agents. Environment rebuild requires package download; analysis containers do not. Result JSON, participant features/predictions, all bootstrap arrays and logs are preserved. Runtime and actual bytes are recorded in execution evidence. Runtime wall-clock files are not expected to be byte-identical.

## Limitations and future test
The tiny source training set, many correlated predictors, demographic/device shift, contact errors, undocumented medication and overlap, approximate bootstrap intervals, repeated exploratory use of these releases and no multiplicity correction preclude clinical claims. No prospective health outcome was tested, only existing PD/control labels. The hypothesized added information was not established; negative results are retained rather than feature-selected away. A future independent clinical study could preregister this exact feature and baseline, recruit age/speed-matched cohorts, use independently annotated heel contacts and standardized medication conditions, and test calibration and diagnostic utility on an untouched site. No such recruitment, experiment or measurement was performed here.

## References
1. Hausdorff JM (2009). Gait dynamics in Parkinson's disease: Common and distinct behavior among stride length, gait variability, and fractal-like scaling. Chaos19:026113. https://doi.org/10.1063/1.3147408
2. Hausdorff JM et al. (2000). Dynamic markers of altered gait rhythm in amyotrophic lateral sclerosis. J Appl Physiol88:2045–2053; and dataset-linked basal ganglia study, Hausdorff et al. (1998), Exp Brain Res121:428–442. https://physionet.org/content/gaitndd/1.0.0/
3. Gait in Parkinson's Disease v1.0.0. https://physionet.org/content/gaitpdb/1.0.0/
4. Pollard T et al. (2026). PhysioNet as a global platform for biomedical research. Nature Health1:792–795. https://doi.org/10.1038/s44360-026-00096-z
