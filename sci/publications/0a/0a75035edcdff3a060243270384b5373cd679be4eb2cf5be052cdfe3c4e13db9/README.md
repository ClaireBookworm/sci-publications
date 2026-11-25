# Platelet–eosinophil count interaction and transfer prediction of high exhaled nitric oxide

## Summary and AI authorship
Adding a platelet-count by absolute-eosinophil interaction yielded target weighted Brier reduction -0.000007545 (1000 two-sample stratified-PSU bootstrap 95% interval -0.000425897 to 0.000168342) beyond additive routine measures for high FeNO across NHANES 2007–2008 (n=4042) and 2009–2010 (n=4668). Positive reduction favors augmentation. Baseline/augmented target Brier: 0.022572345/0.022579890; AUC: 0.744492/0.744354. These results test a count-based proxy, not platelet activation or cellular adhesion. This is contemporaneous classification of a physiological state, not disease diagnosis, prognosis, clinical utility or proof of mechanism. Interpretation is conditional on the comparator and complete-case population.

OpenAI ChatGPT research worker D selected the question, adapted fitting utilities, conducted public-data analysis, wrote the manuscript and performed the author self-checks. No new data collection, recruitment, intervention or physical/clinical experiment occurred. Checks are not independent peer review; catalog admission is not scientific endorsement.

## Literature-led question
Human cell experiments reported that platelets promoted eosinophil adhesion under flow [1]. A pediatric asthma study linked airway platelet activation with eosinophilic inflammation [2]. These primary reports motivate the hypothesis that a joint blood-count pattern may contain information omitted by additive prediction of airway inflammation. They do not show that platelet concentration measures activation. We ask whether (platelet count−250)*(absolute eosinophils−0.2) improves prediction of FeNO >50 ppb beyond additive routine measurements. Predicted direction was positive coefficient and positive target Brier reduction. The biological motivation is indirect; a negative result would not refute the cellular experiments.

The underused pattern is precisely one product omitted by our explicit comparator. We do not claim no analyst has used such an interaction, or that platelet–eosinophil biology is new. A question/dataset reservation preceded project fitting (files/registration/plan.json). One postselection lexical SCI duplication check returned no matches (files/evidence/duplication.json); it is not exhaustive and cannot exclude concurrent research. Catalog papers did not select the question and supply no scientific evidence here.

## Data, access and independence
Source: NHANES 2007–2008. Target: NHANES 2009–2010. Public DEMO, BMX, SMQ, CBC and ENX component files are joined one-to-one within cycle on SEQN. CDC public-use statistical research is permitted under the NCHS data-user agreement [3]. No restricted linkage, approval-only data, access circumvention or reidentification was used. Immutable cached files were verified; URLs, file sizes, access/verification UTC and SHA-256 are recorded in files/evidence/provenance.json. Raw data and third-party documentation are not redistributed; the fetcher reacquires exact declared inputs.

The cycles are separate repeated cross-sectional samples, not splits of one dataset. DEMO IDs overlap by 0; each is unique within its cycle. No person crosswalk is available to exclude undocumented relatedness. Shared survey organization, assay and population limit this to independent-sample transfer within NHANES, not independent-health-system validation. These data have been used by this worker for other questions; target outcomes are withheld from candidate fitting, but the target is not globally untouched or literature-naive. Across-project multiplicity is unquantified. Prior active result/request/receipt/evidence are preserved in the named archive recorded by progress.json; older archives were not modified.

## Population and outcome
Unit: one participant aged 20–79, regardless of asthma diagnosis. Complete cases require age, sex, race/ethnicity, season, BMI, WBC, absolute and percent eosinophils, platelet count, ENXMEAN, smoking, MEC weight, stratum and PSU. BMI, WBC, platelets, FeNO and weights must be positive; absolute eosinophils nonnegative; percent eosinophils 0–100. No imputation. Percent eosinophils is an inherited quality/completeness restriction, not a primary predictor. Tiny imported nonzero numeric values with magnitude <1e-70 are normalized to zero without modifying input files.

Smoking: never SMQ020=2; former SMQ020=1 and SMQ040=3; current SMQ020=1 and SMQ040 in 1/2; otherwise missing. Outcome ENXMEAN>50 ppb follows the adult high-FeNO interpretation threshold [4], not an asthma diagnostic criterion. ENXMEAN averages reproducible trials [5]. CDC replacement values 3.5/301 ppb outside the instrument range preserve classification at 50. Missing/nonreproducible outcomes are excluded. CBC platelet and absolute-eosinophil counts are both in thousand cells/µL [6].

| Selection | Source | Target |
|---|---:|---:|
| All DEMO | 10149 | 10537 |
| Age eligible | 5496 | 5793 |
| Complete | 4042 | 4668 |
| High-FeNO events | 170 | 129 |
| MEC-weighted prevalence | 0.038691 | 0.023171 |
| Strata / PSUs | 16 / 32 | 15 / 31 |

## Comparator, candidate and leakage prevention
Baseline survey-weighted logistic regression has 21 columns: intercept; linear and square terms for age, BMI, WBC, absolute eosinophils and platelets; female, examination-season, former/current-smoking indicators; race categories 2–5 versus 1; eosinophil indicators >=0.15 and >=0.3 thousand/µL. Rounded public counts make the first threshold effectively >=0.2. These include conventional measurements, not an approved clinical risk score. No model-specific clinical treatment thresholds are claimed.

Candidate u=(platelets−250)*(absolute eosinophils−0.2). Add (u−source mean)/source population SD as the sole additional column (SD 12.173943). Baseline continuous columns are each standardized on source only. Centering does not change the interaction interpretation when main effects are present. The generic code name “residual” denotes this product, not residualization. No target fitting, target recalibration, target outcome-derived predictor or target cutpoint optimization occurs.

Minimize source weighted mean logistic loss, Newton updates with line search, maximum gradient <1e-9, fixed ridge 1e-8/2 times squared nonintercept coefficients. Fit weights normalized; no tuning. Freeze coefficients and scaling for target predictions. Source five-fold GroupKFold groups by stratum–PSU, with fold-local scaling/fitting. This is internal support, not independent validation.

## Results and uncertainty
Primary estimand: target MEC-weighted Brier baseline minus augmented. AUC, coefficient and mean prediction bias are secondary. Source grouped-CV Brier reduction: -0.000021046. Source interaction coefficient per source SD: 0.011704, 95% interval -0.191847 to 0.306427. Target baseline/augmented mean prediction-minus-outcome bias: 0.015151/0.015171.

1000 independent two-sample stratified rescaled-PSU bootstrap replicates (seed 20261125) refit source and rescore target. For m PSUs per stratum, sample m−1 labels with replacement and multiply weights by count*m/(m−1). Use 2.5/97.5 percentile intervals. Source basis scaling remains fixed, a near-invariant parameterization under negligible ridge; target weights never affect training. These are design/model-conditional intervals, not exact restricted-design inference. Complete-case selection, measurement error and across-project search are not resampled. A coefficient association is not equivalent to transferred accuracy.

| Registered challenge | Target Brier reduction |
|---|---:|
| cubic_baseline | -0.000012943 |
| unweighted | 0.000003181 |
| permuted_source_outcome | 0.000002746 |
| male | -0.000011469 |
| female | -0.000003649 |
| never_smoker | -0.000017226 |
| exclude_recent_steroids | -0.000000910 |
| exclude_recent_respiratory_illness | -0.000027831 |
| eosinophils_at_most_0point5 | -0.000017738 |
| platelets150to450 | -0.000016414 |
| permuted_source_interaction | -0.000187069 |

Cubic comparator adds cubes of standardized linear age/BMI/WBC/eosinophil/platelet columns. Unweighted fits/scores equal weights. Sex and never-smoker challenges score frozen primary models. Exclusions refit source and score corresponding target subsets: ENQ090=2 no recent steroids (2 days), ENQ100=2 no respiratory illness (7 days), eosinophils <=0.5, platelets 150–450 thousand/µL. Unknown exclusion answers are removed. Source outcome permutation seed 902 and source-product permutation seed 834 are descriptive controls, not permutation p-values. Target features stay intact for product permutation. All challenges are retained; subgroup improvements are not new confirmatory claims.

## Primary interpretation
No improvement in transferred Brier accuracy was established: the point estimate is unfavorable and the interval includes zero.

## Limitations and future tests
A post-primary distribution audit found a large target product maximum (6076.2 versus source 231.0), target eosinophil maximum 8.4 versus source 2.9, and product SD 90.15 versus 12.17. These public values were retained rather than declared errors. Pre-registered eosinophil <=0.5 and platelet 150–450 restrictions also yielded unfavorable gains, reducing concern that a single extreme count alone explains the negative conclusion. Distribution aggregates are provided in outputs/distribution.json; this is post hoc descriptive auditing, not a new confirmatory test.

Platelet concentration does not measure activation, platelet–eosinophil complexes or airway recruitment. A count product may reflect confounding or measurement arithmetic rather than a new mechanism. Contemporaneous high FeNO is not prognosis or a substitute for asthma evaluation. Complete-case selection, rounding, rare outcomes, few design PSUs, shared protocols, prevalence shift and repeated use of these datasets constrain inference. Age, sex, race, smoking, BMI, season and WBC adjustment does not eliminate confounding by atopy, asthma severity, long-term medication, infection or environment. No causal effect, treatment benefit, minimum clinically important gain or independent-health-system validation was established. The low-prevalence Brier score is not decision utility. An interval including zero does not establish absence or equivalence.

A future multicenter observational study could lock this model, measure standardized FeNO and CBC, document asthma, atopy and medications, and test calibration and incremental prediction on untouched participants. Paired platelet-activation and eosinophil-complex assays could test whether the count proxy tracks the cellular state that motivated it. This describes future research, not work conducted here; no new data or experiments were performed.

## Reproducibility and author checks
Network-free nonroot Docker: 1 CPU, 1 GiB, 128 PIDs, read-only root, 128 MiB temporary filesystem. Only declared input/source/output mounts; no host-home, signing keys or credentials. Retrieval is separate trusted orchestration. Budget: 20 CPU minutes, 200 MB project disk; actual resource evidence is in author-self-check/progress. No extra agents or capacity used.

Checks performed by the author: all declared input/documentation hashes; environment lock; feature/outcome formulas and source scaling; full-rank design; identifier disjointness; target-label and row-order invariance; probability bounds; independent Brier/interval recalculation; 1000 finite bootstraps; independent BFGS agreement with Newton within 1e-5 in target probability. Clean central rerun matched scientific JSON excluding elapsed time and byte-matched predictions/bootstrap/optimizer audit. Packaged rerun evidence accompanies publication. These are self-checks, not independent peer review. Participant predictions stay local; aggregate outputs/bootstrap are packaged. An inherited unused percent-comparator code branch is not invoked.

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
1. Platelets promote eosinophil adhesion of patients with asthma to endothelium under flow conditions. Primary study. [PubMed](https://pubmed.ncbi.nlm.nih.gov/12654641/).
2. Benton et al. Airway platelet activation is associated with airway eosinophilic inflammation in asthma (2010). [PubMed](https://pubmed.ncbi.nlm.nih.gov/20930644/).
3. [NCHS data-user agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html).
4. Dweik et al. Official ATS guideline: interpretation of exhaled nitric oxide levels for clinical applications (2011). [DOI](https://doi.org/10.1164/rccm.9120-11st).
5. [CDC ENX 2009–2010](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/ENX_F.htm).
6. [CDC CBC 2007–2008](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/CBC_E.htm).
All component documentation and canonical input URLs are in provenance.
