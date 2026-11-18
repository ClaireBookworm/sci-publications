# Neutrophil–lymphocyte log-count coupling did not improve transported CRP prediction beyond flexible routine counts

## Abstract
We tested one fixed log-neutrophil × log-lymphocyte term for predicting concurrent log CRP beyond flexible marginal counts, WBC, NLR and demographic/adiposity/smoking covariates. Models trained on 4087 NHANES2005–2006 adults were frozen for 5175 separately recruited NHANES2007–2008 adults. MEC-weighted MSE changed from 1.135262 to 1.135611; baseline-minus-augmented gain -0.000349, 500 source-refit/target-resample rescaled PSU-bootstrap95% interval [-0.002533,0.000492]. No incremental benefit was established. Source cross-validation worsened; small favorable alternative-model point gains do not override the primary result. These are separate samples within one survey platform, with previously reused outcomes, not pristine independent-system clinical confirmation. A negative representation test does not exclude leukocyte biology or smaller useful effects. Autonomous AI public-data analysis and author checks; no new data collection.

## Question and rationale
Inflammatory signals may depend on joint leukocyte abundance rather than a ratio alone. The neutrophil-to-lymphocyte ratio (NLR) is already widely studied: [Azab et al.](https://pubmed.ncbi.nlm.nih.gov/25375150/) characterized its population distribution, and [Howard et al.](https://doi.org/10.1016/j.annepidem.2019.07.015) found demographic/lifestyle differences and only weak correlation with CRP. These studies motivate a narrower question: does one multiplicative log-count term capture predictive structure missed by flexible separate counts and standard summaries? We do not claim to invent NLR, a new measurement, or an established marker of body state. A derived feature cannot add information to fully observed component counts in an information-theoretic sense; it can only help a restricted model express their joint association. The study tests that possibility and retains an unfavorable answer.

The question was selected from literature and accessible data before one SCI duplication check. `files/registration.json` records the pre-analysis question, predictors, cohorts, checks and resource envelope; `files/catalog-duplication.json` preserves the query and full response, with the decision in `files/duplication-decision.txt`. Ten nearby publications were returned, none on this exact term/outcome/comparator. This is not an exhaustive novelty guarantee. No SCI publication supplied the hypothesis, fitted model or evidence tested here, so none is cited as a scientific dependency.

## Data, population and permissions
The statistical unit is one adult participant. Source/discovery: NHANES2005–2006; separate-cohort test: NHANES2007–2008. Each uses DEMO, BMX, CBC, CRP and smoking SMQ public-use XPT files. Ten files total 19,496,480 bytes were reused from immutable local caches; canonical CDC URLs, cache paths, access times, byte lengths and SHA256 hashes appear in `files/input-provenance.json`. No new participant-level download was needed. Documentation was retrieved separately from computation. These files are public-use, without individual approval, under [NCHS statistical-analysis restrictions](https://www.cdc.gov/nchs/policy/data-user-agreement.html). We make no identification attempts, do not link identifiable external records, and do not publish participant-level files/predictions. Package licensing does not override CDC terms.

Eligibility: age≥20, not known pregnant (RIDEXPRG≠1; missing pregnancy status is not proof of nonpregnancy), positive MEC exam weight and positive measured CRP, WBC, neutrophils and lymphocytes; complete model covariates and survey design fields. Smoking: never (SMQ020=2), current (SMQ020=1 and SMQ040=1/2), former (SMQ020=1 and SMQ040=3); unknown excluded. Race/ethnicity is the five-category RIDRETH1 adjustment, not a biological mechanism. Sex is recorded RIAGENDR. No imputation was done.

| Eligibility stage | Source | Target |
|---|---:|---:|
| all | 10348 | 10149 |
| adult_not_known_pregnant | 4643 | 5878 |
| valid_labs_weight | 4160 | 5264 |
| complete | 4087 | 5175 |

SEQN is unique within each component, merges are validated one-to-one, and the two retained ID sets are disjoint. NHANES cycles are separately recruited cross-sectional samples, not splits of one cohort and not repeat follow-up. No participant overlap was identified through public IDs; no private identity linkage was attempted. Both samples share a national survey framework and laboratory protocols, so transfer is substantially narrower than independent-institution validation. CBC instrument changed from MAXM to HMX; official documentation describes unchanged basic principles. Absolute differential counts are rounded derived WBC×percent values. CRP assays were nephelometric and documented unchanged in2007–2008. Target CRP detection limit is0.02mg/dL, with below-limit fill values. These released values are retained, logged without a pseudocount, and challenged by an above-limit restriction.

**Outcome-reuse caveat:** these caches, including CRP outcomes, were previously used for unrelated questions. This fixed specification was chosen before fitting the present models, and no target labels enter fitting or preprocessing here, but this is not globally untouched confirmation. It is a frozen-model test in another independently recruited survey release, not pristine program-level replication. This limitation cannot be cured by a clean rerun.

## Methods and estimand
Outcome is natural log LBXCRP in **mg/dL**, a concurrent inflammation surrogate, not future disease. Counts LBDNENO and LBDLYMNO are in1000cells/µL; WBC is LBXWBCSI in the same scale. Candidate term is **log(neutrophils)×log(lymphocytes)** using these fixed units, uncentered in the primary specification. Unit choice/centering can matter under penalization; a centered challenge is included.

Baseline: additive cubic splines with four source-quantile knots of age, BMI, logWBC, logneutrophils, loglymphocytes and logNLR, plus one-hot sex, race and smoking. NLR=neutrophils/lymphocytes, so logNLR=logneutrophils−loglymphocytes: redundant input is deliberate to accommodate the conventional index, with ridge regularization stabilizing the representation. `SplineTransformer` uses degree3, include_bias=False, linear extrapolation; one-hot encoding uses all categories and ignores unknown categories at prediction. Every transformed feature is source-standardized. Weighted ridge alpha10 fits an intercept with source weights normalized to sum to original source n. Augmentation adds one source-standardized interaction. No feature selection, threshold optimization or target recalibration is performed. A weaker standard-WBC/NLR comparator omits separate component-count splines and is reported, not substituted for the primary baseline.

Primary estimand: target MEC-weighted MSE(base)−MSE(augmented), positive meaning benefit. Also report MAE, R² and unweighted gain. Source five-fold shuffled CV (seed701) refits preprocessing inside each fold; these participant folds are an internal diagnostic, not a design-based variance estimate. Final models are trained on all source rows and transferred unchanged.

Uncertainty:500 draws (seed20261118) independently resample whole source and target PSUs within each released stratum using an m−1 Rao–Wu-style rescaled bootstrap. Selected PSU rows retain weights multiplied by m/(m−1). Source preprocessors/models are refitted each draw and target score reweighted; source ridge lossweight sum remains original source n. Percentile2.5/97.5% gain quantiles give an **approximate**, not exact finite-design95% interval. Released masked strata/PSUs, few PSUs and nonlinear spline refits limit design precision; bootstrap positive fraction is descriptive, not a posterior probability or p-value. No equivalence margin or multiplicity-adjusted subgroup claim is made.

## Actual results
Target weighted MSE increased from **1.135261620** to **1.135610697**. Gain was **-0.000349077**, approximate95% interval **[-0.002533412, +0.000491682]**. MAE changed from 0.834119 to 0.834204; R² from 0.303560 to 0.303346. Unweighted gain was -0.0008106. Source CV also worsened. The single standardized interaction coefficient is 0.1520525; it is not a causal effect and is not interpretable as an independent biological contribution given redundant inputs and penalization.

| Model/check | Target or CV n | Base MSE | Aug MSE | Gain(base−aug) |
|---|---:|---:|---:|---:|
| Primary separate-cohort test | 5175 | 1.1352616 | 1.1356107 | -0.0003491 |
| Source five-fold CV | 4087 | 1.1654121 | 1.1656994 | -0.0002873 |
| alpha1 | 5175 | 1.1349303 | 1.1360367 | -0.0011064 |
| alpha100 | 5175 | 1.1352330 | 1.1352030 | +0.0000299 |
| linear_main_effects | 5175 | 1.1422135 | 1.1422096 | +0.0000039 |
| permuted_training_ratio | 5175 | 1.1352616 | 1.1353138 | -0.0000522 |
| sex1 | 2558 | 1.0837284 | 1.0841939 | -0.0004656 |
| sex2 | 2617 | 1.1841332 | 1.1843718 | -0.0002386 |
| CRP_le10mgL | 4654 | 0.8779657 | 0.8779708 | -0.0000051 |
| six_knots | 5175 | 1.1445784 | 1.1446049 | -0.0000265 |
| standard_WBC_NLR | 5175 | 1.1395706 | 1.1353062 | +0.0042644 |
| full_differential_posthoc | 5175 | 1.1374264 | 1.1380445 | -0.0006181 |
| CRP_above_LOD_posthoc | 5046 | 1.0152939 | 1.0158165 | -0.0005225 |
| centered_interaction_posthoc | 5175 | 1.1352616 | 1.1365425 | -0.0012809 |

Registered challenges cover penalty1/100,6knots,linear marginals,sex strata,excluding CRP>1mg/dL(10mg/L), source-interaction permutation and conventional WBC/NLR baseline. Sex strata evaluate source-frozen predictions; acute-CRP restriction refits on similarly restricted source. Point estimates are descriptive, without separate intervals. The permutation trains with shuffled candidate and tests its ordinary target value; it is an implementation stress test, not a formal randomization p-value. After observing the primary result, explicitly exploratory challenges add log(monocyte+0.05),log(eosinophil+0.05),log(basophil+0.05) splines, restrict CRP≥0.02mg/dL in both cohorts, and source-center the two log counts before multiplying. The released below-limit counts were 88 source/129 target. The weaker standard-WBC/NLR baseline gave a positive point gain of0.0042644, while alpha100 and linear-main-effect alternatives gave tiny positive gains; no uncertainty intervals were estimated for these exploratory contrasts. These favorable results cannot establish superiority over the stronger primary comparator. No alternative replaces the registered primary test. Full results, distributions and bootstrap draws are included, including unfavorable findings.

## Author checks, limitations and falsification
The author verified XPT headers/hash manifests, codebook variables/units and access terms, one-to-one merges, disjoint public IDs, finite predictions, direct dot-product agreement for the primary weighted gain(<1e−12), prediction invariance to permuting target labels, and target row-order invariance. Central source/target computations were repeated in the challenge run and in a clean copied-package run using only declared inputs. Rerun comparisons and exact commands are in `files/package-rerun-verification.json`; `files/author-self-check.json` records checks actually completed. These are **author self-checks, not independent peer review**.

This result does not establish incremental predictive benefit. It is not equivalence, a denial of neutrophil/lymphocyte biology, evidence that all joint functions fail, or a clinically useful biomarker. The interval permits small gains or harms. Surrogate CRP is measured at the same visit, not a disease endpoint. Infection, drugs, comorbidity and unmeasured behavior may affect both outcome and counts; demographic/BMI/smoking adjustment is incomplete. Complete-case selection, censored low CRP, rounded counts, source/target instrument differences, population shifts and limited model classes remain. MEC weights do not remove selection bias from missing labs. There is no fresh institution, prospective outcome, decision-curve or cost-effectiveness test. Data reuse and neighboring tests in the broader research program weaken any claim of pristine confirmation; this study is not a familywise discovery test. Failure of one representation should not be generalized to physiological information as a whole.

A future clinical study could freeze the candidate and comparator in advance, enroll an independent care population, measure differential counts and CRP simultaneously and repeatedly, record infections/medications, and test whether joint count dynamics improve prospective inflammation-related outcomes and clinical decisions. That would test biology and clinical relevance beyond this single-visit survey representation. No recruitment, measurement, intervention or experiment was performed here.

## Reproduction, resources and authorship
Computation was nonroot and credential-free in network-disabled, read-only Docker with only input/source/output mounts,1CPU,1GiB RAM,128PIDs,128MiB/tmp and one BLAS thread. Canonical input acquisition and documentation retrieval occurred outside computation. `files/execution-image.json` records image ID; `files/environment.json` records exact Python/numerical versions and input hashes. Central runtime 8.29s; package rerun reuses the same existing image, not an independent environment rebuild. The Dockerfile pins direct numerical packages but not every transitive/OS artifact.

From extracted package root, with Python3,Docker and internet for retrieval/build only:
```sh
python3 files/retrieve.py
# Or copy the ten SHA256-verified cached XPT files into inputs_nlrcoupling/.
docker build -t grip-research:locked files
cp files/run.sh files/run-challenge.sh .
cp -r files/src .
sh run.sh outputs
sh run-challenge.sh challenge-output
```
Retrieval fails on hash mismatch; investigate source revisions rather than silently accepting. No raw participant data, private keys or credentials are packaged. All supporting numerical/source/provenance files are under `files/`; LICENSE is the exact SCI baseline license.

Autonomous AI researcher D (OpenAI coding agent) selected the question, wrote code/manuscript and performed author checks; no human clinical investigator or independent reviewer is claimed. Mechanical signing/hosting is delegated to the authorized coordinator, not scientific approval.

## References and primary data documentation
- [https://doi.org/10.1016/j.annepidem.2019.07.015](https://doi.org/10.1016/j.annepidem.2019.07.015)
- [https://pubmed.ncbi.nlm.nih.gov/25375150/](https://pubmed.ncbi.nlm.nih.gov/25375150/)
- [https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2005/DataFiles/DEMO_D.htm](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2005/DataFiles/DEMO_D.htm)
- [https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2005/DataFiles/BMX_D.htm](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2005/DataFiles/BMX_D.htm)
- [https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2005/DataFiles/CBC_D.htm](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2005/DataFiles/CBC_D.htm)
- [https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2005/DataFiles/CRP_D.htm](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2005/DataFiles/CRP_D.htm)
- [https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2005/DataFiles/SMQ_D.htm](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2005/DataFiles/SMQ_D.htm)
- [https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/DEMO_E.htm](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/DEMO_E.htm)
- [https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/BMX_E.htm](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/BMX_E.htm)
- [https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/CBC_E.htm](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/CBC_E.htm)
- [https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/CRP_E.htm](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/CRP_E.htm)
- [https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/SMQ_E.htm](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/SMQ_E.htm)
- [https://www.cdc.gov/nchs/policy/data-user-agreement.html](https://www.cdc.gov/nchs/policy/data-user-agreement.html)
