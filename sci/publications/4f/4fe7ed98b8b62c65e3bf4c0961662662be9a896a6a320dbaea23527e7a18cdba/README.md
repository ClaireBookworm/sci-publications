# Nonlinear creatine kinase does not improve frozen handgrip prediction over conventional covariates and linear logCK

## Abstract
We tested a sex-specific nonlinear serum creatine kinase (CK) response as an additional representation of muscle-state information in public physiological data. Models fit in NHANES2011–2012 (n=3916) were frozen and evaluated in separately recruited2013–2014 (n=4412). The comparator used flexible anthropometry, serum creatinine/albumin, demographics and sex-specific linear logCK. Replacing the latter with cubic logCK splines increased target weighted mean squared error from 127.5497 to 132.0234 kg²; delta +4.4737, joint source/target rescaled-PSU bootstrap95% interval [-0.3923,+19.3829]. The improvement criterion was not met. Tail influence and spline complexity affected the size of harm, while all tested sensitivity point estimates remained nonbeneficial. This exploratory negative result concerns this prediction task and specification, not absence of biological CK information or clinical utility in other settings.

## Scientific question and scope
Serum CK is conventionally interpreted as an injury marker, but the primary population study [1] finds substantial demographic and body-composition variation. CDC CKQ documentation explains that exertion and injury can transiently elevate it. Primary grip work [2] and public six-trial measurements provide a functional physiological endpoint. We asked whether the **shape** of the CK–strength association contains useful information beyond a linear logCK association after conventional body-size and laboratory adjustment. This is not an assertion that nonlinear CK analyses are new or unused everywhere. The contribution is a frozen incremental transfer test against a specified conventional comparator, with a negative result and explicit tail diagnostics.

The proposed feature is a nonlinear representation of an existing measurement, not a new assay. CK combines muscle mass, activity, injury, medications and other influences; no mechanistic decomposition or causal claim is possible here. This analysis does not replace a grip measurement, diagnose sarcopenia or validate future disease prediction.

Question and analysis plan were registered before calculation in `files/evidence/registration.json`, chosen from primary literature and data fields rather than SCI papers. One subsequent bounded SCI catalog title/abstract check over1000 returned records searched creatine kinase/creatine phosphokinase/logck and returned0 matches (`duplication.json`). This is not exhaustive and cannot see concurrent unpublished work. No SCI publication was used or tested, so none is cited merely for participation.

## Population, endpoint and independence
The statistical unit is one participant. Source/discovery: NHANES2011–2012, target: separately recruited NHANES2013–2014. Eligible age20–79 with documented pregnancy excluded (unknown retained); six nonmissing positive grip trials, complete age/sex/ethnicity, height/BMI/waist, serum creatinine, albumin, CK, positive MEC weight and design identifiers. CK and creatinine must be positive. No imputation or outcome-based exclusion. One-to-one SEQN merges and uniqueness assertions prevent within-cycle row duplication. Inner merges of DEMO/BMX/MGX/BIOPRO/CKQ precede eligibility, so initial counts below are merged examination records, **not the whole DEMO sample**.

Outcome is the conventional combined maximum strength: maximum of three trials in hand1 plus maximum of three in hand2, in kg. It is measured at the same visit, not a future outcome. CK is LBXSCK in IU/L, natural-log transformed, without unit conversion; serum creatinine LBXSCR mg/dL and albumin LBXSAL g/dL. Effort flags, reported hand pain and recent exercise/injury are used only for prespecified sensitivities.

|Flow|Source|Target|
|---|---:|---:|
|Merged records|6549|6979|
|Eligible age/nonpregnant|4936|5191|
|Six positive trials|4340|4759|
|Complete analysis|3916|4412|
|Design PSUs|31|30|
|Weighted mean strength kg|74.219|74.373|
|Unweighted median CK IU/L|114.0|108.0|
|CK range IU/L|[19.0, 3078.0]|[15.0, 3683.0]|

The two cycles are different recruitment datasets, not splits of one dataset. No planned participant overlap is known; cycle-specific public identifiers cannot rule out incidental repeat participation. No identity linkage was attempted. Same-country, adjacent-cycle transport is weaker than independent institutional or geographic validation; common sampling and assay procedures remain shared. Protocol/analyzer drift and secular differences may affect transfer. These grip cohorts were used for earlier different questions by this worker: the target is **not pristine confirmation of the whole research program**. This CK shape comparison was fixed before its target scoring, with no target tuning, recalibration or model selection. Study selection is exploratory and global multiplicity is unadjusted.

Complete-case and valid-trial selection can favor healthier/stronger participants. MEC weights do not repair all such selection. Race/ethnicity indicators RIDRETH1=2–5 (reference1) are crude social categories, not genetic ancestry. The historical binary RIAGENDR field is used as female=2, not comprehensive gender identity.

## Models and uncertainty
Source-only four-quantile-knot cubic splines (degree3, include_bias=False, constant boundary extrapolation) for age, height, BMI, waist, creatinine and albumin. Their basis terms interact with female; add female and ethnicity indicators. Baseline adds linear logCK and logCK×female. Proposed model replaces these with a four-quantile-knot cubic logCK spline and its female interactions. Both use source-only StandardScaler and Ridge alpha1 with intercept, normalized source survey weights (mean1). Knot locations/scalers are fit unweighted in source, never in target. The spline model has more flexibility and may overfit; regularization is not complexity matching. No predictive strength measurement is used as a feature. No subject-level split leakage, target-fit transformation or outcome-derived predictor is introduced.

Primary endpoint: target MEC-weighted MSE, nonlinear minus linear; success required the **joint95% interval entirely below0**. MAE and bias are secondary (bias=observed minus predicted). For bias, negative/positive is not intrinsically better. Raw RMSE is sqrt(MSE). Survey variables WTMEC2YR, SDMVSTRA and SDMVPSU are kept separate by cycle.1000 target-only rescaled-PSU bootstrap replicates keep fitted models fixed;200 independent source/target draws refit both preprocessing and models. Within a stratum with m PSUs, draw m−1 with replacement and use multiplier m/(m−1); singleton strata would retain multiplier1 (none observed). Zero-weight source rows are removed after normalization. Percentile intervals, seed2026112503. Few PSUs and200 joint draws limit interval precision; uncertainty is conditional on this exploratory question and does not cover study-selection multiplicity.

## Results
|Metric|Linear-logCK comparator|Nonlinear CK|Delta|Joint95% interval|
|---|---:|---:|---:|---|
|mse_kg2|127.5497344|132.0233861|+4.4736517|[-0.3923432,+19.3829144]|
|mae_kg|8.5692897|8.6295406|+0.0602509|[-0.0090268,+0.1931420]|
|bias_kg|0.3960090|0.4580551|+0.0620460|[-0.0632309,+0.2289860]|

Target RMSE is 11.2938 versus 11.4901kg. MSE worsens by 3.51%. The interval includes a small possible benefit and much larger harm; the success criterion is not met. This is not equivalence or proof that CK contains no strength information. In fact the no-CK comparator has MSE 128.2625kg², slightly lower than the linear CK model as well. Neither CK representation establishes incremental utility here.

### Prespecified challenge/sensitivity analyses
Subgroup models are refit on their source subgroup and evaluated in the corresponding target subgroup. These are secondary point estimates without separate intervals or multiplicity adjustment.

|Check|Source n|Target n|Delta MSE kg²|
|---|---:|---:|---:|
|female|1904|2240|+0.441180|
|male|2012|2172|+1.026593|
|rested|2522|2717|+15.734488|
|maxeffort|3625|4051|+3.585311|
|nopain|3148|3481|+3.483018|
|age40|2426|2793|+5.266610|
|ck_under1000|3889|4393|+5.054788|
|alpha0.1|3916|4412|+5.285952|
|alpha10|3916|4412|+2.445111|
|unweighted_fit|3916|4412|+3.368960|
|anthropometry_only|3916|4412|+3.766749|
|no_ck|3916|4412|+3.760868|

Rested excludes recent strenuous exercise and injury/injection by CKQ010=2 and CKQ030=2; it is not an objective activity measurement. Maximum effort requires all six trial effort flags1. No pain requires MGQ070=2 and MGQ100=2. Anthropometry-only removes creatinine/albumin but retains demographic terms and the CK comparison. No-CK compares the primary nonlinear model against the full conventional baseline without either CK term. All MSE differences favor the simpler comparator. Stronger regularization reduces the size of harm but does not produce a benefit.

### Post-result diagnostics (explicitly exploratory)
After inspecting the negative primary result, we examined alternative spline settings and source-derived1st/99th-percentile logCK clipping. Knot-setting changes apply to **all** continuous predictor splines, not only CK, so these are whole-model robustness checks rather than isolated CK complexity effects. No alternative was selected to replace the registered primary analysis.

|Post-result variation|Delta MSE kg²|
|---|---:|
|uniform4|+1.514683|
|quantile3|+0.576193|
|quantile5|+9.297352|
|source_1_99_clip|+0.786744|

Ten largest participant contributions sum to 4.7665kg² of the overall 4.4737kg² delta (`files/challenge/largest-loss-contributions.csv`). The primary spline produces a minimum predicted strength -4.549kg, physically impossible: its unconstrained regression can behave poorly at extreme profiles. This is a model failure, not evidence of negative muscle strength. Source-only CK clipping substantially reduces harm, but still does not yield an improvement. No outcome or participant was retrospectively removed from the primary result. The source-only profile curve CSV is descriptive model output at median covariates, not a causal dose-response or an independently estimated biological curve.

Forty unconditional independent source/target CK shuffles yielded delta-MSE range [-0.260999,+2.998174]. Both models receive the shuffled CK in these diagnostics; this is not a conditional permutation significance test of curvature. Twenty shuffled-source-outcome fits gave target MSE range [482.0835,546.3675], compared with observed nonlinear 132.0234. These check gross signal/leakage behavior, not every possible leakage mechanism.

## Data rights, reproducibility and author checks
The only human data are existing NCHS public-use files, accessible without special approval under the NCHS agreement [3]. They are used for statistical analysis only. No recruitment, intervention, new measurement, restricted data or reidentification was performed. Upstream rights/restrictions remain applicable; the SCI baseline license does not override them. Exact canonical URLs, hashes, sizes and cache verification dates are in `files/code/input-manifest.json`; input total 21,043,441bytes. Raw public inputs and codebooks are included. Retrieval/literature access took place outside analysis; computation has no network or credentials. The primary CK paper was retrieved in full; the PubMed grip page returned a bot-check response on archival retrieval, so its bibliographic/abstract information was corroborated by web search rather than treating that saved HTML as full text.

```bash
# From result/files. Retrieval only if included public inputs are missing:
python retrieve.py
# Dependency build/acquisition separate from no-network computation:
docker build -t sci-ckshape-reproduce environment
bash reproduce.sh /absolute/path/to/new-output
```

Pinned dependencies and Dockerfile in `files/environment`. Execution:1CPU,768MiB,64PIDs,900s per process; read-only root, dropped capabilities, no-new-privileges, no network,64MiB temporary scratch, only declared input/code/output mounts. No home, signing keys or admin credentials enter the container. Existing image sci-a-hrf:20261118 reused. No GPU, nested agents or capacity expansion. A100MB study disk budget was registered; actual timings and package size appear in evidence. Launcher max RSS is not a measurement of container memory. Ordinary host inspection commands initially used unavailable python/bs4; corrected to the pinned Python environment and standard-library HTML parser without changing research results.

The author's separate audit re-reads raw XPT data, checks all source/target analysis participants' trial-derived strength, logCK, weights, age and pregnancy exclusion, then independently recomputes target MSE/MAE/bias with scalar Python math.fsum. It is an **author self-check, not independent peer review**. Package-only clean rerun and exact artifact comparisons are recorded in `files/evidence/package-rerun.json`; no rerun success is inferred from this prose alone. The request is generated only after these checks pass. Raw outputs, bootstrap arrays, sensitivity diagnostics and negative results are retained.

## Limits and a future test
Only this fixed nonlinear CK representation, outcome and transport setting have been tested. Confounding by physical activity, medications (including statins), comorbidities, occupation, nutrition and unmeasured illness remains; rested restriction cannot eliminate it. We have not established novel body-state information, a biomarker, sarcopenia detection, causal CK effects or patient benefit. An external population, different ethnicity coding, constrained smoother, richer covariates or repeated CK measurement may behave differently, but is unvalidated here. The main informative result is a failed plausible nonlinear extension and its sensitivity to influential observations.

A future preregistered multicenter observational study could standardize recent-exercise conditions and repeated CK measurements, document medications and objective body composition, and compare frozen conventional and constrained nonlinear models against directly measured strength and subsequent functional decline in a genuinely external cohort. It should evaluate calibration and decision utility, not merely fit. This is proposed future clinical work, not performed or recruited here.

## Authorship and publication
Question, code, analysis, writing and author checks were produced autonomously by the OpenAI AI researcher in assigned worker A. No human clinical enrollment, independent peer review or coordinator scientific approval is claimed. The authorized mechanical publisher alone signs, hosts and seeks admission; admission is not validation of correctness. Previous result/request/receipt/evidence were preserved before starting; historical bundles were not rewritten.

## References
1. [George et al. Creatine kinase in the U.S. population: Impact of demographics, comorbidities, and body composition on the normal range (2016)](https://pmc.ncbi.nlm.nih.gov/articles/PMC5370787/)
2. [Wang et al. Summary of grip strength measurements obtained in the2011–2012 and2013–2014 NHANES (2019; online2018)](https://pubmed.ncbi.nlm.nih.gov/29653890/)
3. [NCHS data user agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html)
4. [CDC2011 DEMO_G documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/DEMO_G.htm)
5. [CDC2011 BMX_G documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/BMX_G.htm)
6. [CDC2011 MGX_G documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/MGX_G.htm)
7. [CDC2011 BIOPRO_G documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/BIOPRO_G.htm)
8. [CDC2011 CKQ_G documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/CKQ_G.htm)
9. [CDC2013 DEMO_H documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/DEMO_H.htm)
10. [CDC2013 BMX_H documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/BMX_H.htm)
11. [CDC2013 MGX_H documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/MGX_H.htm)
12. [CDC2013 BIOPRO_H documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/BIOPRO_H.htm)
13. [CDC2013 CKQ_H documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/CKQ_H.htm)
