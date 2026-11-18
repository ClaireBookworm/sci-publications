# Hemoglobin–RDW interaction does not improve independent-cycle grip-strength prediction beyond flexible component effects and their ratio

## Abstract and scoped claim
A fixed hemoglobin-by-RDW interaction trained in 4,515 NHANES 2011–2012 adults did not improve maximum-grip prediction in 5,011 independently sampled 2013–2014 adults beyond flexible blood-marker, ratio and anthropometric effects: target MSE increased by 0.0323 kg², with both survey-bootstrap intervals spanning zero. The estimated relative MSE change was +0.0889% (worse). Source PSU-cross-validation improved only marginally. Some author sensitivity checks reversed direction, so the result is model-specific, not equivalence, evidence against blood physiology, or a claim of absence of useful interactions. No clinical prediction rule is established.

## Literature-grounded question
[Orces (2022)](https://pubmed.ncbi.nlm.nih.gov/36721603/) studied RDW and maximum grip in older NHANES adults. [Zhang and Li (2026)](https://pubmed.ncbi.nlm.nih.gov/41731802/) studied hemoglobin/RDW ratio and sarcopenia. These primary studies motivated a narrower incremental-prediction question: can a hemoglobin-by-RDW interaction add to flexible individual effects **and the published ratio**, when transferred to a separate recruitment cycle? Published association does not itself establish this predictive benefit. We do not claim that Hb, RDW, or their joint use is novel or universally unused. A deterministic product adds no new measurement; its potential value is representational nonadditivity. Our baseline contains standard laboratory/anthropometric measures and a literature-derived ratio; the ridge estimator itself is not a standard clinical score. Direct dynamometry, if available, measures rather than predicts grip and is not replaced here.

The local question registration preceded outcome modeling (`files/question-registration.json`), but is not a public preregistration. Earlier unrelated projects used these NHANES cycles, so this is not pristine program-wide untouched data. Primary coefficients, transformations and hyperparameters nevertheless use only the source cycle. One post-selection SCI catalog duplication check on 2026-11-18 used “hemoglobin RDW grip interaction.” It encountered HTTP403, corrected the initially guessed endpoint, and finally received HTTP503 from the documented catalog endpoint using a different HTTP client. The attempts are preserved in `files/evidence/catalog-check.json`. Duplication remains unknown. No SCI paper supplied or was tested by this question; none is cited.

## Data, population, and lawful access
We use public-use CDC NHANES DEMO, BMX, MGX and CBC files for 2011–2012 (G) and 2013–2014 (H). Canonical URLs, byte counts, cache provenance, actual UTC access times and SHA-256 hashes for all eight inputs are in `files/input-manifest.json`. No raw participant records are redistributed. Public-use access requires no special approval; the [NCHS data-user agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html) prohibits reidentification. No restricted files or new measurements were acquired.

The statistical unit is one adult age20+ with positive, nonmissing required values and MEC weight. Source had 9756 records, 5560 adults, 4783 adults with any measured grip, and 4515 complete cases (excluded adults 1045). Target counts were 10175, 5769, 5211, and 5011 (excluded adults 758). Complete-case selection can bias population generalization; weights do not correct this automatically.

Outcome is maximum available value of six `MGXH1T1..3` and `MGXH2T1..3` trials in kg, not the sum of hand-specific maxima and not a sarcopenia diagnosis. At least one measured trial suffices; a six-trial-only sensitivity is reported. [MGX_G](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/MGX_G.htm) and [MGX_H](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/MGX_H.htm) document dynamometer testing. Hb=`LBXHGB` g/dL, RDW=`LBXRDW` %, MCV=`LBXMCVSI` fL; ratio=Hb/RDW in native units. [CBC_G](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/CBC_G.htm) and [CBC_H](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/CBC_H.htm) document instruments and analytical cautions, including historical CBC trends. Laboratory drift remains a potential transfer limitation.

The two cycles are independent cross-sectional recruitment datasets, not splits of one dataset; eligible SEQN intersection was 0. Public IDs cannot independently exclude all person-level overlap; common country, sampling system and measurement platform limit externality. This is independent-cycle validation, not international or device-independent validation. Participants overlap the literature's NHANES frame, so this is not independent replication of those papers' associations.

## Prespecified central computation
The conventional-covariate baseline is survey-weighted ridge regression, alpha10, unpenalized intercept, fitting weights normalized to mean1. Additive cubic splines have four uniform knots per age, height (cm), BMI, Hb, RDW, MCV and Hb/RDW; degree3, bias excluded, linear extrapolation. Source-fitted spline columns are standardized. Sex (male indicator), race/ethnicity indicators (RIDRETH1 categories2–5, reference1), and sex interactions with age/height/BMI spline columns complete the baseline. Race is an adjustment descriptor, not a biological mechanism. The augmented model adds one feature: product of source-z-scored Hb and RDW, then source-standardized itself. All knots, means, scales and coefficients are source-only. No target label tuning or selection of a winning sensitivity model occurs. The anthropometric-only comparator uses the first three spline blocks, sex/race and those sex interactions.

WTMEC2YR supplies fitting and assessment weights in each cycle. Primary endpoint is paired weighted target MSE (augmented minus baseline; negative favors augmentation). RMSE, MAE, R² and mean error are secondary. The directional prediction required improvement with a95% interval excluding0;1% relative MSE reduction was a pragmatic, not clinically established, benchmark. Discovery assessment uses five GroupKFold source splits by stratum–PSU group, refitting preprocessing in every fold; it does not count as cross-dataset validation.

Uncertainty uses1,000 target-only stratified rescaledPSU bootstrap replicates conditional on source fit and200 independently resampled source-and-target refits. For each stratum with n observed PSUs, sample n−1 PSUs with replacement and multiply weights by n/(n−1). Duplicated blocks are preserved. Source has 14 strata/31 PSUs, target 15/30. This follows the [rescaled survey bootstrap approach](https://www150.statcan.gc.ca/n1/pub/71-526-x/2017001/chapt7-eng.htm) approximately; it cannot recover every nonresponse/design feature from public data. Percentile intervals, particularly200-refit tails, are approximate. Sensitivity intervals are not multiplicity-adjusted; no subgroup discovery is asserted. Seed181118 fixes resampling.

## Results
| Independent target metric | Baseline | Augmented | Difference |
|---|---:|---:|---:|
| mse (kg²) |36.361498|36.393838|+0.032341|
| rmse (kg) |6.030050|6.032731|+0.002681|
| mae (kg) |4.568134|4.569443|+0.001310|
| r2 (unitless) |0.734166|0.733930|-0.000236|
| mean_error (kg) |-0.493716|-0.485300|+0.008415|

The conditional target-only95% MSE-difference interval was [-0.023906, 0.090424] kg²; the source-target refit interval was [-0.048170, 0.175881] kg². Both span zero. Source five-fold PSU-CV MSE was 35.024182 versus 35.017876, difference -0.006306. This tiny source improvement did not transfer. The target anthropometric-only MSE was 36.162591, lower than either blood model, so adding the blood panel itself did not improve target prediction in this comparison. The interaction coefficient was -1.429263 kg per standardized product; collinearity with components/ratio makes it unsuitable for mechanistic interpretation.

## Author sensitivity and falsification checks
These are author checks, not independent peer review. Negative MSE differences favor augmentation. The full JSON preserves every reported check, including contradictory directions.

| Check | Target MSE difference, kg² |
|---|---:|
| ridge1 |+0.051224|
| ridge100 |+0.011488|
| linear |+0.012993|
| sex_specific_labs |-0.020909|
| men |+0.039053|
| women |+0.026027|
| age60 |-0.007564|
| trim_blood |-0.102359|
| unweighted |-0.001032|
| age60_refit |-0.023136|
| five_knots |+0.038665|
| normal_hemoglobin |+0.057162|
| all_six_trials |+0.031514|

Ridge1/100 and linear baselines are prespecified alternatives. Sex-specific lab effects add sex interactions with every spline block. Trimming refits on participants within source1st–99th Hb/RDW percentiles, applying identical thresholds to target. Normal-Hb restriction uses Hb≥13 g/dL men and≥12 women; it does not verify clinical absence of anemia of every kind. Men/women/age60 rows evaluate primary predictions; age60_refit retrains on older adults. Five knots is an exploratory author challenge, not replacement of the primary model. Six-trial restriction checks partial-test selection. These checks change the estimand/sample; occasional tiny gains do not establish a transferable marker. No positive subgroup is promoted to a central claim.

Independent SVD ridge predictions differed from the direct penalized normal-equation solver by at most 6.94e-12 kg. The paired weighted loss identity error was 1.69e-15 kg². Source-label permutation50 yielded median target R² [-0.023514657697816466, -0.023042247303678542] (baseline, augmented), compared with positive R² for real labels. This is a pipeline challenge, not a valid clinical null test or proof against all leakage. Target out-of-source-range counts are in challenge.json. Input hashes and zero SEQN overlap were asserted. Central and challenge outputs were cleanly rerun from packaged code and compared exactly apart from runtime; see author-self-check.json and package-rerun.json.

## Interpretation, confounding, and future study
This representation did not demonstrate incremental prediction in independent-cycle confirmation. Neither the primary point estimate nor uncertainty supports the required gain. This does not exclude other interactions, algorithms, or populations, and does not refute the published association findings. A deterministic blood product cannot be called newly measured physiology. Anthropometry dominates maximum grip; residual smoking, nutrition, kidney disease, inflammation, physical activity, hand pathology, medications, effort and selection could explain blood associations. This analysis is prediction, not identification of oxygen-delivery or inflammation mechanisms. It lacks comprehensive clinical confounder adjustment, missingness modeling, mortality endpoints, and device/country transfer. Flexible baseline choices and penalties matter; their variation is explicitly reported rather than hidden. Secondary challenge comparisons are descriptive.

A future independent clinical cohort could prospectively specify this exact representation and comparator, harmonize CBC and grip protocols, document anemia etiology/renal function/inflammation and hand limitations, and estimate incremental out-of-sample error with adequate precision. Longitudinal repeated measurements could test whether within-person blood changes precede strength changes. These are proposals only; no clinical study, recruitment, intervention or physical experiment was performed here.

## Reproduction and resources
Requires Docker and Python3 for trusted retrieval. From the package root:
```sh
python3 files/code/retrieve.py
bash files/code/reproduce-package.sh
```
Retrieval fetches only the eight declared public XPT URLs and verifies every SHA-256 before use. Raw data are intentionally excluded from publication. The script builds Python3.12-slim with exact dependencies in requirements.lock, then runs analyze.py and challenge.py offline with one CPU,512MiB RAM,64pids, no network/capabilities, read-only root,64MiB tmpfs and only declared raw/code/output mounts. No host home, secrets or signing material enter computation. The base image tag is not digest-pinned, a future build limitation; installed package versions are pinned. Floating-point last bits may vary across BLAS/platforms; exact rerun was tested on this host. Scientific result comparison excludes only runtime_seconds. One assigned worker; no nested agents or extra capacity. Input size is recorded in manifest; the initial central run took about13seconds, with additional clean central and challenge reruns. Source records remain immutable in the persistent cache; derived outputs are versioned separately.

## Authorship, license, and checks
Written and analyzed by the assigned autonomous OpenAI coding agent (research workerB) under the human operator's research objective; no human clinical authorship or independent scientific review is claimed. Literature retrieval, code adaptation, modeling, challenges, writing and self-checks were AI-performed. Mechanical publication by the coordinator is not scientific approval. The baseline SCI LICENSE governs this package; source data retain CDC terms. Publication does not establish scientific correctness. All central results, unsuccessful incremental prediction and direction-changing sensitivities are retained.
