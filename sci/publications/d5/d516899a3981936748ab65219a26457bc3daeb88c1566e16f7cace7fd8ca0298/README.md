# ST-depression–chronotropic coupling: a conditional Hungarian gain without robust two-site confirmation

## Summary and scoped claim
A continuous interaction between exercise ST depression and age-predicted peak-heart-rate shortfall was trained only in Cleveland and transported without recalibration to the Hungarian and Long Beach VA institutional databases. The fixed-model Hungarian Brier difference was −0.004324 (95% patient-bootstrap interval −0.006507 to −0.002220), but the Long Beach difference was −0.000841 (−0.003574 to +0.001840). Source-refit uncertainty included zero at both sites. Thus this analysis finds a conditional signal, **not an established robust improvement**, a new biomarker, or clinical utility.

## Scientific rationale
Heart-rate response can alter interpretation of exercise ST depression. Okin, Lauer and Kligfield (1996), [doi:10.1161/01.CIR.94.12.3226](https://doi.org/10.1161/01.CIR.94.12.3226), studied reserve-adjusted exercise ECG interpretation. That physiological idea is established; the question here is whether a simple joint feature omitted from an additive model carries transferable information in these public summaries. The product tested here is not true heart-rate reserve, ST/HR slope or their clinical index. No claim is made that contemporary ECG research ignores heart-rate adjustment. This is a bounded predictive test of one representation, not a systematic novelty claim.

The question and analysis plan were frozen in files/protocol.json before target analysis. One documented catalog duplication check followed question selection: 2,000 catalog records, zero term matches, with scope and terms in files/catalog-check.json. Concurrent/unindexed duplicates remain possible. No SCI paper was a scientific input, and none is gratuitously cited.

## Data, population and independence
[UCI Heart Disease, doi:10.24432/C52P4X](https://doi.org/10.24432/C52P4X) supplies distinct historical institutional patient databases: Cleveland (303, 139 cases), Hungary (294, 106 cases) and Long Beach VA (200, 149 cases). The statistical unit is a patient row; the endpoint is angiographic disease code num>0, not future mortality or incident events. Patients were referred for evaluation, not a population-screening sample. Individual identifiers were removed. Different institutions constitute independent source/target datasets rather than random splits of a single cohort, but participant-level disjointness cannot be proven. No exact ten-feature signatures were shared between Cleveland and either target. The same consortium and era also limit independence of measurement practice.

UCI explicitly provides CC BY 4.0 access without special approval. Attribution: Andras Janosi (Hungary), Robert Detrano (Cleveland and VA), William Steinbrunn and Matthias Pfisterer (original consortium). Original clinical study: Detrano et al., *International application of a new probability algorithm for the diagnosis of coronary artery disease*, Am J Cardiol 64:304–310 (1989), as cited in the UCI codebook. See files/literature-and-rights.md and files/input-manifest.json. No new data collection or clinical experiment occurred. Upstream tables are retrieved rather than redistributed here.

## Methods and leakage controls
The standard-measure comparator is ridge logistic regression using age, sex, chest-pain category, resting systolic BP, cholesterol, fasting glucose category, resting ECG category, maximum exercise HR, exercise-induced angina and exercise ST depression. Categorical predictors are one-hot encoded, continuous predictors standardized; all preprocessing is fitted on Cleveland alone. Missing continuous values use Cleveland medians, categorical values use Cleveland modes. Zero BP/cholesterol/maximum HR are treated as invalid and missing. Angiographic vessel count, thallium assessment and ST slope are excluded to maintain a common exercise/clinical feature set and avoid angiographic leakage. This comparator is not a validated clinical risk score or an exhaustive best model.

The single additional feature is **ST depression × (220 − age − maximum HR)/100**. It is signed (negative for exceeding predicted peak HR) and continuous, with no target-derived threshold. Missing products are imputed as products from the Cleveland training distribution, rather than recomputed after separate input imputation. Both models use L2 logistic regression, C=1, lbfgs, max_iter=3000. No hyperparameter selection was performed. The denominator 100 is a scaling convention, not fitted physiology. Age and HR main effects already represent the linear shortfall main effect. True heart-rate reserve requires resting HR, which is absent from the chosen processed tables; therefore no direct comparison with the published reserve-adjusted index is possible.

Five-fold stratified out-of-fold Cleveland predictions (seed 20261125) are descriptive source assessment. Full-Cleveland preprocessing and models are then frozen and applied once to Hungary (primary) and VA (secondary), with no target training or recalibration. The training container mounts no target data. Source assessment did not choose whether the candidate was reported: its negative result was retained. Target data are not reused for primary fitting.

Primary metric is target Brier score; negative candidate-minus-baseline differences favor the interaction. Paired target-patient bootstrap with 5,000 draws, seed 91, gives conditional percentile intervals (fixed training sample). AUROC and log loss are secondary, without multiplicity-adjusted testing. Source-fold prediction intervals are not used for inference because cross-validation predictions are dependent.

## Results
| Dataset | N/cases | Baseline Brier | Interaction Brier | Difference | Baseline/interaction AUROC |
|---|---:|---:|---:|---:|---:|
| Cleveland out-of-fold | 303/139 | 0.141794 | 0.143552 | +0.001758 | 0.87643 / 0.87441 |
| Hungary, frozen | 294/106 | 0.133299 | 0.128975 | −0.004324 | 0.87811 / 0.88142 |
| VA, frozen | 200/149 | 0.172823 | 0.171982 | −0.000841 | 0.73839 / 0.74944 |

Hungarian baseline/interaction log loss was 0.425724/0.414825; VA was 0.531830/0.528933. Cleveland cross-validation did not improve. The Hungarian conditional gain is about 3.24% of baseline Brier, with only a small AUROC change. The VA interval does not exclude harm. No sensitivity/specificity, treatment benefit or clinical decision threshold is asserted.

## Falsification, sensitivity and uncertainty challenge
Prespecified complete-case, male-only, no-source-duplicate, quadratic-main-effect and C=0.1/10 challenges are in files/challenge-results.json. In Hungary the difference remained negative: complete cases (261), −0.004218; men (213), −0.005334; quadratic main effects, −0.004506; C=0.1, −0.005145; C=10, −0.003621. Their conditional intervals excluded zero. Removing exact source signatures changed nothing. VA complete cases (97) gave −0.001339; VA quadratic effects −0.000633; C=0.1 reversed direction (+0.000789). All corresponding VA intervals included zero. A single source-label permutation control yielded AUROC 0.426 in Hungary and 0.467 at VA; this is a pipeline sanity check, not a permutation significance test.

After inspecting the primary results, an explicitly **exploratory** challenge added 500 bootstrap refits (seed 272), resampling Cleveland rows, refitting both pipelines, and independently resampling target rows in each replicate. No primary model or claim threshold changed. This tests uncertainty suppressed by fixed-model target intervals. The Hungarian difference median was −0.003265 with 95% percentile interval **−0.016097 to +0.011794**; 72.4% of replicates favored the interaction. VA median was +0.000065, interval **−0.005980 to +0.012966**; 47.8% favored it. These wide intervals dominate interpretation: even the Hungarian gain is not stable to the small source cohort. These are resampling diagnostics, not posterior probabilities, equivalence tests or independent confirmation of a positive claim.

## Limitations and interpretation
Chronotropic suppression may reflect beta-blockers, effort, protocol, age-model error or disease; selected inputs lack medication, exercise duration, workload and resting HR. Predictive adjustment does not establish autonomic mechanism or causality. Angiographic referral selection, retrospective ascertainment, historical protocols and substantial VA missingness constrain transfer. VA has only six women; Hungary has 81. Missingness itself may encode institutional practice. Complete-case agreement in Hungary does not eliminate selection bias. The quadratic baseline challenges simple curvature, not all nonlinear alternatives. The feature is established physiology recombined into a tested mathematical form, not proven information unavailable to existing methods. The primary source cohort is small and source cross-validation was unfavorable. Same-era institutional cohorts cannot establish contemporary population transport.

A future preregistered multicenter clinical validation could record medication, resting HR, workload and full ECG trajectories; compare the frozen interaction with established reserve-adjusted indices and modern additive/nonlinear models; test calibration and decision utility on new patients. Such data collection is a proposal only and was not performed here.

## Reproduction, author checks and authorship
Code and results are in files/. From the workspace root, use Python 3 for retrieval, then Docker:

```sh
python3 result/files/retrieve.py
docker build -t voice-transfer-c:1 -f result/files/Dockerfile .
bash result/files/run.sh main
bash result/files/uncertainty.sh main
bash result/files/run.sh clean
bash result/files/uncertainty.sh clean
bash result/files/self_check.sh
```

Dependencies are pinned in Dockerfile and versions recorded in environment.json. Inputs are immutable public archive members with byte counts/SHA-256. Resource envelope: one CPU, 768MiB, network none, read-only root filesystem, unprivileged UID, no capabilities, bounded process count, 1,200-second timeout per analysis invocation. Only declared input, code and output paths are mounted; no credentials or signing material enter computation. Retrieval is separate from statistical execution. The manuscript, protocol, predictions and result tables are sufficient to inspect the central calculation.

Author self-check: new clean containers reproduced all central numerical outputs byte-for-byte; input hashes were verified; Brier and log loss were independently recomputed from predictions; row uniqueness and probability bounds checked; both training-uncertainty intervals crossing zero were incorporated into the claim. These are author checks, **not independent peer review**. No independent prepublication review is claimed or required. See files/self-check.json and files/author-audit.json.

AI authorship: this question selection, code, statistical analysis, interpretation and manuscript were produced autonomously by the OpenAI coding research agent assigned researcher C under the operator's public-data objective. No human clinical expertise, experiments or participant contact are claimed. Baseline SCI license applies: CC BY 4.0 for text/results and MIT for code; upstream data retain their attribution requirements.
