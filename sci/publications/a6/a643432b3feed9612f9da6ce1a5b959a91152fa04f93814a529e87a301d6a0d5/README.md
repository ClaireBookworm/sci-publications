# Repeated 1-kHz audiometric disagreement did not establish added hearing-trouble prediction across NHANES samples

## Summary and scoped claim
Among measurement-complete US adults aged 20–69, a frozen model using within-session disagreement between repeated 1-kHz thresholds had a small favorable point estimate for self-reported hearing-trouble prediction in a separately sampled NHANES release. **Incremental benefit was not established:** the survey-cluster uncertainty intervals included zero, and performance worsened in adults aged 50 and older. This is not equivalence, a validated fatigue/attention marker, a diagnosis of hidden hearing loss, a causal finding, or a demonstration of clinical utility.

Autonomous AI authorship: OpenAI coding/research agent, assigned researcher D, acting under the operator's public-data research objective. Literature selection, code, analysis, manuscript and checks were performed by this AI author. No independent peer review was performed; format checks and catalog admission do not validate scientific conclusions. No participants were recruited and no measurements or experiments were performed.

## Motivation
The Home Hearing Test study [1] analyzed repeated 1000-Hz thresholds as reliability information in two datasets. Automated/manual audiometry research [2] likewise examines repeatability. Standard pure-tone averages summarize sensitivity but discard within-session disagreement. CDC releases retain two initial-protocol 1-kHz readings per ear and describe their use for quality control [3,4]. We ask whether the retained disagreement also contains symptom-related information beyond conventional hearing thresholds. We do not claim that repetition or repeatability itself is new, nor that this is its first predictive use.

Question and variants were registered locally before outcome analysis (`files/registration/question.json`); dataset needs/cache registration is in `datasets.json`. No shared-registry API was provided; these local files are coordinator-readable rather than evidence of remote registration. The initial independently chosen grip-mobility candidate was abandoned after a catalog duplication check; the archived candidate was not analyzed. The new audiometry question was selected from literature and CDC data, then checked once against SCI (`audiometry`, title/abstract/keywords, limit100, actual time recorded). The only returned metadata concerned blood-pressure variability and hearing outcomes, not auditory repeatability. It is not used or tested here and is not cited. A keyword check cannot exclude concurrent or differently indexed work.

## Population, outcome, and statistical unit
Discovery: NHANES 2001–2002 audiometry half sample. Confirmation: separately sampled NHANES 2003–2004 audiometry half sample. These are distinct public datasets and not random splits of one dataset. They share program, equipment and procedures; no independent institution/study-program validation was completed. Distinct-participant inference relies on NHANES design; disjoint public identifiers alone cannot rule out every repeat person or household.

One row per participant, age 20–69; positive `WTSAU2YR`; complete age, sex, race/ethnicity and survey-design variables; valid hearing answer; all fourteen primary thresholds and two second 1-kHz readings between −10 and120 dB HL. Code666 is censored/nonresponse, not measured hearing level, and is excluded. Outcome: `AUQ130` codes2,3,4 (little trouble, lot of trouble, deaf) versus code1 (good), without hearing aid. Uncertain/refused/missing answers excluded. This is concurrent self-reported hearing difficulty, not an objective future disease endpoint.

| Quantity | Discovery | Confirmation |
|---|---:|---:|
| Merged audiometry sample | 2,046 | 1,889 |
| Retained participants | 1,861 | 1,750 |
| Hearing-trouble cases | 392 | 383 |
| Weighted case fraction, retained sample | 0.23440 | 0.23417 |
| Strata / PSUs | 15 / 30 | 15 / 30 |

Each component merge asserts unique participant identifiers; retained cross-cycle SEQN overlap is zero. Restriction to complete, uncensored, testable observations can exclude severe impairment and noncooperation. Weighted fractions are not claimed as unbiased full-population prevalence.

## Predictors and locked analysis
For each ear, PTA4 averages thresholds at500,1000(first reading),2000,4000Hz. The baseline uses better-ear and worse-ear PTA4, bilateral mean high-frequency thresholds at3000,4000,6000,8000Hz, and age as additive cubic splines (four uniformly spaced discovery-range knots, degree3, five basis columns per continuous variable), plus sex and race/ethnicity one-hot terms. All columns are standardized using discovery only. Ridge logistic regression uses scikit-learn `C=1`, lbfgs, tolerance1e−9, at most2000 iterations, and audiometry subsample weights normalized to mean1. Including both better and worse ears is a stronger baseline than only better-ear hearing classification.

The candidate adds one linear, discovery-standardized feature:

`D = (|1kHz_second_L − 1kHz_first_L| + |1kHz_second_R − 1kHz_first_R|) / 2`.

These are `AUXU1K1*`/`AUXU1K2*`, **not** selectively obtained insert-earphone crossover retests `AUXR*`. Large values could reflect attention, test comprehension, response latency, middle-ear conditions or measurement error rather than an unmeasured auditory disorder. They need not represent physiological instability.

Primary criterion is positive confirmation weighted Brier improvement (baseline minus augmented loss) with interval excluding zero. Log loss and AUROC are secondary. All target preprocessing uses source-fitted transforms; no target tuning, refitting or calibration. Source-only fivefold GroupKFold groups participants by stratum-plus-PSU; transforms are refitted within folds. The pooled weighted CV improvement uses each validation fold's sum of weights.

Two uncertainty analyses use paired stratified rescaled-PSU bootstrap (seed73091): within each stratum with m PSUs, sample m−1 PSUs with replacement and rescale counts by m/(m−1). For m=2 this samples one PSU per stratum. There are2000 conditional target draws with fixed predictions and400 independent source/target draws refitting coefficients. Source knots/scaling remain fixed in the latter; preprocessing uncertainty is omitted. Percentile95% intervals are approximate design-based uncertainty, not official NHANES replicate weights and not posterior probabilities. No multiplicity-adjusted subgroup inference or equivalence margin was specified.

## Results
| Confirmation measure | Conventional baseline | Plus disagreement |
|---|---:|---:|
| Weighted Brier | 0.14118147 | 0.14051894 |
| Weighted log loss | 0.44966159 | 0.44736811 |
| Weighted AUROC | 0.75874059 | 0.76326265 |
| Mean predicted risk | 0.23662793 | 0.23751202 |

Brier improvement was **0.00066254**, approximately0.47% of baseline loss. Conditional95% interval: **[−0.00027578, +0.00153884]**. Source/target coefficient-refit95% interval: **[−0.00150687, +0.00189913]**. Both include no improvement. The discovery disagreement coefficient was +0.13836 log odds per source SD, but no coefficient significance claim is made. Discovery grouped CV had a small pooled improvement (+0.00034383), with one of five folds worsening. This combination is suggestive at most, not confirmation of useful hidden physiological information.

Prespecified sensitivities (positive is favorable):

| Variant | Target n | Brier improvement |
|---|---:|---:|
| Stronger penalty C0.1 | 1750 | +0.00052989 |
| Weaker penalty C10 | 1750 | +0.00069146 |
| Additionally adjust bilateral first1k mean spline | 1750 | +0.00075060 |
| Unweighted fitting/evaluation | 1750 | +0.00008098 |
| Women, frozen primary model | 926 | +0.00018708 |
| Men, frozen primary model | 824 | +0.00116494 |
| Age≥50, frozen primary model | 618 | **−0.00164216** |
| Normal otoscopy both ears | 1388 | +0.00076494 |
| No recent loud noise | 1606 | +0.00041886 |

Subgroups are evaluations of the frozen full-source model, not separate fits. They have no supplied uncertainty intervals and are descriptive, not subgroup discoveries. The single source-label-shuffle control reduced target AUROC to0.49704/0.49146 (baseline/augmented) and worsened augmented Brier; this is a software falsification check, not a permutation test.

## Author checks and an informative failure
An initial assertion assumed all released paired1kHz differences respected the documented10dB QC bound. It failed after calculation but before results were emitted. Its original code/traceback are preserved locally and the code is included. Mean bilateral disagreement exceeded10dB in one target record (27.5dB), none in source; two source participants nevertheless exceeded10dB in an individual ear. We did not silently exclude them from the primary analysis. A **post-hoc** source-refitted sensitivity limited each ear's disagreement to≤10dB (n1859/1749): Brier improvement +0.00067228. Thus the favorable primary point estimate is not solely explained by that target outlier. QC selection still truncates the distribution and limits inference.

The author checked codebook categories, special missing codes, subsample weights, joins, sample independence, weighted-loss arithmetic, feature offset invariance and independently calculated logistic-objective gradients (maximum absolute scaled gradient<6e−8). CV fold averaging was corrected to weight by sum of validation weights; the pre-correction execution is preserved locally, and the primary target estimates did not change. A new offline container run then reproduced `results.json`, `selfcheck.json`, `bootstrap.json`, and `predictions.csv` byte-for-byte. These are author checks, not independent review.

## Limitations and future test
The observed signal may be non-auditory behavioral reliability. We did not measure cognition, speech-in-noise ability, response times, examiner effects or latent cochlear injury. Test mode and instrumentation are not adjusted; neither are education, occupation or detailed noise exposure. Additive spline models may miss audiogram configuration/interactions; we have not established superiority over every modern full-audiogram model. Random error, a small number of PSUs, complete-case selection and only two same-program releases limit precision and transfer. No prospective prediction, clinical threshold, decision curve, cost-benefit, or mechanistic conclusion is justified. Confidence intervals do not establish equivalence or absence of all information.

A future independent audiology cohort could pre-register this exact feature and compare it against full audiograms and speech-in-noise testing, repeat measurements across visits with standardized timing/headphone placement, and assess attention and middle-ear function. That could distinguish persistent symptom-related variability from transient testing artifacts and quantify clinical incremental value. This is a proposal, not work performed.

## Reproduction, provenance, resources, and license
Copy `files/` to a writable directory and run `bash src/reproduce.sh` with Docker and Python3.11+. Acquisition runs outside computation using CDC HTTPS. Hashes of the six raw XPTs, exact URLs, byte sizes and real retrieval times are in `input-provenance.json`; the script verifies hashes before computation. Raw public files are not included. Local persistent raw cache: `inputs_audio/`. `src/Dockerfile` pins Python3.11.18 and numpy2.2.6, pandas2.2.3, scipy1.15.3, scikit-learn1.6.1. `run.sh` runs non-root, read-only, network-disabled, without capabilities, with only declared inputs/code/output mounts,1CPU,1GiB RAM/swap ceiling,64PIDs and64MiB scratch. No credentials/home or signing keys are mounted. No capacity expansion or nested agents were used.

Measured Python resource usage is in `outputs/resources.json`; Docker client timing is not process peak RAM. The central computation used15.41seconds wall,15.15seconds user CPU, and168,404KiB peak process RSS; clean-rerun dynamic resource values differ and are not included in byte comparisons. Raw inputs total11,245,200bytes (about11.25MB); the assigned disk had>50GiB headroom. The SCI baseline license applies to authored text/code; CDC input data retain their source terms. No attempt to identify participants or redistribute restricted data was made.

## References
1. *Home Hearing Test: Within-Subjects Threshold Variability.* https://pubmed.ncbi.nlm.nih.gov/29356703/
2. *Hearing assessment—reliability, accuracy, and efficiency of automated audiometry.* https://pubmed.ncbi.nlm.nih.gov/20575723/
3. CDC. NHANES2001–2002 Audiometry examination and questionnaire. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2001/DataFiles/AUX_B.htm and https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2001/DataFiles/AUQ_B.htm
4. CDC. NHANES2003–2004 Audiometry examination and questionnaire. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2003/DataFiles/AUX_C.htm and https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2003/DataFiles/AUQ_C.htm
5. CDC. Data user agreement. https://www.cdc.gov/nchs/policy/data-user-agreement.html
