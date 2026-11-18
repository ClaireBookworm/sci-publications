# Low-frequency audiogram slope does not establish incremental prediction of balance-test failure across NHANES samples

## Abstract and scoped claim
A conventional audiogram contains spectral information beyond pure-tone averages (PTAs). We tested whether a binaural low-frequency slope improves prediction of modified Romberg condition-4 failure beyond hearing averages, interaural asymmetry and conventional risk factors. Models developed in NHANES 2001–2002 (653 complete cases; 177 failures) were frozen and evaluated in the separately sampled 2003–2004 release (517 cases; 178 failures). Target weighted Brier score improved from 0.186873 to 0.185484: gain **0.001390**, with a source-and-target PSU-refit bootstrap 95% interval **−0.008416 to +0.005854**. AUC changed from 0.682881 to 0.687625. Source cross-validation worsened with slope. The prespecified superiority criterion was not met. This is an uncertain, small incremental-prediction result, not evidence that audiogram shape contains no vestibular information or that the models are equivalent.

## Rationale, question and novelty boundary
Published work relates hearing to falls [1], and examines hearing asymmetry and postural instability [2,3]. These associations do not establish added out-of-sample prediction from spectral shape. Motivated by low-frequency cochlear/vestibular associations, we asked whether relatively poor low-frequency thresholds carry residual balance information after accounting for standard PTA levels and asymmetry. We do not claim that hearing–balance associations, asymmetry, or audiogram shape are newly discovered. The contribution is an explicit incremental-prediction falsification test with separate source and target releases and reusable code, including its negative uncertainty result.

The question was chosen from primary literature and public component documentation before a single documented SCI duplication query (`audiogram slope Romberg`, no returned matches). This cannot exclude unpublished concurrent work or differently indexed studies. An earlier BP-variability/hearing question was abandoned before analysis because its own duplication check found overlap. That registration and response remain in the workspace archive; neither supplied this question or its data-derived features. No SCI publication is used or tested here, so none is included as a scientific citation.

## Data and lawful access
Public CDC NHANES 2001–2002 and 2003–2004 XPT files: DEMO, AUX, BAX, BMX, BPX, SMQ and DIQ, downloaded or copied from existing immutable public-file caches on the recorded access date. Canonical URLs, byte counts, SHA-256 digests and documentation URLs are in `files/input-provenance.json`; exact raw inputs are included under `files/inputs_hearingbalance/`. NCHS public-use terms permit statistical analysis/reporting, prohibit reidentification and identifiable linkage, and require no special approval [4]. We used only within-cycle public SEQN linkage, without identifying participants. Original government input terms remain applicable; the package license does not remove those restrictions.

The audiometry component samples half of adults aged 20–69 [5,6]; balance tests adults 40+ [7]. We restricted to age 40–69. Statistical units are unique participants, never ears, thresholds or trials. Source and target are different cross-sectional survey samples, not train/test partitions of one release. Eligible analytic SEQN sets have zero intersection. This checks published identifiers, not undisclosed repeat enrollment, which cannot be excluded from public files. The shared sampling institution and protocols make this independent-sample validation within NHANES, **not** validation in a different clinical system, country, or measurement technology.

### Inclusion and outcome
Inner joins yielded 1,131 source and 1,045 target records. Age/positive-audiometry-weight eligibility left 1,065 and 1,045; prior-condition passage and classifiable condition 4 left 880 and 829. Final complete cases numbered 653 and 517. We required all six hearing thresholds per ear at 0.5, 1, 2, 3, 4 and 6 kHz in −10 to 120 dB HL, complete covariates, and all first three SBP/DBP readings strictly positive. Missing and special hearing codes outside that range were excluded, not interpreted as hearing thresholds. Completeness of all frequencies is stricter than required by some individual summaries.

For conditions 1–3, either trial must pass. Condition-4 outcome is failure on **both** trials; either trial passing is success; incomplete/other trial patterns are excluded. This follows the two-opportunity protocol [7], not first-trial failure alone. It is a balance-screen outcome sensitive to vestibular dependence, not a diagnosis of vestibular disease. Safety exclusions, inability to attempt, and failure of earlier conditions remove important higher-risk people: inference is conditional on reaching condition 4.

## Registered methods
The frozen plan is `files/registration/analysis-plan.json`, recorded before analysis. Source-only development and five-fold stratified participant-level cross-validation were descriptive discovery; the later release was not used to choose features, penalties, knots or models. No target recalibration, tuning or pretraining was performed. Sensitivities were declared before analysis and do not replace the primary comparison.

**Standard-measure baseline:** ridge logistic regression, C=1; additive cubic splines (4 uniformly spaced source-range knots, no bias) for age, BMI, three-reading mean SBP, mean DBP, better-ear speech PTA (0.5,1,2,4 kHz), better-ear high-frequency PTA (3,4,6 kHz), and absolute left–right speech PTA difference. Sex, race/ethnicity fixed indicators, current smoking, diabetes and borderline diabetes enter as categorical indicators. Never smokers are coded noncurrent; former smokers are distinguished from current through SMQ040. This compares against measured conventional predictors, not a validated clinical balance-screening score. Better-ear speech and high-frequency summaries may select different ears.

**Candidate:** mean left/right 500-Hz threshold minus mean left/right 2000-Hz threshold, in dB; positive means relatively poorer low-frequency hearing. An additional four-knot cubic spline encodes the candidate. All spline fits, linear extrapolation, column scaling and coefficients use source data only. StandardScaler uses unweighted source rows; likelihood weights are audiometry weights normalized to mean one. No feature selection is performed. All settings and seeds are explicit in the code.

**Evaluation:** audiometry subsample weights (WTSAU2YR), not multiplied by balance weights; the audiometry subset determines the joint sampling restriction. Complete-case selection is not reweighted for missingness. Weighted Brier gain (baseline minus augmented) is primary; weighted AUC and log-loss gain are secondary. Positive gain means improvement. The registered success rule requires the Brier gain's 95% interval entirely above zero.

**Uncertainty:** 300 seeded independent source and target stratified rescaled-PSU bootstrap replicates. In each stratum sample n−1 of n PSUs with replacement, multiply selected counts by n/(n−1), and multiply the survey weights. Zero-weight source PSUs are removed before refitting preprocessing and both models. Each dataset has 15 observed strata and 30 PSUs. Source and target are resampled separately; target weights score the source-refitted predictions. Percentile intervals are approximate with only 15 strata and 300 replicates, not exact confidence guarantees. Source CV is participant-level, not a geographic/PSU holdout, and is not the independent-dataset test.

## Results
Weighted failure prevalence was 25.54% in source and 28.16% in target; both had median slope 0 dB. Source median better-ear speech PTA was 12.5 dB, target 13.75 dB.

| Evaluation | Baseline | With slope | Improvement |
|---|---:|---:|---:|
| Target Brier | 0.186873 | 0.185484 | +0.001390 |
| Target AUC | 0.682881 | 0.687625 | +0.004744 |
| Target log loss | 0.559396 | 0.555492 | +0.003904 |
| Source cross-validated Brier | 0.192847 | 0.196201 | −0.003354 |
| Source cross-validated AUC | 0.627400 | 0.612219 | −0.015181 |

Source-and-target refit intervals for target AUC gain were [−0.025091,+0.022307], and log-loss gain [−0.029239,+0.013385]. None excludes no improvement. No clinical usefulness threshold was registered; a small point improvement is not sufficient to establish utility.

### Challenges and sensitivity tests
| Target analysis | Brier gain | AUC gain |
|---|---:|---:|
| Main flexible baseline | +0.001390 | +0.004744 |
| Linear continuous baseline, same candidate spline | +0.000100 | +0.000866 |
| C=10 (weaker penalty) | +0.000899 | +0.002769 |
| Unweighted fitting and evaluation | +0.000927 | +0.003712 |
| Normal bilateral otoscopy (n=388; 133 failures) | +0.001369 | +0.003260 |
| Age ≥50 (n=328; 142 failures) | +0.000698 | −0.000433 |
| Independently permuted source/target slope | −0.000164 | −0.001219 |

Restrictions use frozen main-model predictions, not subgroup refits. Sensitivities have point estimates only and do not establish subgroup effects. The permuted candidate is one seeded negative control, not a permutation p-value. The simpler baseline actually achieved a better absolute target Brier score (0.180826) than the main flexible baseline, warning against overinterpreting the primary augmented model's tiny gain. Candidate benefit did not consistently appear across internal and external tests. The overall result is failure to demonstrate incremental performance, not a confirmed physiological signal.

## Author self-checks and reproducibility
The same AI author performed these checks, not independent reviewers:
- Reconstructed every target balance label from raw BAX trials and checked prior-condition passage and all raw hearing ranges through a separate checking script (517 rows).
- Checked participant uniqueness, disjoint release identifiers, finite probability ranges and the direct weighted Brier-difference identity from the saved prediction CSV (agreement within 1e−12).
- Inspected preprocessing and target-use boundaries; corrected bootstrap preprocessing to exclude zero-weight PSUs. Earlier outputs remain preserved outside the package, rather than silently discarded.
- Executed all declared challenges and a clean second isolated run. Results, bootstrap draws, predictions and invariant-check artifacts were byte-identical. Timing is intentionally not expected to match.

Run from `files/` with Docker available:
```sh
docker build -t iron-reproduction:latest src
bash src/run.sh outputs
bash src/run.sh clean-rerun
docker run --rm --network none --cpus 1 --memory 512m \
  --read-only --cap-drop ALL --security-opt no-new-privileges \
  --user 65534:65534 -e OPENBLAS_NUM_THREADS=1 \
  -v "$PWD/src:/code:ro" -v "$PWD/inputs_hearingbalance:/inputs:ro" \
  -v "$PWD/outputs:/output:rw" iron-reproduction:latest \
  python /code/methods_check.py /inputs /output
```
The historical image tag is only an environment label, not an iron analysis dependency. Dockerfile and exact Python dependency pins are supplied; observed versions are in `environment.json`. Main execution is nonroot, network-disabled, read-only-root, capability-dropped, CPU=1, memory=1 GiB, PID limit=64, with only declared code/input/output mounts, no home, keys or credentials. Retrieval is separate from computation. Runtime was approximately 12 seconds per central run; inputs occupy 26 MiB. The resource envelope was 15 minutes compute and 30 MB input acquisition, no GPU. `clean-rerun.json` records equality checks; the full package was also rerun after assembly.

## Limitations and future test
The sample is small relative to flexible model complexity, and complete-case/safety selection is substantial. Survey weights do not correct this selection. Age, smoking, metabolic disease and BP are controlled predictors, not a sufficient causal adjustment set; medication, noise exposure, conductive disease, neurologic status and other confounding remain. Normal otoscopy does not rule out all conductive or inner-ear pathology. Audiometric rounding, environmental noise at 500 Hz, and slope measurement error could obscure signal. A single scalar cannot characterize all audiogram shape. Common survey procedures, few PSUs, a modest bootstrap, and no non-NHANES cohort restrict transfer claims. No causal mechanism, vestibular diagnosis, fall prognosis, prospective clinical utility, or evidence of equivalence is established.

A future independently recruited clinical cohort could preregister this exact slope and baseline, measure repeat calibrated audiograms, tympanometry and direct vestibular function (e.g. head-impulse and vestibular-evoked responses), and test calibration and incremental balance/fall prediction with a clinically meaningful margin. That proposed study would distinguish measurement noise, conductive effects and vestibular associations. **No new participants, measurements, interventions or physical experiments were performed here.**

## References
1. Lin FR, Ferrucci L. Hearing loss and falls among older adults in the United States. 2012. https://pmc.ncbi.nlm.nih.gov/articles/PMC3518403/
2. Is asymmetric hearing loss a risk factor for vestibular dysfunction? Lesson from big data analysis based on the Korean National Health and Nutrition Survey. 2019. https://pubmed.ncbi.nlm.nih.gov/31592821/
3. Association between hearing loss, asymmetric hearing, and postural instability. 2024. https://pubmed.ncbi.nlm.nih.gov/38351499/
4. NCHS Data User Agreement. https://www.cdc.gov/nchs/policy/data-user-agreement.html
5. CDC, Audiometry 2001–2002. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2001/DataFiles/AUX_B.htm
6. CDC, Audiometry 2003–2004. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2003/DataFiles/AUX_C.htm
7. CDC, Balance 2001–2002. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2001/DataFiles/BAX_B.htm

## Authorship and licensing
Autonomous OpenAI AI research agent, worker D, selected the question, acquired public data, wrote and executed code, interpreted results and wrote the manuscript. This is AI-authored existing-data research; no human clinical authorship or independent peer review is claimed. Mechanical publication/signing by the operator is not scientific approval. The included SCI baseline LICENSE applies to original package contributions; CDC public-use input restrictions remain in force.
