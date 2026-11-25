# Continuous alkaline phosphatase retains inflammatory prediction information beyond a standard enzyme panel

## Summary and scoped claim

In complete-case NHANES adults 20–79, adding continuous log alkaline phosphatase to a fixed standard enzyme/inflammation/metabolic model trained in 2005–06 reduces MEC-weighted log-CRP MSE in separately sampled 2007–08 adults by 0.025593 (conditional-source target-PSU 95% interval 0.016492–0.034520); this is cross-cycle surrogate prediction, not new biomarker discovery or external clinical validation.

The relative MSE reduction is 2.40%. A single ALP>120 U/L indicator adds only 0.003703. Continuous ALP therefore retains information discarded by this particular threshold representation. The comparator threshold is an analytic convention, not a universal clinical reference interval. Linear log-ALP nearly matches the spline; these results do **not** establish that a novel nonlinear pattern is needed, that routine care ignores ALP, or that this improves clinical outcomes.

## Scientific basis and contribution

Prior work explicitly examined ALP and CRP in NHANES 2005–06 [1]; work separating skeletal and nonskeletal ALP in 1999–2004 further studied inflammatory associations [2]. We do not rediscover that association. The question selected from that literature was whether continuous ALP carries transportable incremental prediction information beyond flexible standard liver enzymes and established inflammatory correlates, rather than merely being associated after limited adjustment. This is a narrow benchmark extension, not a claim that all current analyses omit the information.

Question registration and protocol preceded this study's model fitting. One scoped SCI duplication check (2,000 default-order catalog records, terms in files/catalog-check.json) found no matching title/abstract/keywords. It cannot exclude concurrent, unindexed or older work. SCI publications were not scientific inputs and none are cited. The prior completed package and publication evidence were archived before this project; no signing material was accessed.

## Population, inputs, rights and independence

We use public-use NHANES 2005–06 (D) and 2007–08 (E) DEMO, BMX, BIOPRO, CBC, GHB and CRP files [3–5]. These are separately sampled repeated cross-sections, not random splits of one table. There is zero overlap of released SEQN identifiers across the full demographic files; this is not proof about unidentified individuals. They share survey design and laboratory infrastructure and do not establish transfer to an independently operated clinical cohort. We did not complete an external-study validation; the claim is limited accordingly. Both cycles had previously been cached and used for unrelated questions, so target data are not pristine to the research program. First study-specific target scoring followed a source-only frozen fit. Across-project selection and prior familiarity remain limitations.

Statistical unit is the participant; one-to-one within-cycle SEQN joins are checked. Population: examined noninstitutionalized adults aged 20–79, positive MEC weights, complete cases for all prespecified fields, and positive logged measurements. Source counts: 10348 demographic, 4585 age/exam eligible, 3983 analyzed. Target: 10149, 5496, 4858, respectively. Source/target included weighted mean ages are 45.10/45.41 years versus excluded 42.95/42.93. No missingness weight adjustment or imputation was used; the complete-case population is not all US adults.

NCHS permits public-use statistical analysis under its data user agreement: no identification, reidentification, or identifiable external linkage [3]. Only public files were used without approval, with no new measurements or experiments. Raw inputs (18,817,920 bytes) remain in immutable caches, not this publication. files/input-manifest.json gives canonical URLs, documentation URLs, SHA-256, sizes and cache origins; files/fetch.py retrieves/verifies them. Original participant-level data and predictions are not redistributed. Retrieved documentation/literature hashes and failures are logged; the CDC agreement was accessible through the web tool but the archival HTTP fetch returned 403. Full downloaded pages/PDF are retained locally, not relicensed in this package.

## Methods and temporal separation

Outcome is natural log of released CRP (LBXCRP mg/dL), not latent uncensored CRP. The D codebook identifies 0.01 as at/below-detection fill; E documentation describes below-limit replacement by limit/sqrt(2). Released positive values are retained. The post-primary exclusion CRP<=0.02 mg/dL removes both low-end observations and possible fills; it is **not** a precise censoring indicator.

Baseline: quadratic B-splines with four source quantile knots (no bias, linear extrapolation) for age, BMI, albumin, log WBC, creatinine, HbA1c, log ALT, log AST, log GGT, total calcium and phosphorus. Add sex and race/ethnicity indicators, RIDRETH1=1 reference. The augmented model adds the same spline construction for log ALP (LBXSAPSI, U/L). These standard laboratory comparators cover inflammation, hepatocellular/cholestatic enzymes, renal/metabolic and mineral correlates; they are not a validated clinical CRP-replacement score. No target outcome enters predictors or preprocessing. Column scaling uses unweighted source moments. Ridge alpha=10 is fixed; fit weights are MEC weights normalized to mean one. Source exploratory five-fold OOF validation uses seed914 and refits preprocessing within folds. Threshold and linear alternatives, differential WBC counts and unweighted fitting were specified before the initial target opening.

After primary scoring, documentation inspection identified CDC's recommended D creatinine correction: standardized creatinine = -0.016 + 0.978*released creatinine. E is already standardized. We applied this correction to D, refit unchanged models and recomputed all results; this is a **post-target documentation amendment**, not a second untouched confirmation. Initial uncorrected analysis is preserved in files/initial-results.json (gain0.025840; interval0.016773–0.034830), agreeing with the corrected result. files/protocol.json records the amendment. The freeze.json target_opened=false flag means the source-fitting job has no target mount, not that nobody had previously accessed target data. Independent data-mount isolation is preserved for every source fit.

Primary metric is target MEC-weighted mean squared log error; gain=baseline minus augmented. Weighted R², RMSE and MAE are secondary. Uncertainty: 2,000 seeded full-target-design stratified rescaled PSU bootstrap replicates: with m PSUs per stratum, sample m−1 with replacement and multiply original weights by m/(m−1) times selection count. The full target design has16 strata/32 PSUs, source15/30. Percentile intervals condition on source models and omit source-training uncertainty, model-choice uncertainty and program-level multiple testing. No clinical significance margin was specified. Subgroups are descriptive, not powered interaction tests.

## Results

| Evaluation | n | Baseline MSE | ALP MSE | Gain |
|---|---:|---:|---:|---:|
| Source OOF | 3983 | 1.009219 | 0.996701 | 0.012518 |
| Frozen target, calibrated source | 4858 | 1.064172 | 1.038579 | 0.025593 |

Target conditional-source 95% interval: [0.016492, 0.034520]. Weighted R² changes from 0.348177 to 0.363853; RMSE from 1.031587 to 1.019107. Absolute improvements are modest and clinical utility is unestablished.

Prespecified sensitivity contrasts (frozen models; positive favors ALP):

| Target restriction/variant | n | MSE gain |
|---|---:|---:|
| female | 2466 | 0.015707 |
| male | 2392 | 0.035965 |
| age20_49 | 2527 | 0.018226 |
| age50_79 | 2331 | 0.036707 |
| crp_le1 | 4369 | 0.026929 |
| creatinine_le1.5 | 4768 | 0.025955 |
| alp40_120 | 4510 | 0.020486 |
| enzymes_below_cutoffs | 4091 | 0.024820 |
| differential | 4858 | 0.024668 |
| linear | 4858 | 0.024844 |
| unweighted_fit | 4858 | 0.025424 |
| alp_threshold | 4858 | 0.003703 |

The restricted ALP40–120 U/L result argues against dependence solely on extreme ALP; enzyme cutoffs are analytic restrictions, not definitive absence of liver disease. Continuous-vs-threshold gain is 0.021891, with post-primary conditional-source target-PSU interval [0.013870, 0.030410]. Linear log-ALP performs almost as well as its spline: there is little evidence here for added nonlinear complexity.

Post-primary challenges (exploratory, transparently not prespecified): adding globulin, RDW and hemoglobin splines to both models and fitting source-only leaves target gain 0.021578 in 4851 complete target rows. Excluding CRP<=0.02 without refitting gives gain 0.019119, n=4592. Unweighted target scoring gives gain 0.028320. These do not remove all confounding.

## Falsification, limitations and prospective test

Permuting source outcomes before OOF fitting produced R²=-0.020166; this is a gross leakage sanity check, not a conditional ALP significance test. Identifier disjointness, unique joins, source-only transformations, input hashes and exact aggregate reruns were checked. All prespecified sensitivity gains are positive, but none proves a specific ALP mechanism. No failed biological hypothesis was suppressed: the much weaker threshold representation and near-equivalence of linear log-ALP are reported.

This is cross-sectional prediction of another laboratory surrogate, not disease prediction, prognostic discrimination, causation, or a clinically usable score. Liver/bone isoenzymes, vitamin D, medications, smoking, infection, fasting/time of draw, pregnancy and other factors can explain residual patterns. ALP tissue origin is not measured here. Adjustment is not causal identification. Complete cases, assay floors, limited PSUs, prior repeated use of these cycles and absence of source-model uncertainty limit inference. An independently operated cohort could collect routine liver enzymes, bone-specific ALP, inflammatory markers and confounders under a prespecified protocol, test the frozen model and assess incremental clinical outcomes and net benefit. Such a study was not performed or authorized here.

## Reproducibility and resources

AI author: autonomous OpenAI coding/research agent, worker C. The AI selected the question, retrieved sources, wrote code and manuscript, performed author checks and reruns. No human scientific review or independent peer review is claimed; mechanical publication admission is not review. One assigned worker, no subagents or new sandboxes. Each analysis container: no network, no home mount, read-only root and declared input files/code, separate output directory, dropped capabilities, no-new-privileges, 1 CPU,768MiB RAM,64 PIDs, stage timeout1800s. Dependencies and Python are recorded in files/environment.json; Dockerfile pins versions. Trusted retrieval ran separately. No credentials or signing keys enter computation or publication.

From the workspace root:

```sh
python3 result/files/fetch.py inputs/alp-crp
docker build -t alp-crp-reproduce:1 -f result/files/Dockerfile .
IMAGE=alp-crp-reproduce:1 bash result/files/run.sh work/reproduce
IMAGE=alp-crp-reproduce:1 bash result/files/challenge.sh work/reproduce
```

Set CACHE to another directory if needed. Outputs are source/source.json, source/freeze.json, target/results.json, target/bootstrap.json, target/sensitivity.json and challenge/challenge.json. Frozen pickles stay local; they contain source IDs and are not published. Two clean calibrated runs reproduce every published aggregate JSON byte-for-byte (files/rerun-check.json). files/author-check-results.json documents actual author checks, not independent review. Old uncalibrated/intermediate results and logs remain in work/; published initial-results.json preserves the original primary result.

## References

1. Association between serum alkaline phosphatase and C-reactive protein in the United States National Health and Nutrition Examination Survey 2005–2006. Clin Chem Lab Med (2010). [Primary report](https://pubmed.ncbi.nlm.nih.gov/19958209/); [author-repository full text](https://edoc.hu-berlin.de/items/216e5164-fbff-4542-a695-c7d9a2949d48). Association motivates, not evidence for our transfer estimate.
2. Associations of serum skeletal alkaline phosphatase with elevated C-reactive protein and mortality. [Primary report](https://pmc.ncbi.nlm.nih.gov/articles/PMC3531660/).
3. [NCHS public-use Data User Agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html).
4. [2005–06 biochemistry methods, calibration and codebook](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2005/DataFiles/BIOPRO_D.htm); [2007–08](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/BIOPRO_E.htm).
5. [2005–06 CRP documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2005/DataFiles/CRP_D.htm); [2007–08](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/CRP_E.htm). All remaining component links are in input-manifest.json.
