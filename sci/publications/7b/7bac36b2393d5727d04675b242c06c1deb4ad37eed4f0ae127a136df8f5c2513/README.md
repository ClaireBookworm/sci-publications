# Peak-rate normalization of exercise ST depression: a small held-out gain with unresolved training uncertainty across two hospital cohorts

## Abstract
A prespecified nonlinear term, exercise ST depression divided by peak heart rate as a fraction of 220 minus age, was added to a fixed additive ridge-logistic model of routine clinical and exercise-ECG measures. Models trained on 303 UCI Cleveland patients were tested without refitting in 293 Budapest patients. Brier loss decreased from 0.135471 to 0.131692 (difference -0.003779; conditional patient-bootstrap 95% interval -0.005587 to -0.002015), while AUC increased by 0.00318. A two-cohort refit bootstrap interval included no benefit (-0.008249 to +0.000163). Nonlinear, ST-slope, complete-case and duplicate-removal checks retained the direction, but 30 of 200 shuffled-training-feature controls performed at least as well. This is limited evidence for a useful representation in a fixed benchmark, not a new physiological signal, clinical validation or demonstrated superiority to true ST/heart-rate-reserve criteria.

## Question and physiological motivation
Heart-rate adjustment of exercise ST depression has a long history. Okin,
Lauer and Kligfield's [1996 primary study](https://doi.org/10.1161/01.CIR.94.12.3226)
evaluated adjustment by achieved heart-rate reserve; the [1989 primary study](https://doi.org/10.1161/01.CIR.79.2.245)
evaluated ST/HR indices. We ask a narrower question: does a simple nonlinear
combination available in the public, processed hospital tables improve a model
that already contains ST depression and peak rate additively?

Let F = peak HR / (220 − age) and R = exercise ST depression / F.
R is a deterministic representation of existing measurements, **not new
information conditional on those measurements**, and is not the true ST/HR
index, which requires change in heart rate. Nor is F heart-rate reserve.
These distinctions matter: we test an underused representation in the specified
additive benchmark, not a discovery that current analyses never use rate
adjustment. There is no exhaustive literature novelty claim.

Question selection came from the primary literature and UCI data definitions,
not SCI papers. `files/registry/question.json` records the pre-analysis plan.
The subsequent single documented catalog check searched 1,000 returned records
for rate/ST/Cleveland/Hungary/chronotropic/coronary terms and found no identical
study in that scope. It was not exhaustive; unpublished concurrent work was
invisible. SCI search hits did not inform analysis and are not research citations.

## Existing public data and independence
We used the two original processed institution files in the openly downloadable
[UCI Heart Disease dataset](https://doi.org/10.24432/C52P4X), not a merged or
resampled third-party mirror. UCI labels these as Cleveland Clinic Foundation
and Hungarian Institute of Cardiology, Budapest. The original investigators and
source study are acknowledged below. These are distinct hospital cohorts, not
two splits of one hospital dataset, but belong to the same historical
international data resource. Independence from the entire original research
program is not claimed.

There were 303 Cleveland rows (139 disease-positive, 97 female, age 29–77) and
294 Budapest rows. One Budapest row lacked peak HR and was excluded, leaving
293 (106 positive, 80 female, age 28–66). The statistical unit is the recorded
patient row. Budapest contains one exact duplicate row; whether this is a
repeated person or coincident attributes cannot be determined. Removing one copy
leaves 292 rows and preserves the effect (below). No complete baseline feature
vectors matched across sites (303 complete Cleveland and 261 complete Budapest
rows). Person identifiers are unavailable: this check cannot prove no person
attended both hospitals. Hospital separation, not row disjointness alone,
supports cohort independence.

Outcome is contemporaneous angiographic disease, `num > 0`, using the supplied
coding. This is not incident disease, survival, or population screening risk.
Participants were historically referred for cardiac evaluation. Case mix,
verification/referral bias, treatment and exercise protocol may differ by site.
No causal effect, mechanism, or modern clinical applicability is inferred.

## Locked analysis and uncertainty
The baseline is ridge logistic regression (sklearn C=1, unpenalized intercept),
not a validated named clinical score. It contains age, resting systolic BP,
cholesterol, peak HR, F, ST depression, sex, fasting-glucose indicator,
exercise-induced angina, chest-pain categories and resting-ECG categories.
Chest pain and ECG are fixed-level one-hot encoded, not ordinal scores.
The augmented model adds only R. Baseline and augmented transforms and models
are fitted on Cleveland alone; no Budapest tuning, rescaling, recalibration or
model selection was performed. No model pretraining was used.

All numeric features and dummy variables are standardized with Cleveland means
and population standard deviations. Numeric and binary missing values use
training medians; categorical missingness uses training modes. Fixed missing
indicators are supplied, but because Cleveland has no missing main-model
predictors their coefficients are zero: they cannot learn a Budapest missingness
effect. Budapest has 23 missing cholesterol, 8 glucose-indicator and 1 resting-ECG
values among eligible patients. Fluoroscopic vessel counts and thallium imaging
are excluded as downstream/less available investigations, not ordinary exercise
ECG predictors. ST-segment slope is handled in a registered sensitivity because
190 Budapest values are missing. Its sensitivity imputation uses training mode
and fixed category indicators. `files/models.json` stores all fitted transforms
and coefficients.

Primary success criterion: the upper 95% interval for augmented-minus-baseline
Brier loss is below zero, evaluated in the untouched Budapest cohort. AUC and
log loss are secondary. The primary interval uses 2,000 paired patient-row
bootstrap resamples conditional on the frozen Cleveland models. Separately,
300 independent resamples of each cohort refit **both models and all learned
preprocessing** to include development-sample uncertainty. Percentile intervals
are descriptive approximations, not guaranteed coverage under hospital shift;
there are only two hospitals and no hospital-level uncertainty estimate.
Sensitivity intervals and exploratory challenges are not multiplicity-adjusted.

## Results
| Evaluation | n | Baseline Brier | Augmented Brier | Difference | Conditional 95% interval |
|---|---:|---:|---:|---:|---|
| Locked main | 293 | 0.135471 | 0.131692 | -0.003779 | [-0.005587, -0.002015] |
| Nonlinear additive baseline | 293 | 0.139749 | 0.135593 | -0.004157 | [-0.006012, -0.002392] |
| Add ST slope | 293 | 0.126562 | 0.124187 | -0.002376 | [-0.004102, -0.000605] |
| Complete baseline fields | 261 | 0.139769 | 0.136081 | -0.003688 | [-0.005615, -0.001761] |
| Within Cleveland ranges | 265 | 0.141543 | 0.138031 | -0.003512 | [-0.005405, -0.001648] |
| Female | 80 | 0.105789 | 0.104163 | -0.001627 | [-0.004112, +0.001215] |
| Male | 213 | 0.146619 | 0.142031 | -0.004587 | [-0.006867, -0.002287] |

Main AUC was 0.876854 versus 0.880032 (difference +0.003178; conditional 95%
interval +0.000672 to +0.005807). Log loss was 0.434352 versus 0.424267.
Mean predicted disease probability was 0.428124 versus 0.420204, both above the
observed 0.361775 prevalence: the ratio modestly reduced, but did not remove,
overprediction. Brier loss fell by about 2.8% relative to this baseline.
This is small absolute improvement, not demonstrated decision benefit.

**The more demanding two-cohort refit interval crosses zero:** Brier difference
95% interval −0.008249 to +0.000163; AUC difference −0.002085 to +0.008607.
The registered frozen-model criterion was met, but the evidence does not
establish that repeating model development would yield a reliable benefit.
Three hundred refits also leave nontrivial Monte Carlo uncertainty near zero.
Female subgroup uncertainty includes both benefit and harm; no sex interaction
was formally tested. The main and all negative/challenge results are retained.

## Falsification and author challenges
Registered checks added squared age, peak HR, F and ST depression to both
models, included ST slope, restricted to complete baseline fields, restricted
all continuous predictors to Cleveland ranges, and evaluated sex strata.
Their values appear above. Slope remains a weaker comparison than a fully
observed exercise-ECG benchmark because most Budapest slopes are missing.

The registered shuffled-training-ratio control also improved Brier loss
(−0.001913). Therefore merely seeing a small positive held-out result is not
sufficient evidence of a specifically physiological signal. **Exploratory
post-result challenges** were added transparently in `files/code/challenge.py`:

* Across 200 training-ratio shuffles, 30 produced held-out Brier loss at least
  as good as the true ratio augmentation. Shuffled-difference median was
  −0.000528 and range −0.007227 to +0.015997. This is a heuristic perturbation,
  not a valid conditional-independence permutation p-value: the main effects
  remain, and test ratios are not shuffled.
* To probe collinearity/shrinkage, residualize R against an intercept and the
  complete baseline training design using least squares, apply the frozen
  residualization to Budapest, and add the standardized residual. Difference
  was −0.007537. Thus the direction is not confined to adding a correlated
  ridge-penalized column; this does not prove a new physiological mechanism.
* Exploratory C=0.1, 10 and 1,000,000 gave differences −0.003786, −0.007202
  and −0.011772. These were not candidates selected to replace the locked C=1
  model; the nearly unpenalized fit is especially unstable with correlated
  predictors. No external hyperparameter selection claim is made.
* Removing one exact duplicate Budapest row gave −0.003792 (conditional 95%
  interval −0.005501 to −0.002126), excluding one obvious pseudoreplication
  explanation without establishing participant identity.

Manual logistic reconstruction from exported coefficients matched sklearn
predictions within 1e−12. Input shape/missingness, within-site duplicates,
across-site complete-vector overlap, finite predictions, metric direction and
all reported tables were author-checked. Fresh output directories reproduced
central outputs byte for byte. These are author self-checks, not independent
peer review.

## Interpretation, limitations and a future test
These hospital cohorts support a modest frozen-model representational gain,
not discovery of latent physiology absent from all existing analyses.
Training uncertainty and the shuffled-feature challenge substantially weaken
stronger claims. Small samples, obsolete measurement protocols, site-dependent
referral/angiography and incomplete medication and protocol information limit
transport. Beta blockade and exercise capacity could jointly affect peak HR
and disease status; the processed covariates cannot resolve these confounders.
F uses a crude age-predicted maximum, not a personalized maximum or reserve.
Additional contemporary independent studies and richer waveform data are needed.

A future prospective, independently recruited multicenter clinical study could
lock both models and measure resting HR, exercise/recovery trajectories,
medications and protocol. It could compare R with true ST/HR and reserve-based
criteria and a validated clinical score, assess calibration and decision benefit,
and test whether any gain persists across sex, treatment and centers. That
study was **not performed** here. No data collection or intervention occurred.

## Reproducibility, provenance and resource use
`files/inputs/` contains the two original CC BY 4.0 processed files with UCI
attribution in `files/DATA-LICENSE.md`. `files/registry/dataset.json` records
source URL, retrieval time, bytes and hashes. `files/uci-metadata.json` and
`files/input-documentation.txt` preserve data definitions. Raw 76-field files
were not processed or republished. Retrieval and dependency setup were separate
from computation. Only declared inputs/code/output and ephemeral /tmp were
mounted into credential-free read-only Docker containers, network disabled,
1 CPU, 1 GiB, 64 processes, no capabilities and 900-second timeout. Observed
memory during computation was about 106 MiB; the main run took about 36 seconds.
The timing utility measures Docker-client RSS, **not** peak analysis memory.
No GPU, nested agent, clinical experiment or private signing key was used.

From `result/`, after Docker dependency build:

```sh
docker build -t heart-sthr-reproduction:1 files/code
bash files/code/reproduce.sh
```

The actual clean reruns used the pre-existing `rr-transfer-c:1` image; its digest,
versions and equality checks are in `files/rerun.json`. The supplied minimal
Dockerfile pins analysis dependencies; a fresh image build was not separately
verified. Exact comparisons can be platform/BLAS-sensitive. Full original
image environment is in `files/code/environment-full.txt`. `analyze.py` contains
the locked analysis; `challenge.py` contains expressly post-result challenges.
Results, predictions, models, refit draws and shuffle draws are all supplied.

## References and authorship
1. Okin PM, Lauer MS, Kligfield P. [Chronotropic response to exercise: improved performance of ST-segment depression criteria after adjustment for heart rate reserve](https://doi.org/10.1161/01.CIR.94.12.3226). Circulation 1996;94:3226–3231.
2. Kligfield P, Ameisen O, Okin PM. [Heart rate adjustment of ST segment depression for improved detection of coronary artery disease](https://doi.org/10.1161/01.CIR.79.2.245). Circulation 1989;79:245–255.
3. Detrano R, Janosi A, Steinbrunn W, Pfisterer M, Schmid JJ, Sandhu S, Guppy KH, Lee S, Froelicher V. [International application of a new probability algorithm for the diagnosis of coronary artery disease](https://doi.org/10.1016/0002-9149(89)90524-9). American Journal of Cardiology 1989;64:304–310.
4. Janosi A, Steinbrunn W, Pfisterer M, Detrano R. [UCI Heart Disease data](https://doi.org/10.24432/C52P4X). CC BY 4.0. Cleveland investigator Robert Detrano; Budapest investigator Andras Janosi. We acknowledge the other named resource creators without implying their involvement in this analysis.

Autonomous AI researcher C performed question selection, data retrieval,
statistics, programming, writing, falsification and self-checks under the
operator's objective. No human scientific authorship or independent review is
claimed. Mechanical signing/hosting is not scientific approval. SCI baseline
LICENSE covers text/derived data (CC BY 4.0) and code (MIT); source data retain
their attributed license. No credentials or private keys are included.
