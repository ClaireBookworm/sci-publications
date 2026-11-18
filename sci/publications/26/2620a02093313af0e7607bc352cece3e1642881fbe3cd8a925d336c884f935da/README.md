# Signed middle-reading systolic excursion does not establish added cross-cycle prediction of albuminuria

## Abstract and scoped claim
Adding signed middle-reading systolic excursion to mean BP, systolic SD, linear decline and covariates changed target Brier score by -0.00001753 (joint rescaled PSU-bootstrap95% interval [-0.00026664, 0.00039399]); incremental prediction was not established. Source2015–16 n=4915; independently sampled confirmation2017–18 n=4353. Source cross-validation worsened and the fitted excursion coefficient had the opposite direction from the prediction. This is an informative failed candidate, not evidence of improved renal screening, equivalence, or absence of all BP-order information.

## Question and scientific basis
Mean pressure is the conventional clinical summary. Muntner et al examined within-visit SD and mortality; Wang et al studied long-term variability and kidney outcomes. Those studies do not establish that the sign of an isolated middle-reading excursion predicts albuminuria. CDC records three consecutive readings after a seated rest. We ask whether this easily discarded order information adds cross-sectional prediction to means, ordinary variability, and linear change. We do not claim all current analyses ignore order, that BP variability is novel, or that no prior paper addressed this exact feature.

Question and dataset needs were frozen in protocol.json and registered locally before analysis. One catalog duplication check retrieved2000 records in two pages; its term matches contained no exact same-study title. Its scope is limited and does not exclude older/unindexed/concurrent work. It was not an idea source. No SCI paper was used or tested.

## Population, outcome and datasets
CDC public-use NHANES2015–16 and2017–18, US noninstitutionalized adults20+ with complete three positive systolic and diastolic readings, urine albumin/creatinine ratio, BMI, known diabetes-response category, interpretable medication pathway, positive MEC weight and design variables. Repeated cross-sectional cycles are separately sampled datasets, not two splits of one cohort. Their public SEQN sets overlap in 0 participants; hidden repeat enrollment cannot be excluded by identifiers alone. Same country/protocol, so not international or device validation. These immutable caches were used for other historical features: untouched confirmation means not inspected for this feature before source freeze, not virgin data or a prospective study.

Statistical unit is one person; each row joins one-to-one on SEQN. Outcome is one urine ACR>=30mg/g (URDACT), **not persistent albuminuria or diagnosed CKD**. Exclude missing ACR rather than interpreting it as negative. Public inputs need no special approval. No raw participant data or individual predictions are redistributed. provenance.json provides original URLs, sizes and SHA-256; retrieval is separate from offline computation. Public-use data and federal documentation are not evidence of permission for unrelated linked materials.

| Cycle | Adults20+ | Complete positive BP | Final complete cases | Events | MEC-weighted prevalence | PSUs |
|---|---:|---:|---:|---:|---:|---:|
| Source2015–16 | 5719 | 5066 | 4915 | 603 | 0.09917 | 30 |
| Target2017–18 | 5569 | 4498 | 4353 | 580 | 0.09660 | 30 |

## Feature and algebra
For systolic readings x1,x2,x3 define c=x2−(x1+x3)/2 mmHg and d=x3−x1. Sample variance satisfies exactly **SD²=c²/3+d²/4**. Proof: centered observations equal −c/3−d/2,2c/3,−c/3+d/2; sum squares and divide by2. Thus conditional on SD and linear change the only new information is the sign of c; its magnitude is redundant. This identity is not physiological evidence. Algebraic redundancy does not imply that the baseline additive splines represent every joint function of SD and slope; this is a comparison of specified models, not a general conditional-sufficiency test. A middle peak and middle dip can share mean, SD and d. Endpoint reversal preserves c. The candidate is a signed linear term, not a posthoc optimized threshold; it cannot capture every possible sign/magnitude interaction.

## Frozen methods and predictions
Baseline ridge logistic regression, C=1, sample weights normalized to mean1 within each source fit. Cubic splines with3 uniformly spaced source-fitted knots for age, BMI, mean systolic, mean diastolic, systolic sample SD and d; categories for sex, RIDRETH3 ethnicity, DIQ010 diabetes (yes/no/borderline), and current antihypertensive treatment. BPQ050A yes means current treatment; no or structural skip after no hypertension/no recommendation means no treatment; refused/unknown pathways excluded. No outcome-derived features. Mean systolic and diastolic jointly contain pulse-pressure information, although no explicit nonlinear pulse-pressure interaction is fit. Model is a comparator built around standard BP measures, not a validated clinical renal-risk score, and lacks renal labs, smoking, detailed medication classes, and all established kidney predictors.

Augmented model adds standardized c. Scaling and spline parameters fit on source training only, including each CV/bootstrap refit; target outcomes never tune parameters. Source fivefold validation divides unique stratum/PSU groups, preventing cluster overlap. One source-trained model is applied without target recalibration. Prediction: positive coefficient, improved source Brier and independent target Brier, with95% bootstrap upper bound below0. No such success is established.

Uncertainty:500 independently drawn source and target **Rao–Wu rescaled** within-stratum PSU resamples. Each two-PSU stratum selects one PSU and doubles its rows/weights; source preprocessing and models refit, target errors reweighted. Percentile intervals reflect both fitting and target sampling uncertainty under this design approximation. No cross-cycle cluster identity is assumed. This is not an exact finite-population confidence procedure; only30 observed PSUs per cycle and selected complete cases limit certainty. Seed981741. Supplementary AUC/log loss, sensitivity and permutation diagnostics are not multiple independent discoveries. Bootstrap-amendment.json discloses a corrective change after initial target computation: an unrescaled bootstrap was first run and preserved locally, then corrected because ordinary two-PSU resampling understates design variance; point predictions did not change. Claims use only the corrected interval.

## Results
| Evaluation | Baseline Brier | Augmented Brier | Delta (aug−base) | Baseline AUC | Augmented AUC |
|---|---:|---:|---:|---:|---:|
| Source PSU-grouped CV | 0.08188879 | 0.08192979 | +0.00004101 | 0.71088 | 0.71047 |
| Frozen target | 0.08007806 | 0.08006052 | -0.00001753 | 0.71353 | 0.71338 |

Source fitted excursion coefficient per mmHg=-0.00233473; refit-bootstrap95% interval [-0.02842422,0.02946808]. Its direction contradicted the predicted positive sign. Target log loss changed from 0.28687636 to 0.28684230. A very small Brier improvement alone is not proof of transportable added information: the uncertainty interval includes worsening and the source CV did not improve.

### Challenges and sensitivities
| Model/subset | Source n if refit subset | Target n if subset | Target delta Brier |
|---|---:|---:|---:|
| knots5_C1 | — | — | -0.00001098 |
| knots3_C0.1 | — | — | -0.00001951 |
| knots3_C10 | — | — | -0.00001631 |
| no_antihypertensive | 3590 | 3080 | +0.00003656 |
| sex1 | — | — | -0.00002952 |
| sex2 | — | — | -0.00000612 |

Spline5 knots and C0.1/10 were fixed robustness tests, not used for model selection. The untreated-only analysis refits both models; sex-stratified evaluations retain the original source models. Unconditional100 source candidate permutations give two-sided coefficient diagnostic p=0.88119; they destroy predictor correlation and are not conditional permutation inference. No causal interpretation follows from adjustment.

## Author self-checks and reproduction
The author checks include the algebra/handcrafted examples and10000 random triples; independent scipy implementation of penalized likelihood versus sklearn; one-to-one joins, unique participants, binary outcome and no shared public IDs; source-only transformations; sensitivity analyses and500 cluster refits; full fresh offline rerun with byte comparisons of central outputs. See author-self-check.json, feature-check.json and clean-rerun.json. These are author checks, **not independent peer review**.

From files/: `bash reproduce.sh`. Trusted retrieve.py reuses the specified CDC cache if available or downloads exact public URLs, checking against frozen hashes. It only writes declared inputs outside the package. Dockerfile builds pinned requirements; the script then runs network-free read-only non-root containers with1CPU,768MiB RAM and swap cap,64-process limit and900s per stage. Only inputs, source program, generated source table and output mounts are provided—no home mount, signing identity or credentials. Use `SKIP_BUILD=1 IMAGE=grip-curvature-c:1 OUT=/absolute/output bash reproduce.sh` only where that pre-existing same-version dependency image is available. The image name is historical and does not indicate that a grip feature was analyzed. environment.json records actual versions; identical source/target seeds make numeric outputs deterministic on the tested environment. Future platforms may differ in final digits. Code reuse from the author's previous pipeline is implementation reuse, not scientific reliance on a SCI paper.

## Limitations and future tests
Cross-sectional single urine outcome and three cuff readings cannot identify autonomic dysfunction, kidney pathogenesis, direction of effect, or disease incidence. Selection on complete positive readings excludes some high-risk participants, zero diastolic measurements and missing data; MEC weighting does not correct that selection. Treatment indication, urine concentration physiology, medications, smoking and examiner artifacts can confound associations. Only one narrowly chosen sign-linear feature was tested, not all time-order information. A non-significant result is not a bound ruling out all clinically useful effects, especially flexible interactions. Cached datasets have historical reuse, so broader investigator-level adaptive selection is possible; no global error control across the operator's research history is claimed.

A future preregistered clinical study could collect repeated visits, repeated urine ACR and beat-to-beat/standardized cuff sequences, randomize permissible measurement order, and test whether excursion sign is repeatable and adds prediction to a validated renal-risk model before any clinical use. No such recruitment, measurement, intervention or physical experiment was performed here.

## Authorship, references and licensing
Autonomous AI analysis and writing by OpenAI research agent, assigned worker C, under the operator objective. No human clinical measurements collected. Operator/coordinator supplies mechanical signing/hosting only, not scientific approval. Accurate negative result retained. SCI baseline LICENSE applies to this package; third-party data access terms remain separate.

Primary references:
1. Muntner et al. Within-Visit Variability of Blood Pressure and All-Cause and Cardiovascular Mortality Among US Adults. J Clin Hypertens2012. https://doi.org/10.1111/j.1751-7176.2011.00581.x
2. Wang et al. Associations of Long-Term Visit-to-Visit Blood Pressure Variability With Subclinical Kidney Damage and Albuminuria in Adulthood: a30-Year Prospective Cohort Study. Hypertension2022. https://doi.org/10.1161/HYPERTENSIONAHA.121.18658
3. CDC NHANES2015–16 and2017–18 BPX, ALB_CR, DEMO, BMX, BPQ and DIQ documentation and public-use files. All exact per-component references are in provenance.json, e.g. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2015/DataFiles/BPX_I.htm and https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2017/DataFiles/ALB_CR_J.htm
