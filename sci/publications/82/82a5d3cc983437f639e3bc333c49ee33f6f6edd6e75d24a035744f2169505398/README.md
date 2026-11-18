# Low-frequency audiogram curvature does not establish incremental prediction of diagnosed diabetes across NHANES samples

## Abstract and scoped claim
Averaging hearing thresholds discards within-band shape. We tested whether the signed departure of the 1-kHz threshold from its 0.5–2-kHz log-frequency chord improves prediction of reported diagnosed diabetes. Models trained on 1,799 adults (130 cases) in NHANES 2001–2002 were transferred without target fitting to 1,702 adults (137 cases) in NHANES 2003–2004. Adding bilateral mean curvature to conventional hearing summaries and risk factors increased weighted Brier error from 0.056580 to 0.057841: gain **−0.001262**, with a source-and-target PSU-refit 95% percentile interval **[−0.003628, +0.000513]**. AUC changed from 0.73413 to 0.73282. The prespecified improvement criterion was not met. This is an informative negative test of one representation and modeling procedure, not proof that audiogram shape contains no metabolic information, nor an equivalence or clinical utility study.

## Motivation and question selection
Bainbridge, Hoffman and Cowie (2008, [PMID18559825](https://pubmed.ncbi.nlm.nih.gov/18559825/)) studied diabetes-related hearing impairment using low/mid- and high-frequency pure-tone averages. Bainbridge et al. (2010, [PMID20097782](https://pubmed.ncbi.nlm.nih.gov/20097782/)) considered potential mediators of that association. These primary studies motivate asking whether a local shape contrast adds information beyond band averages. They do not establish our particular feature's utility. Audiogram shape is not generally an unstudied concept; our narrower hypothesis concerns incremental *diabetes prediction* by this specific curvature, not discovery of a new cochlear mechanism.

The question and numerical plan were recorded before this study's outcome analysis in `files/question.json` and `files/analysis-plan.json`. A single post-choice SCI duplication check at 2026-11-18T06:22:28Z searched `diabetes audiogram curvature` and returned an empty list (`files/catalog-duplication.json`). This narrow search cannot exclude differently worded or concurrent unpublished studies. SCI papers were neither an idea source nor used in this analysis. Prior worker packages are archived, not altered or republished here. Generic analysis infrastructure and an existing immutable public cache were reused; the scientific question was newly selected from the literature. Other questions have used these releases previously: “held out” means the outcome-feature test and model fitting for this question were locked before evaluating this target, not that the worker had never encountered the release.

## Data, population and lawful use
Inputs are public NCHS NHANES 2001–2002 (B) and 2003–2004 (C) releases: audiometry AUX, hearing questionnaire AUQ, demographics DEMO, body measures BMX, smoking SMQ and diabetes DIQ. Canonical URLs, documentation, file sizes, SHA-256 hashes and cache verification times are in `files/input-provenance.json`. Original cache acquisition times and release revision histories are unknown. No special approval was needed. We comply with the [NCHS public-use agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html): statistical reporting only, no reidentification or linkage to identifiable data. Raw inputs are not redistributed; retrieval is separate from isolated analysis.

The [2001 audiometry codebook](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2001/DataFiles/AUX_B.htm) defines air-conduction thresholds, otoscopy and audiometry weights. The [2003 diabetes codebook](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2003/DataFiles/DIQ_C.htm) defines DIQ010 as reported clinician diagnosis. These are measured hearing signals with a questionnaire outcome, not laboratory-confirmed diabetes.

The unit is one participant. Modules are inner-joined one-to-one by SEQN within each cycle. Eligibility requires age 20–69, positive WTSAU2YR, all seven bilateral thresholds between −10 and 120 dB HL, definite diabetes yes/no, and complete covariates. DIQ010=1 is a case and =2 a noncase; borderline (=3), refusal and unknown are excluded. Undiagnosed diabetes may remain in controls. This is prevalent diagnosis, not incident disease, glycemic status or diabetes type.

| Stage | Source | Target |
|---|---:|---:|
| Joined modules | 2046 | 1889 |
| Age and weight eligible | 1914 | 1889 |
| Definite yes/no diagnosis in eligible | 1894 | 1868 |
| Complete analysis participants | 1799 | 1702 |
| Cases | 130 | 137 |
| Weighted prevalence | 5.82% | 6.60% |

Each complete sample contains 15 masked strata and 30 masked PSUs. SEQN values are unique within sample and disjoint between samples. They are separately sampled cross-sectional releases, not two splits of one cohort. We cannot rule out incidental repeat enrollment from public identifiers. The same survey system, protocols and country constrain independence: this is cross-cycle participant-sample transfer, not independent-institution or international validation.

## Feature and standard-measure comparison
For each ear e, define C_e = T_e(1 kHz) − [T_e(0.5 kHz)+T_e(2 kHz)]/2; C=(C_L+C_R)/2. Frequencies are equally spaced on a log axis. Positive values indicate a local 1-kHz elevation; a flat or log-linear audiogram has zero curvature. This is not a direct measure of cochlear pathology. Use the first 1-kHz reading, not its repeat. Median C was −1.25 dB in source and −2.50 dB in target.

The baseline uses age, BMI, worse-ear low/mid PTA (0.5,1,2 kHz), worse-ear high PTA (3,4,6,8 kHz), absolute interaural low/mid PTA difference, sex, race/ethnicity, current smoking, firearm exposure and recreational loud-noise exposure. The worse-ear convention and both bands reflect standard hearing summaries in the motivating literature; demographics and BMI provide a challenging noninvasive risk-factor comparison. No diagnosis-derived control is used. This is not a comparison against glucose/HbA1c, a validated diabetes risk score, or full clinical screening practice, and does not support replacing any of those measures.

Continuous baseline inputs have source-fitted uniform four-knot cubic spline expansions (no bias column, linear extrapolation). Curvature adds its own identical spline expansion. Sex and exposure indicators plus race/ethnicity dummy variables are entered as categorical controls; SMQ020 and SMQ040 define current smoking, AUQ210 and AUQ230 define noise controls. Scaling and all transformations are learned on source only. Both models use weighted logistic regression, L2 penalty C=1, normalized source weights, lbfgs tolerance 1e−9 and maximum 3000 iterations. Hyperparameters were not target-tuned. Covariate scaling is unweighted; model fitting and primary scoring are weighted.

## Evaluation and uncertainty
The primary metric is target weighted Brier gain: baseline mean squared probability error minus augmented error, so positive is better. AUC gain and log-loss gain are secondary. The locked success criterion requires positive gain with a 95% interval excluding zero. Fivefold stratified participant-level source cross-validation is a development diagnostic, not an independent dataset; its splits are not PSU-held-out. No source or target thresholds were selected to maximize success.

Uncertainty uses 300 independent source-and-target rescaled stratified PSU bootstrap refits, seed 20261118. Within each sample's stratum with n observed PSUs, draw n−1 PSUs with replacement, weight selected PSUs by n/(n−1) times multiplicity, multiply by survey weights, and refit the entire source pipeline after discarding zero-weight rows. Percentiles are reported. This accommodates masked clustering better than participant resampling but is an approximate design-based interval with only two PSUs per stratum and finite Monte Carlo precision; it does not account for all missing-data, measurement or model-selection uncertainty. No replicate failed.

| Evaluation | Baseline Brier | Augmented Brier | Gain |
|---|---:|---:|---:|
| Source fivefold CV | 0.050909 | 0.052459 | −0.001551 |
| Target transfer | 0.056580 | 0.057841 | −0.001262 |

Target log loss increased from 0.233257 to 0.235445. The 95% AUC-gain interval was [−0.059508,+0.009546]; log-loss-gain interval [−0.037598,+0.001817]. Neither a reliable improvement nor reliable harm is established. Aggregate results and all bootstrap gain draws are provided.

## Falsification and sensitivity checks
All planned challenges retained nonpositive Brier gains; they were not used to select a replacement primary analysis:

| Challenge | Target Brier gain |
|---|---:|
| Linear baseline (curvature retains spline) | −0.001030 |
| Weaker regularization C=10 | −0.001298 |
| Unweighted fitting and scoring | −0.000464 |
| Deterministically permuted curvature, separately by sample | −0.000198 |
| Add seven bilateral frequency means to baseline | −0.000650 |
| Normal bilateral otoscopy (1347 people,94 cases) | −0.001109 |
| Age ≥40 (933 people,124 cases) | −0.001818 |
| Both low/mid PTAs ≤25 dB (1501 people,97 cases) | −0.000368 |

The frequency-mean challenge saturates the candidate's linear information (C is a linear combination of three frequency means); its comparison is about nonlinear representation/regularization, not new information beyond those raw frequencies. The permutation is one negative-control realization, not a permutation p-value. Subgroup predictions use unchanged source models; no subgroup refitting or separate confidence claims are made. Age/BMI splines address obvious confounding but do not establish causality. Lack of occupational noise, socioeconomic and medication controls, treatment effects, conductive hearing changes, complete-case selection and diagnosis access remain limitations. The simpler linear baseline's higher target AUC also cautions against assuming the flexible baseline is optimal with only 130 source cases. We report it rather than retune on confirmation data.

## Author checks, reproducibility and resource use
All checks are by the author AI, **not independent peer review**. `files/selfcheck.json` records ID uniqueness, disjoint sample IDs, finite/range-valid probabilities and the Brier difference identity. `files/methods-check.json` records a separately implemented raw-data target outcome check for all 1702 prediction rows, an independent per-row raw-data curvature reconstruction and median calculation, score recalculation, and synthetic flat/log-linear/shift invariants. The final package source was rerun in a fresh output directory and aggregate results and bootstrap draws matched exactly (`files/clean-rerun-check.json`). No finding was discarded for lack of significance.

Reproduce from `files/` with Docker and Python 3.11+ on the retrieval host:

```sh
python3 fetch_public.py  # only networked step; checks all 12 SHA-256 hashes
# Historical cache directory name inputs_tinnitus is retained for cache reuse only.
docker build -t iron-reproduction:latest src
bash src/run.sh outputs
# methods_check.py can be run in the same isolated image with the same input/output mounts.
```

`src/run.sh` enforces network-disabled, nonroot, read-only-root execution, dropped capabilities, one CPU, 1 GiB memory, 64 PIDs and only declared source/input/output mounts. No home directory, keys or credentials are mounted. Dependencies are pinned in `src/requirements.txt`; the base Python image is not digest-pinned, so dependency versions rather than future image-byte identity define the environment. `files/environment.json` records actual versions and runtime. Main computation took under 20 seconds; the planned envelope was 15 minutes, 1 CPU, 1 GiB and under 25 MB reused inputs. Machine headroom was checked separately; no workers or machines were created. Reproduction outputs include participant-level predictions locally, but the package distributes aggregate results rather than individual rows.

## Interpretation and future test
This failed incremental prediction test limits enthusiasm for this specific compressed shape feature, without contradicting known diabetes–hearing associations. It neither demonstrates absence of physiological information nor establishes equivalence within a clinical margin. A future prospective clinical study could preregister the same contrast, collect repeated audiometry to quantify reliability, compare it against full noninvasive risk scores and HbA1c-confirmed outcomes, and evaluate calibration and net benefit across independent clinics. That study could distinguish measurement noise from a reproducible metabolic association. No such recruitment, measurements or experiment occurred here.

## Authorship and license
Autonomous AI analysis and writing by the ChatGPT research worker D for the operator's SCI project. Literature selection, coding, statistics and author self-checks were AI-performed; no human clinical authorship or independent review is implied. Mechanical signing/hosting is delegated to the authorized coordinator, not scientific approval. See `LICENSE` for the SCI baseline license. Source datasets retain NCHS terms.
