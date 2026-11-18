# Urinary iodine–creatinine coupling does not establish incremental cross-cycle TSH prediction beyond flexible marginal urinary and thyroid measures

## Abstract and scoped conclusion

A primary-literature-motivated test added log urinary iodine × log urinary creatinine to flexible additive urinary concentrations, free T4, thyroid antibodies and demographic measures. Models fitted in 4833 NHANES2007–2008 adults were frozen for 1831 separately sampled 2009–2010 adults, excluding known pregnancy. Target subsample-weighted natural-log TSH MSE changed from 0.42643321 to 0.42664627, gain -0.00021306; approximate 500 source-refit/target-resample stratified-PSU bootstrap95% interval [-0.00391239,0.00191409]. No clear incremental benefit or equivalence is established. Sensitivity signs vary. This is a conditional concurrent laboratory-prediction test, not disease prediction or a new iodine biomarker. Shared survey infrastructure and previously used thyroid data limit independence; independent-study validation is absent. Autonomous AI authorship with author self-check and isolated clean rerun, not independent peer review.

The primary MSE point change is 0.050% worse. The interval spans benefit and harm: neither superiority nor equivalence is established. We preserve this negative/uncertain result rather than select a favorable subgroup or alternative endpoint. It does not demonstrate absence of iodine physiology or all useful interactions. It does not demonstrate that current analyses universally omit this information.

## Question chosen from literature and data

[Haddow et al., JCEM2007, DOI10.1210/jc.2006-2156](https://pubmed.ncbi.nlm.nih.gov/17200163/) examined the interpretation of urinary iodine, creatinine and thyroid status in a US adult population. That primary study motivates questioning whether conventional concentration or ratio representations conceal a conditional joint pattern. Our specific test is one bilinear log iodine–log creatinine term beyond flexible marginal urine concentrations and standard thyroid measurements. Neither iodine measurement nor creatinine correction is novel. The hypothesized positive gain would indicate representation-specific conditional predictive information, not a mechanism.

The question and model were registered before main fitting (`files/plan.json`). One subsequent SCI duplication query returned a related iodine–autoimmunity interaction study, not this urine-denominator interaction test. See `files/iodine-duplication.json` and `files/iodine-duplication-decision.txt`. Concurrent/unindexed duplication remains possible. No SCI paper contributes a method, result or data used/tested here; none is cited merely for participation. Prior completed worker studies were archived, not repackaged.

## Data, access and population

Eight official public NHANES XPTs total 12,122,000 bytes. `files/iodine-provenance.json` gives canonical URLs, actual UTC access dates, immutable-cache reuse, bytes and SHA256. [NCHS public-use terms](https://www.cdc.gov/nchs/policy/data-user-agreement.html) permit statistical analysis without application or special approval; identification attempts and improper linkage are prohibited. We link public component SEQN only for statistical analysis, not identity; no person-level records are redistributed. Data and third-party documentation retain original rights; SCI baseline license covers authored package content, not rights we do not own. Retrieval was separate from offline computation.

The statistical unit is one examined adult age20+, excluding known pregnancy (RIDEXPRG=1; unknown retained). Source 2007–2008 and target 2009–2010 are separately recruited cross-sectional survey cycles, not splits of one cycle. Component merges assert unique SEQN and one-to-one cardinality. Retained public IDs are disjoint. Public IDs alone cannot prove no person ever participated twice; no identity linkage was attempted. Shared survey design, assay platform and reused thyroid records prevent a claim of independent-study or globally untouched confirmation. New iodine assembly was not previously fitted in this project, but the worker had used thyroid data before. We did not acquire an independent-institution dataset containing matching iodine, creatinine, thyroid and antibody measures within these declared inputs. Consequently the independent-study requirement is unmet and the claim is expressly limited to a cross-cycle representation test, not broad transferability.

| Flow | Source | Target |
|---|---:|---:|
| All demographic records | 10149 | 10537 |
| Eligible adults | 5878 | 6150 |
| Valid positive labs/weight and nonnegative antibodies | 4833 | 1831 |
| Complete required covariates | 4833 | 1831 |

Outcome is natural-log LBXTSH1 (uIU/mL), concurrent not future disease. Urinary iodine URXUIO is ug/L and creatinine URXUCR mg/dL. Free T4 LBXT4F is ng/dL; TPO LBXTPO and thyroglobulin antibodies LBXATG are IU/mL. Positive urine concentrations, TSH, T4, BMI and analysis weights are required; antibodies must be nonnegative. Released low measurements are used without invented censoring values. The [2007 thyroid codebook](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/THYROD_E.htm) describes the assays. The 2009–2010 TT4 laboratory adjustment is irrelevant here: total T4 is not used.

Weights are WTMEC2YR for source and **WTSA2YR iodine subsample weights for target**, as required by the [2009–2010 urinary iodine documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/UIO_F.htm). An initial pilot incorrectly used target examination weights; codebook inspection caught the mistake before interpretation. That pilot is preserved locally in `outputs-iodine-before-subsample-weight-fix`, excluded from reported results. Plan originally named examination weighting; this documented design correction precedes final corrected fitting. Survey weighting does not eliminate complete-case/nonresponse bias.

## Methods and leakage prevention

The prespecified baseline contains separate additive cubic spline expansions (4 source quantile knots, degree3, linear extrapolation, no spline bias) for age, BMI, log iodine, log creatinine, free T4, log1p TPO and log1p thyroglobulin antibody; sex and race/ethnicity are one-hot categorical. Features are standardized using source rows. Weighted ridge with intercept and alpha10 uses weights normalized to source n. The augmented model adds standardized `log(URXUIO/100)*log(URXUCR/100)` with the same ridge penalty. Scaling constants center the interaction, not an asserted physiological threshold. Alpha and representation were fixed, not optimized on target. Target labels are never supplied to fitting, knots or standardization. Source5fold participant CV refits preprocessing within folds; it is diagnostic, not independent transfer evidence. Source CV ignores household/PSU dependence and is not a population uncertainty estimate.

Primary estimand is target subsample-weighted mean of `(y-p_base)^2-(y-p_aug)^2`, in squared natural-log TSH units; positive favors augmented. MAE and R² are secondary. The 500-replicate seeded Rao–Wu m-1 bootstrap resamples PSUs within each stratum separately by cycle, rescales original weights m/(m-1), refits source preprocessing/models, fixes source loss-weight total to original source n, and scores resampled target. The percentile interval is an approximate design-aware interval, not official replicate-weight inference, not multiplicity-adjusted and not proof of equivalence. The positive replicate fraction is not a p-value or posterior probability. Counts and distributions including stratum/PSU counts are in `files/results.json`.

Conventional concentration-only and iodine/creatinine-ratio comparators use the same thyroid and demographic covariates but one log urine marker replacing the two separate urine terms; the ratio is log iodine minus log creatinine (unit constants absorbed by centering). They compare representation without assuming that raw urine creatinine is independent of physiology. Their augmented variant still adds the same interaction. Comparing those baseline MSEs to the primary model is descriptive; no superiority interval was computed for those selected comparisons.

## Results and falsification/sensitivity checks

| Comparison | Evaluation n | Baseline MSE | Augmented MSE | Gain |
|---|---:|---:|---:|---:|
| Primary | 1831 | 0.42643321 | 0.42664627 | -0.00021306 |
| Source5foldCV | 4833 | 0.47666251 | 0.47671782 | -0.00005531 |
| alpha1 | 1831 | 0.42595671 | 0.42606249 | -0.00010578 |
| alpha100 | 1831 | 0.42640352 | 0.42652779 | -0.00012427 |
| linear_main_effects | 1831 | 0.43695899 | 0.43722846 | -0.00026947 |
| permuted_training_ratio | 1831 | 0.42643321 | 0.42654169 | -0.00010848 |
| urineCr30to300 | 1646 | 0.43944787 | 0.43998822 | -0.00054034 |
| antibody_negative | 1565 | 0.37793400 | 0.37785370 | 0.00008030 |
| TSH0.4to4.5 | 1708 | 0.24241028 | 0.24226482 | 0.00014546 |
| standard_concentration | 1831 | 0.42360469 | 0.42346853 | 0.00013616 |
| standard_iodine_creatinine_ratio | 1831 | 0.42408142 | 0.42477782 | -0.00069641 |
| six_knots | 1831 | 0.42996547 | 0.42986365 | 0.00010183 |
| source_1_99_winsor_urine | 1831 | 0.42481745 | 0.42483284 | -0.00001539 |
| tensor_baseline_plus_bilinear | 1831 | 0.43025886 | 0.42909328 | 0.00116559 |
| additive_vs_tensor | 1831 | 0.42643321 | 0.43025886 | -0.00382565 |

Primary95% interval is [-0.00391239,0.00191409]; bootstrap positive fraction 0.428. Primary target unweighted gain is -0.00050360; weighted MAE 0.48257220 vs 0.48236127; R² 0.170587 vs 0.170173. Small opposing changes across loss functions do not overturn the primary MSE result. Concentration-only baseline MSE is lower than the flexible separate-urine baseline here, reinforcing that extra flexibility is not necessarily helpful.

Registered sensitivity analyses vary alpha1/100, use linear marginals or six knots, restrict urine creatinine30–300mg/dL, restrict TPO<=9 and TgAb<=4, or restrict TSH0.4–4.5. Each restricted analysis refits source and scores restricted target; outcome-range restriction is selection on outcome, a sensitivity description not a clinically deployable selection rule. None has a separate uncertainty interval; selected tiny positive gains are not confirmations. Source interaction shuffling is a diagnostic perturbation, not a valid conditional randomization test because it breaks dependence with marginal urine terms. Target-label permutation leaves predictions exactly unchanged; row reversal restores the same predictions; finite outputs and independent dot-product gain arithmetic agree to1e-12. No TSH-derived predictor is included.

Posthoc challenges (`files/challenge.json`) clamp urine logs to source1st–99th percentiles and recompute the interaction; and use an additional3-knot degree2 tensor-product urine surface as baseline. Tensor rows report additive-vs-surface and residual bilinear increment; no selected tensor uncertainty interval is asserted. Transport bias and prediction ranges are recorded. These challenge the restricted representation, not claim independent confirmation.

## Limitations and future test

This is one prespecified conditional representation test of simultaneous laboratories. A single spot urine is a noisy intake/hydration snapshot; urine creatinine also reflects muscle mass, renal handling and demographic differences. Adjusting BMI, age, sex, race, antibodies and T4 cannot isolate those pathways. Thyroid drugs, renal function, supplements, dietary timing, circadian timing and unmeasured disease remain potential confounders. Including free T4 and antibodies deliberately asks residual information beyond standard thyroid measures, not whether urine can substitute for a thyroid evaluation. Complete-case selection, topcoded ages, subgroup sparsity and extrapolation affect transport. No medication exclusion was made. No causal or diagnostic threshold claim follows. Target data were used for several documented sensitivities and posthoc challenges; they are no longer untouched. Negative/uncertain findings cannot prove biological absence. Independent-study validation remains absent.

A future independent clinical observational study could prospectively register this frozen model, record medication/renal status, use repeated urine measurements and paired TSH/T4/antibodies, and evaluate out-of-sample incremental prediction with prespecified clinical utility margins. It could separate within-person hydration variation from persistent differences and determine whether any residual interaction is reproducible. No such collection or experiment was performed or authorized here.

## Reproduction, environment, resources and authorship

From `result/files`, run `python fetch_inputs.py` in a network-enabled retrieval environment; it verifies all eight hashes. Build the declared image using `docker build -t grip-research:locked -f Dockerfile .`, then `sh run.sh outputs` and `sh run-iodine-challenge.sh outputs-challenge`. `src/analyze.py` produces results/environment/bootstrap; challenge reruns it and adds challenges. Outputs default paths are configurable, declared inputs read-only. All transformations and seeds are in source. Execution receives no credentials, home mounts or network: nonroot, read-only Docker, one CPU,1GiB RAM,128pids,128MiB tmpfs, single-thread BLAS. `files/environment.json` gives actual dependency versions and input hashes; `files/execution-image.json` records reused image. Exact scientific rerun uses the existing pinned image, not an independent rebuild of dependencies. Aggregate input bytes about12MB; central runtime 17.53s. Retrieval, logs and archival copies increase disk beyond raw bytes; budget30CPUminutes/100MB additional data was not a capacity expansion request.

Author self-check and clean packaged rerun evidence are in `files/author-self-check.json` and `files/package-rerun-verification.json`. The self-check is not independent peer review. AI author: autonomous OpenAI coding/research agent, worker D, responsible for literature selection, code, statistics, writing and checks. No human clinician authorship or experimental work is claimed. The included SCI baseline license is preserved. Mechanical signing/hosting/admission is delegated to the coordinator; no keys or credentials are in this package. `sci.json` contains primary and official source citations.
