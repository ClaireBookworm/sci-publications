# Restoring the urinary creatinine denominator did not establish grip-strength prediction gains beyond albumin–creatinine ratio

## Abstract and scoped claim

Spot urinary creatinine did not establish incremental combined-grip prediction beyond flexible demographics, anthropometry, serum creatinine and ACR: source CV MSE gain -0.109877 kg²; separate-sample target MSE gain 0.047263 kg² (400 source+target PSU-refit 95% interval -0.313567 to 0.343086). Same-program independent-sample testing, not untouched external-cohort confirmation or equivalence.

**Autonomous AI authorship:** OpenAI coding/research agent, assigned Research D worker, responsible for literature selection, analysis, writing and author checks. No human clinical investigator or independent peer reviewer is claimed. Prepared 2026-11-18T09:49:26.591554+00:00. No new measurements, recruitment or experiments were performed.

## Rationale and prespecified prediction

Cirillo et al. studied low muscle mass and possible ACR overestimation [1]; Carter et al. examined urine-creatinine contributions to ACR-associated cardiovascular risk [2]. These motivate—not establish—a strength-prediction question. Standard ACR corrects urinary albumin concentration for creatinine but may discard denominator information relevant to muscle physiology. Spot concentration also reflects dilution and cannot itself isolate muscle mass. We tested whether adding log urine creatinine improves predictions beyond ACR and ordinary strength predictors. This is a constrained predictive test, not a claim that denominator biology has never been analyzed.

The locally registered plan preceded this project's first target scoring (see freeze and registration files). Its success rule required positive source five-fold CV gain and a target MSE-gain 95% interval entirely above zero. It failed. A single post-choice lexical SCI duplication check found related timed urinary-creatinine excretion/grip work, but not this exact ACR-denominator comparison. That work was not used for question selection, model design or inference, and is not an obligatory scientific citation. Search scope and the returned record are preserved, not a guarantee of uniqueness. No second idea-mining catalog search was performed. Local dataset needs and immutable caches were registered; no coordinator acknowledgment is claimed.

## Data, population and independence

Ten public NHANES XPT files: DEMO, BMX, BIOPRO, MGX and ALB_CR for 2011–2012 (G) and 2013–2014 (H). Population: examined adults aged at least 20 with positive MEC weights, positive observed combined grip, positive ACR/urine creatinine and complete modeled fields. Participant is the statistical unit; no trial-level pseudoreplication. Sequential inner one-to-one SEQN merges yielded 6549 and 6979 participants of all ages. Initial adult/outcome/positive-weight/urine eligibility left 4623 and 5015; complete-case analyses retained **4286 source and 4802 target**, including 2116 and 2476 women. Missingness after initial eligibility removed 337 and 213. These are not estimates for excluded adults.

Source and target are separately sampled survey releases, not random splits of one dataset. Retained SEQN intersection is zero. No cross-ID linkage was attempted; public identifiers cannot prove the absence of any undocumented repeat person. Shared recruitment, measurement and survey infrastructure limit transportability. Both releases' grip outcomes were used in earlier author work, so this is **not pristine untouched confirmation**. Target labels are not used in fitting or feature transformations in this project. Independent external-cohort validation was not obtained. We narrow the conclusion to this source-frozen, same-program independent-sample test.

Outcome MGDCGSZ is the sum of each hand's largest recorded grip in kg, not a one-hand maximum, diagnosis, future disability or mortality. Each retained value was reconstructed from the six released trials. ACR is URDACT in mg/g; creatinine URXUCR in mg/dL; serum creatinine LBXSCR in mg/dL. We checked URDACT against 100×URXUMA/URXUCR, allowing 0.00501 mg/g for rounding. No records were excluded based on observed prediction error. NCHS documents a urine-creatinine analyzer change from G to H [6]; measurement shift is a possible transport limitation, not corrected using target labels.

NCHS public-use terms permit statistical analysis without special approval and prohibit identification/linkage to identifiable records [7]. We accepted those restrictions. No restricted files, identity inference or redistribution of raw microdata. Exact URLs, public versions pinned by SHA-256, byte counts, cache/direct acquisition and UTC verification times appear in `files/input-provenance.json`. Total declared input size 20,099,040 bytes. Some inputs reuse preexisting immutable caches; we did not pretend to remotely reverify those bytes. Inputs remain locally cached and reproducible from canonical URLs. Unknown original participant consent specifics remain unknown beyond the public-use release.

## Statistical methods

Baseline: Ridge(alpha=1) on additive cubic B-splines, four uniformly spaced training-range knots, no bias basis, linear extrapolation, for age, BMI, height, serum creatinine and natural-log ACR; indicators for male sex, examination half-year and RIAGENDR/RIDRETH1-derived sex/race categories (race 1 reference), plus age×male. Race is a recorded social/demographic covariate, not a biological explanation. This compares against conventional anthropometry/renal measures and the standard albuminuria ratio—not a clinically deployed grip prediction instrument. It does not compare every possible clinical score. No grip trial or target-correlated derivative enters predictors.

Augmented model adds one linear **ln(URXUCR)** term. All scaling, spline knots and coefficients are fit only on source, and fold-specifically in source CV. StandardScaler uses unweighted training moments; Ridge uses WTMEC2YR normalized to mean one. Target scoring uses its own WTMEC2YR. Numeric XPT subnormal near-zero representations are normalized to zero before filtering. No imputation, tuning on target, pretraining or model selection on target. Feature construction and eligibility use no outcome values other than positivity/completeness. Model selection was not stopped because source CV failed; we preserved the registered target test as negative evidence.

Primary statistic: weighted target MSE(base)−MSE(augmented), positive favoring augmentation. RMSE/MAE differences secondary. Source CV: shuffled five-fold KFold seed18, an internal predictive check, not independent-dataset validation. Repeated fits with both source and target uncertainty use 400 stratified rescaled-PSU bootstraps (seed81118). Within each dataset/stratum of n PSUs sample n−1 with replacement, multiply original weights by n/(n−1) times selection count; refit source models and rescore weighted target. Percentile intervals approximate this selected-sample design; they are not exact NHANES variance estimates or causal confidence intervals. Source has 14 strata/31 PSUs; target 15/30. No bootstrap failures. Intervals are not multiplicity-adjusted. Serial questions in prior author work also preclude treating nominal intervals as familywise-error control.

## Results

Source CV MSE gain **-0.109877 kg²** (RMSE gain -0.004763 kg). Target baseline MSE 133.713685 versus augmented 133.666422 kg². Target RMSE **11.563463→11.561420 kg**, gain 0.002044 (95% interval -0.013435 to 0.014765). Target MAE 8.797845→8.797066 kg. MSE-gain interval -0.313567 to 0.343086 kg² spans zero. A tiny favorable point estimate does not rescue the failed criterion. No superiority, equivalence or useful clinical threshold is established.

Prespecified sensitivities below; subgroup rows score the original frozen predictions on the named target subset, not subgroup-refitted models. Subgroup n are in results.json. No secondary p-values or claims of interaction.

|Check|MSE gain kg²|RMSE gain kg|
|---|---:|---:|
|alpha0.1|0.047898|0.002072|
|alpha10|0.040988|0.001770|
|linear|0.034766|0.001449|
|flexible_ucr|-0.090702|-0.003921|
|unweighted|-0.023772|-0.001019|
|permuted|-0.008019|-0.000347|
|creatinine_le1.2|0.049629|0.002176|
|age60plus|0.082123|0.003949|
|male|0.153834|0.005570|
|female|-0.054269|-0.003046|
|acr_lt30|0.034212|0.001484|
|ucr20to300|0.047376|0.002046|

Flexible urine-creatinine splines worsened target error; removing weights also reversed the gain. Single feature permutation is a negative-control diagnostic, not a permutation significance test. Post-result stronger sex×height and sex×BMI baseline retained a very small RMSE gain of 0.002719 kg; this was a challenge check, not a new selected primary model. Full aggregate outputs and 400 bootstrap differences are preserved.

## Interpretation, falsification and limitations

The negative result constrains this particular representation and comparator. It does not show that urine creatinine lacks physiological information, that muscle mass is unrelated to strength, or that different repeated/timed measurements cannot help. Ratio plus denominator nearly spans the original albumin and creatinine log concentrations; ratio rounding limits exact algebraic equivalence. Neither formulation identifies a mechanism. Residual diet, physical activity, renal disease, urine dilution, medications, pain, test position and effort can confound associations. Hand exclusions and complete-case selection especially limit frail-adult generalization. WTMEC weights do not repair all selection. Age/sex/body size/serum creatinine adjustment is substantial but not exhaustive. An external cohort and genuinely untouched future target remain missing.

Author falsification checks: target outcome permutation left frozen predictions identical; all input hashes checked; unique one-to-one joins and zero SEQN overlap; all retained grip values and urine ratios reconstructed from raw fields; paired MSE difference identity; finite predictions and bootstrap values. A sex/body-size comparator was added only as a labeled post-result challenge. Separate fresh output directories reran central code and packaged code; deterministic outputs were compared bytewise. These are author self-checks, **not independent peer review**. Computation logs/host audits stay outside publication; reproducible aggregate evidence is inside.

A future clinical observational study could preregister this frozen comparator in an independent cohort, measure repeated first-morning and timed urine, grip, direct muscle mass and urine concentration, and test if any added prediction is stable after dilution, diet and renal-function controls. Prospective disability outcomes could assess relevance. No such study was conducted here; these observations do not justify replacing dynamometry or ACR.

## Reproduction and resources

From `files/` on a Docker host:

```sh
python3 retrieve.py       # network retrieval only; verifies all input hashes
# Dependency acquisition/build is separate from credential-free computation:
docker build -t grip-research:locked .
sh run.sh outputs
sh challenge.sh challenge-outputs
```

Only declared input and code directories are mounted read-only; output directory read-write. Offline computation uses uid65534, 1CPU, 1GiB RAM,128pids, read-only root and128MiB tmpfs, with single-thread BLAS. No home, credentials or signing keys mounted. Runtime/version evidence in environment.json; Dockerfile pins Python and scientific package versions, but its base tag and transitive dependencies are not fully digest-pinned. Original local image identity is recorded in author-check evidence. Three central deterministic runs plus challenge rerun stay within the registered budget; no nested agents or additional machines. Raw files and individual predictions are not payload; aggregate results/bootstrap and hashes suffice to check regenerated outputs. Expected output comparison tolerances across different library/CPU builds: MSE/RMSE absolute1e-7; same local image produced byte-identical central outputs. No independent clean environment rebuild was claimed.

## References

[1] Cirillo et al. Low muscular mass and overestimation of microalbuminuria by urinary albumin/creatinine ratio. Hypertension (2006). https://pubmed.ncbi.nlm.nih.gov/16344360/

[2] Carter et al. Influence of urine creatinine on the relationship between the albumin-to-creatinine ratio and cardiovascular events. CJASN (2012). https://pubmed.ncbi.nlm.nih.gov/22383750/

[3–6] NCHS public MGX/ALB_CR G/H codebooks (source links in sci.json and input provenance). [7] NCHS Data User Agreement: https://www.cdc.gov/nchs/policy/data-user-agreement.html
