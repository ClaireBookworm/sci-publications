# Triglyceride–HDL interaction adds no established fasting-insulin prediction beyond flexible lipids and their ratio: a cross-survey redundancy audit

## Abstract
We tested log(TG/100)×log(HDL/50) beyond flexible standard measurements including log(TG/HDL), motivated by primary insulin-resistance research on the conventional ratio. Models trained in1957 NHANES2013–14 adults were frozen for1761 separately sampled2015–16 adults without reported diabetes/treatment. Target weighted log-insulin MSE was0.215009168 versus0.215009026; gain0.000000142,500-replicate source-refit/target-resample PSU95% interval[-0.004632,0.000650]. Source CV worsened. Posthoc algebra and numerical checks show this product is redundant with quadratic log-lipids and log-ratio; ridge differences reflect parameterization and spline-tail extrapolation, not new measured information. This narrow negative representation test does not refute lipid–insulin biology or establish equivalence. Validation uses distinct samples sharing NHANES infrastructure, not an independent clinical system. AI-authored and author-self-checked, not independently peer reviewed.

## Motivation and scope
McLaughlin et al. studied258 overweight volunteers using an insulin-suppression test and evaluated routine metabolic markers, including triglycerides and the TG/HDL ratio ([primary study,2003](https://pubmed.ncbi.nlm.nih.gov/14623617/)). That motivates, but does not establish, our question: does the **joint coupling** of the two lipids add useful prediction beyond their flexible individual values and the conventional ratio? We do not claim novelty of TG/HDL, or that insulin is a direct insulin-sensitivity measurement. No new assay, patient collection, intervention or experiment was performed. This is existing-data computation and an informative failed candidate, not discovery of a new physiological mechanism.

We chose this question from primary literature and the public codebooks before one documented SCI catalog duplication check (`files/duplication.json`). Two related papers concerned TG–glucose predicting CRP and TG–waist predicting insulin, not this conditional TG–HDL test. Those results were neither used nor tested and are not scientific citations here. Concurrent unpublished work is not discoverable. The catalog revealed prior use of2013–14 as a target in another project; therefore this is a question-specific holdout protocol, not globally pristine data or a formally registered clinical study. Generic analysis code was adapted from earlier author work, without using earlier scientific findings to choose the question.

## Population, data and independence
Public NHANES2013–14 is the source;2015–16 is the second dataset. The unit is a participant, merged one-to-one by public SEQN across DEMO, BMX, INS, TRIGLY, HDL, GLU and DIQ. These are separately recruited cross-sectional survey samples, **not random splits of one dataset**. Public IDs are disjoint, but this does not prove no individual ever participated twice; no identity linkage was attempted. Shared national survey, assay and processing infrastructure limits independence: validation in an independently operated cohort has **not** been completed. Transfer claims are restricted accordingly.

Eligibility: age≥20, pregnancy not recorded as positive (missing status allowed), DIQ010 no/borderline, no affirmative DIQ050 insulin or DIQ070 diabetes-pill use; reported fasting8–24h; positive fasting weight, insulin, triglyceride, HDL, glucose, BMI and waist, and complete model/design variables. Missing treatment responses are not proof of no treatment. Undiagnosed diabetes and lipid medication remain possible. Fasting hours include reported minutes/60. No lab imputation was performed by us; public detection-limit substitutions are retained.

Source flow:10175 total→4938 eligible→1957 valid/complete. Target:9971→4739→1761. Loss to completeness is substantial; fasting weights do not automatically correct complete-case selection. Insulin outcome is natural log µU/mL, a concurrent physiological proxy, not future disease or clamp-derived insulin resistance. The [source insulin](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/INS_H.htm) and [target insulin](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2015/DataFiles/INS_I.htm) documentation specify the same Tosoh assay and fasting weights; no cross-assay insulin conversion is needed for these two cycles. Values below1µU/mL are sensitivity-excluded. For glucose we use the documented backward transformation `0.9776×target +0.4994`mg/dL to the source instrument scale ([CDC glucose notes](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2015/DataFiles/GLU_I.htm)); raw-target glucose is a challenge. Triglyceride/HDL mg/dL variables are LBXTR/LBDHDD ([TG notes](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/TRIGLY_H.htm),[HDL notes](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2015/DataFiles/HDL_I.htm)).

## Frozen comparison and uncertainty
Baseline: separate cubic marginal splines of age, BMI, waist, fasting glucose, logTG, logHDL, log(TG/HDL), fasting duration;4quantile knots, degree3, no spline bias, linear extrapolation. Sex and race/ethnicity are one-hot coded. The combined design is source-standardized. Ridge alpha10, unpenalized intercept, uses source fasting WTSAF2YR normalized to n. The augmented design adds source-standardized `h=log(TG/100)×log(HDL/50)`. Including a flexible conventional ratio is deliberate: omitting it could mistake a known marker for new information. The comparator is a strong standard-measurement model, not a validated clinical decision score. This does not assess replacing direct insulin measurement.

All preprocessing and coefficients are fitted in source only. Fivefold source CV uses shuffled participant folds(seed701) with fold-local preprocessing; shared PSUs can make CV optimistic, so it is exploratory. Target labels score frozen predictions only. No target tuning of the primary model was performed. Scaling/knots are unweighted; fitting/scoring use fasting weights. The primary metric is target weighted MSE in log-insulin², with gain=baseline−augmented. MAE, R² and unweighted gain are descriptive.

The500-replicate Rao–Wu m−1 bootstrap independently samples whole PSUs within strata in each cycle, multiplying weights by m/(m−1), refits source preprocessing/models, and resamples target. Source weight total stays fixed to original source n, preserving ridge scaling. The percentile2.5–97.5% interval approximates the masked public survey design, conditional on observed samples/specification. It does not include model-search history, nonresponse corrections, between-laboratory transport, or all population uncertainties. No subgroup p-values or multiplicity-adjusted claims are made.

## Results
|Comparison|Evaluation n|Baseline MSE|Augmented MSE|Gain|
|---|---:|---:|---:|---:|
|Frozen target|1761|0.215009168|0.215009026|0.000000142|
|Source fivefold CV|1957|0.248438841|0.248559202|-0.000120361|
|alpha1|1761|0.214717498|0.214723878|-0.000006380|
|alpha100|1761|0.215238785|0.215144677|0.000094108|
|linear_main_effects|1761|0.238934829|0.239043132|-0.000108303|
|permuted_training_ratio|1761|0.215009168|0.215148829|-0.000139660|
|insulin_ge1|1761|0.214908765|0.214908799|-0.000000034|
|glucose_lt126|1689|0.213948965|0.213894386|0.000054579|
|TG_le400|1746|0.212886561|0.212540094|0.000346467|
|sex1|860|0.226209584|0.226209989|-0.000000405|
|sex2|901|0.204427914|0.204427255|0.000000660|
|six_knots|1761|0.219327731|0.219269373|0.000058358|
|residualized_interaction|1761|0.215009168|0.215009168|0.000000000|
|omit_LBXGLU|1761|0.240601705|0.240650738|-0.000049033|
|omit_logratio|1761|0.216006070|0.215842112|0.000163958|
|omit_BMXBMI_BMXWAIST|1761|0.317385328|0.317343609|0.000041719|
|uncalibrated_target_glucose|1761|0.215892279|0.215891888|0.000000391|
|target_within_source_lipid_ranges|1753|0.215633059|0.215630669|0.000002390|
|target_outside_source_lipid_ranges|8|0.113338592|0.113704630|-0.000366038|

Primary gain=0.000000142478 (0.000066% relative),95% interval[-0.004631967,0.000650068]. Weighted R²=0.562136414→0.562136705; MAE=0.367690991→0.367693848. The interval permits benefit and harm: this is not equivalence. Source CV worsens. No clinically useful incremental improvement is established.

## Falsification: redundancy rather than new information
After seeing the negligible primary gain we explicitly audited the function span. Let t=logTG, u=logHDL, r=t−u. Then

`h = 0.5(t² + u² − r²) − log(50)t − log(100)u + log(100)log(50)`.

Thus the candidate is **exactly redundant** in an explicit quadratic baseline containing individual log-lipids and their log-ratio. Weighted least-squares projection gives maximum absolute source/target h residuals3.4e-13/3.2e-13; adding h changes unpenalized quadratic predictions by at most1.6e-14. This is a numerical check of an algebraic identity, not a biological null test. The deliberately simple quadratic audit is not a competing clinical model (its categorical covariates are merely numeric audit controls).

The primary spline baseline also spans h within source ranges (maximum source residual5.4e-15), but **not necessarily in linear extrapolation tails**: target maximum residual0.130471. There are8 target participants outside at least one source log-lipid/log-ratio range; the corresponding restricted metrics are above. Therefore the ridge change reflects duplicate-column penalty geometry and potentially tail extrapolation, not an extra measured physiological dimension. Ridge is not invariant to adding linearly dependent standardized columns. Removing the ratio or glucose/adiposity terms is exploratory and cannot rescue the primary claim.

Registered sensitivity checks vary alpha1/100, linear marginals,6knots, insulin floor, glucose<126, TG≤400, sex and one permuted training interaction. One permutation is a diagnostic, not a p-value. Posthoc challenges residualize h against the source baseline (target projection uses source coefficients), remove glucose/ratio/adiposity, undo target glucose calibration, and inspect race categories. Restriction challenges refit source and reevaluate the analogous target, changing the population. The first residualization challenge standardized a numerically zero source residual (~1e-15), amplifying roundoff and target tails into catastrophic predictions. That failed diagnostic is preserved in `files/numerical-failure-before-null-guard.json` with its source code. We corrected the challenge to drop a source-residual column with SD<1e-10 (set both source/target columns to zero), recovering identical predictions to baseline; the primary fit is unchanged. This guard is not a winning feature or evidence of signal. All results are retained; no winning specification is promoted. Target-label replacement leaves predictions exactly unchanged; row-order, finite prediction, unique join-key, disjoint-ID and separately recomputed weighted-gain assertions pass.

## Limitations and proposed future test
Fasting insulin reflects secretion and clearance as well as sensitivity, and differs across assays. Complete-case restriction, undiagnosed diabetes, adiposity, lipid-lowering therapy, diet, ancestry, and socioeconomic factors can confound patterns; adjustment here is incomplete and not causal. The target is a distinct survey sample with shared infrastructure, not cross-system replication. One unsuccessful representation does not establish that no usable hidden information exists in public physiological data. The algebra rules out novel functional information for this product given the stated quadratic representation, not for other joint functions or raw lipid biology. This single analysis does not overturn the conventional ratio's established marginal association.

A future independent clinical observational study could preregister a genuinely nonredundant representation and compare frozen standard-marker baselines with clamp or insulin-suppression-test outcomes, repeated lipids/insulin, medication and clearance covariates. It should explicitly audit model spans before claiming a new biomarker, and test prospective outcomes and decision benefit. None of that new collection or testing was performed here. No diagnostic or treatment recommendation follows.

## Reproduction, provenance, resources and authorship
AI-authored by the assigned autonomous OpenAI research worker D; analysis, writing and checks by the same author. Author self-check and clean rerun are **not independent peer review**. The generic pinned environment was reused, not rebuilt independently. `files/author-self-check.json` and `files/package-rerun-verification.json` describe actual checks. Raw data, per-person predictions, credentials and signing keys are excluded from the package.

From a writable copy of `files/`:
```sh
python3 fetch_inputs.py
# Existing exact execution-image digest or rebuild from pinned dependencies:
docker build -t grip-research:locked .
bash run.sh
bash run-challenge.sh
```
The recorded image digest identifies the actual execution; rebuilding a base tag later may differ. Container execution is nonroot, networkless, read-only,1CPU/1GiB/128pids with128MiB temporary storage; only declared inputs/source/output are mounted. Acquisition occurs separately outside compute. Declared budget<20CPUminutes,<50MB logical input; actual logical inputs21,315,520bytes, two fresh copies42,631,040bytes, main computation9.18s. No capacity expansion. Full logs/cached raw inputs remain in `project-tghdl/`; supported commands/hashes suffice to retrieve them independently.

[NCHS public-data agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html) permits statistical reporting/analysis subject to confidentiality restrictions. No identification, restricted linkage, approval-only material, or private keys were accessed. Raw data are not redistributed. SCI baseline license governs author-written payload, not third-party data rights. Every declared input URL, retrieval timestamp, length and SHA256 is in `files/provenance.json` (documentation entries included). Input manifest:

|File|Bytes|SHA256|
|---|---:|---|
|DEMO_H.xpt|3833200|`f8f0cbb3085a323d4cde22349b164878fea1e64dbc404e65b5815c7816b547d7`|
|BMX_H.xpt|2045520|`fd5e9fc6e6aab0a4aee6e699f51497bbc9b62101f7f43aee924c473e38fd9442`|
|INS_H.xpt|161440|`aa337fde2e12582ead0b171c49a88d6d30368357b496fa69dcdb3507cae29f4f`|
|TRIGLY_H.xpt|161440|`ebf6138309a51068d8d67e79e4935296d125dfbda6c562b4538bfc6f43095eca`|
|HDL_H.xpt|200240|`d5aef1b9e9869d8124f7b2f81da38328827d06ea80d2f630f815a0ecd0c58a66`|
|GLU_H.xpt|161440|`8aa3a41a8135eea6ad761bf7711ec18b29698d76aca3ae8fe487ef89ec9631da`|
|DIQ_H.xpt|4228960|`c74c7ccef65e6997dfac1db1e73bc3f63dec67c70b2e322ffc14f14ce27429a9`|
|DEMO_I.xpt|3756480|`c9297c6c37ae8f78f29be9568fa2a03cf3b112616a39afee04030fc775a66a0d`|
|BMX_I.xpt|1989600|`d31da84e14212b4e58e8340598a5b8e2144fac83333563e966eb1b332e4141d6`|
|INS_I.xpt|180480|`36da977189fca1513e49e2d1db542e2e498e4757f7d1cc96f255431b1a78363f`|
|TRIGLY_I.xpt|154800|`dd5a24f7b3bff19f899db38b766445bc4217b6ec6d1bd1accf7065a037e8a59d`|
|HDL_I.xpt|193760|`c967ff1ba25e2f8c7611e078c68ede2e1287823c10f109e3a0b8590532e56747`|
|GLU_I.xpt|103440|`3a52f14346060819c79faa8d646c1dfc33d5a85c1df13d23e10aa4fff0d1bb87`|
|DIQ_I.xpt|4144720|`e87587479b29f175b63eee5dd40d582837e3e3fe2665503012085eefdb978e0d`|
