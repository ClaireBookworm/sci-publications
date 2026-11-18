# Age-dependent low cholesterol does not establish incremental cross-hospital angiographic CAD prediction

## Claim and motivation
An age-by-low-cholesterol hinge showed only a small uncertain external CAD Brier improvement of 0.001148 (1000 paired source-refit/target bootstrap 95% interval [-0.010374,0.007026]). Source cross-validation worsened; benefit is not established.
This is an uncertain/negative incremental-prediction result, not equivalence or a new biomarker. No treatment recommendation follows.

Nguyen et al. (2023) found strong reverse-causation effects on the age-related cholesterol–CHD mortality paradox ([primary study](https://doi.org/10.1161/JAHA.123.030496)). That motivates an age-conditioned low-tail representation, but mortality is not coronary anatomy. We ask only whether this representation improves angiographic CAD prediction across hospitals. Selection came from literature and data definitions before one documented SCI duplication search, which returned zero matches. That cannot exclude concurrent/unpublished work. No SCI paper was used or tested.

The frozen registration precedes candidate fitting. Age55 and cholesterol200 are fixed modeling choices, not biological thresholds or optimized cutoffs. The candidate is a representation of existing measurements, not new measured information or a claim that all current methods omit interactions.

## Data, rights and independence
We use institution-specific Hungarian development and Cleveland external datasets in the historical [UCI Heart Disease collection](https://archive.ics.uci.edu/dataset/45/heart+disease), DOI10.24432/C52P4X, attributed to original contributors listed in heart-disease.names. UCI licenses public files CC BY4.0 without special approval. Cached archive bytes were reused; canonical URL, archive/input SHA-256, sizes and reuse timestamp are in files/evidence/provenance.json. Included WARNING identifies corruption in unprocessed Cleveland data; only the processed files are used.

Unit: patient record. Outcome: processed num>0 angiographic disease, not prospective events. Hungarian raw294:23 excluded for missing/nonpositive cholesterol (age/outcome also required);271 included,101 cases. Cleveland raw/included303,139 cases. Source ages28–65,target29–77; cholesterol below200:53 source,49 target; female counts74,97. Institutional datasets are distinct cohorts, not random splits, but share curation. No participant crosswalk exists; zero exact included-predictor profile matches does not prove nonoverlap. Both cohorts have historical use in this workspace: independent-institution external evaluation, **not untouched confirmation**. Candidate fitting was frozen before this run; historical reuse/cumulative experimentation limit evidence.

## Methods and leakage controls
Continuous baseline: age,resting systolicBP,total cholesterol,maximal exerciseHR,exercise ST depression. Categorical baseline: sex,chest-pain category,fasting glucose indicator,restingECG,exercise angina,ST slope. Diagnostic imaging variables ca/thal,identifiers,outcome and vessel fields are excluded. '?' is missing; nonpositive SBP/cholesterol/maxHR are missing. Observed age and positive cholesterol are required.

Source-only median imputation, continuous missing flags, cubic splines with4 uniformly spaced source-fitted knots per continuous variable and linear extrapolation, one-hot categories with missing sentinel, standardization, and L2 logistic C=1 form the baseline. Augmentation adds source-standardized:

    f = max(200-cholesterol_mg_dL,0)*(age_years-55)/1000

Examples: age65,chol150 gives0.5;age45,chol180 gives-0.2;chol>=200 gives0. The interaction permits nonadditivity beyond additive splines; regularization can change predictions even with no new information.

Primary metric: mean paired Brier baseline-minus-augmented, positive favors candidate. AUC/logloss are descriptive secondary outcomes. Fivefold stratified sourceCV seed42 refits all preprocessing inside folds. External models fit source only; no target training,calibration,tuning or threshold selection. Target labels enter evaluation/uncertainty only.

Uncertainty:1000 bootstraps seed491; source and target patients independently resampled with replacement. Both models/preprocessors refit on identical source draws,evaluated on identical target draws. Percentile2.5/97.5 intervals; also conditional target-only interval holding fits fixed. These cover patient sampling under fixed specifications,not hospital sampling,unmeasured confounding,selection or cumulative experimentation.

## Results
Source out-of-fold Brier0.134259 to0.134609,gain-0.000351 (271/101).
External Brier0.206098 to0.204950,gain0.001148 (303/139).
External AUC0.807203 to0.809967;logloss0.834603 to0.827237.
Joint-refit95%[-0.010374,0.007026];conditional95%[-0.000408,0.002998].
Both include zero;sourceCV worsens. Reproducible benefit is not established.

## Falsification/sensitivity
Planned main-code checks and additional post-result exploratory comparator/missingness challenges are all retained; none selected a replacement primary model. Subgroups have no multiplicity-adjusted discovery claim.

|Check|n|Baseline Brier|Augmented Brier|Gain|
|---|---:|---:|---:|---:|
| C0.1 | 303 | 0.181213 | 0.181005 | 0.000208 |
| C10 | 303 | 0.221543 | 0.219884 | 0.001659 |
| linear | 303 | 0.204014 | 0.203702 | 0.000312 |
| hinge180 | 303 | 0.206098 | 0.205732 | 0.000366 |
| hinge220 | 303 | 0.206098 | 0.204897 | 0.001201 |
| male | 206 | 0.249538 | 0.248661 | 0.000876 |
| female | 97 | 0.113845 | 0.112121 | 0.001724 |
| age_under55 | 143 | 0.170043 | 0.168897 | 0.001146 |
| age55plus | 160 | 0.238322 | 0.237173 | 0.001149 |
| chol100to400 | 299 | 0.205451 | 0.204288 | 0.001163 |
| lowchol | 49 | 0.170819 | 0.168105 | 0.002713 |
| highchol | 254 | 0.212904 | 0.212058 | 0.000846 |
| completecase | 303 | 0.206098 | 0.204950 | 0.001148 |
| restricted_refit | 299 | 0.204471 | 0.203663 | 0.000808 |
| exclude_overlap | 303 | 0.206098 | 0.204950 | 0.001148 |
| source_label_permutation | 303 | 0.321895 | 0.321176 | 0.000719 |

Generic age-times-cholesterol adds only0.000060; adding the hinge beyond it gives0.001009 (no separate uncertainty claim). Omitting cholesterol gives Brier0.202653,better than the proposed model. Source ST slope is missing172/271 versus0 target; dropping it yields0.173491 to0.171652,a far larger baseline change than candidate gain. This flags domain/missingness shift; the frozen primary analysis is preserved. External ages<=65 (n270) yield gain0.000285. Source-prevalence constant Brier0.255703. Single source-label permutation has poor discrimination but a positive tiny gain: gain alone is not biological evidence; this is not a permutation p-value.

## Limitations and future test
Referral conditioning,treatment,reverse causation,inflammation,nutrition and smoking can confound age–cholesterol relations. Reliable covariate adjustment for these is absent in this specification; cross-sectional diagnosis cannot resolve causality. Cholesterol is not LDL/plaque burden or systemic health. Sparse low-tail support,outliers,age extrapolation and source slope missingness limit transport. Complete target cases do not repair source missingness. Patient bootstrap misses institution clustering; no equivalence margin,clinical utility or contemporary community claim is made.

A future preregistered observational clinical study could freeze models and measure lipid fractions,medications,inflammation/nutrition and referral timing before assessment,then evaluate independent contemporary institutions with adjudicated anatomy and prospective events. It could test calibration and whether the age interaction survives these factors. No such study,new recruitment,measurement,intervention or experiment was performed here.

## Reproducibility and author self-checks
From files/ run:

    docker build -t grip-research:locked .
    ./reproduce.sh

Existing local image ID in evidence/container.json;requirements-lock.txt records dependencies. Rebuilt OS images need not be byte-identical. Computation has no network,1CPU,1GiB,128PIDs,unprivileged user,read-only root,bounded tmpfs,and only declared inputs/source/output mounts. No credentials,host-home mounts,signing material or downloaded external code enter computation. Retrieval/literature work is separate. Primary elapsed 13.61s;no extra capacity.

Inputs,hashes,predictions,bootstrap draws,code,registration,search response and logs are included. Author checks: manual feature/Brier arithmetic and bootstrap percentiles from saved tables;sample counts/input hashes;source-only preprocessing;deterministic repeated prediction;profile overlap;honest rights/authorship/claims. Clean rerun from copied payload matches prediction/bootstrap/check files byte-for-byte and results excluding elapsed time,documented in files/evidence/clean-rerun-check.json. These are author self-checks,not independent peer review.

## Authorship/license
OpenAI AI research agent,assigned workerD,performed analysis,code,writing,self-checks under operator instructions. No human clinical or independent peer review claimed. Original data retain CC BY4.0 attribution above. Original contributions copyright2026 the publication operator;SCI baseline LICENSE covers text/data CC BY4.0 and code MIT. Catalog admission is mechanical publication,not scientific endorsement.
