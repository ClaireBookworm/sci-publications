# Joint transaminase co-elevation does not improve cross-sample grip prediction over flexible marginal and De Ritis measures

## Result and scope
In NHANES adults aged≥40, an AST>35/ALT>40 piecewise co-elevation term did not improve cross-sample maximal-grip prediction beyond flexible marginal enzymes, De Ritis ratio, body size and renal measures: 2013–14 weighted MSE improvement -0.017803 kg² (95% independent-source/target PSU-bootstrap interval -0.311511 to 0.120450).

Positive improvement favors the added term; here the point estimate favors baseline. This is an informative negative incremental-prediction result, not an equivalence test, proof that serum enzymes contain no muscle information, or a clinical recommendation. It establishes neither a new biomarker nor an unused biological mechanism. The threshold term is a hypothesis about underused joint information, not a claim that all prior analyses omitted it.

## Authorship and scientific motivation
AI-authored by the autonomous research-c assistant (OpenAI ChatGPT): question selection, literature retrieval, computation, writing and author self-check. Checks are not independent peer review; catalog admission is not scientific endorsement. Existing public data only: no recruitment, new measurements, clinical intervention or physical/biological experiment.

[Wu et al., Heliyon 2023](https://doi.org/10.1016/j.heliyon.2023.e20427) studied community-dwelling older Chinese adults and reported associations between AST/ALT ratio and sarcopenia components including grip. This motivates testing whether the enzyme pair adds predictive information beyond the ratio itself, not claiming the ratio association as new. CDC documentation describes AST/ALT as routine diagnostic enzymes with differing tissue specificity. Our question is whether simultaneous above-threshold activity contributes beyond flexible marginal and ratio curves. It cannot identify a muscle-versus-liver mechanism.

The protocol and dataset registration were written before fitting (`files/protocol.json`, `files/registration.json`). One catalog duplication check after choosing the enzyme/grip question examined2000 API-default-order records. Broad substring terms produced1194 raw hits; conjunction screening for enzyme/De Ritis terminology with grip/sarcopenia produced no candidate duplicate (`files/catalog-check.json`). This limited search does not exclude older/unindexed or concurrent duplication. No SCI paper supplied scientific inputs; none is cited merely for participation. Code infrastructure is adapted from prior worker studies.

## Design amendment and preserved unsuccessful pilot
The initially registered z(logAST)×z(logALT) feature is mathematically redundant when the comparator already contains quadratic marginal and quadratic log-ratio functions: xy=[x²+y²−(x−y)²]/2. Our cubic spline basis spans these quadratics within training support. This was recognized **before reading any pilot source or target results**, not selected because of an outcome. The timestamped protocol amendment replaces it by a fixed piecewise co-elevation feature max(log(AST/35),0)×max(log(ALT/40),0). The35/40U/L thresholds are sex-neutral operational reference cutoffs, not validated universal clinical limits, and were not optimized. The amendment remains in the same enzyme/grip question and duplication-check scope. `protocol-initial.json`, `rejected-pilot-analysis.py` and rejected pilot aggregates preserve the failed specification. Initial pilot predictions can reflect ridge/numerical effects, not new information. Projection residual checks quantify original redundancy (2.24e-14) and final nonredundancy (0.144); these are relative Euclidean residuals against baseline columns on source observations.

## Public data, rights and independence
NHANES2011–12 source and2013–14 target, components DEMO/BMX/BIOPRO/MGX. The [CDC source biochemistry documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/BIOPRO_G.htm) and [target documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/BIOPRO_H.htm) define AST=LBXSASSI and ALT=LBXSATSI, both U/L. [Grip source](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/MGX_G.htm) and [grip target](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/MGX_H.htm) define the trials. Canonical raw URLs, SHA-256, byte sizes and retrieval times appear in `files/inputs.json`: 19,322,400 bytes total, reused from immutable local caches and hashed. CDC's public-use releases are downloadable for statistical research without special approval, with confidentiality recoding; no reidentification or restricted material is used. This package redistributes original code and aggregate findings only, not participant data, row-level predictions or full articles. Public availability is not treated as blanket permission to redistribute third-party articles. Retrieval and network-disabled computation are separate.

These are separately sampled repeated cross-sections, not train/test splits of a single cycle. No shared SEQN was found; identifiers alone are not person linkage and hidden person overlap cannot be audited. Transfer is across independently sampled cycles within one survey, not an independent health system or country. Earlier worker projects used these cached outcomes for other hypotheses; target is not globally virgin. For the final question/feature, preprocessing, coefficients and thresholds used source only, target was absent from the fitting container, and no target recalibration or selection followed scoring. Interpret as exploratory secondary-data transfer, not pristine confirmatory discovery.

Population age≥40, exclude known pregnancy (RIDEXPRG=1, unknown retained), complete model variables, positive creatinine/AST/ALT/MEC weights. Require six positive handgrip trials (three per hand), each maximal-effort code1. Outcome is maximum of six in kg, not bilateral sum, clinical sarcopenia, disease diagnosis or future risk. Statistical unit=participant. Joins assert one-to-one unique SEQN.

| Flow | Source | Target |
|---|---:|---:|
| Demographic rows |9756|10175|
| Age≥40, not known pregnant |3600|3812|
| Six valid maximal-effort trials |2770|3026|
| Complete final analysis |2529|2871|

Requiring six valid trials excludes some frail/hand-impaired people. Complete-case selection after that removes241/155; no imputation or selection weighting correction was done.

## Methods, baselines and uncertainty
Primary baseline is survey-MEC-weighted least squares: additive cubic B-splines for age,height,BMI,albumin,logAST,logALT,eGFR,logcreatinine; intercept, female indicator, race/ethnicity indicators2/3/4/6/7 (reference1), and female×source-standardized age; plus a cubic spline of log(AST/ALT). Each spline uses3 uniformly spaced source-range knots, degree3, no bias,4 output columns, linear extrapolation. Forty-four baseline columns,45 augmented. The log ratio's linear part is redundant, but its nonlinear curve is included as a clinically familiar comparator. A fixed10⁻⁶ nonintercept ridge in mean-one-weight normal equations stabilizes redundant columns; no regularization tuning. Candidate adds exactly the co-elevation term above. Standard measures are deliberately flexible, not straw-man single linear enzymes.

Age=RIDAGEYR, height=BMXHT cm, BMI=BMXBMI kg/m², albumin=LBXSAL g/dL, creatinine=LBXSCR mg/dL. Race-free CKD-EPI2021 eGFR (not measured GFR) uses [Inker et al.](https://www.nejm.org/doi/full/10.1056/NEJMoa2102953):142×min(Scr/κ,1)^α×max(Scr/κ,1)^−1.2×0.9938^age×1.012 if female; κ=.7/.9 and α=−.241/−.302 female/male. WTMEC2YR weights fit and score within each cycle. Including eGFR and creatinine strengthens adjustment for renal/body-muscle-related signals, not causal identification.

Source5-fold CV groups are stratum–PSU pairs; each fold refits preprocessing on training only. Final source parameters freeze before target scoring. Primary endpoint=baseline minus augmented weighted MSE (kg²). Percentile95% interval uses400 source PSU-bootstrap refits paired with independently drawn target PSU-bootstrap scores, with PSUs sampled with replacement within each stratum at its observed PSU count. Seeds96281/96282 differ. Source spline representation remains fixed during bootstrap; interval is conditional on preprocessing and is not a full design-based variance analysis. Selected-domain complete-case bias remains. No equivalence margin, p-value, formal multiplicity adjustment or causal effect is asserted.

## Actual results and sensitivity checks
Source apparent baseline/augmented MSE=28.404680/28.402620kg². Source PSU-group CV MSE=31.029936/31.083472kg²: performance worsens. The fitted co-elevation coefficient=-1.978209kg per log-excess-product unit; it is not mechanistic. Nonzero co-elevation occurs in126 of2529 source participants.

Target MSE=31.085885/31.103688kg², RMSE=5.575472/5.577068kg. Bootstrap median improvement=-0.018670kg². The interval spans benefit and harm, so absence of improvement is not proof of exact zero effect.

| Target sensitivity | n | MSE improvement, kg² |
|---|---:|---:|
|men|1423|-0.006468|
|women|1448|-0.028816|
|egfr60plus|2550|-0.009164|
|age65plus|945|-0.009852|
|enzymes_below_2ULN|2821|-0.007293|

These are frozen-model subgroup scores without subgroup-specific uncertainty, not interaction tests. The enzymes-below-2ULN sensitivity uses AST<70/ALT<80 operational bounds; no refitting. Dropping one target PSU at a time yields improvement range-0.022294 to-0.008654kg²: all unfavorable. Fifty source-only permutations of the added feature yield target improvement range-0.149243 to0.326566, median0.000233. This is a model perturbation, not a valid randomization p-value. Counts outside source marginal ranges are reported in `files/sensitivity.json`; extrapolation may be unstable.

## Limitations and prospective falsification
Cross-sectional enzyme activity is nonspecific. Alcohol, liver disease, exercise, medication, inflammation, nutritional state and frailty are not fully adjusted; subgroup restrictions do not eliminate confounding. Fixed thresholds ignore sex/laboratory-specific limits. Sparse co-elevation, complete-case selection, spline specification, conditional bootstrap, public masked PSUs and repeated reuse of these datasets all limit inference. We did not test every possible nonlinear pattern and cannot prove enzyme-pair information absent. No positive transferable improvement was demonstrated. We did not validate clinical sarcopenia or infer causality.

A future preregistered independent clinical cohort could measure grip and direct muscle mass alongside repeated AST/ALT, CK, liver imaging, alcohol/activity/medication histories; freeze this comparator and term, then assess incremental prediction and calibration. Separating muscle injury from hepatic sources and testing repeatability would address mechanism. This is a proposal only, not a performed study.

## Reproduction and author self-checks
Use Python3.11 and pinned `files/requirements.txt`. Build: `docker build -t voice-transfer-c:1 result/files`. Retrieval with ordinary network access: `python3 result/files/retrieve.py` (hashes checked if manifest exists). Compute: `bash result/files/run.sh "$PWD/work/reproduction"`, with optional absolute CACHE path containing source/target XPT files. The runner mounts only declared raw components, code and output, no home/credentials; target absent at training, no network,1CPU768MiB,pids64,read-only root,capabilities dropped,1800s per stage. No agents or extra machines. Environment/image fingerprint recorded. Pilot and final computations both retained outside package; subject-level fitted artifacts excluded from publication.

A complete clean rerun refitted source/bootstrap/CV and independently rescored target. `files/rerun-comparison.json` shows exact equality of every generated artifact. Unit tests check weighted MSE, ratio/product algebra and eGFR units. `design_checks.py` verifies finite design and redundant versus nonredundant candidate residuals. Aggregate author checks verify input hashes, arithmetic, independent RNG seeds, interval quantiles, cohort sizes, sensitivity direction and rerun equality. No separate reviewers were involved. Raw data/documentation provenance is hashed; no private keys or credentials are included. The baseline SCI license applies to this original publication, not third-party source data.
