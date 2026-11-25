# Red-cell dispersion–volume interaction: uncertain incremental prediction of C-reactive protein across NHANES cycles

## Abstract and scoped claim

In exploratory transfer from NHANES 2007–2008 (n=4,850) to 2009–2010 (n=5,230), an RDW-by-MCV interaction reduced weighted log-CRP MSE by 0.005457 (0.48%), but joint source/target PSU-bootstrap uncertainty included no gain (added-minus-baseline MSE 95% interval -0.011052 to 0.005822). Reliable incremental predictive value was not established. A comparator incorporating the conventional absolute-volume-dispersion proxy RDW×MCV/100 had MSE 1.130757; adding the proposed interaction worsened this to 1.132271. Thus the small apparent gain over separate main effects does not establish a new useful biomarker or improvement beyond standard composite information. This is an informative unsuccessful candidate, not an equivalence test or evidence that red-cell physiology has no inflammatory signal. All analyses used existing public data; no new measurements or experiments were conducted.

## Literature-led question and registration

[Lippi et al., 2009](https://pubmed.ncbi.nlm.nih.gov/19391664/) investigated RDW alongside inflammatory markers and other blood indices including MCV in adult outpatients. A [primary NHANES study of RDW, inflammation and fitness](https://pmc.ncbi.nlm.nih.gov/articles/PMC3860736/) supplies physiological context: RDW describes relative red-cell volume variability. We therefore asked whether the inflammatory association of relative dispersion depends on mean volume, beyond conventional separate indices and confounders. This is not a claim that RDW–inflammation association, volume dispersion or interaction modeling is new. The exact product is a modest low-order candidate, not a recovered cell-volume distribution or direct measure of erythropoiesis. We did not systematically establish that current clinical analyses never use these variables jointly.

The question and dataset needs were written in `files/evidence/registration.json` before fitting. Source and target, outcome, model, success criterion, uncertainty and challenges were specified then. The single documented SCI duplication check queried a bounded 1,000-record title/abstract catalog response for distribution width/rdw/corpuscular/crp. Three unrelated substring hits were examined; none was this question. Its incompleteness and concurrent unpublished work prevent a guarantee of no duplication. No SCI paper supplied a scientific result or method used here, so no SCI citation is manufactured.

Direct primary-page retrieval encountered browser challenges (PubMed status203; PMC returned challenge content despite HTTP200); the scientific rationale uses indexed primary-source abstract/context information, not claimed successful full-text access. Retrieval metadata is retained. No full copyrighted article is redistributed.

## Public data, rights and independence

Source: independently recruited [NHANES 2007–2008 CBC](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/CBC_E.htm); target: [NHANES 2009–2010 CBC](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/CBC_F.htm). CRP, DEMO, BMX and SMQ components are inner-linked one-to-one by public SEQN within each cycle. Statistical unit: one participant. Inclusion: age20–79, not known pregnant, positive CRP, RDW, MCV, hemoglobin, WBC, BMI and exam weight, complete age/design variables and valid sex/race/smoking. Unknown pregnancy is retained. Never-smoker SMQ040 structural skips are explicitly retained.

| Stage | Source | Target |
|---|---:|---:|
| Inner-linked participants |6917|7369|
| Age/pregnancy eligible |5261|5595|
| Complete analytic participants |4850|5230|
| Design PSUs |32|31|

Neither analytic set contains singleton strata. Public IDs are unique and disjoint across cycles. Independently recruited cycles provide an independent-dataset test, not two splits of one dataset; incidental reenrollment cannot be excluded from public IDs. Shared sampling, instruments, geography and protocols make this **same-survey cross-cycle transfer**, not independent healthcare-program validation. The worker has previously explored many models in these waves, including CRP in the immediately preceding project. Neither wave is untouched in the broad research program. The primary model was frozen before this run, but this remains exploratory transfer rather than untouched or multiplicity-controlled confirmation. A genuinely new cohort would be needed for stronger confirmation.

Raw NCHS public-use XPT files require no special approval for statistical use. The [NCHS data-use agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html) restricts use to statistical analysis/reporting, forbids reidentification and identifiable linkage; those conditions were respected. No restricted records, identification attempts or administrative credentials were used. Twenty input XPT/dictionary files total 20,156,095 bytes. Canonical URLs, sizes, SHA-256 hashes, cache origin and actual verification times are in `files/code/input-manifest.json`. Existing immutable cache copies were reused; unknown original retrieval dates remain unknown. These government input rights/restrictions are not overridden by the package license.

## Outcome, baselines and leakage prevention

Outcome: natural log `LBXCRP` in **mg/dL**, not mg/L. CBC variables: `LBXRDW` percent, `LBXMCVSI` fL, `LBXHGB` g/dL, log `LBXWBCSI`. This is concurrent prediction of CRP, not future disease or a causal estimate. Direct CRP measurement remains the conventional inflammatory measure; predicted CRP is not proposed as a substitute.

The baseline is ridge regression, alpha1, with separate cubic spline transforms of age, BMI, logWBC, RDW, MCV and hemoglobin (four source quantile knots per variable; include_bias=False; linear extrapolation defaults to constant in sklearn). Binary covariates: female; race categories2–5 (category1 reference); former/current smoking (never reference). This flexible additive comparator includes conventional red-cell indices and major inflammatory correlates rather than comparing against an artificially weak single-marker baseline. The augmented model adds just one product: source-mean-centered and source-SD-scaled RDW times similarly standardized MCV. Source SD uses pandas ddof1. Spline knots, product moments and StandardScaler are fitted only on the current training set and reused unchanged on its target. No target outcomes enter model fitting, preprocessing, hyperparameter selection or feature choice. Categorical coding and positivity exclusions are deterministic. All inputs are positive/complete cases, without imputation.

Fits use WTMEC2YR weights normalized by the training mean; held-out metrics use original target weights. Preprocessing moments and knot quantiles are unweighted, explicit modeling choices. Weighted MSE of logCRP is primary; weighted MAE and mean residual y−prediction are descriptive. Success required negative target ΔMSE **and** an upper joint-bootstrap95% endpoint below zero; the latter failed. The primary penalty is not target-tuned. Reverse-cycle and subgroup analyses are descriptive challenges, not independent confirmations.

## Actual results and uncertainty

| Target metric | Additive baseline | With interaction | Added minus baseline |
|---|---:|---:|---:|
| MSE, log(mg/dL)² |1.136178|1.130721|-0.005457|
| MAE, log(mg/dL) |0.837640|0.835698|-0.001942|
| Mean residual |-0.056304|-0.055263|+0.001041|

Uncertainty uses 1,000 target-only and400 joint source/target rescaled stratified PSU bootstrap replicates (seed2026112585). Within each stratum of m PSUs, sample m−1 with replacement and multiply original weights by m/(m−1) times the multiplicity. Source and target are resampled independently; joint replicates refit both models and source preprocessing. Strata/PSU names belong to each cycle, not shared clusters across cycles. Percentile intervals are approximate design-aware uncertainty, not exact finite-population inference; sparse PSUs and complete-case selection limit calibration.

The target-only ΔMSE95% interval is [-0.010365, -0.000230], whereas the joint interval is [-0.011052, 0.005822]. Reporting only the conditional target interval would overstate the evidence. The source-fit uncertainty changes the interpretation. No equivalence margin or multiplicity adjustment was specified.

## Falsification and sensitivity checks

The registered checks retain the same modeling family. Sex-specific fits refit on each subgroup; CRP≤1mg/dL restricts high inflammatory values; common support keeps RDW and MCV within source2.5th–97.5th percentiles; unweighted fit retains weighted target evaluation; NLR is log(neutrophil/lymphocyte absolute counts), finite only; CRP>.02mg/dL assesses low-end rounding/detection sensitivity.

| Challenge | Training n | Target n | ΔMSE |
|---|---:|---:|---:|
|female|2447|2685|-0.011139|
|male|2403|2545|-0.000269|
|crp_le_1|4366|4730|-0.003556|
|redcell_common_support|4460|4700|-0.001127|
|reverse_cycles|5230|4850|-0.004256|
|alpha0.1|4850|5230|-0.005726|
|alpha10|4850|5230|-0.005431|
|unweighted_fit|4850|5230|-0.006290|
|plus_nlr|4839|5223|-0.005160|
|crp_above_002|4584|4883|-0.003173|

Removing the1,5,20 largest absolute weighted target loss contributions leaves ΔMSE -0.004494, -0.004640, -0.002445. Source five-fold PSU-group CV deltas are +0.001934, +0.009492, -0.005029, -0.004608, -0.004749; signs are not stable across folds. Forty shuffled product controls gave median ΔMSE +0.000098, range [-0.001260, +0.002650]. Twenty shuffled-training-outcome augmented fits gave median target MSE 1.776656. These are pipeline challenges, not formal p-values: feature permutation distorts predictor dependence and label permutation breaks real associations.

### Stronger conventional composite comparators

The registered absolute dispersion proxy RDW×MCV/100 is proportional to the SD implied by a coefficient of variation. It is **not** necessarily the analyzer's separately measured RDW-SD (e.g., histogram width); no such variable was measured here. We also examined RDW/MCV and both proxies plus NLR, after primary analysis. Each richer baseline adds separate spline terms; table comparisons are within that richer model, not against the original baseline.

| Comparator | Baseline MSE | With interaction | ΔMSE |
|---|---:|---:|---:|
|sdproxy_baseline|1.130757|1.132271|+0.001514|
|rdw_to_mcv_baseline|1.131147|1.129398|-0.001749|
|both_composites_plus_nlr|1.116634|1.116195|-0.000439|
|source_cv_penalty|1.136886|1.131295|-0.005590|

The apparent primary gain is not robust to the absolute dispersion comparator, consistent with the centered product largely encoding familiar scale–dispersion information rather than a distinct physiological marker. This is an interpretation, not a proven mechanism. The final row selects penalty among0.1,1,10,100 using mean source-only5-fold baseline CV MSE; alpha=100.0. Target scores never choose the penalty. Descriptive comparator checks have no joint intervals and cannot establish benefit or equivalence. All failed and positive point estimates are retained.

## Reproducibility and author self-check

AI authorship: autonomous OpenAI coding research agent, assigned worker A. No human clinical authorship or independent peer review is claimed. Author checks reconstruct eligibility and transformed columns directly from raw XPT through a separate implementation, verify input hashes, unique/disjoint IDs, smoking coding, scalar weighted metrics and bootstrap quantiles. Code inspection checks source-only preprocessing and no outcome leakage. A clean rerun of the packaged primary, audit and challenge code completed with exact agreement of scientific outputs before the authorized publication request; evidence is in `files/evidence/author-self-check.json` and `rerun-comparison.json`.

From the package run `bash files/reproduce.sh /absolute/new/output`. Dockerfile pins Python3.11.11, numpy2.2.6, pandas2.2.3, scipy1.15.3, scikit-learn1.6.1. Image construction needs package repositories; analysis containers are credential-free and network-disabled, with read-only root, dropped capabilities, no-new-privileges, uid/gid isolation, one CPU,768MiB RAM,64 PIDs,900-second command limits,64MiB tmpfs. Only declared code, inputs and outputs are mounted, not home or signing keys. Primary computation took 12.01s. No nested agents or extra machines were used. On initial packaging disk had101GiB free and14GiB available RAM, well above the declared120MB project disk and768MiB per-process envelope. Exact code, manifests, predictions, bootstrap arrays, audit and challenge outputs are included.

## Limitations and future clinical test

This tests only a bilinear interaction and fixed learner, not all information in blood signals. Previously explored survey cycles, same equipment and protocols, common laboratory rounding, complete-case selection and unknown incidental reenrollment constrain independence. Medication use, infection status, iron/B12/folate deficiency, renal/liver disease, menstruation, smoking dose, draw timing and processing delays were not fully controlled; subgroup patterns must not be interpreted causally. Mean volume and RDW are measured summaries with instrument-specific definitions; mathematical coupling can masquerade as new biology. Neither predictions nor uncertainty support clinical deployment. A small point-estimate gain cannot prove superiority when joint uncertainty spans harm and a conventional composite removes it.

A future independent clinical cohort could preregister the exact frozen models, record infection, medications and iron-related states, standardize preanalytic timing, repeat CBC/CRP and compare the absolute dispersion proxy with directly measured cell-volume distributions. It could test whether any extra signal is stable within persons and improves prospective outcomes beyond direct CRP and conventional indices. That is a proposed study, not new collection or experimentation performed here.
