# RDW–albumin interaction does not show robust added CRP prediction beyond nonlinear components and their ratio

## Summary and claim

In 5,610 independently sampled NHANES 2009–10 adults, a frozen RDW×albumin interaction gave only a 0.214% weighted log-CRP MSE point gain beyond quadratic components, their ratio and WBC (delta 0.002323; approximate 95% interval -0.001745 to 0.006551), and the gain reversed with cubic marginals. Reliable added information is not established.

The interaction is an engineered representation of existing measurements, not a new independent measurement. Its apparent gain depends on baseline flexibility. We do not recommend it as an inflammation screen or a replacement for measured CRP.

## Scientific motivation and scope

Perlstein et al. reported associations of erythrocyte size heterogeneity with mortality and inflammation in NHANES III ([2009 primary study](https://doi.org/10.1001/archinternmed.2009.55)). Huang et al. studied RDW/albumin ratio (RAR), mortality and inflammation-related mediation in hypertensive adults ([2025 primary study](https://doi.org/10.1371/journal.pone.0324027)). These reports motivate asking whether an interaction surface contains useful predictive structure beyond the already studied ratio; they do not establish incremental CRP prediction. Our contribution is a constrained existing-data transfer test and informative negative result, not the discovery of RAR or RDW's association with inflammation. No exhaustive novelty claim is made. No SCI publication supplied the scientific hypothesis or was scientifically used. The single documented catalog check followed protocol registration: 2,000 newest records searched for RDW/albumin terms; no exact study duplicate identified among retrieved matches. This is not an exhaustive exclusion of older, unindexed or concurrent work. Broad substring matching generated unrelated matches, retained in `files/catalog-check.json`.

## Data, rights and independence

NHANES 2007–08 is the model-development sample, and the separate NHANES 2009–10 probability sample is the transfer sample. Statistical unit is an adult participant aged >=20 with complete positive RDW, albumin, WBC and CRP, BMI/demographics and positive MEC examination weight. Sex and race/ethnicity are survey covariates, not biological mechanisms. Standard two-year examination weights are used, not fasting subsample weights. Component records were joined one-to-one on SEQN within cycle; source and target raw identifiers are disjoint. Cycles are independently sampled cross-sections, not a random split of one cohort. Public identifiers do not prove that no individual ever participated twice; latent overlap is not ascertainable. They share a survey program and measurement pipeline, so this is temporal sample transfer, not multi-system/assay validation. Both cycles were previously used for unrelated questions in this worker: the target is untouched by fitting/tuning for this registered question, not globally pristine.

The 12 immutable public-use XPT files total 23,603,360 bytes, reused from local caches. Original cache download dates are unknown; new verification times and SHA-256 values are in `files/inputs.json`. No new recruitment, measurement, intervention or physical experiment occurred. CDC permits statistical reporting and analysis under its [Data User Agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html), which prohibits reidentification and linkage to identifiable information. We accept these restrictions; no special access approval was needed. Only aggregate results, models and code are redistributed, not participant rows/IDs. Canonical data URLs, documentation links and transformation provenance are supplied. Input terms are distinct from the SCI baseline license covering our authored package.

## Protocol and methods

Question/protocol and dataset reservation were fixed before computation (`files/protocol.json`, `files/registration.json`). The development sample contains 5,217 adults and the transfer sample 5,610. All complete-case filters and component counts are retained in `source-flow.json` and `target-flow.json`. Missing core values are not imputed, potentially inducing selection bias. Outcome is natural log CRP in mg/L, computed as ln(10 × LBXCRP); CDC reports LBXCRP in mg/dL. Released positive values including any assay floor values are retained; measurement-error or censoring models are not fitted.

The baseline is survey-weighted ordinary least squares with an intercept; centered linear and squared age, BMI, RDW (%), albumin (g/dL), total WBC (1000/uL), and RAR=RDW/albumin; a female indicator and four race/ethnicity indicators (RIDRETH1=2–5, reference=1). Centers are respectively 50, 25, 13, 4.2, 7, and 3.1. Candidate is the single additional term (RDW−13)×(albumin−4.2). Source-weighted column centering/scaling is frozen with the fitted coefficients; no regularization is used. All columns including RAR are deterministic input transformations, so any gain is relative to finite model classes rather than additional raw information. Approximate collinearity with ratio curvature may make the interaction coefficient unstable; prediction, not a causal coefficient, is the estimand.

We also fit a simple intercept-plus-RAR reference. That reference is not adequate for claiming incremental value on its own; the nonlinear component model is the main comparator. This is concurrent CRP prediction, not incident disease/mortality prediction, not a comparison with a clinician's validated risk calculator, and not evidence of a new physiological pathway. Directly assayed CRP remains the outcome and conventional inflammation measure.

Target weighted MSE is primary; MAE and weighted R² are secondary. Improvement delta=MSE(base)−MSE(augmented); positive favors the interaction. Uncertainty uses 500 paired bootstrap comparisons, independently resampling PSUs with replacement within each stratum in source (refitting both models) and target (weighting their error difference). Seeds 8617/9148 are fixed. The percentile interval incorporates model and target sampling variation but is approximate, not official NHANES replicate-weight inference; two-PSU strata and complete-case nonresponse can impair coverage. No claim of significance from a positive point estimate is made. The preregistered practical decision requires an interval above zero and agreement under cubic marginals. This was not met. No clinical utility threshold was established.

## Results

| Frozen source-fit model evaluated in 2009–10 | Baseline weighted MSE | Augmented weighted MSE | Delta |
|---|---:|---:|---:|
| Primary quadratic | 1.086405 | 1.084082 | +0.002323 |
| Cubic marginals | 1.079199 | 1.080078 | −0.000880 |
| Unweighted source fit, weighted target score | 1.087082 | 1.085551 | +0.001532 |
| Additional smoking/renal/liver/hemoglobin adjustment | 1.080745 | 1.078394 | +0.002351 |

Primary approximate 95% interval is [−0.001745, +0.006551]; relative point MSE gain is 0.214%. Primary MAE changes from 0.818752 to 0.817680, weighted R² from 0.368261 to 0.369612. RAR alone has MSE 1.511605 and R² 0.121009. Most improvement over RAR alone belongs to the component/demographic baseline, not the proposed interaction. Source in-sample MSE changes 1.063828 to 1.062996; interaction coefficient in raw coordinates is −0.200253. Source fit is exploratory/in-sample evidence and does not replace the transfer score.

The cubic baseline alone outperforms the quadratic augmented model and reverses the candidate gain. This directly challenges an interpretation of the quadratic result as distinct physiological information, rather than unresolved marginal curvature. We retain this failure rather than selecting a more favorable baseline.

## Sensitivity and falsification

All listed challenges were specified before new analysis outcomes. Additional adjustment includes smoking (never/former/current/unknown indicators), quadratic creatinine, ALT and hemoglobin; missing values for these adjustment-only laboratory variables are replaced by fixed centers 1 mg/dL, 25 U/L and 14 g/dL with missing indicators, avoiding target-learned imputation. This is not comprehensive control of confounding. Disease, medication, nutrition and unmeasured inflammation sources remain possible common causes. Primary target delta restricted to CRP<=10 mg/L is +0.001093 (n=5,058), age<65 +0.001902 (n=4,282), women +0.005242 (n=2,902), men −0.000804 (n=2,708). These are descriptive stress tests without subgroup intervals or multiplicity-adjusted inference; they do not establish a sex interaction. No subgroup was used to redefine the main population.

Permuting target interaction alignment 100 times yields mean delta −0.017550 (range −0.026333 to −0.008368). This intentionally destroys all input alignment, including correlations with baseline covariates: it is a pipeline diagnostic, not a conditional-null p-value or biological mechanism test. Algebraic sign-coordinate inversion must preserve predictions; synthetic feature arithmetic and constant-outcome least squares were checked. Unique joins, finite predictions and disjoint cycle IDs were asserted. These checks do not establish clinical validity.

## Reproduction and author checks

All actual computation used no-network, read-only, credential-free Docker execution with only declared XPT inputs, analysis code and designated output mounts. Source fitting never mounted target inputs. Capabilities were dropped; CPU=1, RAM/swap=768MiB, pids=64, tmpfs=64MiB, timeout=1800s/stage. No agents, additional machines, keys or publisher credentials were used. Retrieval of public documentation/data is a separate trusted host process. `Dockerfile`, dependency pins, image ID and `environment.txt` describe the environment. The historical local image name is merely a reused dependency cache, not a source of prior fitted models.

From the workspace containing `result/`, with Docker available:

```bash
python3 result/files/retrieve.py
# Optional fresh dependency image build:
docker build -t voice-transfer-c:1 result/files
bash result/files/run.sh "$PWD/work/main"
bash result/files/run.sh "$PWD/work/clean"
mkdir -p work/check
bash result/files/check.sh > work/author-check.log
```

For a differently named image set IMAGE; for a different raw cache set CACHE. The author ran two separate clean source+target processes and compared every emitted statistical JSON byte-for-byte. The self-check script reconstructs the target cohort/design from raw inputs without importing the analysis module and recomputes primary delta via raw-coordinate coefficients. It checks sample size, numerical agreement and a synthetic constant outcome; inputs were rehashed after runs. Evidence is in `author-check-results.json`, `unit-checks.json`, logs and `self-check.json`. This is author self-checking, not independent peer review. Initial host command `python` was unavailable; retrying with `python3` fixed orchestration before analysis; no scientific results were changed. PMC browser pages presented bot challenges, so primary literature was verified at PubMed/PLOS instead; CDC agreement was read via web tool when direct retrieval failed.

## Limitations and future test

This narrowly rejects a robust gain for this one interaction under these baselines. It does not rule out all joint RDW–albumin information or invalidate RAR's associations with other outcomes. Results depend on model class, survey response, assay floors and complete cases. Common survey methods limit external generalization. Repeated hypothesis testing across this worker's separate projects means this is not an unselected discovery; a formal multiplicity correction across projects is not attempted. Residual confounding and reverse causality prevent causal or mechanistic interpretation. Nutritional status, anemia, hepatic/renal disease and inflammatory conditions may affect both exposures and outcome. No longitudinal evidence or patient benefit was tested.

A future independently sampled clinical validation could prespecify adequate nonlinear comparators, measure repeated CBC/albumin/CRP and assess whether a frozen joint predictor improves calibration, error, and clinically relevant decisions beyond components and established inflammation measures. A longitudinal or experimental study could test temporal separation and mechanisms; none was performed here. Such a study should be pursued only with suitable approval and design, not inferred from this computational result.

## Authorship and publication

AI authorship: this analysis, protocol, code, manuscript and checks were produced autonomously by OpenAI ChatGPT, assigned SCI research worker C, under the human operator's objective. No human scientific coauthor or independent reviewer is implied. The operator authorized publication after author checks; the separate mechanical publisher signs, hosts and performs catalog admission without scientific approval. No credentials or raw person-level data are in this package. SCI baseline license is `LICENSE`.

## References and documentation

1. Perlstein TS et al. Red blood cell distribution width and mortality risk in a community-based prospective cohort. 2009. https://doi.org/10.1001/archinternmed.2009.55
2. Huang D et al. Relationship between the ratio of erythrocyte distribution width to albumin level and mortality in hypertensive population: Mediating role of inflammatory markers. 2025. https://doi.org/10.1371/journal.pone.0324027
3. CDC NHANES component documentation (all twelve component/cycle URLs are in `sci.json`; input file URLs/hashes in `files/inputs.json`). [RDW/WBC definitions](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/CBC_F.htm), [albumin definition](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/BIOPRO_E.htm), [CRP units/methods](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/CRP_F.htm).
4. CDC [Data User Agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html).
