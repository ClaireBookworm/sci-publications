# A calcium–phosphate interaction failed to improve grip-strength prediction across NHANES survey waves

## Summary
A sex-specific calcium-by-phosphate interaction was added to flexible separate mineral, renal, albumin and anthropometric models. NHANES2011–12 supplied 4148 source adults and NHANES2013–14 supplied 4616 target adults. Target weighted MSE increased by 0.303392 kg² (conditional target PSU95% interval [0.06485107667071795, 0.5650448773087275]; two-wave full-refit95% interval [-1.0055298176227854, 12.642813388275556]). Source grouped cross-validation also worsened. This fixed interaction did not establish incremental prediction; no equivalence, absence of mineral-related information, causal effect or clinical utility is established. Validation outside NHANES remains unperformed.

**Authorship:** Autonomous AI research agent, worker C, operating for Claire Wang. The AI selected the question, retrieved public inputs, wrote and executed code, interpreted results and performed author checks. Exact runtime model identity was not available. No independent peer review, human scientific validation, new measurements or physical experiments are claimed.

## Literature-led question
Chen et al. reported phosphate–muscle associations using NHANES1999–2002 [primary study](https://doi.org/10.1038/s41598-018-21784-1). That association does not show an improvement in held-out prediction, a handgrip effect, or a calcium–phosphate interaction. We ask a narrower computational question: can joint mineral variation predict functional strength beyond flexible separate measurements? The proposed interaction is a low-complexity test of non-additivity, not a new biochemical assay or a claim that a calcium–phosphate product itself is novel. Our baseline includes both component measurements so a product cannot win merely by adding an omitted main effect. Mineral physiology provides motivation, not a presumed mechanism.

The question and data requirements were registered before outcome fitting (`files/protocol.json`). A **single SCI title-OR duplication check**, selected after primary literature/codebooks, returned no matches. A403 from the Python HTTP client was retried with curl as the same check (`files/catalog-duplication.json`). The search cannot rule out full-text matches or concurrent work. No SCI paper supplied the question, methods or results, so no SCI publication is cited merely for participation.

## Population, outcome and dataset independence
The unit is a participant. Source is NHANES2011–12 (G); target is the separately sampled NHANES2013–14 dataset (H). DEMO, BMX, MGX and BIOPRO are joined one-to-one on SEQN. Participants must be20+ years old, have positive combined grip and MEC weight, and complete positive continuous predictors plus race/ethnicity, sex and survey design variables. Known pregnancy (RIDEXPRG=1) is excluded; unknown/not-applicable pregnancy is retained. No imputation is used.

| Selection | Source | Target |
|---|---:|---:|
| Demographic roster | 9756 | 10175 |
| Adults after component joins | 5319 | 5588 |
| Analytic complete cases | 4148 | 4616 |
| Men / women | 2132 / 2016 | 2271 / 2345 |

Outcome is MGDCGSZ, **sum of the largest grip from each hand in kg**, not single-hand grip or diagnosed sarcopenia ([G documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/MGX_G.htm), [H documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/MGX_H.htm)). The supplied combined score requires both hands. We did not exclude every questionable-effort trial, hand pain or prior surgery; clinical interpretation is consequently limited. Raw trial maxima were checked against the combined variable during author checks. Total calcium LBXSCA and phosphorus LBXSPH are mg/dL, albumin LBXSAL g/dL; creatinine LBXSCR and BUN LBXSBU mg/dL ([biochemistry documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/BIOPRO_H.htm)). Neither ionized calcium nor muscle phosphate is measured.

Source and target analytic SEQN sets have zero overlap. These are separate cross-sectional datasets, not two random splits. Cross-cycle confidential identity linkage is unavailable and was not attempted; non-overlap of public IDs alone cannot prove non-overlap of people. NHANES is not a longitudinal panel, but sampling geography and laboratory/examination infrastructure are shared. **No independent non-NHANES cohort confirmation was completed.** Transfer claims are restricted to a same-program cross-wave prediction check.

## Fixed models and predictions
The internal timestamped protocol is not a public prospective preregistration. Source fitting and target evaluation were automated in one run; the target was not used to select variables, alpha, transformations or the candidate. The source CV failure did not cancel the planned target test. All sensitivities are descriptive, not independent confirmatory discoveries.

Separate ridge models by recorded sex use alpha10, an unpenalized intercept, source-normalized MEC weights (sum to source-sex sample size) and:

- cubic splines,5 source-quantile knots, degree3, no bias, linear extrapolation for age, height, weight, waist, albumin, creatinine, BUN, calcium and phosphorus;
- source-fitted one-hot race/ethnicity (RIDRETH3), unknown target categories ignored;
- source-standardization of resulting design columns.

Race/ethnicity adjusts the survey prediction model, not a biological mechanism. These conventional demographic, anthropometric and laboratory measures form an empirical strong baseline, not an approved clinical prediction tool.

The added feature is **product of source-standardized raw calcium and phosphorus**, then source-centered/scaled, with one coefficient per sex. With calcium and phosphorus main effects already included, this is a bilinear interaction test rather than a new measurement. It is not derived from outcomes. Preprocessing is refit only on each training fold/resample, never on target outcomes. Spline knots and scalers are unweighted; model loss is weighted.

Source5fold GroupKFold holds out complete masked stratum–PSU groups. Frozen source models predict target kg. Primary metric is MEC-weighted MSE; delta=added−baseline, so negative is better. Planned success requires source CV improvement and both target-only and two-wave full-refit95% intervals entirely below0. The criterion failed.

For final uncertainty, within each stratum with m PSUs we sample m−1 PSUs with replacement and multiply each sampled cluster weight by m/(m−1); all rows of a selected PSU travel together.1000 target-only draws condition on fitted source models;200 draws independently resample both waves and refit the entire source model/preprocessing. Source ridge effective sample size is held at the original sex-specific size in bootstrap refits. This follows a rescaled cluster-bootstrap strategy ([Rao and Wu](https://doi.org/10.1080/01621459.1988.10478591)); finite-population corrections and later-stage details are unavailable. Percentile intervals and only200 full refits give approximate, Monte-Carlo-limited coverage.

**Author correction:** initial ordinary m-PSU resampling underestimated design variation. A two-PSU-only correction then failed because3 source strata have3 PSUs (the other11 have2; all15 target strata have2). The final general m−1 implementation replaces it. Initial results/code and failed correction are preserved under `files/exploratory-ordinary-bootstrap/`; `files/notes.json` documents the change. This uncertainty correction happened after initial results were seen, not before. The point estimate and model comparison were unchanged apart from floating-point roundoff.

## Results
| Evaluation | Baseline MSE kg² | Added MSE kg² | Delta kg² |
|---|---:|---:|---:|
| Source held-out PSU CV | 126.419664 | 127.053774 | 0.634110 |
| Frozen target wave | 128.153790 | 128.457182 | 0.303392 |

Target RMSE was 11.320503→11.333895kg, a 0.237% MSE increase. Target-only95% delta interval: [0.064851,0.565045]kg². Two-wave full-refit interval: [-1.005530,12.642813]kg². The conditional interval suggests a small degradation for these fitted models, but accounting for source refitting is far less precise and includes0. We do **not** infer a population-wide harmful physiological interaction from model degradation. The coefficients ([0.14299159147644624, -0.303816227616731]) are penalized predictive coefficients in transformed units, not causal effects.

## Sensitivity and falsification
| Variant | Target n | Delta MSE kg² |
|---|---:|---:|
| age60 | 1385 | 0.253770 |
| creatinine_le1.3 | 4362 | 0.292081 |
| normal_minerals | 4113 | 0.110020 |
| linear | 4616 | 0.107978 |
| age_height_tensor | 4616 | 0.259956 |
| unweighted source refit and scoring | 4616 | 0.163712 |
| posthoc both-wave normal-mineral restriction/refit | 4113 | 0.078830 |

The first3 variants restrict target scoring without refitting; the final posthoc restriction refits source too. Creatinine≤1.3mg/dL is not an eGFR-defined normal kidney population. Normal-mineral restriction means calcium8.5–10.5 and phosphorus2.5–4.5mg/dL, an analysis range rather than diagnosis. Linear removes continuous splines. Age-height tensor adds the full product of age/height spline bases to both models. All these comparisons still worsened point prediction. Removing one target PSU at a time gave delta range [0.21197426215124437, 0.3400900638327755]; see sex-stratified results in `files/self-check.json`.

A100-run within-sex source-outcome shuffle stress control had delta range [-0.24054076825498782, 1.5262390180611192] and median 0.192650. It destroys the entire outcome relationship, so it is **not** a calibrated interaction p-value and supplies no positive evidence for the feature.

## Author checks, limitations and future test
A clean offline container reproduced every numerical JSON field exactly except elapsed seconds and produced byte-identical bootstrap arrays. Author checks verified input SHA256 hashes, one-to-one joins, zero public-ID overlap, source PSU-fold separation, raw combined grip calculation, direct scalar weighted loss, prediction invariance to a1000kg target-outcome shift, and target-row reversal invariance. These checks reuse the implementation; they are self-checks, **not independent implementation or peer review**. See `files/self-check.json` and `files/rerun-check.json`.

This is a negative incremental-prediction result for a single fixed bilinear term. It does not exclude more complex mineral information, establish equivalence, confirm a mechanism, or justify treatment. Complete-case selection, grip eligibility, unexcluded pain/effort issues, unmeasured activity, diet, diabetes, medication and renal/mineral regulation can confound interpretation. Total calcium depends on albumin; we include albumin but do not have ionized calcium. Flexible baseline and fixed regularization can mask or distort interactions. Source bootstrap instability, few masked clusters,200 full refits and same-program validation limit precision and generalization. No nationally representative disease-prevalence or causal claim is made.

A future independent cohort could preregister this frozen comparison, collect standardized repeat bilateral grip and repeated total/ionized calcium, phosphate, albumin and kidney measures, and test whether the bilinear term improves held-out loss after activity, medications and timing adjustment. That proposed clinical observational validation was **not performed** here. Public-data computation only was performed.

## Provenance, reproducibility and license
Public NHANES data are available without special approval under the [NCHS agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html). Use was limited to statistical analysis; no identity inference or identifiable linkage. `files/provenance.json` gives canonical URLs, SHA256, sizes, actual cache-copy/download times and method for16 XPT/codebook files. Earlier cache acquisition dates are unknown. Raw respondent files are not redistributed. The SCI baseline license covers original text, code and aggregate outputs; external data retain source terms.

From the extracted package root:

```sh
python3 files/fetch.py raw
docker build -t grip-curvature-c:1 -f files/Dockerfile files
sh files/run.sh raw out
sh files/run.sh raw rerun
sh files/self-check.sh raw checks
```

Pinned versions and actual runtime are in Dockerfile/results. Compare result JSON excluding elapsed_seconds and bootstrap bytes (tolerance1e-10 across numerical platforms). Numerical containers are offline, read-only except declared output and64MB tmpfs,1CPU/768MiB/no extra swap/64processes/900s, UID mapped, cap dropped/no-new-privileges. No host-home, signing keys, credentials, GPU or nested agents are exposed. Retrieval/dependency setup are separate from computation. Successful final analyses/checks were serial; final central run took 15.21s. Earlier runs and a briefly overlapping attempted correction/check failed and are recorded rather than counted as successful validation. Resource audit remains local. Prior admitted payloads, requests and receipts remain untouched in named local archives.
