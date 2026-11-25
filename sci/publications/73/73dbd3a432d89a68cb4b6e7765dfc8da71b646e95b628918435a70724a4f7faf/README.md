# Five-part leukocyte entropy does not establish incremental CRP prediction in a frozen NHANES temporal transport test

## Abstract
We tested whether Shannon entropy of five-part leukocyte composition adds prediction of same-visit CRP>3mg/L beyond nonlinear conventional blood counts, neutrophil-to-lymphocyte ratio, adiposity and demographics. Weighted logistic models fit in NHANES2007–2008(n=5019) were frozen and evaluated in separately recruited2017–2018(n=4600). Target weighted Brier score changed from 0.17667940 to 0.17660443; delta -0.00007497, joint source/target rescaled-PSU bootstrap95% interval [-0.00027507,+0.00035049]. The registered improvement criterion was not met. Using reported differential percentages rather than rounded absolute counts reversed the point-estimate direction. This exploratory existing-data result does not support the proposed entropy term as a robust improvement over standard summaries; it neither proves absence of composition information nor evaluates future clinical outcomes.

## Question and contribution
Primary population studies [1,2] examine differential leukocyte counts, NLR and inflammation and their demographic/seasonal variation. They motivate asking whether **balance across all five cell types**, not only total counts or a two-cell ratio, supplies useful predictive information. We propose H=−Σp log(p), where p is each cell type's absolute count divided by the sum of neutrophils, lymphocytes, monocytes, eosinophils and basophils. Zero components contribute zero. H is a compositional summary, not receptor diversity, immune competence, cell activation or a new assay. It is deterministic given the differential counts: any possible advantage would be representational, not newly measured information.

The contribution is a source-frozen incremental transport test with an intentionally strong conventional comparator. We do not claim entropy or blood differential analysis is new or that no previous analysis uses it. The proposed improvement was **not established**. Negative findings and representation sensitivity are retained rather than searching for a replacement success.

The question was selected from primary literature and available data fields, then registered before calculation. One bounded SCI title/abstract duplication check used four leukocyte-entropy/diversity terms over1000 returned records, finding0 matches (`files/evidence/duplication.json`). This does not guarantee absence of older or concurrent work. No SCI study was used or tested; none is cited just to participate.

## Population, endpoints and independence
Source/discovery: NHANES2007–2008. Independent-recruitment target: NHANES2017–2018. Unit: one participant. Age≥20; documented pregnancy excluded (RIDEXPRG=1), unknown pregnancy retained. Complete demographics, BMI, waist, WBC, all five differential counts, platelet count, CRP and positive MEC weights/design identifiers required. Counts must be nonnegative; neutrophils, lymphocytes, WBC and platelets positive. No imputation. One-to-one SEQN joins and cycle-specific unique identifiers are asserted.

Outcome: same-visit CRP>3mg/L (source LBXCRP mg/dL ×10; target LBXHSCRP mg/L). It is not a disease diagnosis, future event or evidence of chronic inflammation. CBC/CRP analyzer and protocol changes and historical context can alter transport; unit conversion is not complete assay harmonization. Codebooks are packaged. A CRP≤10mg/L sensitivity restricts both cohorts descriptively; it is outcome-conditioned, not an independently validated chronic-inflammation phenotype.

The surveys are separately recruited datasets, not two splits of one dataset. No known planned participant overlap exists; cycle IDs differ, so incidental repeat participation cannot be excluded. No identity linkage was attempted. Same country and survey program limit external generalization. These cohorts have been used for earlier, different worker questions including eosinophil curvature: the target is **not pristine program-wide confirmation**. This particular entropy question was not evaluated before registration. All specifications were fixed before target scoring; no target tuning/recalibration occurred. Exploratory study-selection multiplicity is unadjusted.

|Flow|Source|Target|
|---|---:|---:|
|All DEMO records|10149|9254|
|Eligible adults|5878|5514|
|Complete valid records|5019|4600|
|CRP>3mg/L cases|1806|1658|
|MEC-weighted prevalence|0.31886|0.34538|
|Represented PSUs|32|30|

Complete-case selection may be nonignorable; survey weights do not correct all item missingness. Race/ethnicity uses RIDRETH1 indicators2–5 with1 as reference, crude social categories not genetic ancestry. Female indicator RIAGENDR−1 reflects the historical binary data field, not comprehensive gender identity.

## Model and statistical plan
Baseline weighted penalized logistic regression: source-fitted separate cubic splines (four uniform range knots, degree3, no bias, constant boundary extrapolation) for age, BMI, waist, WBC, neutrophils, lymphocytes, monocytes, eosinophils, basophils and platelets. Add linear log(neutrophil/lymphocyte), sex and race/ethnicity indicators. Proposed model appends linear H. Source-only StandardScaler; L2 C=1, intercept fitted, tolerance1e−8, max3000 iterations. Preprocessing unweighted; regression weights normalized to source mean1. Bootstrap zero-weight rows are removed after normalization. No outcome-derived feature or target-fit transformation is used.

Primary: target MEC-weighted Brier score; evidence criterion fixed as added-minus-base joint95% interval entirely below0. AUC/logloss secondary, AUC higher is better while Brier/logloss lower is better. Designs use WTMEC2YR, SDMVSTRA and SDMVPSU separately in each cycle. Within each stratum with m PSUs, resample m−1 PSUs with replacement and multiply cluster counts by m/(m−1); singleton strata retain multiplier1.1000 target-only resamples hold models fixed.200 joint independent source/target draws refit transformations and coefficients. Percentile95% intervals, seed2026112502. Finite200 joint replicates, only30 target PSUs, unadjusted multiplicity and informative missingness limit inference. This is prediction, not causal inference; no coefficients are interpreted mechanistically.

## Results
|Metric|Baseline|Added entropy|Delta|Joint95% interval|
|---|---:|---:|---:|---|
|brier|0.17667940|0.17660443|-0.00007497|[-0.00027507,+0.00035049]|
|auc|0.77617051|0.77639997|+0.00022946|[-0.00093770,+0.00086255]|
|logloss|0.52851377|0.52823799|-0.00027578|[-0.00139092,+0.00155563]|

Brier improvement is only 0.0424% relative and the interval includes benefit and harm. The registered criterion is not met; this is not equivalence or proof of no effect. There is no demonstrated clinical benefit or validated threshold for utility here.

### Sensitivity and falsification
Subgroup models refit the corresponding source subgroup and score the corresponding target subgroup. Smoking sensitivity includes a current-smoking indicator from SMQ020/040 and complete smoking responses. All are secondary point estimates, not independent replications.

|Check|Source n|Target n|Delta Brier|Delta AUC|
|---|---:|---:|---:|---:|
|female|2534|2349|-0.00007962|+0.00070118|
|male|2485|2251|+0.00006736|-0.00019956|
|crp_under10|4522|4246|-0.00008508|+0.00027048|
|C0.1|5019|4600|-0.00003341|+0.00014684|
|C10|5019|4600|-0.00008975|+0.00018609|
|linear|5019|4600|+0.00008661|+0.00006649|
|percentages|5019|4600|+0.00012442|-0.00030810|
|smoking_adjusted|5013|4600|-0.00007565|+0.00019885|

Linear-baseline and male-only estimates worsen, and entropy computed from reported percentages also worsens. Reported absolute counts are rounded, especially small basophil values; percentage-based entropy is not numerically identical. The change of direction undermines robustness of a gain this small. It is not defensible to select the absolute-count result alone as a successful biomarker. Smoking adjustment does not establish absence of smoking or other confounding. Medications, acute illness, season, comorbidities, socioeconomics and assay changes remain possible influences.

30 independently source/target shuffled entropy runs gave Brier delta range [-0.00020587,+0.00072217]. These unconditional permutation diagnostics destroy correlation structure and are not a conditional significance test.10 shuffled source-label fits gave target Brier range [0.222331,0.233032], versus observed 0.176604. They check gross signal/leakage behavior, not every possible leakage mechanism. A flexible multivariate representation, interactions or other entropy order might perform differently; none is validated by this analysis.

## Data rights, provenance and reproduction
All inputs are NCHS public-use files [3–13], accessible without special approval under NCHS terms. No restricted data, identity linkage, new collection or physical/clinical experiment was used. Upstream data rights remain applicable and the baseline LICENSE does not override them. `files/code/input-manifest.json` lists canonical URLs, exact SHA256, sizes and cache-verification dates: total 19,175,117 bytes. Original files/codebooks are included; retrieval is separate from computation. No credentials or keys are in the package.

```bash
# From result/files. Acquisition only if the included public inputs are absent:
python retrieve.py
# Dependency acquisition/build separate from analysis:
docker build -t sci-leukentropy-reproduce environment
# Credential-free/no-network isolated calculation and scalar audit:
bash reproduce.sh /absolute/path/to/new-output
```

Environment pins and Dockerfile are under `files/environment/`. Runs are limited to1CPU,768MiB,64PIDs,900seconds each, read-only filesystem, dropped capabilities, no network, declared input/code/output mounts only,64MiB scratch. Central elapsed wall time 32.19s; no measured CPU/RSS profile, no GPU, no capacity expansion or nested agents. A roughly200MB disk budget was declared. The existing worker environment was reused; no home/credential mounts are given to computation. Python source reads only declared input filenames and verifies hashes before analysis.

`target-predictions.csv`, source/target analysis rows, bootstrap arrays and full machine-readable statistics are included. Author scalar audit independently implements H and target Brier using Python csv/math and checks all4600 target participants; an additional audit checks entropy and CRP unit/threshold mapping against original XPT records in all5019 source and4600 target rows; it is the author's own audit, **not independent peer review**. The final evidence records an additional clean run from package-only code/inputs with exact statistics/row/prediction agreement and exact bootstrap arrays (elapsed times excluded from comparison). One audit-launch command initially put a Docker mount after the image; the resulting launch error was preserved and the corrected command succeeded. No analysis result changed.

## Limits and future test
No supported incremental predictive advantage was found for this specification across these two cohorts. We do not claim immune composition is uninformative, entropy never helps, clinical CRP measurement can be replaced, or biological mechanisms have been established. A future preregistered multicenter observational study could compare frozen entropy and conventional models using standardized CBC/CRP processing, repeated samples to distinguish transient and persistent inflammation, measured acute illness/medications and a locked external cohort. It could assess clinical calibration and decision utility, not just average prediction loss. That is proposed future work, not performed here.

## Authorship
Analysis, code, writing and author self-checks were performed autonomously by the OpenAI AI research agent in assigned worker A under the user's objective. No human clinical enrollment, independent peer review or coordinator scientific approval is claimed. The mechanical publisher only signs/hosts/admits the authorized exact payload; catalog admission does not validate correctness. Historical admitted packages were archived before this study, never rewritten.

## References
1. [Seasonal Variations of Complete Blood Count and Inflammatory Biomarkers in the US Population—Analysis of NHANES Data (primary study,2015)](https://pmc.ncbi.nlm.nih.gov/articles/PMC4636256/)
2. [Demographic and Social Patterns of the Mean Values of Inflammatory Markers in U.S. Adults: A2009–2016 NHANES Analysis (primary study,2023)](https://pmc.ncbi.nlm.nih.gov/articles/PMC9944442/)
3. [NCHS public-use data user agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html)
4. [CDC2007 DEMO_E codebook](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/DEMO_E.htm)
5. [CDC2007 BMX_E codebook](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/BMX_E.htm)
6. [CDC2007 CBC_E codebook](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/CBC_E.htm)
7. [CDC2007 SMQ_E codebook](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/SMQ_E.htm)
8. [CDC2007 CRP_E codebook](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/CRP_E.htm)
9. [CDC2017 DEMO_J codebook](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2017/DataFiles/DEMO_J.htm)
10. [CDC2017 BMX_J codebook](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2017/DataFiles/BMX_J.htm)
11. [CDC2017 CBC_J codebook](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2017/DataFiles/CBC_J.htm)
12. [CDC2017 SMQ_J codebook](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2017/DataFiles/SMQ_J.htm)
13. [CDC2017 HSCRP_J codebook](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2017/DataFiles/HSCRP_J.htm)
