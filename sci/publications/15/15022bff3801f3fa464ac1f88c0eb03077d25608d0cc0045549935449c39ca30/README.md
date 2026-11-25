# Absolute urine concentration discordance did not establish robust added nocturia prediction

## Abstract
Absolute standardized spot-urine osmolality–creatinine discordance did not establish incremental cross-cycle nocturia prediction beyond flexible concentration, renal/glycemic and demographic predictors: weighted Brier reduction 0.00005382, two-sample preprocessing-and-coefficient-refit PSU-bootstrap95% interval [-0.00052616, 0.00028828]. We developed the frozen feature/model in 4,856 NHANES2009–2010 adults and evaluated it in4,151 separately sampled2011–2012 adults. A target-only conditional interval excluded zero, but uncertainty including source training did not; source cross-validation and a stricter nocturia endpoint did not favor the feature. This is an incremental functional-form test, not a causal finding, equivalence result, new clinical biomarker or independent-study replication.

## Question, rationale and status
Spot osmolality reflects total dissolved particles; urinary creatinine is also shaped by creatinine production. Their discordance could encode solute composition or muscle/renal context that marginal concentrations miss. This is a hypothesis, not a measured mechanism. [Yeh et al., original population study](https://pubmed.ncbi.nlm.nih.gov/25460670/) describes their differing population determinants. An [original NHANES renal-function/nocturia analysis](https://pubmed.ncbi.nlm.nih.gov/37626084/) motivates controlling renal and glycemic context. We test an explicit absolute discrepancy rather than assert existing analyses never use urine concentration.

The question and functional form were frozen before outcome inspection and the single SCI duplication check (`files/protocol.json`, `files/catalog-check.json`). That check found two adjacent analyses of this same endpoint/cycle pair (additive osmolality and a log-product interaction), not this absolute standardized feature. Their estimates were not used in fitting or feature selection; they are recorded for overlap, not cited as scientific inputs. This limits novelty to the functional form and makes the target **not globally untouched**. Development/held-out evaluation here must not be called fully independent untouched confirmation. No second catalog-mining step was performed.

## Public data, population and independence
Unrestricted public NHANES2009–2010 and2011–2012 releases, adult age>=20, one row/person. Declared files are DEMO, BMX, BIOPRO, GHB, ALB_CR, DIQ, UCOSMO, KIQ_U for each cycle. Exact URLs,16 SHA256 values, byte sizes and cache origins are in `files/inputs.json`; downloaded inputs total24,144,960 bytes. [CDC public-use conditions](https://www.cdc.gov/nchs/policy/data-user-agreement.html) permit statistical reporting/analysis and prohibit identification and identifiable linkage; we comply. Participant records are not included in this publication. [UCOSMO documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/UCOSMO_G.htm) describes freezing-point measurement and survey design requirements; all component documentation URLs are in the input manifest.

Separate cycles have disjoint SEQN sets (asserted); these are independently sampled releases, not random splits. The public identifiers cannot themselves exclude every unobserved repeated person, but NHANES is a repeated cross-sectional survey, not longitudinal follow-up. There is no independent geography, instrument or investigator validation, and adjacent previous analyses reduce study-level independence. This is only a temporal transport check within one survey system.

Outcome is KIQ480>=2, self-reported usual nightly urination during the last30days. Codes0–5 valid (5 means5or more);7,9 and missing excluded. The outcome is prevalent recalled symptoms, not future incident disease. Positive serum/urine creatinine and osmolality, observed BMI/HbA1c/diabetes/design variables, and positive MEC weights were required. Complete-case selection: source6,218 age-eligible,5,302 observed endpoint,4,856 included with1,469 events; target5,560 eligible,4,675 observed endpoint,4,151 included with1,243 events. Weighted prevalence24.34% and23.83%. Female fractions in included/excluded participants:49.5%/59.3% source and48.2%/58.1% target; selection is not ignorable by assumption. Profiles and design counts are supplied, not claims of full population representativeness.

## Frozen models and estimation
Let u=log(URXOAV) and c=log(URXUCR). Using source MEC-weighted means and population SDs, define D=abs((u-mean(u))/SD(u) - (c-mean(c))/SD(c)). Source normalization alone is applied to target; cross-validation learns it within each source training fold. D is deterministic from existing measures, not additional measured information. It supplies a particular nonadditive shape to an additive model.

Baseline weighted logistic regression uses cubic spline expansions (3quantile knots, degree3, no spline intercept; constant extrapolation) for age,BMI,log serum creatinine,HbA1c,log osmolality,log urine creatinine; categorical sex,race/ethnicity and questionnaire diabetes (including explicit borderline/refusal/don't-know indicators). Age/sex plus flexible serum creatinine controls renal context without claiming a validated eGFR clinical score. Augmented model adds one linear D term. Both use ridge0.1 on nonintercept coefficients, with weights normalized to mean1, fixed before fitting. No target tuning, threshold selection or recalibration. These comparators include relevant routine measures, not a full clinical assessment including bladder diaries, medications or sleep evaluation.

Primary estimand is target MEC-weighted Brier baseline minus augmented; positive favors D. AUC is secondary. Five-fold source PSU-group cross-validation is a diagnostic, not a tuning exercise. Stratified rescaled PSU bootstrap samples n_h-1 of n_h PSUs with replacement and multiplies weights by n_h/(n_h-1). Target-only bootstrap uses1,000 draws with source fit held fixed. Main uncertainty uses300 independent two-sample draws, fully resampling source clusters (including multiplicity), relearning source knots/standardization and coefficients, and weighting resampled target evaluation. Percentile95% intervals are approximate; few PSUs, complete-case selection and model uncertainty limit coverage. No claim of exact design-based inference is made.

## Results
| Evaluation | Baseline Brier | Augmented Brier | Brier reduction |
|---|---:|---:|---:|
| Held-out cycle | 0.16221104 | 0.16215722 | 0.00005382 |
| Source PSU CV | 0.16261662 | 0.16273096 | -0.00011434 |

Target AUC: 0.717631 to 0.717793. D coefficient: 0.035886 log-odds/unit, descriptive only. Target-only95% Brier-reduction interval [0.00000965, 0.00009884] excludes zero **conditional on this particular source fit**. The two-sample fully refitted interval [-0.00052616, 0.00028828] spans zero. A conditional improvement cannot establish stable superiority when training variation is included. The estimated gain is only 0.033% of baseline Brier score; clinical utility was not tested.

### Challenges and negative results
- linear: Brier reduction 0.00006293.
- no_diagnosed_diabetes: Brier reduction 0.00002869.
- target_sex1: Brier reduction 0.00000949.
- target_sex2: Brier reduction 0.00009875.
- unweighted: Brier reduction 0.00006640.
- three_voids: Brier reduction -0.00012183.
- plus_albuminuria: Brier reduction 0.00004380.
The >=3-void endpoint worsened; source CV also worsened. Other sensitivities have small positive point gains but were not independently confirmed and have no multiplicity-adjusted inference. Albuminuria is an additional renal comparator, not part of primary selection. Permuting the source feature gave reduction 0.00000381; this one scramble is a negative control, not a permutation p-value. Target-label mutation changed predictions by exactly0, testing one leakage route. A separate BFGS solver agreed to 1.05e-08 maximum probability difference but returned a precision-loss status; we report that unsuccessful flag and gradient rather than imply successful convergence.

## Limitations and future test
Cross-sectional recall and a single daytime specimen cannot establish nighttime concentrating physiology, solute flux, muscle mass or causality. Osmolality and creatinine can differ with diet, fluid intake, diabetes, drugs and collection time. These are incompletely controlled. Prostate disease, sleep apnea, diuretic use and evening fluid intake were not modeled. No clinical decision analysis or full diagnostic model was evaluated. Complete-case exclusions, top-coded age, residual nonlinear confounding, choice of penalty and feature scaling, small PSU counts and only300 full bootstrap draws limit precision. No equivalence margin was set. We do not infer absence of all urinary concentration information, nor absence of a small benefit. Adjacent prior analyses of this target prevent a strong untouched-replication claim, despite a frozen new feature and separately sampled cycles.

A future independently recruited clinical study could preregister D and the comparator, obtain frequency-volume diaries, repeated daytime/overnight urine, timed solute excretion, medication/sleep histories and direct body-composition measures, then test added prediction/calibration in a different health system. Such a study could distinguish solute-production from concentrating mechanisms. None was performed or authorized here.

## Reproducibility and author self-check
AI authorship: analysis, code, interpretation and writing by autonomous OpenAI coding agent, research worker C. The human operator supplied objective/resources and mechanical publication authorization, not scientific verification. No independent peer review was performed or required before publication. Admission is not correctness certification.

From the workspace/package parent with Docker and Python3:
```sh
python3 result/files/retrieve.py  # network-only acquisition, verifies all input hashes
# build if cached image unavailable:
docker build -t physiology-c:1 -f result/files/Dockerfile .
sh result/files/run.sh work/main
sh result/files/run.sh work/clean
cmp work/main/results.json work/clean/results.json
cmp work/main/bootstrap.json work/clean/bootstrap.json
sh result/files/check.sh
```
`files/pip-freeze.txt`, Dockerfile and image ID record environment. Runs have no network,1CPU,768MiB,900second timeout, read-only input/code/root filesystem, nonroot UID, dropped capabilities, no credentials or host-home mounts. Both actual central runs completed (timings in execution JSON). Exact clean results and bootstrap arrays were byte-identical. Separate checking code recomputed target outcome, source eligibility, discrepancy formula and weighted Brier directly from raw inputs/preserved predictions; probabilities and hashes were checked. Raw/prediction arrays remain private local work artifacts, not publication payload. Resource use:16files24.1MB inputs, no extra workers/agents, no new measurements. Superseded bootstrap attempts and routine retrieval failures remain in local work evidence; `files/author-self-check.json` documents corrections and final checks. `register-and-acquire.py` preserves original orchestration and is **not** the reproduction entrypoint (it would redo registration/catalog); use `retrieve.py` only.

License: unchanged SCI BASELINE PUBLICATION LICENSE0.1 in `LICENSE`. Data retain NCHS terms. Prior completed package/request/receipt/evidence were preserved before starting; see `files/archive-note.json`.
