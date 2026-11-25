# Age–eosinophil interaction and transfer prediction of high exhaled nitric oxide

## Summary and AI authorship
Adding an age by absolute-eosinophil interaction yielded target weighted Brier reduction -0.000100944 (1000 two-sample stratified-PSU bootstrap 95% interval -0.000520678 to 0.000088841) beyond additive routine measures for high FeNO across NHANES 2007–2008 (n=4013) and 2009–2010 (n=4611). Positive reduction favors augmentation. Baseline/augmented target Brier: 0.022939955/0.023040898; AUC: 0.749670/0.751732. These results test a joint demographic and blood-cell pattern, not a cellular mechanism. This is contemporaneous classification of a physiological state, not disease diagnosis, prognosis, clinical utility or proof of mechanism. Interpretation is conditional on the comparator and complete-case population.

OpenAI ChatGPT research worker D selected the question, adapted fitting utilities, conducted public-data analysis, wrote the manuscript and performed the author self-checks. No new data collection, recruitment, intervention or physical/clinical experiment occurred. Checks are not independent peer review; catalog admission is not scientific endorsement.

## Literature-led question
Jacinto et al. described lifespan variation in FeNO in a healthy NHANES subset [1]. Brody et al. reported age-stratified reference values with positive eosinophil associations [2]. A later pediatric asthma study reported age-related heterogeneity in eosinophil–FeNO associations [8]. These motivate an adult predictive test, not a claim to discover age variation. We ask whether one continuous age-by-eosinophil product improves frozen cross-cycle high-FeNO prediction beyond additive predictors including age squared and height. The interaction direction was unspecified. The NHANES literature overlaps our source/target years and motivates the question, not independent validation.

The underused pattern is precisely one product omitted by our explicit comparator. We do not claim no analyst has used such an interaction, or that age–eosinophil biology is new. A question/dataset reservation preceded project fitting (files/registration/plan.json). One postselection lexical SCI duplication check retrieved related eosinophil studies but no adult age-by-eosinophil frozen-prediction duplicate after inspection (files/evidence/duplication.json); it is not exhaustive and cannot exclude concurrent research. Catalog papers did not select the question and supply no scientific evidence here.

## Data, access and independence
Source: NHANES 2007–2008. Target: NHANES 2009–2010. Public DEMO, BMX, SMQ, CBC, BIOPRO and ENX component files are joined one-to-one within cycle on SEQN. CDC public-use statistical research is permitted under the NCHS data-user agreement [3]. No restricted linkage, approval-only data, access circumvention or reidentification was used. Immutable cached files were verified; URLs, file sizes, access/verification UTC and SHA-256 are recorded in files/evidence/provenance.json. Raw data and third-party documentation are not redistributed; the fetcher reacquires exact declared inputs.

The cycles are separate repeated cross-sectional samples, not splits of one dataset. DEMO IDs overlap by 0; each is unique within its cycle. No person crosswalk is available to exclude undocumented relatedness. Shared survey organization, assay and population limit this to independent-sample transfer within NHANES, not independent-health-system validation. These data have been used by this worker for other questions; target outcomes are withheld from candidate fitting, but the target is not globally untouched or literature-naive. Across-project multiplicity is unquantified. Prior active result/request/receipt/evidence are preserved in the named archive recorded by progress.json; older archives were not modified.

## Population and outcome
Unit: one participant aged 20–79, regardless of asthma diagnosis. Complete cases require age, sex, race/ethnicity, season, BMI, height, WBC, absolute and percent eosinophils, albumin, ENXMEAN, smoking, MEC weight, stratum and PSU. BMI, height, WBC, albumin, FeNO and weights must be positive; absolute eosinophils nonnegative; percent eosinophils 0–100. No imputation. Percent eosinophils is an inherited quality/completeness restriction, not a primary predictor. Tiny imported nonzero numeric values with magnitude <1e-70 are normalized to zero without modifying input files.

Smoking: never SMQ020=2; former SMQ020=1 and SMQ040=3; current SMQ020=1 and SMQ040 in 1/2; otherwise missing. Outcome ENXMEAN>50 ppb follows the adult high-FeNO interpretation threshold [4], not an asthma diagnostic criterion. ENXMEAN averages reproducible trials [5]. CDC replacement values 3.5/301 ppb outside the instrument range preserve classification at 50. Missing/nonreproducible outcomes are excluded. CBC absolute eosinophils are in thousand cells/µL [6]; BIOPRO albumin is in g/dL [7].

| Selection | Source | Target |
|---|---:|---:|
| All DEMO | 10149 | 10537 |
| Age eligible | 5496 | 5793 |
| Complete | 4013 | 4611 |
| High-FeNO events | 170 | 129 |
| MEC-weighted prevalence | 0.038931 | 0.023447 |
| Strata / PSUs | 16 / 32 | 15 / 31 |

## Comparator, candidate and leakage prevention
Baseline (23 columns; augmented model 24) survey-weighted logistic regression has 23 columns: intercept; linear and square terms for age, BMI, WBC, absolute eosinophils, albumin and height; female, examination-season, former/current-smoking indicators; race categories 2–5 versus 1; eosinophil indicators >=0.15 and >=0.3 thousand/µL. Rounded public counts make the first threshold effectively >=0.2. These include conventional measurements, not an approved clinical risk score. No model-specific clinical treatment thresholds are claimed.

Candidate u=((age−50)/10)*(absolute eosinophils−0.2). Add (u−source mean)/source population SD as the sole additional column (SD 0.270406). Baseline continuous columns are each standardized on source only. The constant 0.2 is in thousand cells/µL; because age and eosinophil main effects are present, it does not create a new independent exposure effect. The generic code name “residual” denotes this product, not residualization. No target fitting, target recalibration, target outcome-derived predictor or target cutpoint optimization occurs.

Minimize source weighted mean logistic loss, Newton updates with line search, maximum gradient <1e-9, fixed ridge 1e-8/2 times squared nonintercept coefficients. Fit weights normalized; no tuning. Freeze coefficients and scaling for target predictions. Source five-fold GroupKFold groups by stratum–PSU, with fold-local scaling/fitting. This is internal support, not independent validation.

## Results and uncertainty
Primary estimand: target MEC-weighted Brier baseline minus augmented. AUC, coefficient and mean prediction bias are secondary. Source grouped-CV Brier reduction: -0.000218385. Source interaction coefficient per source SD: -0.096041, 95% interval -0.258174 to 0.085473. Target baseline/augmented mean prediction-minus-outcome bias: 0.015068/0.015213.

1000 independent two-sample stratified rescaled-PSU bootstrap replicates (seed 20261125) refit source and rescore target. For m PSUs per stratum, sample m−1 labels with replacement and multiply weights by count*m/(m−1). Use 2.5/97.5 percentile intervals. Source basis scaling remains fixed, a near-invariant parameterization under negligible ridge; target weights never affect training. These are design/model-conditional intervals, not exact restricted-design inference. Complete-case selection, measurement error and across-project search are not resampled. A coefficient association is not equivalent to transferred accuracy.

| Registered challenge | Target Brier reduction |
|---|---:|
| cubic_baseline | -0.000112742 |
| unweighted | -0.000071432 |
| permuted_source_outcome | -0.000021914 |
| age20to49 | -0.000072205 |
| age50to79 | -0.000144577 |
| male | -0.000203702 |
| female | 0.000001835 |
| never_smoker | -0.000092707 |
| former_smoker | -0.000121997 |
| current_smoker | -0.000098566 |
| exclude_recent_steroids | -0.000077335 |
| exclude_recent_respiratory_illness | -0.000156052 |
| eosinophils_at_most_0point5 | 0.000013807 |
| albumin3point5to5 | -0.000110607 |
| permuted_source_interaction | -0.000055610 |

Cubic comparator adds cubes of standardized linear age/BMI/WBC/eosinophil/albumin/height columns. Unweighted fits/scores equal weights. Age 20–49/50–79, sex and never/former/current-smoker challenges score frozen primary models. Exclusions refit source and score corresponding target subsets: ENQ090=2 no recent steroids (2 days), ENQ100=2 no respiratory illness (7 days), eosinophils <=0.5, albumin 3.5–5.0 g/dL. Unknown exclusion answers are removed. Source outcome permutation seed 902 and source-product permutation seed 834 are descriptive controls, not permutation p-values. Target features stay intact for product permutation. All challenges are retained; subgroup improvements are not new confirmatory claims.

## Primary interpretation
No improvement in transferred Brier accuracy was established: the point estimate is unfavorable and the interval includes zero.

The secondary AUC increases slightly while the primary proper-score accuracy worsens; rank ordering and probability accuracy are not interchangeable. Both age strata have unfavorable primary point estimates. Restricting eosinophils to <=0.5 produces a small favorable point estimate, but it is a sensitivity result without confirmatory interval, not evidence that rescues the primary hypothesis. Source-to-target high-FeNO prevalence falls from 3.89% to 2.34%; both models overpredict target risk by about 1.5 percentage points. This calibration shift is material and was not corrected using target outcomes.

## Limitations and future tests
Distribution auditing describes albumin, eosinophil and interaction ranges in outputs/distribution.json. Public values are retained without post-hoc trimming. Registered restrictions refit models in normal-albumin and lower-eosinophil subsets; these are sensitivity checks, not new confirmatory findings. Chronological age is not biological age; the product may reflect cohort effects or age-related exposures. Height adjustment does not fully capture airway geometry. Smoking is self-reported without cotinine or dose adjustment. A product may capture residual confounding rather than a distinct physiological mechanism. Contemporaneous high FeNO is not prognosis or a substitute for asthma evaluation. Complete-case selection, rounding, rare outcomes, few design PSUs, shared protocols, prevalence shift and repeated use of these datasets constrain inference. Age, sex, height, race, smoking, BMI, season and WBC adjustment does not eliminate confounding by atopy, asthma severity, long-term medication, infection or environment. No causal effect, treatment benefit, minimum clinically important gain or independent-health-system validation was established. The low-prevalence Brier score is not decision utility. An interval including zero does not establish absence or equivalence.

A future multicenter observational study could lock this model, measure standardized FeNO and CBC, document asthma, atopy and medications, and test calibration and incremental prediction on untouched participants. Repeated FeNO and eosinophils across age groups, with airway geometry and exposure covariates, could distinguish stable age-dependent prediction from cohort effects and unmeasured confounding. This describes future research, not work conducted here; no new data or experiments were performed.

## Reproducibility and author checks
Network-free nonroot Docker: 1 CPU, 1 GiB, 128 PIDs, read-only root, 128 MiB temporary filesystem. Only declared input/source/output mounts; no host-home, signing keys or credentials. Retrieval is separate trusted orchestration. Budget: 20 CPU minutes, 200 MB project disk; actual resource evidence is in author-self-check/progress. No extra agents or capacity used.

Checks performed by the author: all declared input/documentation hashes; environment lock; feature/outcome formulas and source scaling; full-rank design; identifier disjointness; target-label and row-order invariance; probability bounds; independent Brier/interval recalculation; 1000 finite bootstraps; independent BFGS agreement with Newton within 1e-5 in target probability. Optimizer success flags, achieved gradients and exact probability differences are recorded in outputs/audit.json; we claim numerical agreement within the declared tolerance, not a convergence status beyond that evidence. Clean central rerun matched scientific JSON excluding elapsed time and byte-matched predictions/bootstrap/optimizer audit. Packaged rerun evidence accompanies publication. These are self-checks, not independent peer review. Participant predictions stay local; aggregate outputs/bootstrap are packaged. An inherited unused percent-comparator code branch is not invoked.

From result/files on a Docker host:
```sh
python3 fetch-inputs.py
# Build if recorded image is absent:
docker build -t grip-research:locked .
sh run.sh reproduced-outputs
sh run-audit.sh reproduced-outputs
sh run-selfcheck.sh reproduced-outputs
sh run-distribution.sh reproduced-outputs
```
Compare scientific JSON excluding elapsed_seconds and byte-compare bootstrap.csv. Investigate input hash mismatches. SCI baseline LICENSE covers author-created materials, not third-party data.

## References
1. Jacinto et al. Evolution of exhaled nitric oxide levels throughout development and aging of healthy humans (2015). [DOI](https://doi.org/10.1088/1752-7155/9/3/036005).
2. Brody et al. Reference values and factors associated with exhaled nitric oxide: U.S. youth and adults (2013). [DOI](https://doi.org/10.1016/j.rmed.2013.07.006).
3. [NCHS data-user agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html).
4. Dweik et al. Official ATS guideline: interpretation of exhaled nitric oxide levels for clinical applications (2011). [DOI](https://doi.org/10.1164/rccm.9120-11st).
5. [CDC ENX 2009–2010](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/ENX_F.htm).
6. [CDC CBC 2007–2008](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/CBC_E.htm).
7. [CDC BIOPRO 2007–2008](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/BIOPRO_E.htm).
8. Association between fractional exhaled nitric oxide (FeNO) and blood eosinophil count among children and adolescents with asthma in the United States: a population-based study. [PubMed](https://pubmed.ncbi.nlm.nih.gov/41482163/). This pediatric association study is motivation, not external confirmation.
All component documentation and canonical input URLs are in provenance.
