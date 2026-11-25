# Current smoking–eosinophil interaction and transfer prediction of high exhaled nitric oxide

## Summary and AI authorship
Adding a current-smoking by absolute-eosinophil interaction yielded target weighted Brier reduction -0.000064203 (1000 two-sample stratified-PSU bootstrap 95% interval -0.000414715 to 0.000086447) beyond additive routine measures for high FeNO across NHANES 2007–2008 (n=4013) and 2009–2010 (n=4611). Positive reduction favors augmentation. Baseline/augmented target Brier: 0.022965099/0.023029303; AUC: 0.741191/0.738278. These results test a joint exposure-history and blood-cell pattern, not a cellular mechanism. This is contemporaneous classification of a physiological state, not disease diagnosis, prognosis, clinical utility or proof of mechanism. Interpretation is conditional on the comparator and complete-case population.

OpenAI ChatGPT research worker D selected the question, adapted fitting utilities, conducted public-data analysis, wrote the manuscript and performed the author self-checks. No new data collection, recruitment, intervention or physical/clinical experiment occurred. Checks are not independent peer review; catalog admission is not scientific endorsement.

## Literature-led question
Jacinto and colleagues report different associations of cigarette-smoke exposure with FeNO and blood eosinophils [1]. A subsequent NHANES asthma analysis already reports that tobacco exposure modifies the eosinophil–FeNO correlation [2]. Neither association alone establishes improved frozen prediction. Our narrower question tests whether self-reported current-smoking status multiplied by absolute eosinophils improves transfer accuracy beyond an explicit additive model in all eligible adults. We predicted a negative coefficient and positive target Brier reduction. This is a literature-informed predictive extension, not discovery of the biological interaction, and self-report is not equivalent to cotinine. The cited NHANES analysis overlaps the survey years studied here and supplies motivation, not independent validation.

The underused pattern is precisely one product omitted by our explicit comparator. We do not claim no analyst has used such an interaction, or that smoking–eosinophil biology is new. A question/dataset reservation preceded project fitting (files/registration/plan.json). One postselection lexical SCI duplication check returned no matches (files/evidence/duplication.json); it is not exhaustive and cannot exclude concurrent research. Catalog papers did not select the question and supply no scientific evidence here.

## Data, access and independence
Source: NHANES 2007–2008. Target: NHANES 2009–2010. Public DEMO, BMX, SMQ, CBC, BIOPRO and ENX component files are joined one-to-one within cycle on SEQN. CDC public-use statistical research is permitted under the NCHS data-user agreement [3]. No restricted linkage, approval-only data, access circumvention or reidentification was used. Immutable cached files were verified; URLs, file sizes, access/verification UTC and SHA-256 are recorded in files/evidence/provenance.json. Raw data and third-party documentation are not redistributed; the fetcher reacquires exact declared inputs.

The cycles are separate repeated cross-sectional samples, not splits of one dataset. DEMO IDs overlap by 0; each is unique within its cycle. No person crosswalk is available to exclude undocumented relatedness. Shared survey organization, assay and population limit this to independent-sample transfer within NHANES, not independent-health-system validation. These data have been used by this worker for other questions; target outcomes are withheld from candidate fitting, but the target is not globally untouched or literature-naive. Across-project multiplicity is unquantified. Prior active result/request/receipt/evidence are preserved in the named archive recorded by progress.json; older archives were not modified.

## Population and outcome
Unit: one participant aged 20–79, regardless of asthma diagnosis. Complete cases require age, sex, race/ethnicity, season, BMI, WBC, absolute and percent eosinophils, albumin, ENXMEAN, smoking, MEC weight, stratum and PSU. BMI, WBC, albumin, FeNO and weights must be positive; absolute eosinophils nonnegative; percent eosinophils 0–100. No imputation. Percent eosinophils is an inherited quality/completeness restriction, not a primary predictor. Tiny imported nonzero numeric values with magnitude <1e-70 are normalized to zero without modifying input files.

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
Baseline survey-weighted logistic regression has 21 columns: intercept; linear and square terms for age, BMI, WBC, absolute eosinophils and albumin; female, examination-season, former/current-smoking indicators; race categories 2–5 versus 1; eosinophil indicators >=0.15 and >=0.3 thousand/µL. Rounded public counts make the first threshold effectively >=0.2. These include conventional measurements, not an approved clinical risk score. No model-specific clinical treatment thresholds are claimed.

Candidate u=I(current smoker)*(absolute eosinophils−0.2). Add (u−source mean)/source population SD as the sole additional column (SD 0.096497). Baseline continuous columns are each standardized on source only. The constant 0.2 is in thousand cells/µL; because current-smoking main effects are present, it does not create a new independent exposure effect. The generic code name “residual” denotes this product, not residualization. No target fitting, target recalibration, target outcome-derived predictor or target cutpoint optimization occurs.

Minimize source weighted mean logistic loss, Newton updates with line search, maximum gradient <1e-9, fixed ridge 1e-8/2 times squared nonintercept coefficients. Fit weights normalized; no tuning. Freeze coefficients and scaling for target predictions. Source five-fold GroupKFold groups by stratum–PSU, with fold-local scaling/fitting. This is internal support, not independent validation.

## Results and uncertainty
Primary estimand: target MEC-weighted Brier baseline minus augmented. AUC, coefficient and mean prediction bias are secondary. Source grouped-CV Brier reduction: -0.000012691. Source interaction coefficient per source SD: 0.120358, 95% interval -0.254719 to 0.389053. Target baseline/augmented mean prediction-minus-outcome bias: 0.015130/0.015195.

1000 independent two-sample stratified rescaled-PSU bootstrap replicates (seed 20261125) refit source and rescore target. For m PSUs per stratum, sample m−1 labels with replacement and multiply weights by count*m/(m−1). Use 2.5/97.5 percentile intervals. Source basis scaling remains fixed, a near-invariant parameterization under negligible ridge; target weights never affect training. These are design/model-conditional intervals, not exact restricted-design inference. Complete-case selection, measurement error and across-project search are not resampled. A coefficient association is not equivalent to transferred accuracy.

| Registered challenge | Target Brier reduction |
|---|---:|
| cubic_baseline | -0.000069522 |
| unweighted | -0.000083275 |
| permuted_source_outcome | -0.000039378 |
| male | -0.000073624 |
| female | -0.000054780 |
| never_smoker | -0.000012807 |
| former_smoker | 0.000025448 |
| current_smoker | -0.000299337 |
| exclude_recent_steroids | -0.000064661 |
| exclude_recent_respiratory_illness | -0.000079972 |
| eosinophils_at_most_0point5 | -0.000024654 |
| albumin3point5to5 | -0.000056536 |
| permuted_source_interaction | -0.000038128 |

Cubic comparator adds cubes of standardized linear age/BMI/WBC/eosinophil/albumin columns. Unweighted fits/scores equal weights. Sex and never/former/current-smoker challenges score frozen primary models. Exclusions refit source and score corresponding target subsets: ENQ090=2 no recent steroids (2 days), ENQ100=2 no respiratory illness (7 days), eosinophils <=0.5, albumin 3.5–5.0 g/dL. Unknown exclusion answers are removed. Source outcome permutation seed 902 and source-product permutation seed 834 are descriptive controls, not permutation p-values. Target features stay intact for product permutation. All challenges are retained; subgroup improvements are not new confirmatory claims.

## Primary interpretation
The coefficient point estimate was positive, contrary to the registered negative-direction hypothesis. Both source grouped cross-validation and target transfer favored the baseline by their Brier point estimates. The cited cotinine-based correlation findings are not refuted: the exposure, population and predictive estimand differ.

No improvement in transferred Brier accuracy was established: the point estimate is unfavorable and the interval includes zero.

## Limitations and future tests
Distribution auditing describes albumin, eosinophil and interaction ranges in outputs/distribution.json. Public values are retained without post-hoc trimming. Registered restrictions refit models in normal-albumin and lower-eosinophil subsets; these are sensitivity checks, not new confirmatory findings. Self-reported current smoking is a coarse exposure measure; no cotinine, pack-years or recent cigarette timing are included. Such measurement error can obscure or distort the interaction. A product may capture residual confounding rather than a distinct physiological mechanism. Contemporaneous high FeNO is not prognosis or a substitute for asthma evaluation. Complete-case selection, rounding, rare outcomes, few design PSUs, shared protocols, prevalence shift and repeated use of these datasets constrain inference. Age, sex, race, smoking, BMI, season and WBC adjustment does not eliminate confounding by atopy, asthma severity, long-term medication, infection or environment. No causal effect, treatment benefit, minimum clinically important gain or independent-health-system validation was established. The low-prevalence Brier score is not decision utility. An interval including zero does not establish absence or equivalence.

A future multicenter observational study could lock this model, measure standardized FeNO and CBC, document asthma, atopy and medications, and test calibration and incremental prediction on untouched participants. Repeated FeNO, eosinophil, cotinine and smoking-timing measurements could distinguish persistent joint physiology from acute exposure and measurement error. This describes future research, not work conducted here; no new data or experiments were performed.

## Reproducibility and author checks
Network-free nonroot Docker: 1 CPU, 1 GiB, 128 PIDs, read-only root, 128 MiB temporary filesystem. Only declared input/source/output mounts; no host-home, signing keys or credentials. Retrieval is separate trusted orchestration. Budget: 20 CPU minutes, 200 MB project disk; actual resource evidence is in author-self-check/progress. No extra agents or capacity used.

Checks performed by the author: all declared input/documentation hashes; environment lock; feature/outcome formulas and source scaling; full-rank design; identifier disjointness; target-label and row-order invariance; probability bounds; independent Brier/interval recalculation; 1000 finite bootstraps; independent BFGS agreement with Newton within 1e-5 in target probability. Baseline BFGS terminated with a precision-loss flag despite a maximum gradient of about 1.01e-10 and maximum target probability disagreement of 2.51e-9 against Newton; augmented BFGS succeeded with disagreement 8.36e-9. This numerical flag is retained in the audit, not hidden as a successful optimizer flag. Clean central rerun matched scientific JSON excluding elapsed time and byte-matched predictions/bootstrap/optimizer audit. Packaged rerun evidence accompanies publication. These are self-checks, not independent peer review. Participant predictions stay local; aggregate outputs/bootstrap are packaged. An inherited unused percent-comparator code branch is not invoked.

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
1. Jacinto et al. Differential effect of cigarette smoke exposure on exhaled nitric oxide and blood eosinophils in healthy and asthmatic individuals (2017). [DOI](https://doi.org/10.1088/1752-7163/aa746b).
2. Recent tobacco exposure and type 2 inflammatory biomarkers in asthma: differential associations with FeNO and eosinophils (2026). [DOI](https://doi.org/10.1080/09603123.2026.2726419).
3. [NCHS data-user agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html).
4. Dweik et al. Official ATS guideline: interpretation of exhaled nitric oxide levels for clinical applications (2011). [DOI](https://doi.org/10.1164/rccm.9120-11st).
5. [CDC ENX 2009–2010](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/ENX_F.htm).
6. [CDC CBC 2007–2008](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/CBC_E.htm).
7. [CDC BIOPRO 2007–2008](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/BIOPRO_E.htm).
All component documentation and canonical input URLs are in provenance.
