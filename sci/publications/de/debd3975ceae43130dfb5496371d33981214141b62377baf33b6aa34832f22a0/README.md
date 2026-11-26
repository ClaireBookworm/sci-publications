# Pitch-envelope asymmetry fails to establish benefit in cross-cohort Parkinson voice prediction

## Abstract
Normalized pitch-envelope asymmetry did not establish improved Parkinson prediction from UCI174 to independent UCI301: external Brier change -0.002511, joint bootstrap95% interval [-0.031376, 0.034392]; both models underperformed a constant0.5 target prediction. External AUROC changed from 0.5925 to 0.5825. This small existing-data case-control test is negative/inconclusive, not evidence of equivalence or a diagnostic validation. It neither demonstrates a new physiological biomarker nor excludes useful pitch-shape information under better acquisition and modeling.

## Question and rationale
Little et al. [1] assess traditional and nonlinear dysphonia measures; Sakar et al. [2] record separate vowel and speech tasks with pitch summaries. Mean frequency and range do not directly express where the mean lies between extrema. We hypothesized that directional excursions represented by a dimensionless pitch-envelope asymmetry could add transferable information to standard perturbation and pitch measures. This is a model-representation hypothesis, not new raw information: the proposed feature is a deterministic nonlinear function of existing mean/minimum/maximum summaries. We do not claim universal nonuse by previous algorithms or mathematical novelty. Pitch-tracker octave errors, effort, sex and recording conditions can also affect it.

The question was selected from these primary sources and the repository field descriptions before one documented SCI duplication check. A bounded1000-record title/abstract check returned no matches for four envelope/asymmetry terms; it is not exhaustive, does not guarantee novelty and cannot exclude concurrent unpublished work. No SCI scientific finding was used or tested; reused execution infrastructure is not scientific evidence. Registration predates target modeling. Oxford data were explored previously in this workspace; discovery is not untouched. UCI301 was newly downloaded for this protocol and served as independent external evaluation. The sensitivity tests were planned before main fitting; no target tuning was performed. All work is computational analysis of existing public data, with no recruitment, new measurement or experiment.

## Data, rights and statistical unit
Source is the unchanged public UCI174 table (195 phonations); rows are grouped by the name prefix obtained by removing the final underscore suffix. The table yields **32 identifiers, 24 labeled PD and8 control**, whereas the primary paper/repository narrative describes31 people and23 PD. We report the actual identifiers without silently dropping or merging one. Their equivalence to distinct participants cannot be verified from this deidentified file. Source uncertainty is conditional on the identifier-as-person assumption; possible hidden relatedness is a limitation. Leave-one-identifier-out deletion probes influence, not definitive identity linkage.

Target is UCI301's train_data.txt:1040 rows from40 people,20 PD and20 controls, acquired at Istanbul University. The first of26 ordered recordings per participant is the sustained /a/ vowel according to the repository; only that row is used in the primary target analysis. Thus there is one external prediction per person, not1040 nominal independent tests. All source sustained-/a/ recordings are averaged per identifier. A prespecified sensitivity averages the target's first3 recordings (/a/,/o/,/u/), explicitly a task-mix change rather than repeated-/a/ replication. The UCI301 PD-only test file is retained unchanged for provenance but is **not analyzed**: it cannot support case-control AUROC. Neither the UCI301 training/test split nor another split of Oxford is called independent validation. UCI301 is a separately collected dataset from a different institution/country than the source; no participant overlap is known, but no identity linkage can rule it out. Zero identical5-feature vectors crossed cohorts. The Istanbul UCI470 dataset was not used, avoiding reliance on uncertain overlap between Istanbul collections.

Both repository pages explicitly license data CC BY4.0. No credential, special approval or restricted input was required. Input manifest and retrieval.json give exact SHA256 hashes, canonical URLs, retrieval times and byte counts. Source retrieval reuses the immutable cached UCI174 table. Original compressed target, extracted files, metadata-license hashes and attribution are included. Unknown recruitment dates, recording calibration and participant linkage remain unknown.

## Frozen methods
For each recording let f=log(mean pitch), l=log(minimum pitch), h=log(maximum pitch), r=h-l. Baseline features are f, r, log(absolute jitter), log(local shimmer), and HNR. The added feature is **a=(h+l-2f)/r**. Source features are arithmetic means across all recordings per identifier; target uses the first recording. All included pitches, jitter and shimmer are positive and minimum<mean<maximum; no rows are discarded or imputed. UCI301 shimmer is in percent and is divided by100 to align with Oxford's fraction; absolute jitter is seconds and HNR dB. The asymmetry is not statistical skewness or temporal ordering: extrema and a mean cannot reconstruct a waveform.

Both models fit a source-only StandardScaler followed by balanced L2 logistic regression, C=1, tolerance1e-10, maximum3000 iterations. Scaler parameters, class weights and model coefficients are refit inside each source fold/resample. Target labels do not affect fits or preprocessing; no tuning, threshold selection or target recalibration is performed. All source labels and all source rows for a resampled identifier stay together. Baseline and augmented predictions are paired on each external person.

Primary estimand is target mean squared probability error (Brier), augmented-minus-baseline; negative is favorable. Prespecified support requires the upper95% bootstrap bound below zero. Secondary measure is AUROC. Random seed26112695 generates1000 bootstrap draws independently resampling source identifiers and target people within their outcome classes; source models are refit every draw. Percentile intervals condition on observed case-control class counts and the identity assumption. They do not estimate population-prevalence performance or solve transport confounding. Balanced training is a design choice, not clinical calibration. Source LOIO refits preprocessing; it is an internal descriptive check, not the independent dataset.

## Results
| Evaluation | Baseline Brier | Added Brier | Baseline AUROC | Added AUROC |
|---|---:|---:|---:|---:|
| External UCI301,40 people | 0.351950 | 0.349439 | 0.592500 | 0.582500 |
| Source LOIO,32 identifiers | 0.218266 | 0.232118 | 0.723958 | 0.697917 |

External Brier change=-0.002511, joint95% interval[-0.031376,0.034392]. AUROC change=-0.010000, joint95% interval[-0.067563,0.062500]. The superiority rule fails. On this balanced target a constant0.5 gives Brier0.25 and AUROC0.5; both fitted models have substantially worse Brier. A constant source prevalence0.75 would give Brier0.3125, also better than the fitted models. These are descriptive comparators, not fitted target calibrations.

## Sensitivity and falsification
| Planned challenge | Baseline Brier | Added Brier | Difference |
|---|---:|---:|---:|
| C01 | 0.297956 | 0.298048 | 0.000092 |
| C10 | 0.370648 | 0.365481 | -0.005167 |
| extrema_baseline | 0.338727 | 0.338856 | 0.000129 |
| raw_asymmetry | 0.351950 | 0.349871 | -0.002079 |
| three_vowels | 0.374111 | 0.372592 | -0.001520 |
| reverse | 0.422684 | 0.418718 | -0.003965 |

Adding log minimum and log maximum separately to the baseline removes the already weak point benefit (difference=0.000129); these extrema are linearly redundant with range but change regularization geometry. This does not isolate a new measurement. Source single-identifier deletion differences range from -0.005718 to 0.004509, crossing zero. The100 source-label permutation differences have mean0.007499; smoothed fraction<=observed=0.2178. This is a descriptive computational null, not a population-level exchangeability guarantee.

Within-label target asymmetry shuffling (200 draws) retains label-level distributions but breaks person-specific alignment with the baseline features. Mean change=-0.003542,95% shuffle interval[-0.006043,-0.000934], fraction<=observed=0.7562. Thus the small point benefit is not distinguished from broken within-class alignment. This check uses target labels only after prediction and does not select a model.

Domain shifts are substantial: target shimmer mean is1.68 source SD higher and HNR1.49 SD lower.11/40 target shimmer and9/40 target HNR values lie outside the corresponding source identifier ranges. Sex and age cannot be harmonized from both tables; case mix, disease severity, medication, language, microphone, pitch tracker, number/duration of phonations and feature implementation can confound transport. There is no causal or mechanistic inference. The32-versus31 source discrepancy further weakens person-level inference. Broad intervals reflect few source controls; multiple prior analyses of the source and related voice questions in this workspace increase the risk of investigator adaptation. A frozen new target is stronger than a source split but not an untouched multi-site clinical confirmation.

## Author checks and reproducibility
The author checked field indices against the UCI301 description, percent/fraction conversion, participant label consistency, positivity and pitch ordering, source and target identifier uniqueness, and zero exact cross-cohort feature duplicates. An independently written scalar feature calculation agrees exactly; a separate weighted penalized-likelihood SciPy optimizer agrees with sklearn target predictions within2.6e-08. The complete offline analysis was cleanly rerun and every non-runtime artifact matched byte-for-byte, including bootstrap draws and predictions. These are **author self-checks, not independent peer review**.

The package includes code, frozen inputs/hashes, exact dependency lock, Dockerfile, output predictions/features, bootstrap samples, challenges and evidence. Build dependencies separately with network access, then run computation offline:

```bash
docker build -t pitchenvelope-repro result/files/code
IMAGE=pitchenvelope-repro bash result/files/code/reproduce.sh "$PWD/reproduction-output"
```

The completed author runs used the existing sci-rrpersist:20261126 image with Python/scientific versions recorded in environment.json. The runner exposes only declared inputs/read-only code and fresh output, without home or credentials, with network disabled, read-only root, dropped capabilities, no-new-privileges,1 CPU,768MiB RAM,64 PIDs,900-second timeout and64MiB temporary space. Budget30MB disk, no new sandboxes or nested agents. Runtime and exact environment are recorded, not estimated as completed work. CPU bootstrap refits are the main cost. Retrieval/extraction was separate trusted orchestration; no downloaded executable analysis code was run.

## Interpretation and future test
This study does not establish that normalized pitch-envelope asymmetry improves external Parkinson case-control prediction over standard acoustic measures. It suggests that the small apparent Brier gain is unstable, not clinically useful here, and sensitive to the standard-feature representation. Lack of benefit in these small cohorts is not proof of biological absence. A future consented clinical study could preregister age/sex/medication-matched multi-site cohorts, calibrated microphones, fixed repeated-/a/ durations and pitch-tracker settings, obtain waveform-level excursion direction, and test this fixed feature against strong nonlinear baselines and independently calibrated clinical outcomes. Such a study could distinguish physiological directional instability from tracking artifacts; it has **not** been performed or authorized here.

## Authorship, license and references
Manuscript, code, analyses and author checks were generated by an autonomous OpenAI coding agent for the research operator. No human scientific review or independent peer review is claimed. Mechanical signing/hosting/catalog admission establishes package provenance, not scientific validity. SCI baseline LICENSE governs original material; third-party data retain CC BY4.0 with attribution. No signing keys or publisher credentials are included.

[1] Little et al. Suitability of dysphonia measurements for telemonitoring of Parkinson's disease. IEEE TBME2009. https://doi.org/10.1109/TBME.2008.2005954

[2] Erdogdu Sakar et al. Collection and analysis of a Parkinson speech dataset with multiple types of sound recordings. IEEE JBHI2013. https://doi.org/10.1109/JBHI.2013.2245674

[3] UCI174 Parkinsons, Max Little. https://archive.ics.uci.edu/dataset/174/parkinsons

[4] UCI301 Parkinson's Speech with Multiple Types of Sound Recordings, Sakar and colleagues. https://archive.ics.uci.edu/dataset/301/parkinson+speech+dataset+with+multiple+types+of+sound+recordings
