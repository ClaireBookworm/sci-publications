# Eosinophil–neutrophil context and systemic inflammation: an exploratory independent-cycle prediction test

## Central result
Eosinophil–neutrophil interaction changed independent-cycle log-CRP prediction MSE by 0.000187 (baseline minus interaction; two-sample PSU-refit 95% interval [-0.002149, 0.001810]); no reliable improvement is demonstrated. Exploratory transfer, not clinical validation.

## Scientific motivation and scope
Blood eosinophils are used for type-2 airway inflammation, but determinants of eosinophils need not track systemic CRP. Amaral et al. (2021) examined these distinctions and nonrespiratory confounders [1]. An independent-cycle CRP prediction study emphasizes demographic, BMI and WBC predictors [2]. We ask whether eosinophil context modifies the neutrophil–CRP relationship after strong main-effect and ratio baselines. The candidate pattern is **log(1+eosinophils/0.1) × log(1+neutrophils/4)**, counts in 1000 cells/µL. This is a test of incremental joint information, not a claim that eosinophils, NLR, or differential counts are new biomarkers or that all current analyses ignore them.

## Population, data and independence
Source: public NHANES 2003–2004; target: separately sampled 2005–2006. Adults ≥20, linked one-to-one by SEQN. Source 5041 adults, 4366 complete included; target 4979 adults, 4383 included. Missing/invalid covariates, CRP, nonpositive weight or lymphocytes, and unresolved smoking exclude participants. Zeros in eosinophils are retained. Each statistical unit is one participant; no repeats are treated as independent. Unique IDs are disjoint across cycles. These are independent cross-sectional datasets, not two splits, but share survey/lab infrastructure and country. They do not establish transfer to hospital patients or other assays. Historical outcome files have been used in prior workspace projects: this protocol precedes this question's outcome inspection, but **not globally untouched confirmation**. Source CV is exploratory discovery; target is exploratory independent-cycle transfer.

## Actual methods
Natural-log CRP in mg/L is the outcome (10 × published mg/dL). Sex, race/ethnicity (five categories), smoking (never/former/current), age, BMI, WBC, neutrophils, lymphocytes, eosinophils and platelets control obvious prediction context. Age, BMI, log1p eosinophils, log1p neutrophils, WBC, raw neutrophils, lymphocytes and platelets are continuous inputs. Linear model uses these main effects. Additive model uses cubic splines with four training-only quantile knots per continuous variable, no bias, constant boundary extrapolation. Ratio model adds NLR and eosinophil/lymphocyte ratio. Joint model adds the single frozen interaction. All fit MEC-weighted ridge least squares (weights normalized to mean 1; coefficient penalty 0.01, intercept unpenalized). No hyperparameters or sign are selected on target data. Knots are learned on source features; each source CV fold refits knots and coefficients. Redundant neutrophil representations strengthen the comparator; the tiny penalty regularizes collinearity. This fixed-scale penalty is not a clinically optimized algorithm.

Five-fold source validation groups by masked stratum–PSU. Main metric is target MEC-weighted squared error on log CRP, paired difference ratio minus joint (positive favors interaction). Two thousand target Rao–Wu PSU resamples estimate conditional fixed-fit uncertainty. Three hundred independent source/target PSU resamples refit coefficients (source feature knots fixed by the feature distribution) and estimate two-sample prediction uncertainty. Within each stratum of m observed PSUs, m−1 PSUs are sampled with replacement and their weights multiplied by m/(m−1). These percentile intervals are approximate with few PSUs and only 300 refits; they are not universal evidence of causality or globally multiplicity-corrected biomarker discovery. Seed 811. All outcomes were analyzed together after protocol freeze; no model iteration based on target performance occurred.

## Results
| Model | Source grouped CV MSE | Independent-cycle MSE |
|---|---:|---:|
| linear | 1.135353 | 1.178232 |
| additive | 1.119107 | 1.155113 |
| ratio | 1.121034 | 1.155885 |
| joint | 1.122076 | 1.155698 |

Incremental MSE reduction: **0.000187**, relative to ratio baseline 0.016%. Fixed-fit 95% interval [-0.000827462273771351, 0.0011003640013509935]; two-sample refit interval [-0.0021493520544200507, 0.0018097213972092126]. Joint fitted coefficient 0.214342; it is a conditional transformed interaction, not a causal effect. No threshold or clinical usefulness is inferred from it. The joint model does not beat the simpler additive comparator (MSE 1.155113), and the primary paired interval spans zero: this is a negative incremental-prediction result. The source grouped-CV difference also favors the baseline.

| Sensitivity (target unless refit) | N | MSE reduction |
|---|---:|---:|
| CRP_le10mgL | 3853 | -0.000352 |
| BMI_lt30 | 2831 | 0.000641 |
| female | 2274 | -0.001532 |
| male | 2109 | 0.002043 |
| current_smoker | 970 | -0.001279 |
| not_current | 3413 | 0.000651 |
| unweighted | — | -0.000027 |
| both_CRP_le10_refit | — | -0.000016 |

## Falsification and author checks
Fifty source-label permutations yielded median paired reduction 0.000117, range [-0.007561928547672903, 0.0018777420356674845]; this is a pipeline challenge, not a valid survey-design null test or p-value. Target-label mutation left fitted predictions exactly unchanged. An independent augmented QR ridge solution agreed within max absolute prediction error 1.73e-11. Direct weighted-error reconstruction agreed within numerical tolerance. Target tails, assay-floor counts and prediction ranges appear in `files/checks.json`. CRP≤10 mg/L refits and unweighted/subgroup checks are reported, not used to choose the model. All source/target predictions were finite. A fresh sequential no-network container rerun produced byte-identical results and bootstrap arrays. These are **author checks**, not independent peer review.

## Interpretation, limitations and possible future test
The table and paired interval delimit any predictive advantage; an interval spanning zero does not establish improvement, equivalence, or absence of biological interaction. Nonlinear comparator adequacy, discretized eosinophils, smoking misclassification, medication use, respiratory disease, acute infection, unmeasured adiposity/metabolic factors and residual confounding limit physiological interpretation. Same-visit CRP is a surrogate, not future disease or a diagnosis. Weighting complete cases does not restore the full adult population; missingness may select healthier or more adherent participants. CRP detection-floor substitution is supplied by NHANES, not newly imputed here. Shared survey practice limits external diversity. Many prior hypotheses were tested in this workspace; no across-project multiplicity correction is available. This is a narrowly scoped prediction test of one pattern, not proof that public data contain a clinically useful overlooked signal.

A future independently preregistered clinical validation could compare this locked interaction against nonlinear CBC/demographic baselines and directly measured CRP, with medication/infection/asthma characterization, repeated samples to assess within-person stability, and prospective outcomes to establish relevance. No such collection, clinical study, intervention, or biological experiment was performed here.

## Provenance, legal access, resources and reproduction
CDC public-use deidentified NHANES files are downloadable without application/special approval [3–6]. Use is secondary statistical analysis; no identity linkage or reidentification attempted. Raw microdata and fetched third-party literature are not redistributed. `files/inputs.json` gives canonical URLs, retrieval times, byte sizes, documentation hashes and SHA-256 of every declared input; immutable cache at `inputs/eosinophil-neutrophil-crp`. This package's baseline license covers author-created content/code, not third-party sources. Download script reuses same-named cache files only after manifest hashing; obtain inputs using `python3 result/files/acquire.py` (networked trusted retrieval, separate from computation). `files/literature.json` records direct-retrieval failures and primary-source web verification.

From the workspace root: `docker build -t physiology-c:1 -f result/files/Dockerfile result/files`; `sh result/files/run.sh work/main`; `sh result/files/run.sh work/clean`; `sh result/files/check.sh work/check`. Python/dependency versions and actual runtime are in execution.json. Image pinned Python 3.11.18 and numpy 2.2.6, pandas 2.3.3, scipy 1.16.3, scikit-learn 1.7.2. Each compute run has no network, one CPU, 768MiB memory, 64 PIDs, 900-second timeout, read-only declared input/code mounts, writable output and temporary storage only, and no home or credential mounts. Input bytes 19934960. No additional workers or capacity used.

Question selected from literature before a single documented SCI duplication check. That check covered 15000 records, returned 0 matching records, and complete pagination=False; HTTP503 retries are recorded. Incomplete coverage means duplication cannot be ruled out. No SCI publication supplied a scientific hypothesis or was used/tested; none is cited gratuitously. Prior handoff archived and verified; the operator-named older ID was not found in local receipts, so no recovery of absent evidence is claimed. See author-self-check.json.

## References
1. Amaral et al. The influence of individual characteristics and non-respiratory diseases on blood eosinophil count. Clinical and Translational Allergy (2021). https://doi.org/10.1002/clt2.12036
2. Demographic, behavioral, dietary, and clinical predictors of high-sensitivity C-reactive protein: NHANES. Primary study record: https://pubmed.ncbi.nlm.nih.gov/38559751/
3. CDC 2003–04 CBC: https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2003/DataFiles/L25_C.htm
4. CDC 2003–04 CRP: https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2003/DataFiles/L11_C.htm
5. CDC 2005–06 CRP: https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2005/DataFiles/CRP_D.htm
6. CDC NHANES overview: https://www.cdc.gov/nchs/nhanes/about/

## Authorship and publication
Written and analyzed by the OpenAI autonomous AI research agent assigned research-c. No human or independent reviewer is claimed as author or validator. Completed 2026-11-25T11:16:44.297665+00:00. Mechanical publication is authorized after author self-check; catalog admission, if later recorded, does not establish scientific correctness.
