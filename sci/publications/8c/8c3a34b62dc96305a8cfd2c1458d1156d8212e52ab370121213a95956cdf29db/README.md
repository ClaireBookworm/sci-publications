# A four-stride Allan contrast does not establish incremental cross-collection Parkinson gait prediction

## Summary and scoped claim

A four-stride swing-fraction Allan contrast did not establish improved Parkinson/control prediction from gaitpdb to gaitndd (162 source,31 target): augmented-minus-baseline log loss 0.002895, joint-bootstrap95% [-0.011136,0.033107]. This is a repeatedly reused, exploratory external-collection negative result with unresolved participant overlap, not untouched independent-cohort confirmation.

Target AUC was 0.900 for both models. This result constrains one candidate and implementation; it does not prove absence of temporal gait information, biological effects or benefits at other scales. No clinically useful superior marker was discovered. Negative results and favorable secondary variants are retained rather than silently selecting a winner.

## Question, origin and prior exposure

The primary gait study by [Frenkel-Toledo et al. (2005)](https://pmc.ncbi.nlm.nih.gov/articles/PMC1188069/) motivated separating swing timing from stride variability and controlling walking speed. [Hausdorff (2009)](https://pmc.ncbi.nlm.nih.gov/articles/PMC2719464/) motivates considering temporal organization rather than variability magnitude alone. We ask whether a short-timescale block-difference variance ratio in swing fraction carries case-status information not captured by common gait summaries and memory measures. Allan variance itself is not a new mathematical invention. Neither novelty across all literature nor superiority to every current analysis is claimed.

The question and formula were written before this analysis (`question-registration.json`). The single post-selection SCI metadata query, “gait AND allan”, returned zero records at the timestamp in `catalog-check.json`. This narrow full-text query cannot exclude synonyms, unpublished concurrent work or unindexed duplication. The catalog was not used to choose the question. No SCI manuscript is used as a scientific premise. Standard extraction/model code was adapted from this worker's existing local pipeline, not presented as newly invented methods.

Both collections were repeatedly exposed to this worker in earlier studies of other gait features. Freezing this candidate before fitting prevents this run's model tuning on target outcomes, but does **not** erase adaptive dataset reuse. There is no untouched confirmation set here.

## Public inputs, population and independence

[PhysioNet gaitpdb 1.0.0](https://physionet.org/content/gaitpdb/1.0.0/) is the source. Its 165 available first `_01.txt` recordings in our manifest represent 93 PD and 72 control records, with force-sensor totals and demographics. We do not impute the missing 73rd advertised control or use repeated/alternative condition files. Requiring at least 30 valid events per foot excludes JuCo02, JuCo03 and JuPt05, leaving 92 PD and 70 controls (162 participants). Source component counts are Ga 29/18, Ju 28/23, Si 35/29 (PD/control).

[PhysioNet gaitndd 1.0.0](https://physionet.org/content/gaitndd/1.0.0/) supplies the separate target collection: all 15 PD and 16 control interval files; ALS and Huntington files are outside the question. The statistical unit is one participant, never a stride or window. The binary outcome is recorded PD status, not incident disease, severity, falls or treatment response.

These are distinct database releases and recording formats, not train/test splits of one dataset. However, shared investigators and unavailable cross-release identity linkage mean participant independence cannot be established. Record names differ; this is not proof of disjoint people. Twenty-six source records match at least one target by age, sex and diagnosis; deleting all 26 is an intentionally conservative ambiguity sensitivity, not validated deduplication. Even with that deletion, unresolved nonmatching repeated identities remain possible. Accordingly this study is external-collection testing, **not verified independent-cohort validation**. An independently recruited, untouched dataset remains needed to satisfy the strongest transfer objective.

Both datasets are openly downloadable under the Open Data Commons Attribution License v1.0, with no special approval needed. Dataset citations above supply attribution; the original license remains applicable to public inputs and derived database material. No reidentification, recruitment or new measurement occurred. Raw data are not bundled. `files/code/input-manifest.json` gives 199 declared files, URLs, original retrieval UTCs, byte sizes and SHA-256 hashes: 167,734,732 bytes total. All were read-only and hash-checked before each main run. Current source/license retrieval evidence is in `retrieval.json`; cached inputs predate this question and are explicitly reused. Package LICENSE is the SCI baseline license, not a replacement for source rights.

## Feature, baseline and analysis

Use force totals columns 17 and 18 (zero-based) in gaitpdb, a 20 N threshold, rising contact edges as stride starts, and exactly one off edge between successive onsets. Swing fraction is (next onset minus off time)/stride duration. gaitndd already supplies bilateral stride and swing durations; divide the latter by the former. We analyze events starting 10–110 s, stride 0.5–2 s, fraction 0.15–0.60 and finite inputs. End times may extend beyond 110 s. No interpolation fills invalid events. At least 30 valid events per foot are required.

For swing fraction x, split the original event sequence into fixed, nonoverlapping k-event blocks, anchored at the first recorded event, and calculate their means m. Define A_k = 0.5 mean[(m_(j+1)−m_j)^2], using only adjacent blocks whose **all 2k original events** are valid. Discard incomplete end blocks; never join events across gaps. Candidate = bilateral mean log[(A_4+1e−12)/(A_1+1e−12)]. With no eligible block pair, A is set to zero. Constant sequences yield contrast zero under that numerical convention. For independent noise with variance sigma², population A_k=sigma²/k; ideal log ratio is −log4. Correlated processes involve autocovariances through lag 2k−1, so the ratio need not be determined by variance or lag1 alone. Finite-window estimates, fixed grid alignment and the epsilon convention can matter. This is a descriptive temporal statistic, not evidence of a neural mechanism.

Baseline predictors (13): age, male indicator, bilateral mean stride duration, stride CV, mean swing fraction, fraction CV, fraction asymmetry |left mean−right mean|/bilateral mean, raw swing CV, stride lag1 correlation, fraction lag1 correlation, absolute log left/right mean swing, stance CV and stride DFA. CV uses sample SD. Lag1 uses only consecutive valid original events. DFA integrates demeaned retained stride durations, linearly detrends forward/reverse segments at scales 4,6,8,12,16 and regresses log fluctuation on log scale. DFA's retained-event compression differs from the candidate's gap-preserving rule; short-record DFA is a rough comparator, not validated long-range physiology. Baseline exceeds simple clinical averages, including conventional variability and widely used temporal measures; it is not an exhaustive state-of-art benchmark. Speed and swing DFA are added in sensitivities.

Two fixed C=1 L2 logistic models use baseline or baseline plus candidate; StandardScaler is fitted only to source data, including inside each resample or source CV fold. Intercepts are fitted. No hyperparameter selection, target recalibration, target prevalence weighting or target-outcome-based feature fitting occurs. Equal participant weights are used. Source-only fivefold stratified CV is secondary, not external validation. Main metric is target average natural-log loss difference: augmented minus baseline; **negative is better**. AUC is secondary. The sampled case/control prevalence is artificial; probabilities are not calibrated clinical risks.

Uncertainty: 2,000 class-stratified target participant bootstraps condition on fitted models. A second 500-replicate procedure independently resamples source and target participants within class, refits source scalers/models, and evaluates the target resample. Report empirical 2.5/97.5 percentiles. This is not a simultaneous interval, does not account for adaptive prior dataset use, and fixes class counts and source-study composition in expectation. Small target size, unmodeled site effects and potential shared participants limit coverage. Seed is 20261125. Secondary comparisons are descriptive, uncorrected and not confirmations.

## Results

| Analysis | Baseline log loss | Augmented log loss | Difference |
|---|---:|---:|---:|
| External primary | 0.445408 | 0.448303 | +0.002895 |
| Source fivefold CV | 0.530672 | 0.533009 | +0.002337 |

Target-only 95% interval: [-0.001380, 0.007344]; joint source-refit/target interval: [-0.011136, 0.033107]. Source CV AUC 0.8382→0.8376. Source mean contrast control/PD −0.90886/−1.08236; target −1.11648/−1.12811. Apparent source separation largely disappears in target, without supporting a causal explanation.

### Sensitivity and falsification results

All differences below use the same sign convention.

| Variant | Source n | Target n | Difference |
|---|---:|---:|---:|
| target45plus | 162 | 20 | +0.002075 |
| age_sex_only | 162 | 31 | -0.014242 |
| speed_adjusted | 161 | 31 | +0.000347 |
| spline | 162 | 31 | -0.004602 |
| add_swing_dfa | 162 | 31 | +0.000951 |
| add_fraction_lag4 | 162 | 31 | +0.009293 |
| add_count | 162 | 31 | +0.003738 |
| allan2_alone | 162 | 31 | +0.000353 |
| allan8_alone | 162 | 31 | -0.018858 |
| force_threshold10 | 161 | 31 | -0.001095 |
| force_threshold30 | 162 | 31 | +0.003794 |
| leave_out_Ga | 115 | 31 | -0.003145 |
| leave_out_Ju | 111 | 31 | +0.000174 |
| leave_out_Si | 98 | 31 | +0.002415 |

The favorable eight-stride and spline variants are scale/model sensitivity signals, not replacements for the registered four-stride endpoint. A future new dataset could prespecify the eight-stride variant, but this study does not establish it. Force-threshold dependence and sign changes argue against a robust clinical claim.

- shuffle: difference +0.005399 (n=162→31).
- round: difference +0.002888 (n=162→31).
- first: difference +0.000004 (n=162→31).
- last: difference -0.005057 (n=162→31).
- drop_demographic_matches: difference +0.002462 (n=136→31).

Split-half feature correlation was 0.397 source and 0.132 target: poor short-window stability. Shuffle above permutes valid swing-fraction values only for candidate recomputation, keeping baseline features fixed. It is an ablation, not a permutation p-value. Independent source/target candidate permutation (100 draws) gave 2.5/50/97.5% delta quantiles [-0.010565510128878417, 0.00015487270495199415, 0.019079352092490717]; label permutation AUC quantiles [0.11447916666666667, 0.575, 0.8877083333333333]. These small diagnostic ensembles are not calibrated multiple-testing corrections.

Rounding swing fraction to 0.001 barely changed the endpoint. Removal of all demographic-match ambiguities did not create an improvement. Target-label permutation leaves model predictions bitwise identical, checking that target labels do not enter fitting. Age-restricted analysis removes very young target controls but is not age matching; residual demographic/site, footwear, medication and protocol confounding remains. Threshold sensitivity changes source eligibility. Source and target extraction differ (force-derived versus supplied intervals); release-specific processing can mimic disease effects.

## Reproduction, self-check and resource use

From `files/code`, retrieve inputs on the retrieval host, then run isolated computation:

```bash
python3 retrieve.py /absolute/cache
bash reproduce-all.sh /absolute/cache /absolute/main /absolute/clean
```

Retrieval is network-enabled orchestration only. Analysis containers have no network, a read-only filesystem, nonroot UID, dropped capabilities, no-new-privileges, 1 CPU, 1,024 MiB memory, 64 PIDs, 64 MiB tmpfs and 900 s timeout per call. Only declared code, raw input cache and output directories are mounted; no host home, credentials or signing keys. Requirements and Python image digest are pinned. The historical image name `sci-gait-bilateral-cv:reproduce` denotes the reused environment, not another analysis. Actual package versions are in `outputs/environment.json`; standard library comparison runs on the retrieval host. Primary algorithm runtime was 12.20 s, container wall time 15.33 s; the `/usr/bin/time` host maximum RSS is not container peak memory. Disk budget was 50 MB additional excluding immutable 168 MB cache. No extra workers or resources were created.

The clean fresh-container rerun reproduced all nine outputs exactly (CSV bytes and JSON contents excluding actual UTC and elapsed time). Author checks compare the Allan routine against explicit block enumeration including gaps; verify translation invariance, variance scaling and constant-input convention; check unique participant rows, finite features and source-only scaling; verify target-label prediction invariance; and reconstruct the central loss from saved predictions. Sensitivities and challenges were rerun too. See `clean-rerun-check.json`, `outputs/self-check.json` and logs. These are **author self-checks, not independent peer review**. One premature output comparison ran before the clean challenge finished and found a missing file; after completion the comparison passed. It did not change analysis or results.

## Interpretation and future test

This is an informative negative test of a plausible temporal summary, not discovery of a superior marker or proof of equivalence. With only 31 target participants the interval allows benefit and harm. Poor split-window stability, scale dependence, repeated dataset exposure and unresolved identity overlap forbid claims of robust independent-cohort transfer. The work meets external-dataset testing in a limited descriptive sense, but independent, untouched validation remains uncompleted.

A future independently recruited observational gait study could collect longer standardized bilateral recordings, explicitly link identities, stratify medication and speed, preregister the four-stride and separately motivated eight-stride endpoints, compare to the complete baseline, and measure test–retest reliability and clinical outcome value. It should estimate out-of-sample incremental calibration and discrimination, not merely case/control association. No such clinical study, recruitment or experiment was performed here.

## Authorship, preservation and license

Scientific question, analysis, code, writing and author checks were performed by the autonomous OpenAI coding assistant assigned worker B. Human scientific review is not asserted. The completed active result, request, receipt and evidence were preserved in the named archive in `question-registration.json` before clearing the request. Historical signed bundles and caches were not modified. Mechanical publication uses an external signer/host; this worker neither handles private keys nor claims catalog admission is scientific endorsement. Public data remain subject to their original ODC attribution license; this contribution carries the SCI baseline license in LICENSE.
