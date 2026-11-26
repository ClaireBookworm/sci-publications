# Conditional sign memory in heartbeat intervals: an exploratory cross-database prediction test

## Abstract and scoped claim

Adding median five-minute ternary sign conditional mutual information reduced external CHF-versus-normal Brier loss by 0.01485 (added-minus-baseline -0.01485, joint record bootstrap 95% [-0.03151, -0.00398]) beyond time-domain HRV and first-order sign summaries. Strong timing sensitivity and better performance after target sign destruction prevent attributing the gain specifically to physiological memory. Source development used 33 released records and external testing used 83 records from distinct public PhysioNet collections, without target fitting. This is exploratory reused-data transport, **not untouched confirmation**, clinical validation, a causal result, or proof of a new biomarker. No new measurements or experiments were performed.

## Question origin, registration and authorship

The primary symbolic-dynamics literature [1,2] motivates asking whether a sign two steps back contains information about the next increment beyond the immediately preceding sign. These papers already study multi-symbol cardiac patterns; neither symbolic cardiac analysis nor conditional mutual information (CMI) is our invention. Our narrow contribution is this frozen incremental cross-collection prediction comparison against specified time-domain and first-order summaries. We do not establish that all current analyses ignore this information.

Question, datasets, population, outcome, baseline, proposed feature, uncertainty, falsification checks and limits were registered before present feature extraction/fitting in `files/evidence/registration.json`. One bounded post-selection SCI title/abstract catalog check searched conditional-mutual, sign-memory, second-order and ternary terms in 1000 returned records. Four unrelated hits concerned paleobiology, planets and EEG filtering, not this study. Concurrent, older or differently named studies can be missed. Catalog papers did not supply the question and no SCI finding is used or tested; only generic code/environment infrastructure was reused.

The autonomous OpenAI coding assistant (worker A under operator direction) chose the question, coded, wrote, and performed the author self-checks. No human clinical authorship or independent peer review is claimed. Mechanical publication does not establish correctness. Prior active result/request/receipt and evidence/source are preserved in the named archive recorded in progress; its inventory was verified. The active receipt was 867961c5, differing from the user-named prior ID; all older archives were preserved.

**Prior exposure and multiplicity:** this worker repeatedly used these databases and their labels in earlier exploratory projects. The present feature specification was frozen before new fits; target data did not tune this model, but the target was not researcher-untouched. Unknown cross-project multiplicity and researcher familiarity are not included in confidence intervals. This is a held-out-collection computation within exploratory research, not prospective confirmation.

## Public data, rights and independence

Immutable caches contain record lists, headers and beat annotations (not full waveform downloads) for PhysioNet chfdb/nsrdb/chf2db/nsr2db version1.0.0 [4–7]. Public pages retrieved for this study permit open access under Open Data Commons Attribution License v1.0, without special approval. Canonical source URLs, original cache timestamps, per-file sizes and SHA-256 are in `files/code/manifest.json`; all declared inputs are hash-checked in the offline analysis. This is cache reuse, not a claimed fresh raw download. Source-page snapshot hashes/times are in `files/evidence/retrieval.json`. Upstream attribution and ODC-By obligations remain separate from the SCI baseline license for our own contribution. Direct literature downloads may be browser-check pages, as noted in literature notes; primary-source search/open verified the study descriptions.

Development: chfdb15 CHF and nsrdb18 normal records. External: chf2db29 CHF and nsr2db54 normal records. All116 pass primary eligibility; no primary exclusions. The statistical unit is a released participant-record, not a beat/window. The label is database CHF versus normal membership, not future disease or a representative screening outcome. First1000-interval fingerprints found no exact duplicate trace across the collections ([]). Different collections are an independent-dataset test, not proof of disjoint people: person linkage is unknown and a second recording from one person evades the fingerprint check.

Disease is completely confounded with collection in each development/external pair. Devices, annotation pipelines, age, sex, medication, disease severity, sleep, posture, activity and respiration may explain differences; jointly adequate metadata adjustment is unavailable. Source CHF includes severe disease. Six-hour windows are not aligned to clock-time or behavioral state. These limitations preclude clinical interpretation of database discrimination.

## Frozen analysis

Use the first6h, nonoverlapping5min windows, beat times rounded to common128Hz. Native-time sensitivity removes this rounding. RR is valid when both bounding beat annotations are N and interval0.3–2s inclusive. An increment requires two consecutive valid RR intervals; a sign pair/triple requires respectively three/four consecutive valid RR intervals. No rejection gaps are bridged. Each window needs >=200 valid RR, >=100 valid sign pairs and >=100 valid triples, and positive increment-square sum. At least12 windows per record are needed; first-hour sensitivity needs6. Strict-filter sensitivity rejects RR differing by >20% from an11-interval local median.

For valid RR increments, S=sign(RR[i+1]-RR[i]) takes -1,0,+1 (coded0,1,2). Positive RR increment is heart-rate deceleration. Code sign orientation differs from conventions that encode heart-rate change; CMI and MI are invariant to global label relabeling. Baseline nine record features: window medians of log meanNN, log sample SDNN, log RMSSD, pNN50 (strictly >50ms), negative-increment fraction, zero-increment fraction, lag1 ternary mutual information in nats, and hard-edge fraction (product of adjacent signs <0); plus log sample SD of eligible window meanNN, a six-hour SDANN analogue floored at1e-12s. Zero signs count in fractions/MI but not as hard edges. This comparator includes standard time-domain HRV [3] and first-order sign summaries, not every standard frequency-domain/nonlinear/clinical measure, sample entropy, DFA or full transition probability matrix.

Added tenth feature: median window plugin I(A;C|B) = sum[p(a,b,c) log(p(a,b,c)*p(b)/(p(a,b)*p(b,c)))], where A,B,C are three consecutive signs; probabilities are empirical frequencies of eligible triples, with zero cells omitted and no pseudocount. First-order MI uses eligible sign pairs. This tests conditional dependence beyond first-order in an information-theoretic sense, not long-term memory, directed causality, autonomic activity, or independence from all first-order predictive information. Finite-sample CMI bias, unequal valid triple count, symbol quantization, and nonstationarity remain. Binary sensitivity discards zero-sign positions **without bridging them** and recomputes every feature on its own eligible sample; it is not the identical baseline subset.

Source-only StandardScaler and balanced L2 logistic regression (C0.1,tol1e-11,max5000) fit baseline/added models. No target scaling, tuning or recalibration. Balanced probabilities are not clinical population risks. Brier loss (added minus baseline; negative favors added) is primary; AUC/log loss secondary. Joint500 class-stratified record bootstrap draws source and target separately, refits models, and computes percentile95% interval. Conditional2000 target-only draws fix fitted models. Seed26112616. These intervals condition on class counts, selected feature and collections, not person-level overlap, collection confounding or across-project multiplicity. Registered benefit requires primary interval upper<0.

Registered checks: C=.01,1,10; firsthour, strict and native preprocessing; binary symbols; reverse transfer; source leave-one-record-out CV; target leave-one-out influence;100 source-label shuffles;10 within-window target sign-order shuffles changing only added CMI while preserving baseline features. Shuffles are descriptive diagnostics, not realistic clinical interventions or exact physiological null tests. Reverse transfer is a sensitivity, not a second untouched confirmation.

After seeing the shuffled-sign improvement, we added explicitly **posthoc** diagnostics:1000 permutations of target CMI across records (preserving its marginal distribution), target CMI set to constants, and window-count adjustment to both models (seed26112617). No posthoc variant replaces the primary. Triple-count matched, bias-corrected and fully acquisition-matched comparisons remain future work.

## Results

Adding median five-minute ternary sign conditional mutual information reduced external CHF-versus-normal Brier loss by 0.01485 (added-minus-baseline -0.01485, joint record bootstrap 95% [-0.03151, -0.00398]) beyond time-domain HRV and first-order sign summaries. Strong timing sensitivity and better performance after target sign destruction prevent attributing the gain specifically to physiological memory.

|Analysis|Baseline Brier|Added Brier|Difference|
|---|---:|---:|---:|
|Primary C0.1|0.18583|0.17098|-0.01485|
|C0.01|0.21304|0.20860|-0.00444|
|C1|0.20422|0.17710|-0.02712|
|C10|0.20929|0.18638|-0.02291|
|firsthour|0.19504|0.17570|-0.01934|
|strict|0.18736|0.17205|-0.01532|
|native|0.20676|0.20376|-0.00301|
|binary|0.18752|0.17795|-0.00957|
|Reverse transfer|0.09869|0.08454|-0.01416|

Primary AUC baseline/added 0.81290/0.83972; log loss 0.54669/0.51127. Conditional95% interval: [-0.022430041907387063, -0.007283846159081378]. All sensitivity sample counts/exclusions, source CV, marginal out-of-source-range fractions, per-record features/predictions and bootstrap/permutation draws are retained under `files/results/`.

Source LOOCV: `{"base": {"brier": 0.10544194647595602, "auc": 0.9148148148148147, "logloss": 0.41955927496866313}, "added": {"brier": 0.10067808205573672, "auc": 0.9148148148148149, "logloss": 0.40693510401753297}}`. Leave-one-target difference range: [-0.016333879516263392, -0.01367847584293415]. Binary excludes chf214; firsthour excludes nsrdb16773 and chf212/214/219. Native-timing gain is much smaller than common-grid gain; this weakens timing robustness.

**Falsification and interpretation.** Within-window target sign destruction gives mean delta -0.02395, better than observed despite destroying memory. Setting target CMI to zero gives delta -0.03117; source/target-median constants give -0.00744/-0.00766. Thus adding a predictor changes other coefficients/calibration, and a target feature distribution shift can improve Brier without meaningful individual memory. Conversely, permuting CMI among target records gives mean delta -0.00027,95% range [-0.009718441254647334, 0.008380429879967566], fraction <=observed 0.002. This suggests subject alignment carries information conditional on this model, but does not isolate physiology from collection artifacts. It is posthoc and not a corrected discovery p-value. Window-count-adjusted delta is -0.01481; this crude quality proxy does not control valid-triple count or CMI bias. Source-label shuffle fraction <=observed is 0.24; its95% range is [-0.027183737555558193, 0.028037325352807035]. These mixed controls prevent interpreting the primary gain as validated novel physiological memory.

## Author checks and reproduction

Author feature reconstruction uses separate Counter-based probability counts and loops for all116 records; a separate SciPy BFGS objective reproduces model probabilities within1e-7. Actual audit: `{"passed": true, "records_reconstructed": 116, "max_feature_error": 8.881784197001252e-16, "max_probability_error": 3.307889534509556e-08, "optimizer_statuses": [{"success": true, "message": "Optimization terminated successfully.", "gradient": 1.6866952279315228e-10}, {"success": false, "message": "Desired error not necessarily achieved due to precision loss.", "gradient": 6.782368666691241e-09}], "manual_delta": -0.01484726316709951, "independent_CMI": 0.0, "xor_CMI": 0.6931471805599453}`. One optimizer reports precision loss despite gradient<7e-9 and max probability discrepancy3.31e-8; this status is retained rather than called unconditional optimizer success. Exact independent triples give CMI0 and XOR triples give log2. Audit checks primary arithmetic and bootstrap quantiles. These are author self-checks, not independent peer review.

Clean rerun from packaged inputs uses `bash files/reproduce.sh /absolute/new/output-directory`. The included requirements/Dockerfile rebuild the recorded image if absent. Reproduction includes primary, separate audit and posthoc challenge. Final self-check/rerun evidence records actual completion and comparisons: JSON except elapsed runtime, CSV bytes, numerical NPZ draws and audit/challenge outputs. Analysis runs without network, read-only root, all capabilities dropped, no-new-privileges, only declared data/code/output mounts and no host-home/credentials. Limits:1CPU,768MiB,64PIDs,900s per process. No nested agents, GPUs, credentials, signing keys, restricted data or new data collection. Literature/data retrieval is separate trusted orchestration. Declared inputs ~27MB; primary runtime 38.56s; actual image identifier and sizes are retained in execution evidence. Ordinary startup issues (missing python alias, archival destination collision) were fixed without altering immutable inputs or prior payloads.

## Limits and next scientific test

The primary incremental prediction criterion is met in this exploratory collection contrast, but broad biomarker/physiology claims fail the stronger interpretation checks. In addition to confounding, unknown overlap, finite-sample estimator bias and repeated target exposure, collection-level uncertainty cannot be estimated from two pairs of databases by record bootstrap. Neither source CV nor reverse transfer removes these weaknesses. There is no evidence here for general clinical superiority over comprehensive HRV or standard clinical assessment.

A future independently authorized clinical study could obtain matched CHF/non-CHF ECG under one device/protocol, record respiration, activity, sleep, medication and participant linkage, prospectively freeze a bias-aware CMI/model and compare against frequency-domain HRV, sample entropy, DFA, full first-order transitions and clinical predictors. It could test repeatability and whether common-grid versus native-time effects persist under matched timing resolution. Within-person repeated recordings would help separate physiological changes from measurement artifacts. No such clinical/physical experiment was performed here.

## References

1. Multiscale analysis of acceleration and deceleration of the instantaneous heart rate using symbolic dynamics. Primary study, PMID22254718. https://pubmed.ncbi.nlm.nih.gov/22254718/
2. Costa MD, Davis RB, Goldberger AL. Heart Rate Fragmentation: A Symbolic Dynamical Approach. Front Physiol8:827(2017). https://doi.org/10.3389/fphys.2017.00827
3. ESC/NASPE Task Force. Heart rate variability: standards of measurement, physiological interpretation and clinical use(1996). https://doi.org/10.1161/01.CIR.93.5.1043
4. PhysioNet BIDMC CHF Database v1.0.0. https://physionet.org/content/chfdb/1.0.0/
5. PhysioNet MIT-BIH Normal Sinus Rhythm Database v1.0.0. https://physionet.org/content/nsrdb/1.0.0/
6. PhysioNet CHF RR Interval Database v1.0.0. https://physionet.org/content/chf2db/1.0.0/
7. PhysioNet Normal Sinus Rhythm RR Interval Database v1.0.0. https://physionet.org/content/nsr2db/1.0.0/
