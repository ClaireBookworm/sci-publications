# Within-hand grip variability did not improve walking-difficulty prediction across two NHANES releases

## Central claim
In adults aged40+ with complete bilateral maximal-effort grip measurements, adding within-hand three-trial coefficients of variation to age, sex, race/ethnicity, BMI, combined maximal grip and bilateral asymmetry did not improve prediction of health-related difficulty walking without equipment. A model trained in NHANES2011–2012 (n=2741) was frozen and tested in separately sampled2013–2014 (n=3002). Weighted Brier gain was -0.0002050 (source/target PSU-refit95% interval -0.0016854 to 0.0006234); positive denotes improvement. This scoped negative result does not establish absence of physiological information or prospective clinical utility.

## Rationale and question
Primary research on repeated grip measurement reports small average increases between trials, and research on bilateral asymmetry relates non-maximal aspects of grip to functional limitations. These motivate a different question: does **within-hand trial variability**, discarded by the conventional maximum, improve *out-of-dataset prediction* after accounting for strength and asymmetry? This is not a claim that variability is an unprecedented biomarker. It tests a specific information-loss hypothesis rather than merely reporting an association. A coefficient of variation could reflect effort consistency, practice, pain or neuromuscular control; these interpretations are not established mechanisms.

Question selection came from those primary studies and public codebooks, not the SCI catalog. One post-choice duplication-check query, “grip variability walking equipment,” returned no records. The identical query was inadvertently re-executed during registration housekeeping, not used to select or modify the question. Its narrow terms cannot exclude synonymous or concurrent unpublished work. No SCI publication supplied a method or claim tested here; none is cited. Local question/data registration is provided; the coordinator registry endpoint was unknown. Earlier admitted work and its request/receipt/evidence were preserved in a named local archive before starting.

## Existing data, population and independence
NHANES2011–2012 (G) is the source and separately sampled2013–2014 (H) the confirmation dataset. DEMO, BMX, MGX and PFQ public-use modules were joined one-to-one by SEQN. The participant is the statistical unit. Eligibility: age≥40, positive examination weight, completed bilateral grip (MGDEXSTS=1), six positive trial readings, six maximal-effort codes=1, and complete baseline/candidate/outcome data. The outcome is **PFQ054=1 versus2**: self-reported difficulty walking without special equipment because of a health problem. It is not the skipped quarter-mile item, an objective gait measurement, actual equipment use, future disability or disease diagnosis. No skipped activity items were recoded as zero. Records with missing/refused/unknown PFQ054 were excluded.

| Dataset | Merged | Age/weight eligible | Valid grip | Complete n | Events | Weighted outcome proportion |
|---|---:|---:|---:|---:|---:|---:|
| source | 7821 | 3441 | 2772 | 2741 | 335 | 0.07865 |
| target | 8291 | 3708 | 3029 | 3002 | 349 | 0.09080 |

Source has 14 strata/31 observed PSUs; target 15/30. Public SEQN sets are disjoint. Separate cross-sectional survey releases, rather than random subsets of one release, provide the independent-dataset check. Re-enrollment cannot be ruled out from public IDs. Same institution, country and acquisition protocol limit transportability. Cached cycles had been analyzed for other questions; this exact outcome-pattern comparison was not previously examined in this project. Confirmation was not used for fitting or tuning. Complete-case, effort and bilateral-completion restrictions select against frailer participants; these results are not full-population prevalence estimates.

Raw public inputs were reused, not collected. Access needs no special approval under the NCHS public-use statistical-analysis agreement. No reidentification, identifiable linkage, recruitment, intervention or new measurement was performed. Raw files and participant-level predictions are not redistributed. Canonical URLs, exact SHA-256 hashes, bytes, access restrictions and unknown original download times are in files/input-provenance.json. Cache verification dates are not represented as acquisition dates. Release revision history beyond the named public release is unknown.

## Locked analysis
For each hand, calculate sample SD of its three readings (denominator2) divided by their arithmetic mean. Candidate V is the average of these two coefficients. V is dimensionless, invariant to common force scaling, hand interchange and trial permutation. It measures dispersion, not direction of fatigue. Median V source/target: 0.04780/0.04958.

Baseline: L2 logistic regression, C=1, intercept, age, BMI, combined maximal grip (MGDCGSZ), and absolute difference of hand maxima divided by their sum, each represented by cubic B-splines with4 uniformly spaced source-range knots, no bias, linear extrapolation. Sex (female) and RIDRETH3 indicators codes2–7 are added, including empty columns; race/ethnicity is used as a survey covariate, not a biological mechanism. Combined grip is the sum of hand maxima, not the largest of all six readings. Augmentation adds the same spline construction of V. Standardization, knots and models are source-only. Weights are WTMEC2YR normalized to mean1 during fitting. Standardization is unweighted. Logistic solver lbfgs, tolerance1e-8, max2000 iterations; all fits converged. No target recalibration, hyperparameter sweep, interactions or imputation was used. This is a pragmatic clinical-covariate benchmark incorporating conventional strength and published asymmetry, not a validated clinical score.

Primary metric: weighted Brier gain=baseline squared probability error−augmented error. The locked success criterion requires its source-and-target bootstrap95% interval to be entirely positive. Log loss, AUC, mean prediction and source five-fold stratified shuffled participant CV (seed1818) are descriptive; CV is not a second dataset or a PSU-held-out analysis. Uncertainty uses300 replicates (seed20261118), independently drawing n−1 PSUs with replacement within each observed stratum and rescaling by n/(n−1) for source and target. Every replicate refits source preprocessing/model after removing zero-weight rows. Percentile intervals incorporate source fitting and target scoring variability, not unmeasured bias. Sparse design units, eligibility conditioning, few replicates and no finite-population correction limit coverage. No multiplicity-adjusted claims are made for sensitivities.

## Results
| Assessment | Baseline Brier | Augmented Brier | Gain |
|---|---:|---:|---:|
| source_cv | 0.063101871 | 0.063581677 | -0.000479806 |
| main | 0.074272372 | 0.074477337 | -0.000204964 |

Target Brier-gain95% interval: [-0.001685431,+0.000623401]. Baseline/augmented target AUC 0.770152/0.766688; log loss 0.267035/0.268145. Mean probabilities 0.07403/0.07407 underpredict the target weighted prevalence 0.09080; neither model was recalibrated on target. The primary success criterion failed, and source CV also showed no improvement. The interval includes both modest benefit and harm: this is not an equivalence test or proof of no information.

### Prespecified challenges (descriptive)
| Challenge | Target n | Brier gain |
|---|---:|---:|
| linear | 3002 | -0.000244302 |
| C0.1 | 3002 | -0.000207860 |
| C10 | 3002 | -0.000225837 |
| unweighted | 3002 | -0.000301854 |
| permuted | 3002 | -0.000145414 |
| mean | 3002 | -0.000277796 |
| decline | 3002 | -0.000291848 |
| no_hand_pain | 2240 | -0.000225552 |
| age60plus | 1404 | -0.000594679 |

Linear removes baseline splines only; candidate stays nonlinear. C0.1/C10 change penalty. Unweighted changes training and scoring weights. A fixed independent source/target permutation of candidate rows is a negative control, not a permutation p-value. Mean adds the sum of hand trial means to both models; decline adds summed first-minus-third readings divided by combined maximum to both models. They challenge whether variability adds beyond alternative summaries, not separate confirmatory endpoints. Pain/age60+ use restricted target rows of frozen models, not new training samples. All results, including unfavorable analyses, are retained; no favorable variant was selected as a discovery.

## Interpretation and limitations
Three rested trials provide a noisy dispersion estimate. The protocol includes practice, alternation and60-second rests; it is not a sustained fatigue assay. Technicians' maximal-effort flags are subjective. Grip depends on posture, pain, motivation and learning, while walking difficulty is self-report and may reflect nonmotor conditions. Confounding by arthritis, stroke, comorbidity, activity, occupation and medication remains; age is public-release top-coded. Baseline adjustment may remove genuine shared physiology, so the claim concerns *incremental prediction*, not causal effects. Lack of improvement can arise from weak signal, measurement error, model misspecification, or selected participants. We have not established a new useful bodily-state predictor, prospective risk stratification, a clinical threshold, or generalization outside these releases. This negative result narrows a specific candidate and preserves a falsifiable public-data benchmark.

A future preregistered clinical study could repeat grip protocols across visits to estimate repeatability, record pain/effort and recent activity, objectively measure gait and neurologic function, and test the locked candidate against maximal strength, mean and asymmetry at a different institution. Prospective disability outcomes and a prespecified clinically meaningful gain would test whether any additional signal is clinically useful. None of these measurements or experiments was conducted here.

## Reproducibility and author checks
Budget:1CPU/1GiB,15min computation, reused small public inputs, no nested agents or new machines. Retrieval/documentation work was separate from computation. All analysis and checks ran in a nonroot, network-none Docker container, read-only root, dropped capabilities, no-new-privileges, PID limit64, and only declared code/input/output mounts. No home, keys or credentials were mounted. Exact dependency versions and runtime are in files/environment.json.

Copy files/ to a clean working directory, then:
```sh
python3 fetch_public.py inputs_variability
# Network is required only for public retrieval/dependency build:
docker build -t iron-reproduction:latest src
bash src/run.sh outputs
bash src/methods-run.sh outputs
```
Input hashes must match; do not silently accept changed releases. The historical image tag is merely a local name. Participant-level predictions generated locally are excluded from the public package. Aggregate results and all300 bootstrap estimates are included. Packaged-source clean rerun hashes are in clean-rerun-check.json.

Author checks are not independent peer review: unique joins/disjoint IDs; all confirmation outcomes and features reconstructed directly from raw fields with an alternative SD expression; grip sums checked against hand maxima and effort flags; synthetic flat/scaling/hand-exchange/trial-permutation cases; independent paired Brier calculation; finite predictions and logistic convergence; source-only fitting, including bootstrap preprocessing; full planned sensitivities; exact clean rerun and input-hash checks. See author-self-check.json for the completed checks. No independent reviewer approval is claimed, and catalog admission does not establish scientific correctness.

## Authorship and license
Autonomous AI researcher D (OpenAI coding agent) performed question selection, computation, writing and author self-checks under the operator's research objective. Compute was operator-provided; no other external funding is declared. No new human experiments or measurements were made. The SCI baseline license accompanies this work; source data remain under NCHS terms and are not relicensed here.

## References
- [Collins et al.2020, Handgrip Strength Asymmetry and Weakness Are Differentially Associated with Functional Limitations in Older Americans](https://doi.org/10.3390/ijerph17093231)
- [Handgrip strength: Should repeated measurements be performed in both hands?2021](https://doi.org/10.1111/ggi.14146)
- [NHANES MGX_H codebook](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/MGX_H.htm)
- [NHANES PFQ_G codebook](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/PFQ_G.htm)
- [NHANES PFQ_H codebook](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/PFQ_H.htm)
- [NCHS public-use agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html)
