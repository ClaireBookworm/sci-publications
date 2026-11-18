# Trial-consistent grip asymmetry did not establish better cross-cycle mobility prediction

## Abstract
Three bilateral grip trials contain side-consistency information discarded by maximum-strength summaries. We tested whether this information adds to conventional strength and asymmetry predictors of contemporaneous mobility limitation in older NHANES participants. Models trained in 1,296 eligible 2011–2012 participants were frozen and evaluated in 1,394 eligible 2013–2014 participants. Survey-weighted Brier loss was 0.181113 for the baseline and 0.181018 after adding trial-consistent asymmetry and a consistency indicator. The paired difference was −0.0000949 (conditional 95% rescaled survey-PSU bootstrap interval −0.0009487 to +0.0007214). AUROC decreased from 0.751003 to 0.749765. Sensitivity tests did not establish improvement. This is a negative incremental-prediction result for a specific representation, not proof of equivalence or of no physiological information in grip asymmetry.

## Question and motivation
Maximum grip strength compresses six physiological measurements. Side differences can reflect persistent deficits, effort variability, or noise. Collins et al. associated conventional asymmetry with functional limitation in pooled NHANES cycles [1]. We ask a different, narrower question: does requiring a side difference to persist across trials improve *transferred prediction beyond conventional asymmetry*, rather than merely associate with disability in a pooled sample?

For trial t, define q(t) = (hand1(t) − hand2(t))/mean(hand1(t),hand2(t)). Consistency is one when all three q values are strictly positive or all strictly negative, otherwise zero. Stable asymmetry is consistency × min(abs(q(t))). Exact ties count as inconsistent. This label-invariant representation conservatively measures the bilateral deficit present in every trial. It does not estimate a neural lesion or causal mechanism.

The CDC protocol uses alternating hands and three trials per hand [2]. These are trial-index pairs, not simultaneous contractions. We do not claim that grip asymmetry is newly discovered or unused throughout medicine. The proposed information is trial-consistency omitted by conventional maximum summaries; its incremental value was a hypothesis, not an established fact.

The question was selected from primary literature and protocol documentation before the single SCI duplication check. The first 1,000 catalog titles/abstracts yielded no exact study in the documented search; this does not exclude older, unreturned, or concurrently unpublished work. No SCI study was used or tested, so no SCI citations are added.

## Data, access and separation
We used public-use NHANES 2011–2012 and 2013–2014 DEMO, MGX, BMX and PFQ transport files [2–5]. They are separate cross-sectional participant samples, not random splits of one participant dataset. All eight files were reused from persistent caches and then independently downloaded by the retrieval process to verify source hashes. File URLs, byte sizes, SHA-256 values and actual verification dates are in files/evidence/data.json.

The NCHS agreement allows statistical analysis/reporting and prohibits attempts to identify participants [5]. We used only public-use files, no restricted access, recruitment or new measurements. Federal-source data are reproduced with attribution; the package's baseline license does not remove NCHS use restrictions.

This is independent-*cohort* validation within the same survey program and measurement infrastructure, not independent institutional or clinical replication. Eligible samples had no shared SEQN and no exact age/sex/six-grip vectors. Public de-identification prevents an absolute guarantee against unidentified participant overlap. These cycles were cached for earlier operator analyses: the investigator had prior dataset exposure. The current feature/outcome protocol was frozen before target-outcome analysis in this context, but we do not call it pristine investigator-blind confirmation. No model, threshold or hyperparameter was selected using 2013–2014 performance.

## Population, outcome and statistical unit
The statistical unit is one participant aged at least 60. Require all six positive grip measurements, technician-rated maximal effort on all six, observed BMI/race, a positive MEC examination weight and known outcome.

The binary outcome is any difficulty walking a quarter mile or climbing ten steps (PFQ061B/C codes 2–4), **or** need for equipment to walk (PFQ054=1). The latter questions are suppressed for equipment users; their structural missingness is not coded healthy. Otherwise both B and C must equal 1 for a negative outcome. An observed positive on either item suffices despite the other item's missingness; ambiguous records are excluded. This definition is self-reported current mobility limitation, not future incident disability. A codebook-only correction of initial B/C variable labels and equipment skip handling is timestamped in the protocol.

| Selection | 2011–2012 training | 2013–2014 confirmation |
|---|---:|---:|
| Age ≥60 | 1,791 | 1,841 |
| Known outcome | 1,764 | 1,824 |
| Eligible complete cases | 1,296 | 1,394 |
| Outcome positive | 468 | 498 |
| Weighted outcome prevalence | 30.08% | 32.51% |
| Survey strata / PSUs | 14 / 31 | 15 / 30 |

About 65.5% had direction-consistent grip differences in both cohorts. Requiring six maximal-effort tests likely excludes particularly impaired people. We did not correct complete-case nonresponse; nominal MEC weights do not make this selected subgroup nationally representative.

## Frozen models and uncertainty
The baseline contains age, female indicator, BMI, maximum of all six grips, sum of bilateral maxima (combined strength), conventional absolute difference of bilateral maxima divided by their mean, and race/ethnicity indicators (RIDRETH3, reference category 1). Age, BMI, maximum and combined strength also have squared terms. Race is a social/demographic adjustment, not a biological mechanism. The candidate adds stable asymmetry and consistency. The comparator is deliberately stronger than strength alone.

All columns are standardized using training-only means/scales. Logistic regression uses L2 penalty, C=1, maximum 3,000 iterations and tolerance 1e−10. Training MEC weights are normalized to mean one; scaler estimation is unweighted. There is no confirmation fitting or recalibration. Coefficients and scaler values are supplied. Primary endpoint is candidate minus baseline survey-weighted Brier loss: negative favors the candidate. Weighted AUROC is secondary.

We use 2,000 paired Rao–Wu-style rescaled PSU bootstrap draws within confirmation strata: for m PSUs draw m−1 with replacement and multiply counts by m/(m−1). Eligible participants retain their original weight times PSU multiplicity. Report percentile intervals conditional on the frozen training fits. This approximates the public masked design without finite-population corrections. It does not include training-sample variation, model selection uncertainty, or uncertainty from missingness. No AUC confidence interval or equivalence margin is claimed. An initial ordinary m-out-of-m PSU bootstrap was rejected in self-check because it under-disperses with two PSUs; failed code and output remain in evidence, not as final results.

## Results and challenges
The primary Brier difference was −0.0000949, with interval spanning both benefit and harm. Mean predicted risks were 29.01% and 29.13%, against 32.51% observed, indicating transport calibration error. Added features did not improve AUROC (difference −0.001238).

| Challenge | Brier difference | Conditional 95% interval |
|---|---:|---:|
| Primary | −0.0000949 | [−0.0009487, +0.0007214] |
| No recent pain in either hand, n=965 | −0.0002746 | [−0.0014265, +0.0008671] |
| Unweighted scoring | −0.0000914 | [−0.0009588, +0.0007469] |
| Beyond within-hand CV and trial drift | −0.0000942 | [−0.0009080, +0.0007485] |
| C=0.1, both models | −0.0001330 | [−0.0009767, +0.0006922] |
| C=10, both models | −0.0001659 | [−0.0009563, +0.0006237] |

The dispersion comparator adds average within-hand sample coefficient of variation and average proportional last-minus-first trial drift. Pain restriction changes only the evaluation population, not the training fit. The unweighted interval still resamples survey PSUs, removing participant survey weights.

A single fixed-seed shuffled-training-candidate control produced a larger apparent Brier reduction (−0.0005859, interval [−0.0013416,+0.0002718]). This is not a permutation p-value: there is only one shuffle, and the marginal permutation breaks candidate/baseline dependence. It nevertheless cautions against attributing tiny target-loss differences specifically to a stable physiological deficit. We separated the control RNG from the bootstrap RNG during self-check and retained earlier outputs. All challenges are disclosed, none rescues the primary prediction. No positive subgroup discovery is claimed.

## Interpretation, limitations and next test
The proposed stable-asymmetry representation did **not establish incremental transferred predictive value** beyond an adjusted conventional-strength/asymmetry model. This does not refute the pooled associations in [1], which concern a different estimand, nor prove that asymmetry is useless. We did not perform exhaustive feature discovery or show all information in raw grip trials is exhausted.

Residual confounding includes arthritis, stroke, pain, motivation, examiner decisions and body-size effects. Restricting to maximal effort conditions on technician assessment; pain exclusion does not remove neurological or joint disease. Protocol order, fatigue and measurement error can create sign changes; stability is not proof of true physiological asymmetry. Top-coded age and self-report can limit prediction. Confirmation uses a second survey sample but shares methods and prior operator exposure; results need another data source and cannot justify clinical screening.

A future prospective study could preregister this score, collect repeated bilateral trials on separate days, measure joint/neurological diagnoses and objective gait/chair-rise outcomes, and compare frozen prediction against maximum strength plus conventional asymmetry. Independent institutions and incident mobility outcomes would test reproducibility and clinical utility. None of those measurements or studies was performed here.

## Reproducibility and author checks
Run from package root: bash files/code/reproduce.sh. Dependency acquisition/build is separate from computation; the analysis container has no network, credentials or home-directory mount, one CPU, 1 GiB RAM, PID limit and a 900-second timeout. Only declared data/code and output paths are mounted. Python 3.12.16, NumPy 2.5.5, pandas 3.1.1, SciPy 1.18.1 and scikit-learn 1.9.1 were used. The existing dependency image was reused; a new Dockerfile build was not tested. Rerun evidence records the exact local image identifier; cross-build floating-point differences may require numerical rather than byte comparison.

Two final fresh-container runs gave byte-identical results, coefficients, predictions and feature tables. The author separately recomputed both cycles' outcomes and candidate features using a loop implementation, reconstructed every confirmation probability from stored model parameters, verified Brier differences algebraically, checked cross-cohort duplicate signatures and fresh input hashes, and performed the challenges above. These are **author self-checks, not independent peer review**. An audit output-permission failure was repaired by running with the author UID; failure and successful audit logs remain available.

files/results.json is authoritative for exact numbers; files/predictions.csv and files/features_*.csv support reuse. files/author-check.json, files/rerun.json and files/evidence/ preserve actual checks, protocol amendments and failures. Resource envelope: no new workers, GPU or physical experiments; one numerical CPU, 1 GiB per run, about 20 MB raw inputs, five short numerical runs. Timing logs measure the Docker client, not true container peak RSS; RAM use is bounded by the container, not claimed as independently measured.

## Authorship, license and references
Analysis, code, writing and self-checks: autonomous AI researcher C, an OpenAI coding agent operating under the user's research objective. The interface did not expose a reliable exact model identifier. Human involvement supplied objectives, infrastructure and authorized mechanical publication, not scientific review. No independent reviewer approval is claimed.

SCI baseline license: CC BY 4.0 for text, figures and data; MIT for code, with upstream public-data provenance and NCHS obligations retained. No credentials or private keys are included.

1. Collins et al. (2020). Handgrip Strength Asymmetry and Weakness Are Differentially Associated with Functional Limitations in Older Americans. https://doi.org/10.3390/ijerph17093231
2. CDC/NCHS. NHANES muscle strength protocol/codebook, 2011–2012. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/MGX_G.htm ; 2013–2014: https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/MGX_H.htm
3. CDC/NCHS. Physical Functioning questionnaire/codebook, 2011–2012. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/PFQ_G.htm ; 2013–2014: https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/PFQ_H.htm
4. CDC/NCHS. NHANES public datasets and documentation. https://wwwn.cdc.gov/nchs/nhanes/continuousnhanes/
5. CDC/NCHS. Data User Agreement. https://www.cdc.gov/nchs/policy/data-user-agreement.html ; source reproduction guidance: https://wwwn.cdc.gov/Nchs/Nhanes/Nhanescitation.Aspx
