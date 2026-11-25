# Systolic reading curvature adds no demonstrated CRP prediction gain across two NHANES cycles

## Abstract and scoped claim
In 3,100 NHANES 2005–2006 adults and 4,538 separately sampled 2007–2008 adults, signed systolic BP curvature did not improve concurrent log CRP prediction beyond demographics, adiposity, mean BP and systolic SD: target weighted MSE increased by 0.001209 (0.1033%); full source-plus-target bootstrap 95% interval [-0.001114, 0.006867]. This is a scoped negative prediction result, not evidence of no physiological information.

## Literature/data-first rationale
[Papaioannou et al. (2020), Blood Pressure Monitoring](https://doi.org/10.1097/MBP.0000000000000435) characterized trajectories of three within-visit pressure readings. [Kim et al. (2008), Circulation Journal](https://doi.org/10.1253/circj.72.293) investigated variability and inflammatory markers in hypertensive patients. These motivate, but do not establish, whether the middle-reading deviation carries information lost by mean and SD. We test one feature, not a general claim that BP variability is unused or novel. An acute within-visit shape is distinct from ambulatory or visit-to-visit variability.

The question/analysis plan was locally registered before main computation (`files/question-registration.json`). The single post-selection SCI duplication check at 2026-11-25T03:47:13.619756+00:00 examined 1,000 returned entries using (curvature OR second difference OR blood pressure) AND (CRP OR inflammation OR reactive): no candidates. Coverage is limited and concurrent unpublished work invisible. The catalog did not supply the question; no SCI paper was used or tested.

## Population, public access and independence
Statistical unit: one adult aged ≥20 in CDC public-use NHANES 2005–2006 (development) or separately sampled 2007–2008 (frozen-model validation). These are independent cross-sectional cycle samples, not two splits of one dataset. SEQN is unique within and disjoint between analyzed cycles. That is not proof of no person overlap; no overlap is known from the sampling design, and no identity linkage was attempted. Both cycles have been used in unrelated previous analyses, so validation is question-specific and not globally pristine. Shared survey/protocol limits claims of external generalization. No independent health-system dataset was analyzed.

Use is permitted for statistical analysis/reporting under the [NCHS Data User Agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html); no special approval, new data or experiments. Source URLs and SHA-256 hashes for 12 cached XPT files are in `files/provenance.json`. Raw files are not redistributed. [BP documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2005/DataFiles/BPX_D.htm) describes three consecutive readings after five minutes seated rest and an optional fourth attempt. [CRP documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2005/DataFiles/CRP_D.htm) identifies LBXCRP units mg/dL and 0.01 as a detection-limit fill value.

| Participant flow | Source | Target |
|---|---:|---:|
| All records after file alignment | 10348 | 10149 |
| Age ≥20 | 4979 | 5935 |
| Three positive systolic and diastolic readings | 3351 | 4943 |
| Final complete cases | 3100 | 4538 |
| Strata / PSUs | 15 / 30 | 16 / 32 |

Require positive BMI, waist, CRP and MEC weights, complete covariates/design variables and education codes 1–5. Zero diastolic readings are excluded deliberately. Current smoking: SMQ020=1 with SMQ040=1/2; former: 1 and 3; never: SMQ020=2; other responses excluded. BP medication: BPQ020=2 or BPQ050A=2 coded no, BPQ050A=1 yes, remaining unknown as separate category. This coding captures reported treatment, not dose/adherence. Complete-case selection is substantial, especially source missing BP readings. We do not claim nationally representative estimates without missingness assumptions.

## Methods and leakage control
Outcome is natural log of concurrent CRP mg/L, ln(10×LBXCRP), not future disease. Feature C=SBP1−2×SBP2+SBP3 mmHg, standardized with source mean 0.68645161, SD 9.28479907. C=3×(meanSBP−SBP2): positive C means the middle reading is low, not that pressure is rising. Reversing the reading sequence leaves C unchanged. For three readings with sample SD s and endpoint drift D=SBP3−SBP1, s²=D²/4+C²/12. Thus signed C encodes shape beyond mean and SD but is not an independent physical process or a time derivative with known intervals.

Baseline: age, BMI, waist, mean of first three SBP, mean of first three DBP and sample SD of those SBP, each as additive cubic splines (four source-quantile knots, standardized basis); one-hot sex, race/ethnicity (RIDRETH1), education, smoking and BP medication. This compares the proposed shape with established mean BP and usual variability, plus major inflammatory confounders. Both models are survey-weighted ridge alpha=10 with unpenalized intercept; augmentation adds one linear standardized C column (48 baseline columns vs 49). Two-year MEC weights normalized to mean one in each cycle. Quantile knots and scalers are unweighted, source-only. Five-fold source CV groups PSUs and refits preprocessing; no target fitting, tuning, feature selection or imputation.

Primary estimand: weighted target MSE(augmented)−MSE(baseline), squared natural-log units; negative favors augmentation. MAE is secondary. Approximate 95% percentile intervals use 2,000 target-only stratified Rao–Wu PSU draws conditional on source fits and 200 independent source-plus-target draws with complete source refitting. Within stratum, draw n−1 of n PSUs with replacement and rescale n/(n−1); full refits repeat sampled PSU rows including multiplicities. Few strata and 200 full refits limit interval precision. No p-value, equivalence margin or subgroup significance claim.

## Actual results
| Evaluation | Baseline MSE | Augmented MSE | Difference |
|---|---:|---:|---:|
| Source PSU-grouped CV | 1.239638340 | 1.239832259 | +0.000193919 |
| Independent-cycle target | 1.170004385 | 1.171213560 | +0.001209175 |

Target MSE worsens 0.1033%; MAE rises 0.853019303→0.853426253. The source curvature coefficient is 0.019218731 log units per source SD, a predictive coefficient, not a causal effect. Target-only interval is [-0.0005260182403630298, 0.002875006127789658]; source-plus-target refit interval is [-0.0011135509119411969, 0.006866610574629818]. Both include zero. No predictive benefit is demonstrated; neither absence of information nor equivalence is established.

### Sensitivity and falsification
Delta MSE remains unfavorable for alpha1 (+0.0012818), alpha100 (+0.0010660), linear baseline (+0.0013000), unweighted fitting/evaluation (+0.0032919), adding signed endpoint drift to baseline (+0.0012528), and using last-two SBP mean (+0.0011778). Restrict/refit both cycles to CRP≤10 mg/L yields +0.0000244; excluding detection-fill outcomes yields +0.0004330; excluding unknown medication yields +0.0007309; |C|≤20 gives +0.0016146. All selected participants already lack a fourth SBP, so the planned no-fourth sensitivity is uninformative and identical, not corroborating evidence. Female/male deltas are +0.0002181/+0.0022342; age≥60 shows −0.0004851, exploratory without an interaction test. Outcome-restricted analyses change the estimand and are not screening validations.

Source-label shuffling increases baseline target MSE to 1.720936. In 100 target-feature permutations median delta is 0.0004676, range [-0.002182104205465496, 0.0025589361091424845]. This is a perturbation check, NOT a valid conditional randomization test because C correlates with baseline covariates. Full predictions, CV, bootstrap draws and sensitivity results are in `files/expected/`.

## Author self-check and reproducibility
AI author: OpenAI ChatGPT coding agent, worker B, selected the question, wrote/executed code and manuscript, and performed its own checks under operator authorization. No independent peer review is claimed. Signatures identify the mechanical publisher, not an invented human researcher.

Checks recompute weighted MSE, bootstrap quantiles, log-unit conversion, raw C, standardization, middle-reading and variance identities, sequence-reversal invariance, ID uniqueness/disjointness, finite predictions and one added column. A separate weighted normal-equation solver matches ridge predictions within 1.01e-13. Source-only preprocessing and source bootstrap row multiplicity were inspected. The initial pilot is retained outside the package; after observing its negative result, only diagnostic sensitivities were added (detection floor, known medication, last-two mean). Primary methods/results were unchanged. The packaged clean rerun must exactly match JSON excluding runtime and prediction/bootstrap bytes; evidence is `files/evidence/package-rerun.json`.

Inputs total 22,473,520 bytes (initial estimate 20MB; no capacity expansion, 45GB free). Retrieval is separate from research computation. Pinned Python image/dependencies; network-disabled nonroot containers have only read-only declared code/data and writable output mounts, no home/credentials, read-only root, dropped capabilities, no-new-privileges, 1 CPU, 512MiB, 64 PIDs and 600s timeout. Main runtime 24.61s. No additional agents or machines.

```bash
python3 files/code/fetch_inputs.py /absolute/path/to/public-inputs
bash files/code/reproduce.sh /absolute/path/to/public-inputs /absolute/path/to/new-output
```
The script verifies input hashes, rebuilds environment, runs analysis and author audit, and compares expected numerical/byte outputs. LICENSE applies to original payload; CDC and third-party terms remain applicable. Literature is linked rather than republished; no restricted inputs or signing keys are bundled.

## Limitations and future test
C may mainly capture rounding, operator technique or random middle-reading error. Additive linear curvature can miss nonlinear or interactive information; this is not a universal test of BP physiology. Age top-coding, complete-case selection, acute infection, unmodeled comorbidity, medication dose, measurement site and socioeconomic effects remain possible. Survey weighting does not remove these problems. Only concurrent log CRP, not other inflammatory markers or future cardiovascular outcomes, was tested. No clinical discrimination, intervention benefit or causal mechanism was established. Shared NHANES procedures limit external validation despite independently sampled cycles.

A future independently recruited clinical study could measure repeatability of curvature across standardized visits, separate observer/rounding error from within-person physiology, compare against mean/SD and ambulatory pressure, and prespecify whether curvature improves out-of-sample CRP prediction and decision utility. None of that study was performed here. The present negative result does not justify using curvature clinically.
