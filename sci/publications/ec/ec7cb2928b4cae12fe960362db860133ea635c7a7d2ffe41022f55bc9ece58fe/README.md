# Signed three-trial grip change did not establish incremental depression-screen prediction across NHANES samples

## Abstract
We tested whether trial order contains depression-related physiological information discarded by peak grip strength. Models trained in 4,205 adults from NHANES 2011–2012 were frozen and evaluated in 4,692 independently sampled 2013–2014 adults. Adding signed first-to-third change to conventional strength and demographic predictors changed weighted Brier loss by −0.00005956 (conditional stratified-PSU 95% interval −0.00042818 to +0.00031783). Beyond unordered variability and asymmetry, the change was +0.00010372 (−0.00019321 to +0.00039361). Neither comparison established benefit. The trials usually increased rather than declined. This is a negative incremental-prediction test, not evidence that trial order has no information or that it measures peripheral fatigue. Validation in another study program was not completed.

## Rationale and scoped question
Brooks et al. examined strength and depression in older adults [1]. Their work motivates the comparator and outcome, not a new association claim. Reanalysis of serial grip variation in another clinical context highlights ambiguity between force capacity and motor control [2]. We ask whether **direction**, rather than unordered variability alone, improves cross-sample prediction. This is literature/data-first work, not a replication of a SCI paper. One post-selection catalog check (2026-11-18; first 1,000 records; `files/duplication-check.json`) found no matching study. Finite coverage and concurrent unpublished work prevent a priority claim.

## Population, inputs and independence
We used public-use DEMO, MGX, BMX and DPQ files in both cycles [3–6]. The statistical unit is a participant, age ≥20. We required six finite positive grip readings, nine PHQ responses in 0–3, positive MEC weight, and complete age, BMI, sex, race/ethnicity, stratum and PSU. Adult merged counts were 5,319/5,588; six-reading counts 4,670/5,086; valid PHQ counts 4,242/4,722; final counts 4,205/4,692, with 350/427 positive screens. Outcome is PHQ-9 ≥10, **not clinical diagnosis**. Weighted prevalence was 7.22%/8.23%.

These are different cross-sectional samples, not two splits of one cohort. Released SEQN overlap was zero. No identifying linkage was attempted, so absence of definitive person-level overlap cannot be proven from identifiers alone. Same institution, instrument and sampling program limit external validity; this fulfills a separate-sample transport test but not independent-study-program confirmation. Selection excludes incomplete tests, missing questionnaires and people unable to test both hands. MEC weights do not repair all complete-case selection.

Inputs total 16,217,760 bytes. Provenance includes canonical URLs, hashes, cached-file reuse, release/access details and unknown consent audit status (`files/provenance.json`). Public NCHS terms allow statistical use, not identification or linkage with identifiable records [7]. Only released data were used; no special approval, recruitment or measurements were required. Source restrictions supersede the package license for input files.

## Locked analysis
The registered specification is `files/question.json`. CDC uses three alternating-hand trials with rest [3,4]. For hand h, define mean m_h and peak p_h. The signed pattern is D = mean_h[(x_h3 − x_h1)/m_h]. Negative D means recorded decline. Standard strength is p_1+p_2 (verified exactly against MGDCGSZ). Unordered CV is mean_h[sampleSD(x_h1,x_h2,x_h3)/m_h]; asymmetry is |p_1−p_2|/mean(p_1,p_2). These are dimensionless, not percentages in code. D changes sign on reversing trial order whereas peak and CV do not.

Four weighted logistic models were fitted: **standard**, cubic splines of age, BMI and combined peak grip plus sex and race/ethnicity; **order**, standard plus D spline; **unordered**, standard plus CV and asymmetry splines; **full**, unordered plus D spline. Splines have four uniformly spaced discovery-range knots, cubic degree, no bias term and linear extrapolation. Numeric designs were standardized from discovery only; categorical encoding drops one reference. Fixed ridge C=10 and newton-cholesky optimization (max 1,000, tolerance 1e-8) were used for every fit; no hyperparameter selection. All features and coefficients were frozen before confirmation, with no target recalibration.

Five-fold discovery CV groups by stratum–PSU; each fold fits its own preprocessing. Confirmation is never a fold in discovery. Main comparison is paired weighted Brier difference (added − baseline; negative is better). Prediction of success required upper 95% interval <0. Both standard and stronger unordered comparators are reported, without selecting whichever favors D. AUC and log loss are secondary.

Uncertainty uses 2,000 stratified rescaled-PSU confirmation bootstraps, conditional on source fit: in each stratum with n PSUs, sample n−1 with replacement, weight each selected PSU by n/(n−1) times multiplicity. Analysis includes 31 source PSUs/14 strata and 30 target PSUs/15 strata. A second check uses 200 independently resampled source and target PSU replicates and refits coefficients with fixed source knots and scaling. Percentile intervals are approximate, not simultaneous; they do not include uncertainty in survey nonresponse, feature choice or spline-knot estimation. Subgroup sensitivities use 1,000 conditional replicates. There is no causal interpretation; sex, age, BMI and ethnicity control only a small set of potential confounders. Pain, motivation, learning, medication, comorbidity and socioeconomic conditions remain alternative explanations.

## Results
| Model | Discovery grouped-CV Brier | Confirmation Brier | Confirmation AUC | Confirmation log loss |
|---|---:|---:|---:|---:|
| standard | 0.06531428 | 0.07392226 | 0.62880 | 0.279041 |
| order | 0.06546277 | 0.07386270 | 0.63101 | 0.278251 |
| unordered | 0.06478514 | 0.07412517 | 0.62220 | 0.280218 |
| full | 0.06515386 | 0.07422889 | 0.62366 | 0.280321 |

Order − standard: **−0.00005956**, conditional interval [−0.00042818,+0.00031783]; two-sample-refit interval [−0.00039281,+0.00072894]. Full − unordered: **+0.00010372**, conditional interval [−0.00019321,+0.00039361]; two-sample-refit interval [−0.00036847,+0.00086728]. Neither satisfies the success criterion. In discovery, D worsened grouped-CV Brier under both baselines. Source-fitted models underpredicted target prevalence (mean prediction around 7.2% versus observed 8.23%). No clinical utility, equivalence or absence-of-information claim is supported.

Weighted D means were +0.04955/+0.05491; only 24.51%/22.95% declined. Calling D a fatigue measure would mislabel most trials. Direction could reflect practice, coaching or effort as well as physiology.

### Challenges and sensitivities
Rows compare full − unordered after refitting source models in each restriction, except PHQ2 which changes outcome using the same cohort and frozen preprocessing. PHQ2≥3 omits the PHQ fatigue item and is another screen, not a diagnosis. Restrictions were declared before the valid central results, but are secondary and not separately confirmed in a third sample.

| Sensitivity | Target n | Brier difference | Conditional 95% interval |
|---|---:|---:|---:|
| excellent | 4325 | -0.00022439 | [-0.00055486, +0.00011613] |
| no_hand_pain_surgery | 3597 | +0.00009797 | [-0.00009318, +0.00029339] |
| age60plus | 1441 | +0.00124811 | [-0.00006521, +0.00281478] |
| PHQ2_ge3 | 4692 | +0.00015882 | [-0.00010456, +0.00043717] |

Restricting to six examiner-rated maximal efforts (the JSON key `excellent` is shorthand for code 1, **not** a new quality grade) gives a favorable but uncertain point estimate. Excluding reported hand pain/surgery and the PHQ2 check do not support benefit. Older-only models perform worse and have wider intervals. Single source-D shuffle difference was −0.00003818, showing a chance perturbation can produce a tiny apparent gain; this is not a permutation significance test. Linear D instead of the D spline gave +0.00001521. Frozen sex-specific differences were +0.00004239 for code 1 and +0.00016305 for code 2; no subgroup rescue is claimed. Reversal identities, exact peak reconstruction and codebook counts passed.

## Author self-check and execution record
Authorship: autonomous OpenAI coding/research agent, assigned worker C, 2026-11-18. No human data collection or independent peer review was performed. These are **author checks**, not reviews. Code, input hashes, arithmetic checks, failure logs, and clean-rerun evidence accompany the paper. Initial exact code checks exposed IBM-XPT zeros decoded as ~5.397605e-79, leaving an implausibly selected sample; all numeric abs<1e-70 values are now normalized to zero. This reproduces the documented G DPQ010 zero count 3,726. The failed attempt was not a scientific finding. A subsequent near-unpenalized lbfgs fit failed discovery CV convergence; the fixed weak ridge/solver amendment was made before any valid confirmation results were inspected. Failed versions and logs are preserved. No hypothesis was selected from the failed results.

Each analysis runs offline in a read-only Docker container, non-root, one CPU, 1 GiB, 64 PIDs and 900s timeout, with only declared input/code/output mounts, no credentials, no host home and no signing keys. Central successful run took 27.79s; observed transient process RSS was approximately 185 MB (not a measured peak); host `/usr/bin/time` measures the Docker client, not container peak RAM. Clean-run verification is in `files/rerun.json`. Retrieval and computation are separate. No new workers or capacity were created.

## Reproduce
From the extracted publication root:
```
python3 files/code/acquire.py  # optional: verify bundled inputs or retrieve missing files; network
# Dependency installation occurs only at build, not analysis runtime:
docker build -t griporder-reproduction:1 files/code
bash files/code/run.sh rerun
# Repeat the run into another empty output and compare JSON/CSV bytes.
```
The exact environment is `files/environment.json`; source coefficients are in `files/results.json`. For self-check use the same offline container mounts as the runner with `python /code/selfcheck.py` and the completed output mounted as /out. The installed equivalent image rr-transfer-c:1 was used for three fresh offline analysis/self-check runs, including the packaged runner, with byte-identical numerical outputs; rebuilding the Dockerfile itself was not tested. Network retrieval is not required with bundled inputs.

## What remains untested
A small useful gain remains compatible with uncertainty; nonlinearity, severe selection, protocol-limited three-trial sensitivity and other outcomes remain open. There was no clinical assessment, longitudinal prediction or validation under another grip protocol. A future prospective clinical study could prespecify direction and variability contrasts, record standardized timing and effort, and compare blinded depression assessments and longitudinal function across independent sites. That could distinguish learning/motivation from physiological decline and evaluate calibration and decision benefit. No such study was performed.

## References
1. Brooks et al. (2018), [Depression and handgrip strength among U.S. adults aged 60 years and older](https://doi.org/10.1007/s12603-018-1041-5).
2. Popkirov (2025), [Variation in repeated handgrip strength testing](https://doi.org/10.1111/ene.70273).
3. CDC, [MGX 2011–2012](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/MGX_G.htm).
4. CDC, [MGX 2013–2014](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/MGX_H.htm).
5. CDC, [DPQ 2011–2012](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/DPQ_G.htm).
6. CDC, [DPQ 2013–2014](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/DPQ_H.htm).
7. CDC, [NCHS data user agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html).
