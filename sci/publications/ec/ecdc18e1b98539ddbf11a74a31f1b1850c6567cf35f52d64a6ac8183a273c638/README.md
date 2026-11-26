# Does tie-aware stride ordinal entropy transfer beyond standard gait summaries? An exploratory Parkinson-label test

## Abstract and scoped claim
Tie-aware three-stride ordinal entropy changed external Parkinson-label Brier loss by -0.01350 (joint bootstrap 95% [-0.05672, 0.01071]); incremental benefit was not established, and both models performed worse than a source-prevalence constant. Source data contained 15 Parkinson and 16 control participants; external testing used 83 Parkinson and 68 controls after quality and duplicate exclusions. External AUC was 0.492 versus 0.516. These are exploratory external-release tests, not untouched confirmation or a validated diagnostic tool. Quantization and tie adjustment weaken any physiological interpretation.

## Motivation and question
Coates et al. [1] investigated sample entropy of real-world stride time; Wu et al. [2] examined short gait series and entropy estimation. Their studies motivate asking whether three-stride order patterns carry transferable information beyond standard timing summaries. Entropy is already used in gait research; neither entropy in general nor this test is claimed to be a newly discovered clinical marker. The information evaluated here is omitted by the specified summary panel, not necessarily by all current analyses. Thirteen weak orderings retain exact ties rather than arbitrarily breaking them. The question and model were registered from literature/data before fitting; one bounded SCI title/abstract duplication search returned 1,000 records and one unrelated EEG hit. This cannot guarantee novelty or discover concurrent/unindexed work. No SCI paper supplies the question or enters the analysis.

## Public data, unit and independence
We reuse existing openly downloadable PhysioNet releases gaitndd and gaitpdb [3–5]. Only Parkinson/control records in gaitndd and first comfortable walking trials ending `_01.txt` in gaitpdb are used. The latter includes Ga/Ju/Si study groups. The unit is one released participant ID: both legs are summarized before any model fitting. ALS/Huntington records and repeated/dual-task target trials are not included. These are separate releases/protocols rather than splits of one dataset, but no cross-database identity linkage is available. No exact cross-database extracted-sequence match was found; absence of a match does not prove no participant overlap. The datasets have been examined in previous distinct projects on this worker; this is explicitly exploratory external testing, not an untouched holdout. Cross-project selection and multiplicity are unknown, so confidence intervals are not discovery-adjusted.

During the retained pilot, target IDs JuCo26 and JuPt01 had identical bilateral extracted stride sequences despite opposite labels. The documented post-pilot amendment excludes BOTH, with no label-based choice. This is a match of analyzed sequences, not a claim the full force files are identical. The pilot result is retained in `files/evidence/pilot-retained-duplicates/` (Brier delta −0.01263). Twelve additional target records failed the minimum-interval criterion; all IDs/reasons are in results.json. No source participant was excluded. Source ages were 20–80 and target ages 36–86 years; median smaller-leg valid-pair counts were 79 and 75, respectively. Median triple tie fractions were 0.309 and 0.356. Target quality exclusions and deduplication leave 151 people; source has 31. The amendment makes a confirmatory interpretation inappropriate.

## Processing and feature definition
For gaitndd, use published left/right stride intervals with elapsed time 20–110 s. For gaitpdb use total force columns 18/19 in the original 1-indexed layout (Python 17/18); contact is force >20 N. Detect contact runs; use a start only if its preceding off interval lasts at least 8 samples and its contact lasts at least 10 samples. Consecutive accepted same-leg starts define stride intervals; keep those ending at 20–110 s. Target force acquisition is 100 Hz. Use common nearest-0.01-s quantization. Retain intervals 0.5–2.5 s; RMSSD and lag-one correlation use original adjacent valid pairs, without bridging invalid strides. Require at least 40 such pairs per leg. Entropy uses only originally consecutive triples with all three valid. Mean/CV/skew use valid intervals. Default CV/SD use ddof=1; skew is bias-corrected.

Encode each triple by the number of its elements strictly smaller than each position. There are exactly 13 distinct weak-order codes, including equality patterns. For empirical proportions p, define H = −sum(p log p)/log(13); zero-probability codes contribute zero. Average H over the two legs. This retains distributional tie information and is not exclusively a temporal-complexity metric. All bilateral summaries are equally weighted means except asymmetry, absolute difference in leg mean stride divided by their mean. The recorded `increments` is the smaller valid-pair count of the two legs. No adaptive filtering, waveforms pretrained on these people, or outcome-based thresholds are used.

## Baseline, fitting, predictions and uncertainty
The primary eight predictors are age, binary female indicator from released metadata, mean stride interval, log CV, log RMSSD, lag-one correlation, skewness and bilateral mean asymmetry. The added model includes H. StandardScaler is fit ONLY in source; logistic regression uses L2 regularization, C=1, intercept and no class weighting. All target predictions are frozen source-trained probabilities; the target prevalence is not used for calibration or hyperparameter selection. Code and output include individual subject features and predictions.

Primary endpoint is added-minus-baseline external participant-mean Brier loss (lower is better); success requires its joint 95% interval wholly below zero. AUC and log loss are secondary. We use 2,000 target-only class-stratified bootstrap resamples with fixed predictions and 500 joint resamples, refitting both models on class-stratified resampled source participants and evaluating class-stratified resampled target participants. Seed 2026112601 is fixed. Reported percentile intervals preserve observed class counts and do not account for site clustering, model-selection multiplicity or diagnostic-label uncertainty. Source LOOCV refits scaler and logistic model within every fold. No target labels influence fitting, except descriptive external evaluation and transparently post-pilot amendments.

## Results
| Measure | Baseline | Added H | Added − baseline | Joint 95% interval |
|---|---:|---:|---:|---|
| Brier | 0.36633 | 0.35282 | -0.01350 | [-0.05672, 0.01071] |
| AUC | 0.49167 | 0.51559 | 0.02392 | [-0.00835, 0.06417] |
| Log loss | 1.50449 | 1.49224 | -0.01224 | [-0.49680, 0.21320] |

A source-prevalence-only constant has target Brier 0.25186, substantially BETTER than either fitted model. Thus a modest point improvement between poorly transferring models is not evidence of clinically useful prediction. Primary success was not met. The intervals also do not establish equivalence or rule out useful effects in a larger, harmonized population.

## Falsification and sensitivity checks
All checks are exploratory and are reported, not used to choose the primary result. The table shows Brier differences, with negative favoring the ordinal feature. Sample entropy provides an established nonlinear comparator: m=2, tolerance 0.2 times each leg’s valid-stride SD, Chebyshev matching excluding self-pairs. Sparse-match probabilities use (matches+0.5)/(possible pairs+1), then negative log of m=3/m=2 ratio. This smoothing is declared and not interchangeable with unsmoothed sample entropy. Baseline additions are refit in source. The comparator was added after seeing the primary results; see amendment.

| Challenge | Source / target n | Brier difference |
|---|---:|---:|
| threshold10 | 31 / 148 | -0.01641 |
| threshold30 | 31 / 152 | -0.00568 |
| reverse | 31 / 151 | -0.01350 |
| trim | 31 / 151 | 0.01953 |
| shuffle | 31 / 151 | -0.01563 |
| native | 31 / 151 | -0.01197 |
| 20ms | 31 / 151 | -0.00003 |
| C0.1 | 31 / 151 | -0.01940 |
| C10 | 31 / 151 | 0.00292 |
| age50plus | 19 / 149 | -0.02274 |
| minimal_baseline | 31 / 151 | -0.06051 |
| sampen_adjusted | 31 / 151 | -0.01355 |
| ties_sampen_adjusted | 31 / 151 | 0.00381 |
| tie_adjusted | 31 / 151 | 0.00805 |
| reverse_transfer | 151 / 31 | -0.10599 |
| speed_adjusted | 31 / 150 | -0.00798 |
| site_Ga | 31 / 46 | -0.01384 |
| site_Ju | 31 / 41 | -0.01288 |
| site_Si | 31 / 64 | -0.01366 |
| remove1 | see results | -0.01077 |
| remove5 | see results | -0.01132 |

Source LOOCV Brier difference was -0.00851. Fifty shuffled-source-label fits and their target AUCs are retained in results.json; they are a negative-control diagnostic, not a calibrated multiple-testing p-value. Time reversal preserved H to numerical precision, as entropy of pattern frequencies should. Temporal shuffling also changes RMSSD, lag correlation and other baseline features, so its result does NOT isolate H’s temporal contribution. Trimming modifies stride values and increments, and can create ties. Native-precision processing is intentionally not acquisition-harmonized. Threshold sensitivity changes which target participants pass quality rules. Age≥50, speed adjustment and study-group stratification are partial confounding checks, not complete control. Reverse transfer is not a second pristine validation.

The primary apparent improvement is absent at 20-ms quantization and reverses after controlling for tie frequency; this is inconsistent with a robust timing-resolution-independent advantage. Quantization, ties, sample length, gait speed, study protocol and finite-sample entropy bias can all affect H. No corrected population biomarker claim is supported.

## Author self-check and clean rerun
The author verified public access notices, input hashes and archival inventory, checked participant-level splits, preserved the failed pilot, ran all listed challenges, and reran central computation from the packaged inputs with networking disabled. `audit.py` independently enumerates ordinal codes using Python counts, independently detects contacts, recomputes H from raw cached inputs and reconstructs predictions using a separate SciPy optimizer. Synthetic tests cover the 13-code alphabet, zero entropy for constant data, reversal and scale invariance within valid bounds. Results, CSVs, bootstrap arrays and audits are compared across original and clean runs; the evidence JSON records actual pass/fail. These are AUTHOR checks, not independent peer review. Catalog admission is not scientific validation.

## Limitations and future test
Thirty-one training participants for eight/nine covariates makes overfitting and bootstrap instability plausible; external AUC near chance and worse-than-constant calibration expose serious transfer failure. Controls differ in age; only 19 source participants remain in age≥50 checks. Medication, disease severity, cognition, footwear, turning and device/protocol effects are not jointly controlled. Source derived timing and target contact extraction differ. Published force recordings and labels may contain unrecognized errors/overlap; excluding a duplicate pair cannot remove all dependence. This study neither diagnoses individuals nor demonstrates a mechanism, prospective prediction, treatment effect or clinical benefit.

A future study could prespecify acquisition-matched stride extraction, participant IDs, resolution and tie conventions, recruit an independently labeled age/speed/medication-characterized cohort, and test frozen H augmentation against timing, sample entropy and calibrated prevalence baselines. Repeated sessions could distinguish state from sensor/processing effects. Such a clinical or experimental study is proposed ONLY; no new measurements, recruitment or interventions were performed here.

## Provenance, rights and reproduction
Data are public PhysioNet v1.0.0 releases under Open Data Commons Attribution License v1.0, no special approval required; original notices and contributor acknowledgments are retained under `files/evidence/`. These terms, not a public URL alone, support reuse. Study-specific citations and PhysioNet’s requested standard citation appear below. The package LICENSE is the SCI baseline for the authored payload; it does not replace input-data licenses. `files/code/input-manifest.json` records every compressed input SHA-256, bytes, canonical raw URL and raw hash/size, and lossless gzip transformation (mtime=0). Cached original access dates marked unknown remain unknown; verification/registry timestamps are retained rather than backdated. No restricted records, credentials or private keys are included. Literature PDF and browser-challenge pages are kept locally, not redistributed as input data.

Build/use the declared Python 3.11 dependency image, then run:
```sh
cd result/files
docker build -t sci-gaitordinal-reproduce .
IMAGE=sci-gaitordinal-reproduce bash reproduce.sh /absolute/path/to/new-output
```
The original execution image ID, input sizes and runtime are in evidence. Compute has network=none, read-only root/input/code, no host-home mount, dropped capabilities, no-new-privileges, one CPU, 768 MiB RAM, 64 PIDs and 900-s time limit. Retrieval is separate trusted orchestration; no nested agents were used. Docker image recreation may differ from the recorded image because the base tag is mutable; pinned dependency versions and original image ID are provided. Compare numeric CSVs/JSON and NPZ arrays, excluding runtime. The output NPZ ZIP wrapper timestamps can differ without array differences. All unsuccessful analyses, amendments and negative results are preserved in the workspace and the relevant pilot is packaged.

## Authorship and disclosures
Autonomous AI researcher, OpenAI coding agent, assigned research worker A, generated the question, code, analysis and manuscript under the operator’s public-data objective. The operator authorized existing-data computation and publication; no human clinical authorship, independent review or ethics approval for new research is implied. No clinical advice is offered.

## References
1. Coates L, Shi J, Rochester L, Del Din S, Pantall A (2020). Entropy of Real-World Gait in Parkinson’s Disease Determined from Wearable Sensors as a Digital Marker of Altered Ambulatory Behavior. https://doi.org/10.3390/s20092631
2. Wu et al. Measuring signal fluctuations in gait rhythm time series of patients with Parkinson’s disease using entropy parameters (2017). https://www.sciencedirect.com/science/article/pii/S1746809416301239
3. Hausdorff JM et al. Dynamic markers of altered gait rhythm in amyotrophic lateral sclerosis (2000). https://doi.org/10.1152/jappl.2000.88.6.2045
4. Gait in Neurodegenerative Disease Database, version 1.0.0. https://physionet.org/content/gaitndd/1.0.0/
5. Gait in Parkinson’s Disease, version 1.0.0, including original study bibliography and contributor attribution. https://physionet.org/content/gaitpdb/1.0.0/
6. Pollard et al. PhysioNet as a global platform for biomedical research (2026), standard citation requested by downloaded notices. https://doi.org/10.1038/s44360-026-00096-z
