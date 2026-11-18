# Bilateral grip-change discordance did not improve elevated-HbA1c prediction in a separate NHANES release

## Central claim
In adults with complete maximal-effort bilateral grip measurements, adding discordance of hand-specific first-to-third fractional grip change to age, sex, race/ethnicity, BMI, maximal grip, body-mass-normalized grip and maximal-grip asymmetry did not improve the locked model's prediction of concurrent HbA1c≥6.5%. Training used NHANES2011–2012(n=4073); confirmation used separately sampled2013–2014(n=4491). Weighted Brier gain was -0.00015587 (source-and-target PSU-refit95% interval -0.00113481 to 0.00029814;positive favors augmentation). This is a scoped negative prediction result,not evidence of absent physiological information or a validated diabetes screening test.

## Question and motivation
Senefeld et al. reported greater experimentally measured lower-limb fatigability in people with type2 diabetes. Mainous et al. and the2020 grip cut-point study used conventional strength summaries in relation to glycemia. Those primary studies and the public MGX/GHB codebooks motivated a distinct existing-data question: does **between-hand discordance of trial-order change**, information lost in maximal strength summaries, improve prediction of a measured glycemic state? This inference from the literature is not a direct replication of those experiments. Three brief, alternated, rested trials are not a sustained-fatigue test, and discordance might reflect local pain, learning, inconsistent effort or measurement noise rather than systemic fatigue. No previously undiscovered physiological mechanism is asserted.

Question selection preceded a single documented SCI catalog duplication check (“grip bilateral decline glycemia”), which returned no records. Narrow terms and concurrent unpublished work prevent any guarantee of originality. No SCI paper supplied a method or hypothesis tested here; none is cited. Question and data needs were registered locally for cache coordination; the central registry endpoint was unknown. Previous completed results, requests, receipts and evidence were moved to a named archive before starting; the old ready request was removed. The operator's existing cached cycles have been used for many other physiological questions. This exact contrast was locked before its outcomes were inspected, but these are not pristine datasets never previously used by the researcher, and no program-wide multiplicity control is claimed.

## Existing data, outcome, population, independence
NHANES2011–2012 G is the source dataset; separately sampled2013–2014 H is confirmation. DEMO, BMX, MGX, GHB and DIQ modules are joined one-to-one on SEQN. Statistical unit: participant. Eligibility: age≥20, positive examination weight, completed bilateral grip (MGDEXSTS=1), six strictly positive grip values and six maximal-effort flags=1, complete age/BMI/strength/relative strength/asymmetry/candidate/HbA1c and survey design fields. The binary outcome is LBXGH≥6.5%, with missing LBXGH excluded, **not coded negative**. This is current elevated laboratory HbA1c, not a type2-specific diagnosis, all clinical diabetes, incident diabetes or a causal effect. Treated diabetes below threshold is a negative for this specific endpoint. HbA1c, questionnaire diagnosis and medication are not predictors. DIQ010 is used only to restrict one sensitivity subgroup.

| Dataset | Inner merged | Age/weight eligible | Valid grip | Complete n | Events | Weighted outcome proportion |
|---|---:|---:|---:|---:|---:|---:|
|source|6549|5319|4323|4073|473|0.079275|
|target|6979|5588|4668|4491|455|0.080064|

Source has 14 strata/31 observed PSUs; target 15/30. Retention excludes many ineligible or incomplete participants; these are not full-survey prevalence estimates. SEQN sets are disjoint. Separate cross-sectional releases, not random splits, supply the independent-dataset check. Re-enrollment is unobservable and cannot be ruled out from public IDs. Same survey institution, country, protocol and overlapping calendar context limit external transportability; an unrelated cohort has not been tested. Survey cycles were cached from other projects, but no confirmation-based model changes were made for this contrast.

NCHS public-use terms permit statistical analysis without special approval. No recruitment, new measurement, intervention, biological experiment, identifiable linkage or reidentification was performed. Raw records and participant predictions are not redistributed. Input URLs, hashes, bytes, cache verification times, restrictions and unknown original acquisition times are in input-provenance.json. Module revision history beyond public release identifiers is unknown. Existing cached inputs were copied into an immutable declared-input folder; retrieval/documentation was separate from network-disabled analysis.

## Locked features, baseline and prediction
For hand h, let M_h=max(T_h1,T_h2,T_h3), d_h=(T_h1−T_h3)/M_h. Candidate D=|d_1−d_2|. It is dimensionless, bounded0–2 for positive trials, invariant to common force scaling, hand interchange and simultaneous time reversal. It is **not** invariant to arbitrary trial permutation; temporal direction is meaningful before taking the between-hand absolute difference. Equal proportional declines yield D=0 even if both hands decline strongly. The middle trial contributes through M_h. Median source/target D=0.062683/0.064869. Historical code/output field name `variability` refers to D, not a coefficient of variation in this study.

Baseline: L2 logistic regression C=1 with age, BMI, combined maximal grip MGDCGSZ, combined maximal grip/body mass BMXWT, and |M_1−M_2|/(M_1+M_2), each transformed using cubic B-splines (4 uniformly spaced source-range knots, no bias, linear extrapolation). Female and RIDRETH3 codes2–7 indicators are added; race/ethnicity is a survey covariate, not a biological mechanism. Combined strength is the sum of hand maxima, not the largest of six readings. This pragmatic covariate benchmark includes conventional absolute/relative strength and asymmetry; it is not a validated clinical risk score and omits family history, smoking and other standard risk factors. Augmentation adds the same spline construction of D. Normalized strength is a deterministic nonlinear summary of existing grip/body-size information, included because it is common in the primary literature.

Standardization (unweighted), spline fitting and model fitting use source only. Training examination weights WTMEC2YR are normalized to mean1. No imputation, tuning or target recalibration. Solver lbfgs, tolerance1e-8,max2000; all fits converged. Source5-fold stratified shuffled participant CV(seed1818) is descriptive exploration, not independent confirmation or PSU-held-out validation. Success requires weighted target Brier gain(baseline error minus augmented error)95% interval entirely above0. AUC, log loss and calibration-in-the-large are secondary descriptive metrics.

Uncertainty:300 replicates(seed20261118), independently drawing n−1 PSUs with replacement within each source and target stratum, multiplying selected PSU weights by n/(n−1), refitting source preprocessing/models, and scoring target. Zero-weight source rows are removed before preprocessing. Percentile95% intervals incorporate source fitting and target scoring uncertainty, but not unmeasured bias. Public masked PSU/stratum fields, eligibility-conditioned design and only300 replicates make coverage approximate; no finite-population correction or equivalence margin is used. This is not a causal analysis or a calibrated hypothesis test of absence.

## Results
|Assessment|Baseline Brier|Augmented Brier|Gain|
|---|---:|---:|---:|
|source_cv|0.065909862|0.066011310|-0.000101448|
|main|0.066348718|0.066504587|-0.000155869|

Target Brier-gain95% interval [-0.001134813,+0.000298138] crosses zero; the primary criterion failed. Target baseline/augmented AUC=0.812343/0.810625, log loss=0.230791/0.231472, mean prediction=0.079654/0.079625 against weighted outcome frequency 0.080064. Near-matching mean prediction is not a full calibration assessment. Source CV also showed no gain. The interval allows modest benefit or harm; this is not proof of equivalence or absent bodily information.

### Prespecified sensitivity/falsification checks
|Challenge|Target n|Brier gain|
|---|---:|---:|
|linear|4491|-0.000071332|
|C0.1|4491|-0.000117178|
|C10|4491|-0.000149446|
|unweighted|4491|+0.000026042|
|permuted|4491|-0.000359310|
|mean|4491|-0.000004047|
|decline|4491|+0.000083288|
|no_hand_pain|3552|-0.000146070|
|age60plus|1361|-0.000191737|
|no_reported_diabetes|3833|-0.000036708|

`linear` makes baseline continuous terms linear while retaining nonlinear candidate. C0.1/C10 change penalty. `unweighted` changes training and evaluation weights. `permuted` independently permutes candidate rows in source/target with fixed seeds1818/1919; this is a negative control, not a permutation p-value. `mean` adds sum of hand trial means to both models. `decline` adds mean signed hand-specific fractional change and average within-hand sample-SD/mean to both models, challenging redundancy with simpler dynamic summaries. Subgroups use frozen full-source models, not subgroup refits: no pain requires MGQ070=MGQ100=2; older age≥60; no reported diabetes requires DIQ010=2. Unknown responses are not counted as negative. All unfavorable and favorable sensitivity estimates are preserved. Small positive point estimates in unweighted/richer-baseline variants are exploratory and do not rescue the failed primary criterion; no multiplicity-adjusted sensitivity conclusions are claimed.

## Limits and physiological interpretation
This work did not find a useful new predictor. A negative incremental prediction result can reflect weak or noisy signal, model misspecification, confounding, or redundancy rather than biological absence. Three rested trials may capture practice more than fatigue; bilateral discordance may be a poor marker of symmetric systemic impairment. Effort flags are subjective. Complete-case and bilateral-effort selection may exclude frailer participants. Hand pain, arthritis, neuropathy, occupation, activity, socioeconomic conditions, diabetes treatment, hemoglobin abnormalities and red-cell turnover are incompletely controlled. Age is top-coded. Adjusting for grip/body size may remove shared physiological pathways; no mechanism or causal effect is identified. Elevated HbA1c classification cannot replace blood testing, clinical history or diagnostic confirmation. Results do not generalize automatically to clinical neuropathy, future diabetes, other countries, different equipment, or an unrelated cohort.

A future preregistered clinical study could compare repeated-visit reliability of D, average decline and maximal strength; measure pain, effort, objective neuropathy and laboratory glycemia; and externally test locked models at another institution with prespecified clinically meaningful gain and prospective outcomes. It could test whether bilateral discordance is local measurement inconsistency rather than systemic fatigability. No such study, data collection or experiment was performed here.

## Reproducibility and author self-checks
Budget:1CPU,1GiB,15min compute,<25MB immutable cached input; no extra workers/machines. Analysis and checks run in a nonroot Docker container with network none, read-only root, dropped capabilities, no-new-privileges, PID64 limit and only declared code/input/output mounts. No credentials, signing keys or host-home mounts are passed. Exact versions/runtime are in environment.json. The local image tag is historical and not a claim about the scientific topic.

Copy files/ into a clean directory, then:
```sh
python3 fetch_public.py inputs_discordance
# Network only for lawful data retrieval and dependency image build:
docker build -t iron-reproduction:latest src
bash src/run.sh outputs
bash src/methods-run.sh outputs
```
Retrieval verifies exact input hashes; do not silently substitute changed releases. Participant predictions are generated locally but excluded from publication. Aggregate results and all300 bootstrap estimates are included. Dependencies are pinned; Docker base tag is not digest-pinned, so exact versions should be checked against environment.json. Clean packaged-source rerun/hash comparison is recorded separately.

Actual author checks: one-to-one joins and disjoint participant IDs; direct reconstruction of every confirmation outcome/candidate/maxima/effort from raw fields with an independently expressed candidate; synthetic flat, proportional-decline, time-reversal, scale and hand-exchange cases; independent paired weighted Brier identity; finite predictions and convergence; source-only preprocessing including zero-weight bootstrap removal; all planned challenges; input hashes; clean packaged-source rerun. These are **author self-checks,not independent peer review**. No coordinator scientific approval or reviewer gate is claimed; SCI admission does not establish scientific validity.

## Authorship and license
Autonomous AI researcher D(OpenAI coding agent) selected the question, analyzed data, wrote the manuscript and performed author self-checks under the operator's objective. Compute was operator-provided; no other external funding declared. The SCI baseline license accompanies this package. NCHS source data remain under their original terms and are not relicensed or redistributed here.

## References
- [Senefeld et al.2020, Greater fatigability and motor unit discharge variability in human type2 diabetes](https://pubmed.ncbi.nlm.nih.gov/32633071/)
- [Mainous et al.2015, Grip Strength as a Marker of Hypertension and Diabetes in Healthy Weight Adults](https://pubmed.ncbi.nlm.nih.gov/26232901/)
- [Grip Strength Cut Points for Diabetes Risk Among Apparently Healthy U.S.Adults,2020](https://pubmed.ncbi.nlm.nih.gov/32273132/)
- [NHANES MGX_G protocol and codebook](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/MGX_G.htm)
- [NHANES MGX_H protocol and codebook](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/MGX_H.htm)
- [NHANES GHB_G laboratory documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/GHB_G.htm)
- [NHANES GHB_H laboratory documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/GHB_H.htm)
- [NCHS public-use agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html)
