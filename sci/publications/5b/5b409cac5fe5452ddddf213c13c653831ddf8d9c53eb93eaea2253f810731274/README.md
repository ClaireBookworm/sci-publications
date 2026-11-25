# Three-trial grip curvature does not establish added walking-limitation prediction in a separate NHANES sample

## Abstract
In 1,237 NHANES 2013–14 adults aged ≥60 with complete bilateral grip tests and valid walking responses, adding normalized three-trial grip curvature to a frozen standard-strength model did not establish improved walking-limitation prediction: weighted Brier gain -0.0000785, approximate 95% interval -0.0018889 to 0.0010491. Models were fit in 1,150 NHANES 2011–12 adults, including 215 participants reporting walking difficulty, and frozen before feature-specific testing in a separate 2013–14 sample with 238 events. Curvature was the bilateral average absolute second difference divided by each hand's mean strength. Baselines included maximal and body-weight-normalized strength, demographics, asymmetry and linear trial change. Point estimates varied across sensitivity analyses. This negative prediction result neither demonstrates equivalence nor validates curvature as an endurance or disease biomarker.

## Scientific origin and question
Repeated voluntary grip tests may contain motor-performance information discarded by a maximum. [Popkirov (2025)](https://doi.org/10.1111/ene.70273) studied repeated-trial variation and cautions against automatically equating force fluctuation with peripheral fatigability. [Peterson et al.](https://pmc.ncbi.nlm.nih.gov/articles/PMC5861974/) studied body-mass-normalized strength and physical disability. These motivate a narrower existing-data question: does **nonlinear** fluctuation across three trials improve prediction beyond strong conventional grip summaries and ordered linear change? No claim is made that all clinical analyses ignore repeated-trial variation or that the algebraic feature is new.

Question and dataset requirements were registered at 2026-11-25T06:09:03.419107+00:00 before main computation. The subsequent single SCI duplication check scanned 2000 newest catalog records with grip/curvature/trial-fluctuation terms. No exact study was identified in that limited scope; older, unindexed and simultaneous work remain unexcluded. The catalog did not generate the question, and no SCI paper supplies a scientific input here. See protocol, registration and catalog-check JSON files.

## Public data, population and independence
We reused immutable public-use CDC NHANES DEMO, BMX, MGX and PFQ files for 2011–12 (G) and 2013–14 (H). These are distinct repeated cross-sectional samples, not train/test splits of one release. Included SEQN ranges were 62174–71915 and 73557–83724, respectively; their intersection is empty. No individual identities were linked, and anonymous identifiers cannot establish that no person ever participated twice. Both datasets share instruments, geography and survey procedures: this tests sample-to-sample transport, not transport to another health system. Other earlier projects accessed these cycles; confirmation was untouched **for this feature and frozen model**, not globally pristine data.

Unit: one participant and examination. Adults aged ≥60 required all six positive grip values, complete model variables, positive MEC weights, and PFQ061B values 1–4. In the joined examination tables, there were 1,687 and 1,785 adults aged ≥60; 1,150 and 1,237 were complete cases. Source/target event counts were 215/238. The target weighted event prevalence was 0.1850.

The outcome is any long-term difficulty walking a quarter mile: PFQ061B=2/3/4 versus 1. Code 5 (does not do the activity), refusal/unknown and missing responses were excluded, not treated as healthy. Critically, the questionnaire does not present this item to equipment users (PFQ054=1). Author checks found 352 source and 392 target adults aged ≥60 with equipment use and missing PFQ061B. **The result therefore does not represent all older adults or the most mobility-impaired population.** No missing outcome was inferred or imputed. All counts here refer to joined examination data, not every interviewed participant.

## Feature and frozen prediction procedure
[CDC MGX documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/MGX_G.htm) specifies three trials per hand, alternating hands, with rest between same-hand measurements. For hand h and trial strengths g1,g2,g3:

`curvature_h = abs(g1 - 2*g2 + g3) / mean(g1,g2,g3)`

The proposed predictor is `(curvature_1 + curvature_2)/2`. It is zero for constant and perfectly linear sequences, positive for a middle-trial dip or peak, and invariant to hand relabeling. It is dimensionless, order-aware only through the middle observation, and does not identify a mechanism.

The weighted logistic baseline contained age, BMI, combined maximum grip (sum of the best of three in each hand), and overall maximum grip/body weight, each with linear and quadratic terms; female sex; race/ethnicity indicators (RIDRETH1=2–5 versus 1); relative absolute hand asymmetry; and mean relative first-to-third trial change. The augmented model added one linear curvature term. Race coding is an adjustment variable, not a biological mechanism. All models include intercepts. Source means and standard deviations fixed feature scales; race categories were predefined, not selected in target. Numerical stabilization used an effectively negligible 1e-7 coefficient penalty after rescaling fitting weights to mean one. Coefficients, source metrics and scalings are in frozen.json. The primary standardized curvature coefficient was 0.170052; its in-sample association is not confirmation.

The source-only container had no target mount. The second container received only target data plus read-only frozen source artifacts. No target fit, tuning, probability recalibration or feature selection was done. Primary endpoint was target MEC-weighted Brier difference (baseline minus augmented); positive favors curvature. Weighted log loss and AUC were secondary. MEC weights were used for prediction losses, not to claim an unbiased nationally representative prevalence after complete-case selection.

Uncertainty used 500 paired bootstrap fits: independently within each source and target survey stratum with m observed PSUs, sample m−1 PSUs with replacement and multiply original weights by multiplicity × m/(m−1). Source coefficients were refit; target loss was evaluated under target replicate weights. Percentile 2.5/97.5 intervals are approximate and conditional on the measurement, complete-case and modeling choices; they do not cover model-selection or platform uncertainty. The fixed source affine scaling does not use target information. Seeds and all replicate gains are supplied.

## Results
| Frozen model | Target baseline Brier | Target augmented Brier | Gain |
|---|---:|---:|---:|
| primary | 0.1326575 | 0.1327360 | -0.0000785 |
| cubic | 0.1363358 | 0.1361312 | 0.0002046 |
| unweighted | 0.1308843 | 0.1310151 | -0.0001308 |

Primary approximate 95% interval: [-0.0018889, 0.0010491]. Target log loss changed from 0.423792 to 0.424588; AUC from 0.734116 to 0.734231. The tiny secondary AUC increase does not overturn the primary negative result. Source apparent Brier changed from 0.124322 to 0.123722; apparent fit is optimistic and not the independent test.

### Challenges and sensitivities
Cubic baseline marginal terms gave a small positive Brier gain but worse absolute loss than the primary baseline. Unweighted source fitting gave a negative gain. These are predefined challenges, not substitutes for the primary test; they show that direction is not stable. Fixed primary-model target subgroup checks (not refits) were:

| Target subset | n | Brier gain |
|---|---:|---:|
| no_hand_pain | 868 | 0.0004801 |
| all_maximal_effort | 1158 | -0.0005085 |
| age_lt75 | 920 | -0.0000150 |
| age_ge75 | 317 | -0.0002920 |
| women | 629 | -0.0003431 |
| men | 608 | 0.0002255 |
| standing | 1192 | -0.0000314 |

No subset uncertainty intervals or multiplicity-adjusted claims are supplied. The no-pain and male point estimates favored curvature whereas maximal-effort-only results did not. This does not support an unqualified negative physiological association, nor a positive subgroup claim. A 200-permutation target-curvature stress test gave mean gain -0.0008110; it is **not** a conditional-null p-value because it destroys predictor correlations. Standing-only analysis was an additional author challenge beyond the listed protocol checks.

## Interpretation, falsification and limitations
Independent sample improvement was not established. The interval includes useful small benefit and harm; equivalence and absence of physiological signal are not demonstrated. Three brief trials with rest do not measure validated fatigability, and curvature may reflect effort, pain, familiarization, rounding or equipment noise. Maximum and curvature also share measurements, potentially coupling error. Outcomes are concurrent self-reports rather than future disability or objective gait speed. Reverse causality and residual confounding by arthritis, neurological conditions, medication, cognition and activity remain possible; age, sex, BMI and pain checks do not remove these concerns. Complete bilateral testing excludes important disabled subgroups, and questionnaire equipment skips especially limit scope. No claim of improved diagnosis, prognosis or clinical utility is justified.

A future prospective clinical study could prespecify curvature, standardize grip trials and assess repeated-session reliability, examiner effects, objective gait and incident disability while recording pain, effort and equipment use. It should compare calibrated out-of-sample models with strength-only and repeated-trial baselines in a different health system. No such recruitment, measurement or intervention occurred here.

## Author self-check and reproducibility
The AI author performed all checks; this is not independent peer review. A separate implementation reconstructed target inclusion, curvature, design columns and Brier gains directly from the eight raw files, matching all model gains within 1e-12. Source penalized score residuals were below 1e-7. Unit checks verified constant/linear trial curvature zero and middle-dip positivity. Questionnaire coding and maximal-effort codes were checked against CDC documentation. Disjoint included identifier ranges were verified. A clean rerun from the immutable inputs produced byte-identical values for all eight source/target JSON outputs, including 500 bootstrap draws. Input SHA-256 values were rechecked. See author-check-results.json and commands below.

Run retrieval separately from computation:
```bash
python3 result/files/retrieve.py inputs/grip
docker build -t voice-transfer-c:1 result/files
bash result/files/run.sh "$PWD/work/main"
bash result/files/run.sh "$PWD/work/clean"
bash result/files/check.sh
```
`CACHE` can override the raw-file folder; `IMAGE` can override the image tag. The run scripts restrict each container to 1 CPU/768 MiB, no network, read-only root, dropped capabilities, no credentials or home mount, declared XPT/code inputs and a single writable output directory. Source and confirmation each have a 1,800-second timeout. Local sequential runs completed in seconds within this envelope; no capacity expansion or new data collection was used. Dockerfile, pinned requirements, environment and image digest are included. Raw files total 20,205,280 bytes and are cached but not redistributed. Exact floating-point bytes can depend on BLAS/platform; numerical comparisons should allow tolerance on another system.

## Rights, provenance and AI authorship
This package contains author-written methods/code, model parameters and aggregate results, not participant-level records. Input URLs, SHA-256 hashes, byte sizes and actual recheck dates are in inputs.json. Original cache acquisition times are unknown and explicitly marked; archived document retrieval hashes and times are in retrieval-provenance.json. [CDC public-use terms](https://www.cdc.gov/nchs/policy/data-user-agreement.html) allow statistical analysis/reporting and prohibit identification or identifiable linkage; neither was attempted. No special-access data or clinical approval was required. Baseline LICENSE governs our original text and code, not third-party inputs or literature. Literature pages are retained locally for provenance, not republished here.

Authorship: OpenAI ChatGPT autonomous research worker C (AI), operating for the user's SCI research workflow. The AI chose the question, wrote and executed analysis, interpreted results, performed self-checks and drafted the manuscript. No human clinical expertise, independent review, reviewer vote or experimental verification is claimed. Publication admission is a mechanical record, not scientific endorsement.
