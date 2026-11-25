# Serum protein gap modestly improves frozen cross-cycle prediction of log CRP beyond albumin and blood-count correlates

## Central finding and scope

In complete-case NHANES adults aged 20–79, adding serum total protein minus albumin to a frozen source-trained model of log CRP reduced target MEC-weighted MSE from 1.058677 to 1.022838 in the independently sampled 2007–08 cycle (n=4861); gain 0.035839, target-PSU 95% interval 0.021682–0.049857, conditional on the 2005–06 fit. This is modest within-program transfer of a known marker, not clinical validation or novel biology.

Relative MSE reduction is 3.39%; this is not a clinically important difference established by this study. Baseline predictors include age, sex, race/ethnicity, BMI, albumin, platelet count, WBC and absolute neutrophils/lymphocytes. The outcome is concurrent inflammatory physiology, not future disease. The finding holds directionally in a second independently sampled dataset, but both datasets belong to NHANES. We did not validate in an independently operated cohort and do not claim general clinical transportability.

## Literature-led question and disclosure

Juraschek et al. [1] studied gamma-gap associations with mortality in NHANES1999–2004. That motivated testing incremental prediction of a concurrent inflammatory marker, not repeating mortality analyses. The protein gap is already a clinical measure and available in public biochemical data. We ask what is lost by an otherwise rich model omitting this protein dimension, not whether all existing analyses ignore it. There is no basis for claiming a new biomarker or an exhaustive literature novelty search. Total protein and albumin jointly determine the gap; an equally capable model given both could recover this information. Our comparison does not show a gap transformation beats those two raw inputs. Conditional on albumin, the incremental information originates in total protein.

Question/protocol and dataset needs were recorded before fitting: files/protocol.json and files/registration.json. A single subsequent SCI duplication check at 2026-11-25T07:31:16Z searched gamma gap, protein gap and globulin in 2,000 endpoint-default records, with no matches or retries (files/catalog-check.json). This bounded search does not exclude concurrent/unindexed duplication. No SCI paper was used scientifically, so none is cited gratuitously.

## Data, lawful access and independence

We reused immutable NHANES public-use files DEMO, BMX, BIOPRO, CBC and CRP for 2005–06(D, source) and 2007–08(E, transfer). The official component URLs, cache origin, release identity, bytes and SHA-256 hashes are in files/input-manifest.json; files/input-hash-check.json verifies all ten files on this project. Total input size is 18,593,440 bytes. Official documentation identifies total protein LBXSTP and albumin LBXSAL in g/dL [3,4]; GAP=LBXSTP−LBXSAL. The published globulin variable is not independently measured protein electrophoresis and is not used here.

NCHS permits public-use statistical reporting and analysis subject to no identification or individually identifiable linkage [2]. We used only released within-cycle SEQN joins; no identification, restricted data access, recruitment or new measurements. Raw respondent rows, IDs and fitted pickle are not distributed. The SCI baseline LICENSE applies to authored code, text and aggregate outputs, not a replacement license for CDC inputs. Reproduction retrieves inputs under NCHS terms.

These are separately sampled cross-sectional releases, not two random splits of one dataset. Full demographic released identifiers are disjoint (zero overlap). Disjoint release IDs cannot prove biological non-overlap; actual identity cannot be audited and we do not attempt it. Independence is based on the repeated cross-sectional sampling design, with shared population frame, laboratory and survey infrastructure. Cached cycles had already been used for unrelated questions, so this is not pristine never-seen external confirmation. For this question no target files were mounted during source fitting, all transforms/models were frozen before target scoring, and no target-driven hyperparameter selection occurred. Published motivating mortality analyses concern earlier cycles, not independent confirmation of this result.

## Population, statistical unit and selection

Unit: one examined adult aged20–79 with positive MEC weight WTMEC2YR. Source flow: 10348 demographics → 4585 eligible examined → 3988 complete cases. Target: 10149 → 5496 → 4861. We require nonmissing baseline variables, gap, CRP, hemoglobin, RDW, MCV, creatinine, ALT and GGT; the extra labs enforce identical samples for sensitivities. CRP, WBC, platelets and gap must be positive. There is no imputation. Outcome is natural log positive LBXCRP (mg/dL).

Complete-case selection is substantial and may bias inference. Weighted included/excluded ages are 45.10/42.90 in source and 45.43/42.71 in target; sex/PSU summaries are also supplied. These results are not representative estimates for all US adults.

## Models, separation and uncertainty

Continuous baseline terms: age, BMI, albumin, natural log WBC, natural log platelets, absolute neutrophils and lymphocytes. Categorical terms: female and RIDRETH1 categories2–5 versus1. Each continuous term uses quadratic B-splines with four source quantile knots, no bias and linear extrapolation. The augmented model adds the same spline specification for raw gap(g/dL). This is a flexible standard-correlate baseline, not a validated clinical CRP calculator. Identifiers and outcome-derived features never enter predictors.

Source-only StandardScaler and Ridge(alpha=10) use source MEC weights normalized to mean one. No hyperparameter tuning. Marginal splines are fit together but independently per column; unused sensitivity/gap columns are never supplied to the baseline estimator. Source exploratory five-fold KFold(seed914) refits transformations within folds, but does not hold out survey PSUs. Full-source models are then frozen and passed read-only to target scoring. files/freeze.json records SHA-256. Sensitivity models also use source-only fits. The target is not used for calibration or retraining.

Primary metric is target MEC-weighted baseline MSE minus augmented MSE, positive for improvement. Also report weighted RMSE, MAE and R². Source OOF MSE gain=0.040979; it is exploratory, not a second dataset. Target interval uses2,000 stratified rescaled PSU bootstrap replicates(seed29382), sampling m−1 of m PSUs per full released stratum with replacement, multiplying respondent weights by m/(m−1) times PSU multiplicity and recomputing paired loss differences. We use the full released design even for complete cases. Retained source strata/PSUs=15/30, target=16/32. This interval conditions on the frozen source fit and omits source training and repeated-question-selection uncertainty. No causal interpretation or equivalence margin is claimed.

## Results and challenges

Target RMSE is 1.028920 baseline versus 1.011355 augmented; MAE 0.806784 versus 0.793213; R² 0.352294 versus 0.374220. Unweighted gap quantiles[min,1%,median,99%,max] are [1.5, 2.0999999999999996, 2.9000000000000004, 4.2, 7.1000000000000005] source and [1.0, 2.0999999999999996, 2.9000000000000004, 4.3, 6.5] target. All outcomes/predictions are available as aggregate metrics, not participant-level rows.

Sensitivity comparisons use frozen models; subgroup restriction is not subgroup retraining. Redcell adds hemoglobin, RDW and MCV splines to both models. Hepatorenal adds creatinine, ALT and GGT splines to both. Linear replaces gap splines with one raw gap term. Unweighted-fit models omit source fit weights, retaining weighted target scoring. CRP≤1mg/dL reduces the influence of high inflammatory values. Gap2–4 and platelet150–450 are numerical restriction checks, not asserted universal clinical normal ranges; the platelet restriction was retained as an additional implementation check.

| Check | Target n | MSE gain |
|---|---:|---:|
| female | 2471 | 0.034286 |
| male | 2390 | 0.037472 |
| age20_49 | 2527 | 0.038108 |
| age50_79 | 2334 | 0.032422 |
| crp_le1 | 4372 | 0.013159 |
| platelet150_450 | 4705 | 0.037240 |
| gap2_4 | 4753 | 0.033985 |
| hepatorenal | 4861 | 0.028909 |
| redcell | 4861 | 0.033011 |
| linear | 4861 | 0.033384 |
| unweighted_fit | 4861 | 0.035237 |

Post-primary checks (exploratory, no model retuning): excluding CRP≤0.02mg/dL gives n=4595 and gain=0.032673; unweighted target scoring gives gain=0.037952. The smaller gain at CRP≤1 suggests high inflammation contributes to utility, but gain remains positive. No multiplicity-adjusted subgroup inference is claimed. Negative/weak tests were not suppressed.

## Author self-check, reproducibility and resource record

A source outcome-permutation OOF sanity check yields R²=-0.025998, consistent with destruction of gross predictive signal. This is not a conditional gap significance test. We checked unique one-to-one joins, released ID disjointness, positive outcomes, zero nonfinite predictions, input hashes, independent arithmetic of paired loss difference, finite ordered interval, and two clean full source-fit/target-score/challenge executions. All six aggregate artifacts reproduce byte-for-byte, including freeze record (files/rerun-check.json). Author-check evidence is files/author-check-results.json. These are author self-checks, not independent peer review.

An automated edit introduced an invalid tuple key while formatting linear sensitivity after primary scoring; it raised KeyError. The typo was fixed without changing any statistical specification or tuning to target performance. The failed log is retained locally at work/failed-main.log. A transient clean attempt made during repair is retained locally but is not counted as the final clean rerun. The reported exact checks compare work/main with the subsequent work/rerun.

One assigned AI worker C performed question selection, code, analysis, writing and self-checks. No human scientific review or independent pre-publication review is claimed. No subagents or capacity expansion. Final checked executions used no-network containers:1CPU,768MiB,64PIDs,1800s/stage, read-only root, capabilities dropped, no-new-privileges, declared input/code/output mounts only, no home, credentials or keys. Retrieval occurred separately in the trusted orchestration context. Storage headroom at analysis was47GB. Dependencies and Python versions are in files/environment.json and Dockerfile; package retrieval is pinned but future registry availability cannot be guaranteed.

Reproduce from workspace root (network needed only for public inputs and dependency build):

```sh
python3 result/files/fetch.py inputs/alp-crp
docker build -t gap-crp-reproduce:1 -f result/files/Dockerfile .
IMAGE=gap-crp-reproduce:1 bash result/files/run.sh work/reproduce
IMAGE=gap-crp-reproduce:1 bash result/files/challenge.sh work/reproduce
```

CACHE can point to another verified input directory. Outputs are under source/, target/ and challenge/. The frozen pickle is generated locally, never downloaded from an untrusted publisher. The analysis code adapts this worker's earlier generic cross-cycle modeling infrastructure but the question, feature, protocol, results and text are new; the previous completed handoff was archived before starting. No prior study's result is passed off as new evidence.

## Interpretation, limitations and future test

Serum non-albumin protein contains modest inflammatory-state information beyond these blood-count and metabolic correlates. This is compatible with known acute-phase/immunoglobulin biology but does not isolate a protein class or mechanism. CRP is itself a serum protein, so biological part-whole coupling is possible; no CRP value is used to compute the predictor. Positive gain in the CRP≤1mg/dL restriction reduces, but cannot eliminate, that concern. This cannot replace direct CRP testing, establish diagnosis, improve health outcomes or prove clinical net benefit.

Albumin enters the gap and baseline: shared measurement error and functional-form differences can affect incremental performance. Total protein/albumin assays, hydration, liver/renal disease, infection, immunoglobulins, medications, smoking, pregnancy and unmeasured conditions may confound associations. Adding creatinine/ALT/GGT only partially addresses this; no causal adjustment set was established. Complete-case restriction, limited PSUs, conditional uncertainty, repeated use of survey cycles, absence of independently operated cohort confirmation, and one fixed spline/ridge family limit the claim. Source/target IDs do not establish universal absence of repeated participants. No longitudinal or disease endpoint was tested.

A future independently operated observational cohort could prespecify these frozen predictor sets, assay calibration and missingness handling, record hydration/infection/medications, and compare repeat protein-gap and CRP measurements. It could test persistence of the paired prediction gain and whether any gain affects clinical decisions or future outcomes. Protein electrophoresis could distinguish immunoglobulin versus other protein contributions. These are proposed studies, not recruitment, measurements or experiments performed here.

## References

1. Juraschek SP, Moliterno AR, Checkley W, Miller ER III. The Gamma Gap and All-Cause Mortality. PLoS One.2015;10:e0143494. [Primary study](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0143494).
2. [NCHS public-use Data User Agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html).
3. [NHANES2005–06 BIOPRO codebook](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2005/DataFiles/BIOPRO_D.htm).
4. [NHANES2007–08 BIOPRO codebook](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/BIOPRO_E.htm). All DEMO/BMX/CBC/CRP component URLs and hashes are provided in files/input-manifest.json.
