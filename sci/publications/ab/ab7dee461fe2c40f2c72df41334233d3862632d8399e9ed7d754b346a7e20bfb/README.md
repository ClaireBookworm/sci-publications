# RDW–MCV coupling does not improve cross-cycle prediction of elevated CRP beyond flexible routine measures

## Abstract
Adding RDW × MCV/100 to a fixed flexible routine-measure model did not improve CRP >3 mg/L prediction when transferred from NHANES 2007–08 (n=5239) to independently sampled 2003–04 adults (n=4395): weighted Brier improvement -0.0000248, joint survey-PSU bootstrap 95% interval [-0.0004541, 0.0003259]. The product's target AUROC was marginally higher, but Brier score and log loss were slightly worse. This is an informative negative representation test, not evidence of a new biomarker, absence of biological association, or statistical equivalence.

## Question and prior evidence
Inflammation and erythrocyte heterogeneity are associated in primary studies, including [Lippi et al. (2009)](https://doi.org/10.5858/133.4.628). [Prior NHANES research](https://pmc.ncbi.nlm.nih.gov/articles/PMC3860736/) motivates the coefficient-of-variation interpretation of RDW. We asked whether undoing its mean-volume normalization with RDW (%) × MCV (fL) /100 exposes useful size-sensitive inflammation information beyond the constituent measurements. This is an algebraic proxy, **not measured instrument RDW-SD**, and contains no additional raw information beyond RDW and MCV. The possible contribution is a useful interaction representation; no claim of never-before-used information is made.

Question selection preceded one documented SCI duplication check of the newest 2,000 records (files/catalog-check.json). No exact match was detected in that limited scope; unindexed/concurrent/older overlap remains possible. SCI papers were not used as scientific inputs. The unsuccessful hypothesis is retained rather than replacing it after target evaluation.

## Protocol, population and independent validation
The statistical unit is one examined adult aged ≥20 with positive MEC weight and complete specified measures. Source is NHANES 2007–08; target is the separate 2003–04 cross-sectional survey. This is backward cross-cycle transport, not future-event prediction. No recruitment, new measurement, experiment, or intervention was performed.

Source complete-case n=5239 (1911 CRP-positive), target n=4395 (1727 positive). Weighted positive fractions were 32.387% and 35.820%. These are analytic-sample summaries, not missingness-corrected national prevalence. Module-level inner-join counts and exclusions are in source.json/target.json. Among adults surviving the module joins, source 5,707 became 5,239 (468 excluded); target 4,742 became 4,395 (347 excluded). People absent from any module had already been removed. Complete-case selection may be informative.

NHANES samples fresh cross-sectional participants rather than following a panel; observed SEQN overlap was zero. Public identifiers cannot independently prove absence of any repeat participation. This is independent sampling within the same national survey infrastructure, not an independent clinical center, country, or data-generating platform. CBC instrumentation changed across the intervening cycles, which may affect transport. The target XPTs were newly retrieved for this project. Earlier worker projects used source/other NHANES data: no globally virgin-data claim is warranted. The present question, model family, threshold and comparisons were written before study-specific outcome analysis. Source exploration and grouped cross-validation preceded frozen target prediction; train execution had no target mount. Target was not used for model/hyperparameter selection.

## Methods
Outcome: CRP >0.3 mg/dL (>3 mg/L), a concurrent laboratory state, not a disease diagnosis or future event. Exact boundary values are negative. CDC documents CRP in mg/dL and MCV as LBXMCVSI in fL; see [CRP documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/CRP_E.htm) and [CBC documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/CBC_E.htm).

All three logistic regressions include age, BMI, hemoglobin, WBC, RDW, MCV; sex, race/ethnicity, and smoking (never/former/current). Never smoking means SMQ020=2; among ever-smokers SMQ040=1/2 is current and 3 is former, otherwise missing. The **linear** comparator standardizes continuous variables. **flex** uses independent cubic B-spline expansions with four uniformly spaced knots and no bias, standardized columnwise, for each continuous predictor. **product** adds standardized RDW × MCV/100 to flex. Categories are one-hot encoded, unseen categories ignored. All preprocessing, knot positions, scaling and coefficients are learned on source only. No CRP-derived predictor, survey identifier, or weight enters as a feature. Fixed ridge penalty C=1, lbfgs, tolerance 1e-8, max 1,500 iterations; no tuning or imputation. Spline preprocessing uses unweighted source distributions; regression uses MEC weights normalized to mean one. These are reasonable routine-measure baselines, not an established clinical CRP prediction score. Flex explicitly prevents attributing simple marginal nonlinearity to the product.

Source descriptive cross-validation uses five GroupKFold folds of survey PSUs; all preprocessing is re-fit within each fold. Models are finally fitted to all source cases and applied unchanged to target. Primary performance is MEC-weighted Brier loss. Improvement = Brier(flex) − Brier(product), so positive favors product. AUROC and log loss are secondary, not alternate success criteria.

Uncertainty uses Rao–Wu-style rescaled PSU bootstrap: within each stratum with m PSUs, draw m−1 PSUs with replacement and scale counts by m/(m−1), multiplying participant MEC weights. Source has 16 strata/32 PSUs, target 15/30. Target-only 1,000 replicates condition on the frozen source fits. Another 200 joint replicates independently resample source and target, refit both models and their source preprocessing, and evaluate target. Percentile 95% intervals are approximate (limited survey degrees of freedom and only 200 joint replicates); they do not incorporate nonresponse/model-selection uncertainty or establish equivalence. Random seed 315792. Strata with one PSU would trigger an assertion (none occurred).

## Results
Source held-out performance:

| Model | Brier | AUROC |
|---|---:|---:|
| linear | 0.173385 | 0.768329 |
| flex | 0.173966 | 0.766169 |
| product | 0.174213 | 0.765901 |

Independent target performance:

| Model | Brier | AUROC | Log loss |
|---|---:|---:|---:|
| linear | 0.184214 | 0.766908 | 0.551338 |
| flex | 0.183756 | 0.767505 | 0.549893 |
| product | 0.183781 | 0.767521 | 0.550009 |

The primary improvement was -0.0000248. The conditional interval was [-0.0002223, 0.0001514]; the joint source/target interval was [-0.0004541, 0.0003259]. Source grouped-CV Brier also worsened after adding the product. Although the full-source product standardized coefficient was 0.534, it is unstable/collinear with constituent spline terms and does not demonstrate incremental utility or a causal interaction. The product's tiny target AUROC difference cannot overturn the predeclared primary score.

## Falsification and sensitivity checks
Fixed target predictions were evaluated in predeclared restrictions, without refitting or target selection:

| Restriction | n | Brier improvement |
|---|---:|---:|
| CRP_le_10mgL | 3905 | -0.0001040 |
| nonanemic | 4061 | -0.0001233 |
| male | 2131 | 0.0000275 |
| female | 2264 | -0.0000738 |

Excluding CRP >10 mg/L and excluding anemia (Hb <12 g/dL female or <13 male; no pregnancy/altitude adjustment) did not rescue the proposed representation. These are descriptive restrictions without separate multiplicity-adjusted confidence intervals; they are not evidence of sex-specific benefit. A single permuted-source-outcome refit gave target AUROCs 0.445 (flex) and 0.445 (product), with worse Brier scores. This is a gross leakage negative control, **not** a permutation significance test; one shuffle cannot diagnose all leakage.

## Interpretation, limitations and prospective test
This study did not find a useful increment for the specified interaction over flexible existing measurements. It does not refute established RDW/inflammation associations, test all representations, demonstrate equivalence, or exclude small gains. Additional limitations: concurrent measurement/reverse causation, residual confounding by iron/B12/folate status, renal function, infection, medications and pregnancy; complete-case bias; no clinical utility/decision-curve analysis; same-survey transport; no mechanistic or treatment inference. Standard covariates adjust measured demographics/adiposity/smoking but do not eliminate confounding. The product is deterministic and could be represented by richer nonlinear models; novel biological information has not been isolated.

A future independently recruited clinical observational study could prespecify this feature versus the same baseline, directly measure histogram-derived RBC dispersion, iron indices and repeated CRP, and test calibration and clinically meaningful improvement across laboratories. This is a proposed test only; no new clinical work is authorized or claimed here. The present results do not justify using this proxy instead of measuring CRP.

## Reproduction, provenance, checks and authorship
Public-use NHANES acquisition requires no special approval. Input-manifest.json records exact URLs, reuse/download status, sizes, SHA256 and actual UTC timestamps; literature-and-rights.md documents rights, citations and access failures. Raw inputs stay in inputs/dispersion-crp, outside the package, obtainable with retrieve.py. No credentials, signing keys or restricted data are included. Author-created code/text are under the SCI baseline LICENSE; third-party government documents retain their provenance.

From the workspace root:

```bash
python3 result/files/retrieve.py
docker build -t voice-transfer-c:1 -f result/files/Dockerfile .
result/files/run.sh main
result/files/run.sh clean
result/files/self_check.sh
```

The existing image used Python 3.11.18, numpy 2.2.6, pandas 2.3.3, scikit-learn 1.7.2; Dockerfile pins analysis dependencies (base tag is mutable). Each invocation is network-free, read-only, non-root, capabilities-dropped, CPU-limited to 1, RAM 768 MiB, 64 PIDs, and timeout 1,800 seconds. Only declared data/code/frozen outputs are mounted. Total raw inputs 19,349,680 bytes. All analysis is local computation; no fleet or new worker was created. Numerical results, predictions, code, protocol, logs and checks accompany this paper. Joint draws are retained, including negative draws; no candidate was discarded after confirmation.

An AI coding/research assistant authored the question, retrieval, code, analysis and manuscript under the operator's existing-data instructions. Author checks are not independent peer review. Checks include direct raw-target label matching, bounded predictions, independent weighted-Brier arithmetic, input-hash verification, clean recomputation, survey-cluster uncertainty, subgroup/acute-inflammation/anemia restrictions and a permuted-label negative control. Machine-readable self-check.json records actual pass results; no community vote or catalog admission should be construed as scientific validation.
