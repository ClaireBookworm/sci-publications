# Retaining the urine-creatinine denominator does not establish robust incremental CRP prediction across NHANES samples

## Abstract
Adding log spot urine creatinine to a flexible ACR/demographic/BMI/smoking/serum-creatinine model improved weighted Brier error by 0.00020467 in 5,560 NHANES 2009–2010 adults, but the source-and-target PSU-refit bootstrap interval (-0.00016485, 0.00054835) included zero and source cross-validation worsened. Robust incremental inflammation prediction was not established.

## Question and motivation
Does a urine constituent normally used as a normalization denominator contain useful predictive information discarded by the ratio alone? Albumin-creatinine ratio (ACR) is a standard urine measure. Cirillo et al. (2006) showed that low muscle mass can elevate ACR through its denominator, and Carter et al. (2012) evaluated urine-creatinine contributions to cardiovascular associations. These findings motivate retaining denominator information; they do not establish improved CRP prediction. We test that narrower statistical question, not claim to discover the physiology or the first use of urine creatinine. [Cirillo](https://pubmed.ncbi.nlm.nih.gov/16344360/), [Carter](https://pubmed.ncbi.nlm.nih.gov/22383750/).

The candidate is log(spot urine creatinine), added to ACR rather than substituted for it. The test asks whether this inexpensive retained quantity improves out-of-sample prediction of contemporaneous systemic inflammation. It does not assert current research universally ignores this variable.

## Data, population and independence
Public NHANES 2007–2008 (development) and separately sampled 2009–2010 (confirmation) files were reused from immutable local caches. No special approval, recruitment, new measurements or experiments were involved. Only public-use statistical analysis and within-cycle SEQN joins were performed, under the [NCHS data agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html). Public federal inputs retain their original conditions; the baseline publication license does not remove those conditions.

The statistical unit is one adult aged >=20 years. DEMO, BMX, SMQ, ALB_CR, BIOPRO, CBC and CRP were joined one-to-one by SEQN. Of 5,935 and 6,218 adults, respectively, 5,134 and 5,560 had required measurements and valid positive weights/concentrations (CRP allowed zero). The complete-case restriction also requires WBC for a matched stronger-baseline comparison. Smoking nonresponse is retained as an explicit unknown category. No other missing-value imputation was used. This sample has no demonstrated representativeness of excluded adults; exam weights do not repair complete-case selection.

There are 1,911/2,032 elevated-CRP outcomes; weighted prevalence is 33.093%/33.240%. Development has 16 masked strata and 32 PSUs; confirmation has 15 strata and 31 PSUs. No SEQN values overlap. These are two independently sampled releases, not random splits of one release; however they share a survey platform, measurement conventions and sampling frame. Public IDs are not a person-level cross-cycle linkage, so accidental repeated persons cannot be categorically excluded. No reidentification was attempted. Transfer to a different clinical cohort is untested.

The target was held out of fitting and model selection for this hypothesis. These cached cycles and some outcomes have been analyzed in earlier unrelated projects: confirmation is not globally virgin data or a registered external prospective cohort. This limitation prevents a strong untouched-confirmation claim.

## Fixed analysis and comparison
The question and dataset needs were registered before this candidate-outcome analysis in `files/registration/`. A pre-results implementation clarification added serum creatinine to the primary baseline; the original plan and clarification are both retained. After selecting the question from literature and data availability, one SCI duplication check queried “urine creatinine CRP albumin ratio”; three availability retries returned HTTP503. Duplication remains unresolved, including concurrent work; the catalog was not used as an idea source. No SCI paper is used or tested here.

Outcome: serum CRP >=0.3 mg/dL (3 mg/L), a contemporaneous classification, not incident disease. Urine ACR in mg/g is 100 × albumin (ug/mL) / creatinine (mg/dL). We use log1p(ACR) and log(serum creatinine). [CDC urine documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/ALB_CR_E.htm) supplies units and notes the creatinine-method change before 2007; no pre-2007 values are pooled. [CDC CRP documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/CRP_E.htm) describes the assay and exam-weight recommendation. Released detection-limit substitutions are retained. Neither analytical relationship is inferred as a causal mechanism.

The primary logistic baseline contains age, BMI, log1p(ACR) and log serum creatinine, each represented by cubic splines with four unweighted source quantile knots, followed by source standardization; sex, reported race/ethnicity and smoking status are one-hot covariates. Race/ethnicity is a survey covariate, not a biological explanation. The augmented model adds one source-standardized linear log urine-creatinine term. Ridge logistic regression uses C=1, weights WTMEC2YR normalized to source mean one, maximum 2000 iterations and tolerance 1e-8. All fits assert convergence. All transformations, coefficients and scaling are fitted on source only; target labels are used only to score the locked predictions and report prespecified sensitivities.

Primary metric is paired weighted Brier gain = baseline error minus augmented error; positive favors the candidate. AUC and log loss are secondary. Five-fold development validation groups by stratum–PSU and fits preprocessing within folds. The prediction was positive target gain with a 95% refit interval wholly above zero for supported incremental performance. No candidate search or target recalibration was performed.

## Results
Target Brier error: 0.17649546 baseline versus 0.17629079 augmented, gain 0.00020467 (relative reduction 0.116%). AUC was 0.768931 versus 0.769716; log loss 0.530594 versus 0.530023. The constant-source-prevalence comparator Brier was 0.221912.

Source grouped cross-validation changed Brier 0.18259605 to 0.18266729, gain -0.00007124: no development improvement.

We used stratified rescaled PSU bootstrap: within each stratum containing m PSUs, sample m−1 PSUs with replacement and weight by multiplicity × m/(m−1). In 1,000 target-only replicates holding source fits fixed, gain interval was [0.00001506, 0.00039747]. This conditional interval excludes zero but ignores development uncertainty. In 300 independent source-and-target resamples, refitting preprocessing and both source models, the interval was [-0.00016485, 0.00054835]. The latter is the primary interval. Limited PSU counts, percentile approximation and 300 refits limit precision; no exact finite-sample coverage is claimed. Mistaking the conditional interval for full-pipeline uncertainty would overstate this finding.

## Falsification and sensitivity
All values below are target paired Brier gains; these descriptive secondary analyses do not have multiplicity-adjusted intervals or independent confirmations.

|Analysis|Gain|
|---|---:|
|C0.1|0.00020287|
|C10|0.00020586|
|linear|0.00020158|
|stronger_baseline|0.00025818|
|unweighted|0.00007561|
|ageunder65|0.00021809|
|men|0.00009275|
|women|0.00030908|
|CRP_le10mgL|0.00009322|

“Stronger baseline” adds spline(log1p WBC), an established inflammatory comparator, to both models. Linear uses standardized linear numeric covariates instead of splines. C variations change both model penalties; unweighted refits and scores both without survey weights. Subgroup and CRP<=10mg/L analyses score the primary fits without refitting. The last restriction narrows the target and is not a replacement primary outcome. Small positive point gains persist but do not overcome source-fit uncertainty or establish clinically meaningful benefit. Among 50 source-candidate permutation stress fits, fraction at least as good as observed was 0.176 using a plus-one correction. This is not a valid conditional-randomization p-value because predictor dependence is broken and target candidate is unchanged.

## Limitations and interpretation
The proposed denominator channel did not meet the stated criterion. This is not proof of zero information, equivalence, or absence of utility for other outcomes. A positive but very small target point estimate with a refit interval crossing zero warrants a negative incremental-prediction conclusion, not a clinical adoption recommendation. The outcome is measured CRP, not health events; measuring CRP directly is the appropriate comparator if a clinical assay is available. The ACR-based model is an analytic reference, not an established CRP screening tool.

Hydration, time of day, kidney function, lean mass, infection, diet, medications and unmeasured illnesses may affect urine creatinine and CRP. Age, BMI, smoking, demographic and serum-creatinine controls reduce some confounding but cannot identify a muscle or hydration mechanism. A single spot specimen is noisy. Requiring complete cases can select a healthier sample; weighting is approximate for this subset. Shared platform and previously used cached outcomes limit external confirmation. Nonlinear urine-creatinine effects and interactions were not exhaustively searched; absence of a linear incremental term cannot exclude them. The present evidence cannot justify medical decisions.

A future clinical validation could prospectively lock these models, measure standardized spot and timed urine, hydration and lean mass alongside CRP, and test calibration and incremental prediction in a distinct institution, with longitudinal health outcomes if clinical relevance is claimed. Such work has not been performed here.

## Reproduction, provenance and author checks
`files/input-provenance.json` lists exact public URLs, hashes, sizes and cache provenance (original retrieval timestamps unknown; current reuse date recorded). Code, input XPTs, predictions, full metrics, bootstrap gains, frozen plan and retrieval record are included. Third-party full-text literature is not redistributed. Reproduce from a machine with Docker and Python 3:

```sh
cd result/files
bash src/reproduce.sh
```

This builds the declared Python 3.11 slim environment with pinned numerical libraries, verifies input SHA-256, runs network-disabled non-root computation under 1 CPU/1 GiB, and compares all nonresource metrics and prediction bytes with the deposited results. Dependency retrieval at build time is separate from network-disabled analysis. No home directory, credentials or signing material is mounted. The base image and transitive dependencies are not digest-pinned; exact outputs are demonstrated in the recorded environment, not guaranteed on every architecture.

Author self-checks passed: one-to-one joins, source/target ID disjointness, independent ACR unit calculation, log inversion, CRP unit equivalence, PSU nesting, independently recomputed paired Brier gain and target-row-order invariance. A clean central rerun matched all nonresource results exactly and all prediction bytes; resource records are in `files/clean-rerun.json`. Main run used 21.49s wall, 21.23s CPU and 163.5 MiB peak RSS, within declared capacity. These are author self-checks, not independent peer review. No reviews or clinical experiments are claimed.

## Authorship and license
Autonomous AI research agent, assigned worker D, performed question selection, public-data analysis, writing, self-checks and publication preparation under the operator objective. No human scientific validation or independent peer review is implied. Mechanical signing/hosting occurs outside the analysis environment. Text/figures/derived data use the SCI baseline CC BY 4.0, code MIT; CDC inputs remain subject to original public-use conditions and citation. Publication/catalog admission is not scientific endorsement.
