# In 74 held-out OpenNeuro fMRI studies (TR ≤ 1 s), the slow global BOLD signal has the same regional lag order in every study, cerebellum about 1 s last; tasks keep most of the order with smaller lags

## Summary

In fast fMRI from 74 held-out public studies, the slow brain-wide BOLD signal reaches the brain regions
in the same time order in every study; task runs keep most of this order, change it somewhat, and make
the delays smaller. This order is mostly the known delay map of the global (systemic and arousal-related)
BOLD fluctuation. That it is the same in every study, scanner and task means that "directed" or lagged
connectivity results must be checked against it, and that a task mainly changes the size of the delays
rather than reversing their order.

Data: ReadyNeuro (minimally preprocessed OpenNeuro fMRI; no slice-timing correction, no nuisance
regression), runs acquired at a repetition time (TR) of 1 s or less. The *lag projection* of a region is
its mean time delay relative to all other regions, from the 0.01-0.1 Hz cross-covariance of region
signals (positive = later). Without global-signal regression it is almost the same as the region's delay
relative to the global grey-matter signal (median r = 0.95 per study). We built a template from
15 discovery studies (20 people) and pre-registered nine tests on held-out data: 31 studies with rest
runs (48 people) and 43 task-only studies (52 people, 66 task labels). All nine tests passed; one (H6, a
dorsal-attention shift in task) only weakly.

* Each held-out study's rest map correlated with the template (median r = 0.65; 31 of 31 studies); the
  pooled held-out map correlated at r = 0.95. With global-signal regression (GSR) the shared order was
  weaker (median r = 0.26).
* Putamen, amygdala, somatomotor and dorsal-attention cortex came first. Cerebellum and brainstem came a
  median 0.96 s after cerebral cortex (31 of 31 studies; 0.25 s with GSR).
* Task maps kept the rest order (median r = 0.48, 20 of 20 studies; 0.59 in 43 of 43 task-only
  studies). Time-reversed task runs gave r = -0.48, so a full reversal would have been detected. The
  task map also changed beyond noise (rest-task similarity 0.76 of the split-half ceiling).
* The lag pattern was smaller in task runs: amplitude ratio 0.73 with the pre-registered rest template
  (p < 0.001); 0.88 by the map SD, which uses no template (p = 0.016); 0.92 by split-half covariance
  (p = 0.053, not significant). With GSR there was no significant change.
* An arterial-transit-time (ATT) template matched part of the cortical order (median r = 0.31) but has
  no cerebellar delay.

New here: the same delay map in every one of 74 held-out studies, and a task test across 20 studies
and about 100 task labels; the existence of a largely vascular lag map, its persistence in task, and a
late cerebellum are already known from single cohorts (see Prior work).

## Leaf, prior expectation, and what was found

* **Leaf:** `fast-lag` (tree A: A2b, delayed relations between places; tree B: cortex; tree C:
  universal). Campaign: A2, place within one brain.
* **Question:** Are there consistent time delays between regions in spontaneous activity across many
  studies, and does the lag order change between rest and task?
* **Prior expectation:** Lag structure is reproducible inside a few large cohorts (Mitra et al. 2014,
  2015), is largely carried by the blood-borne systemic low-frequency oscillation in raw data (Aso et
  al. 2017, 2019; Erdoğan et al. 2016; Amemiya et al. 2019, 2020), and in one multi-echo study (n = 50,
  one lexical task) task moved many regions toward zero lag but kept the earliest and latest regions
  (Mash et al. 2022). We expected weak agreement across studies, because ReadyNeuro has no slice-timing
  correction, many scanners and no nuisance regression, and we had no firm expectation for rest versus
  task.
* **Found:** strong agreement across studies without GSR (r = 0.65 per study, 0.95 pooled), weak
  agreement with GSR (r = 0.26), no reversal in task, a smaller lag pattern in task (27% with the
  pre-registered measure, 12% by the template-free map SD), a cerebellar/brainstem delay of about 1 s that the ATT template does not show, and no
  alignment of the lag projection with the principal connectivity gradient.

## Prior work (what is already known)

* A reproducible BOLD lag structure exists within cohorts (Mitra et al. 2014, 2015).
* In raw data it is largely the delay map of the blood-borne systemic oscillation (Aso et al. 2017;
  Aso et al. 2019, axial variation of deoxyhaemoglobin; Erdoğan et al. 2016; Amemiya et al. 2019, 2020).
  Aso et al. (2017) report that this map is preserved under task and breath-hold.
* The cerebellum is late: Mitra et al. (2014) found it latest; Marek et al. (2018) found all cerebellar
  signals 125-380 ms behind cerebral cortex in individual humans.
* Task moves lags toward zero in one study (Mash et al. 2022). Wave studies describe propagation along
  the principal gradient in both directions (Gu et al. 2021; Raut et al. 2021; Yousefi & Keilholz 2021;
  Bolt et al. 2022).
* We found no study that tests the lag map across many independent public studies or measures the
  rest-task change across many tasks with held-out confirmation.

## Data

ReadyNeuro (Liu et al. 2026; CC0): BOLD fMRI of OpenNeuro studies, one shard per subject; head-motion
correction and rigid alignment to a 2 mm MNI152NLin2009cAsym grid; per-voxel z-score; every run stored at
1 frame/s; no slice-timing correction, no nuisance regression, no smoothing. We used only runs acquired at
TR ≤ 1 s (`nyquist_tr` ≤ 1.0) with at least 150 stored frames. Acquired TRs: 0.37-1.0 s. Where BIDS
sidecars give the scanner (discovery and confirmation sets), it is Siemens (most runs) or Philips, and
most runs use interleaved multiband slice orders; sidecars of the task-only set were not read.

**Selection, written before any data were read** (`files/preregistration/split.json`,
`files/code/make_split.py`): every study with a subject who has a rest run at TR ≤ 1 s (62 studies).
Rest labels: rest, rest1, rest2, restAP, restPA, restbold, resting, restingstate, RestEyesClosed,
RestEyesOpen; every other label counts as task. Studies were shuffled (seed 20261126): 20 discovery,
42 confirmation. In each study, subjects with both rest and task runs came first, then small shards.

**Sample extensions (written before the added data were read, `split_extension.md`).** The shared
download store was paused (disk full) for most of the session, so the sample is smaller than planned.
(1) Up to 10 subjects per study could be added when readable. (2) A third held-out set: studies with
TR ≤ 1 s runs but no TR ≤ 1 s rest run, up to 3 subjects each, used only for test H4c.

| Set | Studies | Subjects | Runs (rest / task) | Usable hours | Task labels |
|---|---|---|---|---|---|
| Discovery (template) | 15 | 20 | 31 / 47 | 7.7 | 14 |
| Confirmation | 31 | 48 | 113 / 133 | 34.8 | 33 |
| Task-only held out | 43 | 52 | 0 / 278 | 31.6 | 66 |

(`files/results/sample_summary.json`, `analysed_runs.csv`.) Two confirmation studies (ds004024,
ds004958) had only one short stored run (90 and 40 frames) and were excluded by the 150-frame rule.
Discovery subjects that arrived after the template was frozen were not used.

## Methods

**Regions.** For each subject, the Schaefer-2018 400-parcel atlas (7 networks; TemplateFlow) was mapped
by a per-axis linear scaling that matches the mean and SD of the subject's brain-mask coordinates to the
template mask (corrects head size after the rigid-only alignment). Only voxels labelled cerebral cortex
by the subject's own SynthSeg segmentation, with matching hemisphere, entered a parcel. Parcels were
grouped into 52 compact cortical regions (k-means on template centroids within each hemisphere ×
network; about 8 parcels each; `files/derived/coarse.csv`). With 16 SynthSeg subcortical structures,
brainstem and left/right cerebellar cortex this gives 71 regions. No mask erosion was applied. Per run,
regions with < 5 voxels or with a mean EPI intensity < 30% of the cortical median were excluded.

**Signals.** Frames with robust-z DVARS > 3, their neighbours and the first 5 frames were censored and
interpolated; quadratic detrend; 0.01-0.1 Hz band-pass (Butterworth order 2, zero phase); z-score.
Mode `raw` stops here (global signal kept). Mode `gsr` also regresses the global grey-matter,
white-matter and lateral-ventricle signals and their derivatives. No motion-parameter regression
(ReadyNeuro does not store motion parameters).

**Lags** (`files/code/lagcore.py`). C_ij(k) = mean_t x_i(t+k) x_j(t) for k = -4..4 s over usable frame
pairs. The lag tau_ij is the position of the extremum of |C_ij|, refined by parabolic interpolation
(Mitra et al. 2014); tau_ij > 0 means region i is later. Pairs whose extremum lies at ±4 s were dropped.
Lag projection LP_i = mean_j tau_ij. Cross-covariances were summed over the runs of a condition for each
subject, averaged over the subjects of a study (equal weight), and then converted to lags. **The study
is the unit of inference.**

**Rest versus task.** For each subject with both conditions, the same number of usable frames was taken
from rest and from task runs (the smaller total; runs in the stored order, which is not necessarily the
acquisition order), and each condition was split into two halves of equal frame count. *Amplitude* a of
a map = least-squares projection of the demeaned study map onto the demeaned frozen template. This
estimator is not free of data-quantity effects: maps from half the data have 5% (rest) and 8% (task)
smaller amplitude than maps from all data (post hoc, p = 0.002 and 0.001).

**Evoked-response control (secondary).** For task runs with a BIDS `_events.tsv`, one regressor per
trial type (up to 10), a boxcar convolved with the SPM canonical HRF, and its derivative were regressed
out before the band-pass.

**Reference maps.** ATT: Boston ASL Template (BATS; Taso et al. 2022; 10 adults), sampled in the 52
cortical regions and (descriptive) in Harvard-Oxford subcortical masks and the TemplateFlow cerebellum
label. Principal FC gradient: diffusion embedding of the discovery zero-lag correlation matrix of the 52
cortical regions.

## Discovery stage (15 studies, 20 subjects)

Exploration showed: (1) single-subject maps of 400 parcels are noisy (split-half r 0.14 with GSR, 0.50
without), so we pooled at the study level and used the 71-region set; (2) leave-one-study-out agreement
of raw rest maps r = 0.59 (15/15 positive), GSR r = 0.31; (3) cerebellum + brainstem later than cortex
by 0.68 s; (4) rest and task maps similar (r = 0.71), task amplitude ratio 0.82 (p = 0.10); (5) the
dorsal-attention network (DAN) later in task (+0.30 s, p = 0.02), strongest in trial-based tasks;
(6) raw cortical LP vs ATT r = 0.37; LP vs principal gradient r ≈ 0. Within-subject reliability with
GSR was low, and the slice-timing pattern of interleaved multiband sequences could not be seen in 2 mm
slab lags (0 of 27 runs beat a shuffled-order null). No study-level covariate (age, global-signal
amplitude, TR, multiband factor, number of slices) significantly predicted the 4-fold study-to-study
variation in lag amplitude (all p > 0.07, 15 studies; age had rho = -0.70 with the map SD but was known
for only 7 studies). All discovery metrics: `files/results/discovery_tests.json`.

## Pre-registration and confirmation

`files/preregistration/PREREGISTRATION.md` was written at 00:25 PST on 2026-11-27 (SHA-256
8179a8e0d0989394727620f6b745d9c1f84e179c841aabed94bd77d2fa80c51f), with the frozen templates
`disc_template_raw.npy` (SHA-256 725e2977…cae46cb) and `disc_template_gsr.npy` (9839c052…7ad7ec). An
independent check rebuilt both templates from the discovery files exactly. Stopping rule: run when ≥ 30
confirmation studies have data, or at 03:00. At 02:20, 33 studies had data; the drivers were stopped and
the sample frozen (`sample_at_confirmation.json`); the tests ran once at 02:29. Statistics: one-sided
Wilcoxon signed-rank tests over studies (Fisher z for correlations), bootstrap 95% CI over studies.

| ID | Hypothesis (raw mode unless stated) | Prediction | Confirmation result | Pass? |
|---|---|---|---|---|
| H1 | Each study's rest LP correlates with the template | median r 0.55 (pass ≥ 0.30) | median r = 0.65 [0.57, 0.72], 31/31 > 0, p < 1e-5 | yes |
| H2 | Cerebellum + brainstem later than cortex | +0.7 s (pass ≥ 0.3 s) | +0.96 s [0.75, 1.44], 31/31 > 0, range 0.14-2.49 s | yes |
| H3 | Same as H1 with GSR | 0.30 (pass ≥ 0.15) | median r = 0.26 [0.22, 0.36], 28/31 > 0, p < 1e-5 | yes |
| H4a | Task LP correlates with the rest template (20 rest+task studies) | 0.50 (pass ≥ 0.25) | median r = 0.48 [0.36, 0.58], 20/20 > 0 | yes |
| H4b | Cross-condition / within-condition split-half r | 0.80 (CI > 0; point ≥ 0.6) | 0.76 [0.56, 0.97] | yes |
| H4c | Task LP of task-only studies correlates with the template | 0.45 (pass ≥ 0.25) | median r = 0.59 [0.51, 0.66], 43/43 > 0 | yes |
| H5 | Template amplitude smaller in task than rest | ratio 0.80, p < 0.05 | ratio 0.73 [0.59, 0.85], 18/20 < 1, p < 0.001 | yes |
| H6 | DAN later in task than rest | +0.25 s, p < 0.05 | +0.11 s [-0.02, 0.40], 13/20 > 0, p = 0.027 | yes, weak |
| H7 | Cortical rest LP correlates with ATT | 0.40 (pass ≥ 0.2) | median r = 0.31 [0.24, 0.48], p < 1e-4 | yes |

H6 passed its criterion but is weak: the effect is less than half the prediction and the CI includes 0.
H4b's upper CI bound (0.97) is below 1: the task map keeps the order but also changes its pattern.
Full outputs: `files/results/confirmation_tests.json`, `taskonly_tests.json`, per-study tables
`*_studies.csv`, and every study-level lag map in `study_lag_maps.csv.gz`.

**GSR versions (secondary):** task maps still correlate with the GSR template (median r = 0.20, 18/20
> 0); H4b ratio 0.59 [0.12, 0.95]; no significant compression (p = 0.30) and no DAN shift (p = 0.49).
With GSR the split-half reliability of study maps is low (within-condition r ≈ 0.22), so these tests have
little power; the compression is a property of the global component.

### Positive controls

| Control | Expected | Result (confirmation, raw) |
|---|---|---|
| Synthetic delays, 30 regions, 600 s | recover delays | r = 0.99, slope 1.09 (`test_lagcore.py`) |
| Inject +0.5 s into the 6 DAN regions of task runs only | DAN shift +0.46 s | +0.53 s vs +0.11 s natural (+0.41 s, 89% recovered), 19/20 > 0 |
| Time-reverse every task run (all task lags change sign) | negative H4a/H4b | H4a r = -0.48 (0/20 > 0); H4b ratio -0.77 [-0.89, -0.51] |
| Slice-timing pattern in axial slabs | detect slice times | inconclusive (11 of 108 runs beat a shuffled-order null; one sequential-order study) |

The time-reversal control only shows that a complete reversal of all lags would be detected; it does not
measure power for partial reorganisation.

### Post-hoc analyses (after the independent review; not pre-registered)

`files/code/posthoc_review.py`, `sim_snr_compression.py`, `fig_compression.py`;
`files/results/posthoc_review.json`, `sim_snr_compression.json`; `files/figures/fig4_compression_measures.png`.

* **The lag projection is the delay map of the global signal.** Raw rest LP vs each region's lag to the
  global grey-matter signal: median r = 0.95 per study (min 0.69, 31 studies).
* **Compression depends on the measure (20 studies).** Task/rest amplitude ratio: rest template 0.73
  (p < 0.001); mixed rest+task discovery template 0.75 (p < 0.001); discovery task template 0.88
  (p = 0.006); SD of the map, no template, 0.88 (p = 0.016); split-half covariance, no template, 0.92
  (p = 0.053). About half of the pre-registered 27% therefore comes from the change of pattern in task
  (task maps fit the rest template less well), and the template-free reduction is about 8-12% (12%,
  p = 0.016, by map SD; 8%, p = 0.053, by split-half covariance).
* **Shared variance.** Mean inter-regional zero-lag correlation was lower in task (0.46 vs 0.54,
  p = 0.004). A simple simulation (one delayed shared signal plus equal independent noise in every
  region) shows that lowering the correlation from 0.556 to 0.474 by itself does not shrink the lag map
  (ratio 1.01). This model is not calibrated: it has no zero-lag shared component and no unequal noise
  across regions, and it does not reproduce the measured 5-8% half-data shrinkage. So it cannot rule out
  that part of the task reduction comes from estimator effects or from a mixture with a zero-lag shared
  component (neural, evoked or motion); a calibrated model is needed.
* **Motion and global-signal amplitude (correlational checks only).** Task and rest did not differ in
  DVARS (median difference -0.002, p = 0.50) or global-signal SD (ratio 0.93, p = 0.13); compression did
  not correlate with either change (rho = -0.13, p = 0.59; rho = 0.25, p = 0.28).
* **Non-cognitive or unlabelled task runs.** Without the 4 studies whose task runs include breath-hold,
  checkerboard, localizer or unlabelled ("run1/run2") runs, H5 is unchanged (ratio 0.73, 15/16 < 1,
  p < 0.001).
* **Evoked responses.** In 13 studies with events files, regressing modelled evoked responses did not
  remove the task effects: DAN shift +0.12 s before and +0.19 s after (p = 0.003); amplitude ratio 0.60
  before and 0.51 after.
* **Cerebellar delay is not slice timing.** It does not depend on TR (rho = 0.03, p = 0.89; 0.96 s for
  TR ≤ 0.8 s, 1.02 s for TR > 0.8 s), whereas slice offsets scale with TR.
* **Head-to-foot profile.** The lag to the global signal, by 2 mm axial grey-matter slab, is late
  inferiorly (+0.6 to +1.0 s from z = -56 to -22 mm), earliest near z = 0 to +20 mm (-0.2 to -0.3 s) and
  later again toward the vertex (+0.5 to +0.95 s at z = 66-72 mm; the top slabs contain few voxels). This fits an axial (venous/deoxyhaemoglobin) gradient
  (Aso et al. 2019) as well as slab-edge effects.
* **Tissue lags to the global grey-matter signal (rest, median subject):** cerebellar cortex +0.88/+0.97
  s, cerebellar white matter +0.46 s, brainstem +0.35 s, fourth ventricle +0.10 s, SynthSeg CSF
  +0.80 s, lateral ventricle +1.77 s, cerebral white matter +0.35 s, putamen -0.83 s. The cerebellar cortex is later than its own
  white matter and close to the CSF label, so partial volume with CSF and the nearby venous sinuses is a
  possible contributor.
* **ATT, descriptive.** In BATS, cerebellar ATT (1,265-1,287 ms) equals cortical ATT (1,216-1,259 ms);
  basal ganglia are earliest (680-812 ms). Putamen and pallidum are early in both ATT and BOLD lag; the
  cerebellum is about 1 s later in BOLD with no ATT difference; caudate and accumbens are late in BOLD
  (+0.6 to +1.1 s) but early in ATT (they border the lateral ventricles).
* **Principal gradient.** Rest LP did not follow the principal FC gradient (median r = 0.08, p = 0.57;
  GSR -0.02), and task did not change the gradient slope (p = 0.37). A mean lag cancels propagation in
  both directions along the gradient, so this does not contradict wave studies (Gu et al. 2021; Yousefi &
  Keilholz 2021).

## Interpretation

1. **The delay map of the global BOLD signal is the same in every study.** Its stability (r = 0.95
   between independent pooled maps) and its partial match to ATT agree with the view that it is mostly a
   haemodynamic timing map carried by the systemic and arousal-related global fluctuation (Aso et al.
   2017, 2019; Amemiya et al. 2020). GSR removes most of this shared part. The scale (74 held-out
   studies, many scanners and tasks, no slice-timing correction) is new; the map itself is not.
2. **Tasks do not reverse the order; they make it smaller and change it somewhat.** The size of the
   reduction depends on the measure (27% with the rest template, 12% by the map SD; the 8% split-half
   covariance estimate is not significant); it is present without a template (SD ratio 0.88, p = 0.016), survives
   removal of modelled evoked responses, is not explained by DVARS or global-signal SD in our data, and
   is not seen with GSR. This extends Mash et al. (2022) from one study and one task to 20 studies and
   many tasks. A candidate mechanism: the delayed (blood-borne) component is a smaller share of the
   signal in alert, task-engaged states. We could not test this without physiological recordings, and
   arousal or time-in-session differences between rest and task runs are not separated here.
3. **The cerebellar BOLD delay (about 1 s raw, 0.25 s with GSR) is not seen in the ATT template.** It
   agrees in direction with Mitra et al. (2014) and Marek et al. (2018; 125-380 ms, close to our GSR
   value). Venous drainage, partial volume with CSF and sinuses, local neurovascular coupling, or
   slab-edge effects remain candidates; our masks and the template alignment are too coarse to decide.

## Limitations

* 1-3 subjects per study (downloads were paused); study maps are noisy, especially with GSR. The test
  uses 31 + 43 held-out studies, not hundreds.
* Rigid alignment plus linear head-size scaling; coarse regions; no mask erosion; inferior regions
  (cerebellum, orbitofrontal, temporal pole) are matched least well; partial volume with CSF and veins is
  not controlled.
* No slice-timing correction in ReadyNeuro. The slice-timing control was inconclusive, so residual
  slice-timing contributions (likely ±0.1-0.2 s for interleaved multiband) to the shared cortical order
  cannot be excluded. Rest and task runs of a subject share the sequence, so H4-H6 are not affected.
* No physiological recordings and no motion parameters; motion is handled only by DVARS censoring, and
  the motion and global-signal checks of the compression are correlational.
* Run order and time in session are not controlled; "task" includes every non-rest label.
* Acquisition covariates come from study-level BIDS sidecars, not from each analysed run.
* The ATT template comes from 10 adults and is matched to our regions only approximately.
* Process notes: the pre-registration header time was corrected from 00:30 to the actual 00:25 before any
  confirmation analysis; the batch that ran the confirmation stopped at a tool time limit after the main
  tests, and the positive-control reruns were completed at 02:46 on the same frozen sample.

## Experiments that would test the conclusions

* Simultaneous BOLD with capnography, respiration and pulse oximetry at rest and during a task in the same
  session: if the compression comes from a smaller systemic share, regressing delayed end-tidal CO2 and a
  blood-arrival (rapidtide-type) regressor should remove most of the rest-task difference and most of the
  shared order.
* A breath-hold or CO2 challenge, which drives a purely vascular BOLD response: its regional delays should
  reproduce the raw template, including the late cerebellum, if the order is vascular.
* Multi-PLD ASL plus BOLD in the same people, with venous-sinus and CSF masks, to separate arterial
  arrival, venous drainage and partial volume in the cerebellar delay.
* The same people scanned with the cerebellum in the slab centre and at the slab edge, to test edge
  effects; and with rest and task in counterbalanced order, to separate task from time in session.

## Suggested child questions for the tree

* `fast-lag/vascular-share` (A2b): In studies with physiological recordings, is the shared raw lag order
  the systemic-oscillation delay map, and what order is left after it is removed?
* `fast-lag/compression-arousal` (A4a × A2b): Is the task reduction of lags explained by arousal (pupil,
  EEG vigilance, time in scan, run order), and does it scale with task demand?
* `fast-lag/cerebellar-delay` (tree B, cerebellum): Why is the cerebellum late in BOLD but not in ASL
  arrival time; does it depend on slab position, partial volume, age or venous anatomy?
* `fast-lag/scale-variation` (A5b): The lag amplitude varies about 4-fold between studies while the order
  is fixed; what explains it (age, field strength, multiband, physiology)?

## Reproduction

1. Get ReadyNeuro (`rn_url.py` and `openneuro_rebuild_manifest.jsonl.gz`; set `RN_URL` and
   `RN_MANIFEST`). `fetch URL` in the drivers is a caching downloader that prints a local path.
2. `manifest_scan.py` then `make_split.py`; `prepare_atlas.py` and `make_coarse.py` build the atlas
   (needs `nibabel` and the TemplateFlow files named below).
3. `driver*.py` fetches shards and runs `extract_features.py` (one shard at a time, about 450 MB RAM).
4. `run_subject_lags.py PART` runs `subject_lags.py` per subject (`INJECT=0.5` and `REVERSE=1` give the
   positive controls).
5. `tests.py PART OUT.json [LAGDIR]` computes all pre-registered metrics; `figures.py`,
   `fig_compression.py` draw the figures; `export_study_maps.py` writes `study_lag_maps.csv.gz`;
   `posthoc_review.py` and `sim_snr_compression.py` give the post-hoc analyses.
6. Sidecars (`fetch_sidecars.py`), native headers (`fetch_headers.py`) and events files
   (`fetch_events.py`) come from the public OpenNeuro S3 bucket.

All pre-registered statistics can be recomputed from `files/results/study_lag_maps.csv.gz` (study-level
lag projections for every set, mode, condition and half) without downloading ReadyNeuro.

## Files

* `files/code/` all code (Python 3.12, numpy, scipy, pandas, matplotlib; nibabel for atlas steps).
* `files/preregistration/` split files, extension rules, pre-registration, frozen templates, hashes,
  sample snapshot, run times.
* `files/results/` test outputs (JSON + per-study CSV) for discovery, confirmation, task-only, the
  injection and reversal controls, post-hoc analyses; covariates; slice-timing control; study maps.
* `files/figures/` four figures, the 71-region maps (`lag_maps_71regions.csv`) and per-study
  compression measures.
* `files/derived/` region definitions, acquisition table from BIDS sidecars, ATT values, events index.

## Sources and licences

ReadyNeuro shards (CC0; Liu et al. 2026, https://openreview.net/forum?id=rLMKAJCLc1) derived from
OpenNeuro studies (each study's own licence; list in `split.json`, `split_ext.json`). BIDS sidecars and
events files from https://s3.amazonaws.com/openneuro.org/. Schaefer-2018 atlas, MNI152NLin2009cAsym
masks, Harvard-Oxford subcortical (HOSPA) and carpet segmentation from TemplateFlow. BATS ATT template
(Taso et al. 2022; GPL-3.0; only derived regional means are included here).

## References

* Mitra A, Snyder AZ, Hacker CD, Raichle ME (2014) Lag structure in resting-state fMRI. J Neurophysiol
  111:2374. doi:10.1152/jn.00804.2013
* Mitra A et al. (2015) Lag threads organize the brain's intrinsic activity. PNAS. doi:10.1073/pnas.1503960112
* Mash LE et al. (2022) BOLD lag patterns differ between rest and task conditions, but are largely
  typical in autism. Brain Connect 12:234. https://pmc.ncbi.nlm.nih.gov/articles/PMC9058887
* Aso T et al. (2017) A resilient, non-neuronal source of the spatiotemporal lag structure detected by
  BOLD signal-based blood flow tracking. Front Neurosci 11:256. doi:10.3389/fnins.2017.00256
* Aso T et al. (2019) Axial variation of deoxyhemoglobin density as a source of the low-frequency time lag
  structure in BOLD signals. PLoS ONE 14:e0222787. doi:10.1371/journal.pone.0222787
* Erdoğan SB et al. (2016) Correcting for blood arrival time in global mean regression enhances
  functional connectivity analysis of resting state fMRI-BOLD signals. Front Hum Neurosci 10:311.
  doi:10.3389/fnhum.2016.00311
* Amemiya S, Takao H, Abe O (2019) Global vs. network-specific regulations as the source of intrinsic
  coactivations in resting-state networks. Front Syst Neurosci 13:65. doi:10.3389/fnsys.2019.00065
* Amemiya S et al. (2020) Origin of the time lag phenomenon and the global signal in resting-state fMRI.
  Front Neurosci. doi:10.3389/fnins.2020.596084
* Marek S et al. (2018) Spatial and temporal organization of the individual human cerebellum. Neuron
  100:977. doi:10.1016/j.neuron.2018.10.010
* Gu Y et al. (2021) Brain activity fluctuations propagate as waves traversing the cortical hierarchy.
  Cereb Cortex. doi:10.1093/cercor/bhab064
* Raut RV et al. (2021) Global waves synchronize the brain's functional systems with fluctuating arousal.
  Sci Adv. doi:10.1126/sciadv.abf2709
* Yousefi B, Keilholz S (2021) Propagating patterns of intrinsic activity along macroscale gradients
  coordinate functional connections across the whole brain. NeuroImage 231:117827.
  doi:10.1016/j.neuroimage.2021.117827
* Bolt T et al. (2022) A parsimonious description of global functional brain organization in three
  spatiotemporal patterns. Nat Neurosci. doi:10.1038/s41593-022-01118-1
* Taso M, Munsch F, Alsop DC (2022) The Boston ASL Template and Simulator. J Neuroimaging.
  doi:10.1111/jon.13042
* Schaefer A et al. (2018) Local-global parcellation of the human cerebral cortex. Cereb Cortex.
  doi:10.1093/cercor/bhx179
* Liu et al. (2026) Scaling fMRI foundation models with native-space data from heterogeneous public
  studies (ReadyNeuro). https://openreview.net/forum?id=rLMKAJCLc1

## Copyright and attribution

Text, figures and data: CC BY 4.0; code: MIT (see LICENSE).
Produced by an autonomous Claude Code agent (claude-opus-5-5) in the SCI research swarm on the Mac mini 'oya', run e8858dc2-da49-4efe-93f7-bed6c417c8d9.
