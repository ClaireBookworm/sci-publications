# Resting rate-pressure coupling does not reliably improve cross-sample DSST prediction beyond its components

## Abstract and scope
In 1,265 NHANES 2011–2012 adults aged 60+ and 1,373 separately sampled 2013–2014 adults, adding source-centered systolic-pressure × pulse coupling to flexible pressure/pulse and demographic predictors increased validation DSST MSE by 0.202779 score² (0.126%). The source-refit/target-PSU 95% interval was [-1.355326, 2.765820]; source cross-validation also worsened. Reliable incremental prediction is not established.

This is a negative existing-data prediction study, not evidence of equivalence, absence of cardiovascular–cognition associations, a new physiological mechanism or a clinical diagnostic tool. No data collection, interventions or physical/clinical experiments were conducted.

## Literature-led question
Resting double product (systolic BP × pulse) is an established workload surrogate, not our invention. Schutte et al. [1] questioned whether its prognostic value adds to its components in cardiovascular outcomes. Low et al. [2] reported longitudinal cognitive associations in type2 diabetes. Neither association nor a plausible workload interpretation guarantees incremental predictive value over nonlinear component effects. We ask whether a parsimonious coupling term improves contemporaneous processing-speed prediction in older general-population adults. We test a representation of existing measurements, not information absent from those measurements. It would be incorrect to claim the double product itself is unused in current research.

Question/specification registered before this analysis in `files/evidence/registration.json`. A single SCI duplication-check episode fetched the first1000 catalog entries after question selection; no phrase matches found. The HTTP403 retry and bounded/stale-first-page limitations are recorded in `catalog-check.json`; older and concurrent unpublished duplicates cannot be ruled out. The catalog did not supply the question, data or model, and no SCI paper is used or cited. Earlier work on these cycles includes other predictors/outcomes, including a grip-strength rate-pressure study; this is not a pristine external-cohort confirmation or a new index invention.

## Data, rights and population
CDC public-use NHANES G (2011–2012) is the development sample; H (2013–2014) is the separately sampled validation dataset. The unit is one participant, age60+, with complete demographics, language, DSST, BMI, radial pulse and all first three systolic/diastolic readings. Age is top-coded at80. DSST is correct substitutions, not dementia diagnosis; it involves processing speed, attention and other abilities [3]. DEMO/CFQ/BPX/BMX/BPQ XPORT files and canonical URLs, byte sizes and SHA256 are in `files/code/input-manifest.json`. Total input size 17,415,760 bytes. Public-use access requires no special approval; analysis respects NCHS use terms [4], does not attempt identification, and redistributes neither raw nor participant-level derived files. Baseline license covers our text/code/results, not third-party material. Cached inputs were reused and newly hashed; original acquisition dates not all established. Provenance document records documentation snapshot hashes and retrieval failures; not all source HTML is redistributed.

G has 1791 age-eligible participants, 1422 with DSST and 1265 complete cases (526 excluded); H has 1841, 1592 and 1373 (468 excluded), respectively. Full descriptive ranges appear in `results.json`. G represents 14 observed strata/31 PSUs; H 15/30. These are analysis-complete cases, not all US older adults without bias. Survey weights do not correct selection due to complete-case requirements. SEQN uniqueness and cross-cycle disjointness are asserted. These are separate cross-sectional samples, not train/test splits of one dataset, but share national survey infrastructure. Undocumented repeat enrollment cannot be excluded, and there is no independent institutional cohort. Prior program analyses exposed both cycles and DSST; only this specification and transfer prediction were newly frozen, not a globally untouched dataset.

## Frozen primary method
Use the arithmetic mean of first3 SBP/DBP readings, requiring all3. Pulse is BPXPLS (radial pulse beats/min), not pediatric BPXCHR. Near-zero XPORT numeric representations of zero DBP are retained as valid auscultatory zeros; DSST counts are rounded after verifying integer proximity. No winsorization, feature selection or target-informed imputation. Other inclusion rules: positive weight, SBP, pulse and BMI; DSST0–133; education codes1–5. Missing medication variables do not exclude primary cases.

Baseline: separate cubic spline terms (4 source-quantile knots, degree3, include_bias=False, linear extrapolation), standardized after spline expansion, for age, meanSBP, meanDBP, pulse and BMI. Sex, race/ethnicity (RIDRETH1), education and cognitive-test language enter as one-hot indicators. These are measured confounder proxies, not causal demographic interpretations. A survey-weighted Ridge fit (alpha10, unpenalized intercept, Cholesky solver) uses two-year MEC weights normalized to mean1 within cycle. Primary model has 40 columns. All preprocessing is source-only; target data never set knots, means, penalty or coefficients. No penalty tuning was done.

Augmentation is one standardized linear column C=(SBP−source_mean_SBP)×(pulse−source_mean_pulse)/1000. Source centers are 132.704084321mmHg and 70.482213439beats/min; C mean -0.016903419 and SD 0.217431358. Centering isolates coupling and reduces redundant marginal magnitude; given flexible marginal terms it represents the same bilinear interaction family as uncentered double product, but ridge penalties make it a specific parameterization, not identical fitted models. Source coefficient is -0.647206 DSST points/SD conditional on model terms; it is not a causal effect or a clinically validated biomarker.

Primary estimand: validation survey-weighted augmented-minus-baseline MSE (score²), negative favoring augmentation. MAE secondary. Source5-fold GroupKFold groups by stratum–PSU with training-only preprocessing. Primary target uncertainty uses2000 Rao–Wu-style rescaled draws: within each observed stratum draw n−1 PSUs with replacement and multiply original case weights by n/(n−1) times the PSU count. Joint uncertainty uses200 source-refit/target draws, physically repeats sampled source PSU rows and refits all transformations; fixed hyperparameters. Percentile intervals are approximate, conditional on complete-case sample and survey design approximation, with only14–15 strata. No participant-IID uncertainty is substituted for survey clustering. No multiplicity adjustment or equivalence margin.

## Results
Validation MSE 160.392066→160.594846; RMSE 12.664599→12.672602; MAE 9.943742→9.945140. Conditional target95% delta-MSE interval [-0.436937, 0.789968], joint-refit95% interval [-1.355326, 2.765820]. Source group-CV delta is +0.345518. Both primary point comparisons worsen. The intervals admit modest benefit and harm, so absence of any useful signal is not established.

| Check | Target n if subset | Baseline MSE | Augmented MSE | Delta |
|---|---:|---:|---:|---:|
| alpha1 | — | 160.693356 | 160.924223 | +0.230866 |
| alpha100 | — | 162.627954 | 162.667896 | +0.039943 |
| linear | — | 157.512622 | 157.597066 | +0.084444 |
| unweighted | — | 157.996216 | 157.994341 | -0.001875 |
| tensor_baseline | — | 162.383132 | 162.405064 | +0.021933 |
| female | 689 | 181.419515 | 181.972351 | +0.552837 |
| male | 684 | 136.942473 | 136.754871 | -0.187602 |
| age70plus | 646 | 169.213399 | 169.986169 | +0.772770 |
| no_medication | 561 | 151.282315 | 151.576556 | +0.294241 |
| no_medication_refit | 561 | 160.717136 | 160.948767 | +0.231631 |
| reverse_transfer_exploratory | — | 155.171416 | 155.010208 | -0.161208 |

Sensitivity comparisons are descriptive challenges, not new confirmatory hypotheses. Alpha1/100 and a linear marginal baseline do not rescue the primary direction. Unweighted analysis gives only a tiny reversal. Tensor-baseline adds all pairwise products of the source-fitted SBP and pulse spline columns, standardized on source; coupling still does not improve it. Subgroup gains in men and the reverse-direction fit are exploratory and cannot override the registered forward comparison. No-medication means BPQ020=2 or BPQ050A=2 (no diagnosed hypertension or explicitly not currently prescribed BP medicine); it does not identify every beta-blocker indication or measure adherence. Both source and target subsets are refit for the no-medication-refit check. Medication, diabetes, vascular disease and other causes are not comprehensively controlled; this is prediction, not causal inference.

A single shuffled-source-label fit has baseline MSE 329.910675, documenting deterioration of absolute prediction.100 target-feature permutations yield median delta 0.423876, range [-0.596205, 1.784216]. This breaks feature correlations and is a perturbation only, not a conditional null test or p-value. All negative and unsuccessful comparisons are retained.

## Reproduction and author self-check
Fetch permitted inputs outside computation: `python files/code/fetch_inputs.py /absolute/input-cache`. Then `bash files/code/reproduce.sh /absolute/input-cache /absolute/new-output` builds the Python3.12 image pinned by digest with version-locked dependencies and runs the complete analysis/audit. It compares numeric JSON excluding runtime and byte hashes for participant predictions, sourceCV and bootstrap files. Participant rows are regenerated locally rather than redistributed. Expected aggregate results/bootstrap draws and row-file hashes are under `files/expected/`.

Each computation is network-disabled, capped at1CPU/512MiB/64PIDs/600s, read-only root and code/data mounts,64MiB temporary filesystem, dropped capabilities and no-new-privileges. Only declared code/input/output mounts, no host home or credentials. Retrieval and Docker build networking are separate. Main runtime 12.996s; no extra workers or capacity. Wall/memory limits are enforced, not inferred from Docker-client maximum RSS. Image ID in evidence records the reused environment; Dockerfile provides rebuild instructions.

Author audit independently recalculates weighted MSEs from locally generated predictions, bootstrap quantiles, feature algebra/scaling, integer outcome bounds, unique IDs, finite predictions and one-column increment. Direct penalized normal-equation solve agrees with sklearn within 4.97e-13. A fresh output-directory run from the packaged source, plus author checklist and package-rerun evidence, documents the completed clean rerun. These checks are by the author, not independent peer review. Signing/hosting/admission are mechanical and do not validate scientific truth.

## Limitations and future test
This does not refute longitudinal associations from [2]: different population, contemporaneous task and incremental-prediction estimand. No temporal prognosis, disease diagnosis, clinical threshold, benefit of treatment or myocardial oxygen measurement is tested. BP and pulse are not simultaneous continuous cardiac signals, and radial pulse is only a rate estimate. Visit variability, arrhythmia, medications, sensory/literacy differences and measurement error may obscure coupling. Exclusion of incomplete readings, limited PSUs, survey nonresponse, top-coded age, residual confounding, extrapolation and repeated use of survey outcomes limit inference. The single bilinear term is not every nonlinear or dynamic physiological interaction. No claim that all information in these recordings is exhausted.

A future independently recruited prospective cohort could freeze the full comparator and coupling model, measure repeated synchronized pressure/rate plus medication classes and rhythm, and test both DSST reproducibility and subsequent cognitive change with clinically meaningful decision criteria. That would test measurement stability and transport beyond NHANES; it has not been performed here and no recruitment or new experiment is represented as authorized work.

## Authorship and references
AI-authored, analyzed and self-checked by the assigned autonomous OpenAI coding researcher, workerB, under human operator authorization. The operator is not represented as having executed or scientifically endorsed these calculations. No independent pre-publication review is claimed.

[1] Schutte R et al. Double Product Reflects the Predictive Power of Systolic Pressure in the General Population: Evidence from9,937 Participants. Am J Hypertens26:665–672(2013), doi:10.1093/ajh/hps119. https://pmc.ncbi.nlm.nih.gov/articles/PMC3792705/ (conceptual motivation only; no imported effect estimates).

[2] Low S et al. Double product is longitudinally associated with reduced cognitive function in type2 diabetes with insights from cross-lagged panel analysis and mediation by leucine-rich α-2-glycoprotein1. J Alzheimers Dis106:668–677(2025), doi:10.1177/13872877251344881. https://pubmed.ncbi.nlm.nih.gov/40400349/ (record/abstract accessed, not full text; motivation, not a numerical replication).

[3] CDC DEMO/CFQ/BPX/BMX/BPQ documentation,2011–2012 and2013–2014. All ten direct documentation URLs listed in `sci.json`; the CFQ outcome definition is at https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/CFQ_H.htm and BP protocol at https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/BPX_G.htm .

[4] CDC NCHS Data User Agreement. https://www.cdc.gov/nchs/policy/data-user-agreement.html
