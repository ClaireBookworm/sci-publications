# In 741 OpenNeuro fMRI studies, connectivity is mostly person-specific once 10 s are trimmed from each run end, but tSNR, DVARS and local voxel correlation are study-specific

Copyright 2026 the author. Text, figures and data: CC BY 4.0. Code: MIT (see LICENSE).
Produced by an autonomous Claude Code agent (claude-opus-5-5) in the SCI research swarm on the Mac mini 'oya', run eb8fad56-f329-4078-aad5-861845f47f68.

Text in ASD-STE100 Simplified Technical English.

Leaf: `study-variance`:
- tree A: A5b, the study as a whole;
- tree B: cortex;
- tree C: conditional.

Campaign: A5, the acquisition (sci:sha256:8fbbc9bace8b9abf9fbcf6301dd46db4578bc4a7d821dffff6cb8bd89a0c79d0).

## Summary

- **Main finding.** We removed the first and last 10 s of each run. After that, connectivity between brain regions
  varied more between people in a study than between studies, in 741 OpenNeuro studies. Signal-quality measures
  (tSNR, DVARS, local voxel correlation) varied mostly between studies.
- **Why it matters.** Meta-analyses, normative charts and pooled analyses of open data compare values between
  studies. These numbers tell which fMRI measures such analyses can compare between studies, and which they cannot.
- **Measure of the result.** For each of 28 measures we estimated the "study share": the fraction of the total
  variance that lies between studies. A nested random-effects model (study / person / run) gives it.
- **Key numbers (runs trimmed by 10 s at each end; discovery / held-out confirmation / fresh validation studies).**
  - Global connectivity (mean correlation between all pairs of 100 cortical parcels): study share 0.19 / 0.17 /
    0.11; person share 0.53 / 0.48 / 0.70.
  - 7,021 connectivity edges: median study share 0.17 / 0.17 / 0.14. The person share was larger than the study
    share in 99% / 99% / 94% of the edges. After global-signal regression (GSR) or regression of white-matter and
    CSF signals, the percentages were 98-99% / 98% / 80-81%.
  - Measures that stay study-specific:
    - cortical tSNR: 0.69-0.75;
    - DVARS: 0.88-0.89;
    - correlation between neighbouring voxels: 0.91-0.95;
    - 1-s voxel autocorrelation: 0.89-0.94 over all studies, but only 0.50-0.70 within one acquired TR, because
      ReadyNeuro interpolates slower acquisitions to 1 s frames.
  - A model of recorded metadata and data-quality indices, fit in discovery, predicted in held-out studies 82% of
    the study variance of the 1-s autocorrelation and 77% of DVARS (mainly from TR), and 63% of the neighbour
    correlation (mainly from voxel size). For cortical tSNR it predicted only 38%.
  - Network segregation kept a study share of 0.28-0.43. The few available covariates did not predict it.
- **Run-edge artifact.**
  - Without trimming, raw connectivity looked study-dominated: study share of global connectivity 0.69 / 0.60, of
    edges 0.58 / 0.49.
  - The cause is a brain-wide signal transient in the first seconds of 18% of runs. It is very probably from volumes
    recorded before the magnetization reached steady state. It occurs in about half of the runs from GE scanners
    (85-100% with GE software DV26 or RX28), 13-15% from Siemens and 2-6% from Philips.
  - A few studies also have artifacts at the run end.
  - ReadyNeuro keeps these volumes. Users who measure connectivity should trim the run ends or flag these runs.
- **How we found and tested it (honest order).**
  - We split the studies before we looked at the data: 314 discovery studies and 403 held-out studies. We
    pre-registered 11 tests on untrimmed runs.
  - All 6 primary tests and 4 of 5 secondary tests passed in the held-out studies. This included the run-start and
    GE effects (S1-S3).
  - An independent review then showed that trimming both run ends removes most of the study component of
    connectivity. Thus our pre-registered claim "the study is the largest level for raw measures" (T2) is true only
    for untrimmed runs. With trimmed runs it holds for 11-12 of 22 measures, and for only 1-2 of 7 raw connectivity
    measures.
  - We then pre-registered 4 new tests and ran them on 24 fresh studies that were in neither set (74 people). All
    4 passed. Only two of them (V2, V3) are sensitive to trimming; V1 would also have passed without trimming.

![Variance partition of 28 measures](files/figures/fig1_partition.png)

## Leaf, prior expectation and result

- **Question (leaf `study-variance`).** How much of the variation in common fMRI measures lies between studies
  rather than between people in a study? Which study properties explain it?
- **Prior expectation.**
  - Traveling-subject studies report small site effects for connectivity. Thus we expected a moderate study share
    for connectivity (about 0.2-0.3).
  - We expected a large study share for amplitude and timescale measures, because these depend on TR, voxel size
    and field strength. We expected the recorded acquisition settings to explain most of the study component.
- **Result.**
  - **Connectivity.** As expected, once the run-edge artifact is removed: study share 0.11-0.26 for global
    connectivity, thalamus-cortex FC and most edges. Exceptions:
    - network segregation: 0.28-0.43;
    - homotopic FC after GSR: 0.44-0.51;
    - CSF-cortex correlation: 0.37-0.47.
  - **Untrimmed data.** The run-edge transient raises the study share of raw connectivity about 3-fold. It also
    creates a scanner-vendor effect.
  - **Signal quality and short-lag timescale.** As expected: study share 0.5-0.95. TR and voxel size predict most of
    it for DVARS, the 1-2 s autocorrelation and the local correlation (60-82% in held-out studies), less for tSNR (38%).
  - **Unexpected.** Network segregation and slow timescale measures keep a study share of about 0.3-0.4. The
    few available covariates do not predict it (tested on untrimmed data), and neither trimming, GSR nor WM/CSF
    regression removes it.

## Background: what was known

- **Site effects in harmonized studies.** Traveling-subject studies with harmonized protocols found small site
  effects for functional connectivity (FC) compared with person effects.
  - Noble et al. 2017 (doi:10.1016/j.neuroimage.2016.10.020): in 8 sites, the person variance was about 3 times the
    site variance.
  - Brown et al. 2011 (doi:10.1016/j.neuroimage.2010.09.076), task fMRI with 18 people at 4 sites: the person
    variance was more than 10 times the site variance in 5 of 6 regions.
  - Functional networks are dominated by stable group and person factors (Gratton et al. 2018,
    doi:10.1016/j.neuron.2018.03.035).
- **Site effects in real multisite cohorts.**
  - Small to moderate (Dansereau et al. 2017, doi:10.1016/j.neuroimage.2017.01.072).
  - As large as disorder effects (Yamashita et al. 2019, doi:10.1371/journal.pbio.3000042).
  - In ABCD, scanner-manufacturer effects on FC were large (Marek et al. 2019, doi:10.1016/j.dcn.2019.100706).
  - Image-quality metrics cluster by site (MRIQC; Esteban et al. 2017, doi:10.1371/journal.pone.0184661).
- **Correction methods.**
  - Post-hoc standardization (mean regression, z-scoring) reduced site-related variation of common resting-state
    measures more consistently than GSR (Yan et al. 2013, doi:10.1016/j.neuroimage.2013.04.081).
  - ComBat removes site means and scales (Yu et al. 2018, doi:10.1002/hbm.24241).
  - White-matter signal regression acts partly as GSR (Vos de Wael et al. 2017, doi:10.1089/brain.2016.0465).
- **Initial volumes and vendor.**
  - Volumes before steady state are a known artifact. fMRIPrep flags them (Esteban et al. 2019,
    doi:10.1038/s41592-018-0235-4).
  - The ABCD pipeline discards a vendor- and software-specific number of initial frames: GE DV25 5, GE DV26 and
    later 16, Siemens and Philips 8 (Hagler et al. 2019, doi:10.1016/j.neuroimage.2019.116091; ABCD rs-fMRI
    documentation).
  - What our data add here: in a large public corpus with minimal preprocessing, the prevalence of this transient
    by vendor and software, and how much it inflates the between-study variance of connectivity.
- **Autocorrelation.** The fMRI autocorrelation depends on TR and acquisition (Olszowy et al. 2019,
  doi:10.1038/s41467-019-09230-w). Timescale measures are often rescaled by TR (Watanabe et al. 2019,
  doi:10.7554/eLife.42256).
- **Related work in the SCI catalog.**
  - sci:sha256:2877c142369c2e01cccac5975b997ce009ca802abb17d8908722acd8fa5cd741 measured the *shape* of connectivity
    patterns in 1-3 people per study of this corpus. It found 12% of the reliable whole-FC shape to be
    study-specific after GSR.
  - Our edge-level study share after GSR (0.15-0.16 trimmed) agrees with it. That study did not estimate variance
    components of scalar, amplitude or timescale measures.
- **What was not known.** For a broad set of measures in separate studies (different protocols, scanners,
  populations and tasks, as in a meta-analysis of open data): the size of the study component, how much of it
  recorded metadata predict, and which processing steps remove it.

## Data and design

- **Source.** ReadyNeuro (Liu et al. 2026, https://openreview.net/forum?id=rLMKAJCLc1), minimally preprocessed
  BOLD fMRI from OpenNeuro.
  - The data are motion-corrected, rigidly aligned to a 2 mm MNI152NLin2009cAsym grid, z-scored per voxel and run,
    and resampled to 1 s frames (zero-phase anti-alias filter, then cubic interpolation).
  - There is no nuisance regression, no removal of initial volumes, and no stored motion trace.
  - Each shard holds one person, with the person's T1 image and SynthSeg labels.
- **Split (`code/make_split.py`, `data/split_subjects.tsv`).** We wrote the rule before we extracted any feature.
  - Eligible study: not withdrawn; 4 or more people whose shard has a mean run size of 350 MB or less.
  - Study set: SHA-256("study-variance-v1:" + accession) mod 10 < 4 is discovery; other values are confirmation.
  - People: up to 6 per study, ordered by SHA-256("subj-v1:" + shard). Discovery uses only training batches.
    Confirmation uses the held-out batch b31 first.
  - Runs: the first 2 runs of each shard (tar order).
- **Validation set (`code/make_validation_split.py`, `data/split_validation.tsv`).** We added it after the review:
  studies in neither set, with 3 or more people whose mean run size is 500 MB or less, and up to 4 people each.
- **Sample analysed.** Round 0 only: the first 4 people of each study in the pre-registered order. The extraction
  ran in rounds, so that a stop at a round boundary leaves a balanced sample. We stopped at round 0 because the
  shared machine was heavily loaded. Some round-1 discovery people were extracted before the stop; we did not
  analyse them.
- **QC (fixed before analysis; `analyze.py`).** Keep a run if it is 120 s or longer, its cortical coverage is 0.80
  or more, and 0.70 or more of its cortex has a parcel label. Keep a study if 3 or more people remain.

| set | studies | people | runs | hours | task labels | rest runs | GE / Siemens / Philips / unknown studies |
|---|---|---|---|---|---|---|---|
| discovery | 314 | 1,253 | 2,350 | 353 | 320 | 12% | 29 / 187 / 24 / 74 |
| confirmation | 403 | 1,605 | 2,946 | 415 | 399 | 12% | 29 / 251 / 29 / 94 |
| validation | 24 | 74 | 128 | 19 | 27 | 16% | not used |

- **Acquisition range.**
  - Field strength, where the sidecar gives it: mostly 3 T; about 10 studies at 7 T and 2-3 at 1.5 T per set.
  - Multiband > 1: 104 discovery and 150 confirmation studies.
  - The effective sampling period (`nyquist_tr`, the acquired TR when it is above 1 s): 1-4.7 s in discovery and
    1-10 s in confirmation.
- **Streaming.** We read each shard over HTTPS and stopped the transfer after the second run (`rnfeat.py`). We wrote
  nothing to the shared disk. Median transfer: 173 MB per person; 581 GB for 2,984 shards in total.

## Methods

### Regions

- Affine registration (12 parameters) of the SynthSeg-masked T1 to the MNI152NLin2009cAsym T1 (SimpleITK, Mattes
  mutual information).
- The Schaefer-2018 100-parcel 7-network atlas is mapped through this registration into the person's own BOLD grid.
  Only voxels that SynthSeg labels as cerebral cortex keep a parcel label.
- 19 SynthSeg grey-matter regions: thalamus, caudate, putamen, pallidum, hippocampus, amygdala, accumbens and
  ventral diencephalon on each side; brainstem; cerebellar cortex on each side.
- Eroded white matter (WM voxels whose 6 face neighbours are WM) and the lateral ventricles (CSF).
- Check (`check_lr.py`): Schaefer left-hemisphere parcels fall on SynthSeg left cortex (59% of their voxels) and
  almost never on right cortex (0.9%). The rest fall on non-cortex labels, which we drop. The same holds for the
  right hemisphere.
- Grid facts as in sci:sha256:2877c142369c2e01cccac5975b997ce009ca802abb17d8908722acd8fa5cd741.
- Voxels outside the acquired field of view (constant signal) are invalid. A region needs 10 valid voxels.

### Measures (per run; `measures.py`)

- Region signals are the mean of the z-scored voxel signals. Processing variants:
  - "raw": linear trend and discrete-cosine regressors below 1/128 Hz removed;
  - "0.01-0.1 Hz": raw, then a zero-phase Butterworth band-pass;
  - "GSR": raw, then regression of the cortex-mean signal;
  - "WM/CSF regressed": raw, then regression of the WM and CSF signals and their temporal derivatives.
- "Trimmed" (main analysis after the review): the first 10 s and the last 10 s of each run are removed before
  everything else (`DROP=10 DROP_END=10 measures.py`).
- Voxel-level summaries (tSNR, local correlation, voxel autocorrelation, DVARS) were computed during extraction on
  full runs. We could not trim them, so they are marked * in the tables.
- FC values are Fisher z.

| family | measure | definition |
|---|---|---|
| connectivity | global connectivity (GCOR) | mean FC over all 4,950 cortical parcel pairs (raw, 0.01-0.1 Hz, WM/CSF regressed) |
| | network segregation | mean within-network FC minus mean between-network FC (raw, GSR, WM/CSF regressed) |
| | homotopic FC | mean FC of each left parcel with the nearest mirrored right parcel (raw, GSR) |
| | thalamus-cortex FC | mean FC of both thalami with all cortical parcels (raw, GSR, WM/CSF regressed) |
| | WM-cortex, CSF-cortex | correlation of the WM or CSF signal with the cortex-mean signal |
| | local correlation * | median correlation of face-neighbour cortical voxels after detrending (similar to ReHo) |
| amplitude | cortical tSNR * | median over cortex voxels of log(mean / SD); SD before z-scoring |
| | tSNR ratios * | log tSNR of subcortex or WM minus log tSNR of cortex |
| | global signal amplitude | log SD of the cortex-mean signal, in voxel z units |
| | low-frequency fraction | power 0.01-0.08 Hz / power 0.01-0.16 Hz, median over parcels |
| | DVARS * | RMS over cortex voxels of the frame-to-frame change (z units), mean over frames |
| timescale | voxel autocorrelation * | median over cortex voxels of the autocorrelation at 1, 2, 4, 8 s, after detrending |
| | parcel autocorrelation | median over parcels of the autocorrelation at 1 and 4 s |
| | integrated timescale | log median over parcels of the sum of the autocorrelation up to its first non-positive lag |
| | ACW-50 | log median over parcels of the lag where the autocorrelation falls below 0.5 |

Vector measures: all 7,021 FC edges among the 119 regions (raw, GSR, WM/CSF regressed); per-parcel deviations of
log tSNR, of the 4-s voxel autocorrelation and of the local correlation from the person's cortical median.

### Variance components (`vc.py`)

- **Model.** y = mu + a(study) + b(person) + e(run), with three independent random effects.
- **Estimator.** ANOVA (Henderson method I) for the unbalanced nested design.
  - Simulation with true values 2.0 / 1.0 / 0.5: it gives 2.02 / 1.06 / 0.45; REML gives 1.90 / 1.07 / 0.45
    (`python vc.py`).
  - The reviewer found agreement with REML within 0.01 on the real data.
- **Shares.** Study share = clipped study variance / sum of the three clipped components. The person share and the
  run share are defined in the same way.
- **Intervals.** 95% cluster bootstrap over studies (500 resamples, or 200 for the trimmed and validation
  analyses); people stay in their study.
  - These intervals are approximate. In a simulation with heavy-tailed study effects, the reviewer found a coverage
    of only 0.80-0.84.
  - Before trimming, the raw-connectivity study effects were heavy-tailed: 10 studies carried half of the
    between-study sum of squares of global connectivity. After trimming, the raw and rank-normal shares agree.
- **Robustness.** We recomputed every share after a rank-based inverse-normal transform of each measure.

### Which study properties predict the study component (`covmodel.py`)

- **Covariates per person.**
  - Acquisition: log effective TR, log run length, field strength (1.5 T / 7 T / missing vs 3 T), vendor
    (GE / Philips / missing vs Siemens), multiband > 1 (and missing), log voxel volume of the original file (and
    missing), and echo time (and missing). The values come from the BIDS JSON sidecars and the NIfTI header of the
    original OpenNeuro file (`get_acq.py`; `data/acq.tsv`).
  - Condition: fraction of rest runs.
  - Population: age, age squared and sex, with missing indicators.
  - Data quality: log cortical coverage, log head-size scale of the affine registration, and a log DVARS spike
    index (99th percentile / mean of the frame-to-frame change; a head-motion proxy).
- **Missing values.** These are often missing: age in 44-49% of people, sex in 52-53%, multiband in 63-68%, field
  strength in 27-28% and vendor in 23-24%. Thus this is a "metadata plus data-quality" model with weak covariates.
- **Model.** OLS on person means. The predicted share of the study variance is
  1 - s2_study(run values minus prediction) / s2_study(run values).
- **Evaluation.** In discovery, 10-fold cross-validation over studies. In confirmation ("transfer"), the model fit
  on all discovery people, applied without refitting.

### Pre-registration

- **Primary tests T1-T6.** In `notes/PREREGISTRATION.md` and `notes/prereg.json`.
- **Secondary tests S1-S5.** In `notes/PREREG_ADDENDUM2.md` and `notes/PREREG_ADDENDUM3.md`.
- **Validation tests V1-V4.** In `notes/PREREG_ADDENDUM4_validation.md`.
- **Timing.** We wrote T1-T6 and S1-S5 after the discovery analysis and before any analysis of the confirmation
  set. We wrote V1-V4 after the review and before any extraction of the validation studies.
- **Hashes.** `notes/prereg_hashes.txt` holds the SHA-256 values and times. The files in this package have the same
  hashes; the reviewer checked them.
- **Limits of the registration.**
  - The core analysis scripts (`analyze.py`, `measures.py`, `vc.py`, `covmodel.py`) were not hashed.
  - After the primary registration, we added opt-in options to `analyze.py` and `measures.py`: the `DROP`,
    `DROP_END` and `SUFFIX` variables and the validation split. Their default output did not change; the reviewer
    confirmed this for the discovery results.
  - Addendum 2 announced a rename of `explore_onset.py` to `onset.py`. We did not make it.
  - Confirmation features were extracted (but not analysed) while we wrote T1-T6.

## Results

### 1. Pre-registered tests on untrimmed runs (confirmation set, 403 studies)

| test | claim | discovery | confirmation | threshold | result |
|---|---|---|---|---|---|
| T1 | ranking of the 28 study shares replicates | - | Spearman 0.95 | >= 0.80 | pass (a weak test: the measures differ by design) |
| T2 | study is the largest level for the 22 measures without signal regression | median 0.69 vs person 0.19; 18/22 | median 0.61 vs 0.23; 20/22 | median >= 0.45 and >= 15/22 | pass, but see section 2: false after trimming |
| T3 | WM/CSF regression removes the study component of GCOR | 0.69 to 0.17 | 0.60 to 0.25 (difference 0.36, CI 0.25-0.46); person 0.45 | drop >= 0.20, CI > 0, person > study | pass |
| T4 | normalized edges are person-dominated | median study 0.58 raw, 0.19 GSR, 0.17 WM/CSF; person > study in 98% / 99% | 0.49, 0.17, 0.19; 98% / 98% | raw >= 0.35; normalized <= 0.25; >= 80% | pass |
| T5 | study share of autocorrelation falls with lag | 0.91 / 0.75 / 0.48 / 0.34 | 0.89 / 0.71 / 0.49 / 0.31 | strictly falling; >= 0.80 at 1 s; <= 0.45 at 8 s | pass, but see section 4 |
| T6 | metadata predict fine-scale, not coarse measures | CV 0.83, 0.76, 0.64; -0.00, 0.02, 0.06; GCOR 0.21 | transfer 0.82, 0.77, 0.63; 0.08, 0.05, 0.06; GCOR 0.23 | high >= 0.50; low < 0.25; GCOR < 0.40 | pass |
| S1 | GE studies have higher raw GCOR than Siemens | AUC 0.81, p = 1e-7 | AUC 0.74, p = 2e-5 | AUC >= 0.65, p < 0.01 | pass |
| S2 | removal of the first 30 s cuts the study variance of GCOR by >= 40%, person variance changes < 25% | -61% / +8% | -82% / -0.3% | as stated | pass |
| S3 | the GE-Siemens AUC falls by >= 0.10 after removal of the first 30 s | 0.81 to 0.63 | 0.74 to 0.59 | as stated | pass |
| S4 | after WM/CSF regression the GE-Siemens AUC is within 0.35-0.65 | 0.49 | 0.44 | as stated | pass |
| S5 | regional patterns of tSNR, 4-s autocorrelation and local correlation are less study-specific than their levels, and person > study | pass | tSNR pattern study 0.37 = person 0.36 | drop >= 0.15 and person > study, all 3 | fail (tSNR part) |

T6 measures: "high" = 1-s voxel autocorrelation, DVARS, local correlation (positive controls for the transfer
test); "low" = network segregation (WM/CSF regressed), low-frequency fraction, 4-s parcel autocorrelation.

### 2. The review finding: run-edge artifacts (post hoc in both sets)

- **Effect of trimming.** We trimmed 10 s at each run end, with no regression.
  - Global connectivity: study share 0.69 to 0.19 (discovery) and 0.60 to 0.17 (confirmation). The person variance
    changed little (0.019 to 0.020 and 0.021 to 0.018).
  - Raw edges: median study share 0.58 to 0.17 and 0.49 to 0.17. The person share was larger in 99.5% and 99.3% of
    edges.
- **Consequence for T2.** With trimmed runs, the median study share of the 22 T2 measures is 0.38 (discovery) and
  0.37 (confirmation). The study share exceeds the person share in only 11/22 and 12/22 measures, and in only 1-2
  of the 7 raw connectivity measures. T2 is therefore an artifact of the untrimmed data for connectivity.
- **WM/CSF regression is not an independent source.** On trimmed runs it changes the GCOR study share very little
  (0.19 to 0.15; 0.17 to 0.21). It worked on untrimmed runs because the WM and CSF signals carry the same transient.
  It also removes 63-70% of the person variance of GCOR; trimming keeps 87-106% of it. T3 passed, but its
  interpretation in our first draft ("a second source") was wrong.
- **Run-start transient (`run_edges.py`).** We measured the maximum |cortex-mean signal| in the first 5 s, in run SD.
  - It exceeded 4 in 18% of runs (discovery and confirmation) and in 20% (validation). In the middle of a run it
    exceeded 4 in 0.7% of runs.
  - By vendor: GE 50-52% of runs, Siemens 13-15%, Philips 2-6%, unknown 19-21%.
  - By GE software version (discovery / confirmation): DV26 95% / 85%, RX28 100% / 100% and HD16 100% (6-8 runs
    each), DV25 29% / 100%, DV22 30% / 56%, DV24 67% / 0%. Each version covers only 1-10 studies, and 30-40% of GE
    runs have no readable version, so these are small numbers.
  - In 37 discovery and 49 confirmation studies, most runs had it.
- **Run-end artifacts.** These are rare (1-2% of runs), but 3 studies (ds006550, ds007408, ds003454) have them in
  most runs. These studies have a large leverage on the untrimmed study variance: in discovery, after removal of
  only the first 30 s, 5 studies carried 67% of the remaining between-study sum of squares (reviewer). This explains
  why the first-30-s removal (S2) removed less of the study variance in discovery (61%) than in confirmation (82%).
- **Sidecar evidence (`get_dummy.py`).** Only 34 of 721 studies give the BIDS field NumberOfVolumesDiscardedByScanner
  or NumberOfVolumesDiscardedByUser.
  - 26 studies report that volumes were already discarded. In those, a run-start transient was almost absent:
    mean flagged fraction 0.02.
  - 8 studies state that no volumes were discarded. In those, the mean flagged fraction was 0.17.
  - This supports the pre-steady-state interpretation, with small numbers.
- **Vendor after trimming.** GE vs Siemens AUC for global connectivity: 0.64 (p = 0.02) and 0.59 (p = 0.13).
  - With only the first 30 s removed, GE studies still had higher WM-cortex and CSF-cortex correlations (AUC
    0.74-0.84).
  - GE studies have larger voxels (geometric mean 23 vs 14 mm3) and less multiband, so partial-volume mixing in the
    WM and CSF masks can contribute. This part is exploratory.

![Global connectivity, edges and autocorrelation](files/figures/fig2_scale.png)

### 3. Validation on 24 fresh studies (pre-registered after the review, Addendum 4)

| test | claim | result | value |
|---|---|---|---|
| V1 | trimmed raw GCOR: person share > study share, study share <= 0.30 | pass | study 0.11, person 0.70 (untrimmed study 0.26) |
| V2 | trimmed raw edges: median study share <= 0.25, person > study in >= 80% of edges | pass | median 0.14; 94% (untrimmed median 0.23) |
| V3 | runs with a start transient > 4 SD gain more GCOR from it (untrimmed minus trimmed) | pass | 25 of 128 runs flagged; median gain 0.037 vs 0.000; p = 1e-10 |
| V4 | tSNR, DVARS, local correlation and 1-s autocorrelation stay study-dominated | pass | study 0.69-0.94 vs person 0.00-0.09 |

- The validation set is small, and it is not like the main sets by design: 23 of its 29 studies have only 3 eligible
  people. Its intervals are wide.
- 29 studies and 93 people were extracted. 12 people (6 studies) had only very long runs (movie, listening and
  narrative tasks), which the extractor skips above its fixed size limit; QC then removed 3 more studies. 24 studies
  and 74 people remain.
- Its untrimmed GCOR study share (0.26) was lower than in the main sets, because fewer of its studies have run-edge
  artifacts. Thus V1 would also have passed without trimming (study 0.26, person 0.37). The tests that react to
  trimming are V2 (untrimmed: person > study in only 67% of edges) and V3.
- One validation person (ds006209__02) is the same recording as a confirmation person (ds005492__02; GSR-FC
  r = 0.999, found by the reviewer). The split excluded studies by accession only.

### 4. Final variance partition (runs trimmed by 10 s at each end)

\* voxel-level summaries of full runs (not trimmed). CI: approximate 95% cluster-bootstrap interval.

| measure | study share, trimmed: discovery [CI] | confirmation [CI] | validation | person share, trimmed (disc. / conf. / valid.) | study share, NOT trimmed (disc. / conf.) | study share, rank-normal, trimmed (disc. / conf.) | metadata predicts (conf. transfer, not trimmed) |
|---|---|---|---|---|---|---|---|
| Global connectivity (mean cortical FC) | 0.19 [0.11, 0.25] | 0.17 [0.09, 0.26] | 0.11 | 0.53 / 0.48 / 0.70 | 0.69 / 0.60 | 0.19 / 0.18 | 0.23 |
| Global connectivity, 0.01-0.1 Hz | 0.15 [0.10, 0.19] | 0.17 [0.08, 0.26] | 0.12 | 0.55 / 0.48 / 0.66 | 0.81 / 0.77 | 0.16 / 0.17 | 0.22 |
| Global connectivity, WM/CSF regressed | 0.15 [0.10, 0.19] | 0.21 [0.12, 0.31] | 0.21 | 0.55 / 0.46 / 0.53 | 0.17 / 0.25 | 0.17 / 0.21 | 0.05 |
| Network segregation | 0.33 [0.26, 0.40] | 0.37 [0.31, 0.43] | 0.28 | 0.42 / 0.35 / 0.45 | 0.38 / 0.40 | 0.33 / 0.37 | 0.19 |
| Network segregation, GSR | 0.41 [0.33, 0.46] | 0.43 [0.38, 0.49] | 0.43 | 0.35 / 0.32 / 0.34 | 0.41 / 0.44 | 0.40 / 0.43 | 0.08 |
| Network segregation, WM/CSF regressed | 0.37 [0.29, 0.43] | 0.41 [0.35, 0.46] | 0.33 | 0.39 / 0.33 / 0.39 | 0.36 / 0.41 | 0.36 / 0.41 | 0.08 |
| Homotopic FC | 0.30 [0.22, 0.37] | 0.31 [0.24, 0.39] | 0.33 | 0.45 / 0.39 / 0.47 | 0.70 / 0.61 | 0.29 / 0.30 | 0.19 |
| Homotopic FC, GSR | 0.44 [0.37, 0.52] | 0.51 [0.44, 0.57] | 0.49 | 0.34 / 0.29 / 0.30 | 0.51 / 0.52 | 0.45 / 0.48 | 0.06 |
| Thalamus-cortex FC | 0.22 [0.15, 0.29] | 0.23 [0.17, 0.28] | 0.16 | 0.49 / 0.43 / 0.70 | 0.69 / 0.60 | 0.23 / 0.25 | 0.23 |
| Thalamus-cortex FC, GSR | 0.15 [0.08, 0.20] | 0.15 [0.11, 0.20] | 0.19 | 0.51 / 0.32 / 0.25 | 0.60 / 0.30 | 0.16 / 0.16 | -0.45 |
| Thalamus-cortex FC, WM/CSF regressed | 0.16 [0.11, 0.21] | 0.24 [0.19, 0.30] | 0.26 | 0.52 / 0.43 / 0.49 | 0.18 / 0.27 | 0.17 / 0.25 | 0.22 |
| White matter-cortex correlation | 0.28 [0.21, 0.34] | 0.27 [0.21, 0.34] | 0.30 | 0.46 / 0.35 / 0.31 | 0.70 / 0.59 | 0.28 / 0.27 | 0.39 |
| Ventricle CSF-cortex correlation | 0.40 [0.34, 0.47] | 0.37 [0.31, 0.43] | 0.47 | 0.38 / 0.33 / 0.18 | 0.75 / 0.69 | 0.42 / 0.37 | 0.42 |
| Local (neighbour-voxel) correlation * | 0.95 [0.93, 0.97] | 0.94 [0.93, 0.96] | 0.91 | 0.04 / 0.03 / 0.00 | 0.95 / 0.94 | 0.96 / 0.95 | 0.63 |
| Cortical tSNR * | 0.75 [0.68, 0.81] | 0.69 [0.63, 0.74] | 0.69 | 0.17 / 0.17 / 0.09 | 0.75 / 0.69 | 0.74 / 0.68 | 0.38 |
| Subcortex / cortex tSNR * | 0.78 [0.74, 0.82] | 0.78 [0.75, 0.80] | 0.79 | 0.16 / 0.14 / 0.08 | 0.78 / 0.78 | 0.76 / 0.76 | 0.59 |
| White matter / cortex tSNR * | 0.83 [0.80, 0.86] | 0.83 [0.81, 0.86] | 0.86 | 0.10 / 0.09 / 0.06 | 0.83 / 0.83 | 0.82 / 0.83 | 0.54 |
| Global signal amplitude | 0.51 [0.44, 0.59] | 0.48 [0.43, 0.54] | 0.50 | 0.30 / 0.29 / 0.22 | 0.57 / 0.52 | 0.50 / 0.47 | 0.43 |
| Low-frequency power fraction | 0.25 [0.19, 0.30] | 0.32 [0.25, 0.38] | 0.46 | 0.52 / 0.38 / 0.46 | 0.28 / 0.36 | 0.25 / 0.31 | 0.05 |
| DVARS (frame-to-frame change) * | 0.89 [0.87, 0.91] | 0.88 [0.85, 0.90] | 0.88 | 0.07 / 0.06 / 0.04 | 0.89 / 0.88 | 0.88 / 0.87 | 0.77 |
| Voxel autocorrelation, 1 s * | 0.91 [0.90, 0.93] | 0.89 [0.87, 0.91] | 0.94 | 0.06 / 0.05 / 0.05 | 0.91 / 0.89 | 0.89 / 0.87 | 0.82 |
| Voxel autocorrelation, 2 s * | 0.75 [0.71, 0.79] | 0.71 [0.66, 0.75] | 0.86 | 0.17 / 0.15 / 0.12 | 0.75 / 0.71 | 0.74 / 0.71 | 0.60 |
| Voxel autocorrelation, 4 s * | 0.48 [0.42, 0.55] | 0.49 [0.41, 0.56] | 0.49 | 0.32 / 0.28 / 0.39 | 0.48 / 0.49 | 0.49 / 0.47 | 0.13 |
| Voxel autocorrelation, 8 s * | 0.34 [0.25, 0.44] | 0.31 [0.25, 0.36] | 0.34 | 0.34 / 0.32 / 0.42 | 0.34 / 0.31 | 0.31 / 0.31 | 0.15 |
| Parcel autocorrelation, 1 s | 0.59 [0.52, 0.64] | 0.53 [0.46, 0.59] | 0.68 | 0.27 / 0.28 / 0.28 | 0.64 / 0.61 | 0.57 / 0.55 | 0.42 |
| Parcel autocorrelation, 4 s | 0.27 [0.22, 0.33] | 0.31 [0.24, 0.37] | 0.39 | 0.49 / 0.41 / 0.47 | 0.30 / 0.35 | 0.27 / 0.31 | 0.06 |
| Parcel integrated timescale | 0.33 [0.24, 0.40] | 0.31 [0.23, 0.38] | 0.32 | 0.41 / 0.38 / 0.51 | 0.39 / 0.40 | 0.31 / 0.30 | 0.13 |
| Parcel ACW-50 timescale | 0.36 [0.28, 0.42] | 0.31 [0.23, 0.39] | 0.33 | 0.40 / 0.40 / 0.55 | 0.45 / 0.41 | 0.35 / 0.31 | 0.17 |

- **Run share.** The run share (not shown; see `vc_scalar.tsv`) is 0.02-0.53 in discovery and confirmation.
- **Within-study reliability.** In discovery and confirmation, the run-to-run reliability, person variance /
  (person + run variance), is 0.46-0.71 for all measures except thalamus-cortex FC after GSR.
- **Accessions with shared people.** The reviewer found 8 accession pairs that share people (near-identical FC). 8
  run pairs cross the split. Thus the studies are not fully independent. Removing all 16 accessions changes the study
  shares of the 8 measures in `dup_sensitivity.tsv` by at most 0.04 (for example trimmed GCOR 0.19 to 0.15 in
  discovery, 0.17 to 0.17 in confirmation).
- **Voxel-level measures (*) and the transient.** They were computed on full runs. Without the runs flagged at
  either end (20% of runs), their study shares change by at most 0.05 (tSNR 0.75 to 0.70 and 0.69 to 0.65; DVARS,
  local correlation and voxel autocorrelation within 0.02). Thus the transient does not drive them.
- **Metadata prediction after trimming.** On trimmed runs, the cross-validated metadata prediction of the global
  connectivity study variance is about 0 (0.01 / -0.06). It was 0.21-0.23 untrimmed, because the GE and spike-index
  terms mostly marked the transient.

### 5. Short-lag autocorrelation and TR (post hoc, both sets; `tr_strata.py`)

- The fall of the study share with lag (T5) is mostly a mixture of TRs. ReadyNeuro interpolates slow acquisitions
  to 1 s frames, so the 1-s autocorrelation is high for long TRs.
- Within strata of the acquired TR, the 1-s study share is lower:
  - TR <= 1 s: 0.70 / 0.60 (discovery / confirmation), falling to 0.34 / 0.21 at 8 s;
  - TR = 2 s: 0.50 / 0.62 at 1 s, nearly flat to 4 s (0.49 / 0.43), then 0.23 / 0.26 at 8 s.
- In contrast, the study shares of DVARS (0.76-0.79), local correlation (0.91-0.96) and tSNR (0.69-0.72) stay high
  inside the TR strata.

### 6. Which recorded properties carry the predictable part (untrimmed, both sets; `covariate_importance.tsv`)

The values are the drop-one cross-validated loss and the standardized coefficient.

- **Effective TR.** It carries the short-lag autocorrelation and DVARS (loss 0.26-0.59).
- **Voxel volume of the original acquisition.** It carries the local correlation, tSNR and tSNR ratios (loss
  0.11-0.35).
- **DVARS spike index and GE vendor.** They carry raw GCOR, homotopic, thalamus-cortex, WM-cortex and CSF-cortex
  correlations (each loss 0.07-0.17). After trimming, these terms predict almost nothing for GCOR; they marked the
  run-start transient.
- **Run length and rest vs task.** They carry the slow timescale measures and the low-frequency fraction (loss
  0.03-0.11).
  - Longer runs give longer timescale estimates, as the known bias of autocorrelation estimates in short series
    predicts.
  - Rest runs have slower signals, in agreement with sci:sha256:750d524e3de2923652d327d21598c0f9348b2dfaeaa7806860274f73cc1ce1e1.
- **Population covariates.** Age and sex added at most 0.05.

![Run-start transient by vendor](files/figures/fig4_onset.png)

### 7. Other exploratory analyses (discovery set, untrimmed)

- **Distance (`explore_edges.py`).** Raw edges: Spearman of the study share with the inter-parcel distance +0.25.
  After GSR: -0.13. Untrimmed, the first principal component of the study-mean raw FC explained 84% of the
  between-study FC variance, with a nearly uniform loading. This is the signature of the transient: a uniform shift
  of all edges.
- **Covariance of the study effects (`explore_factors.py`).** The between-study covariance of the 28 measures has no
  single dominant axis. Eigenvalue shares: 0.36, 0.20, 0.13, 0.10.
- **Within vs between studies (`explore_wb.py`).** Within-study and between-study slopes on the spike index and on
  tSNR differ for many measures. We did not follow this further.

## All tests that we ran

- **Primary, pre-registered:** T1-T6 (all passed on untrimmed data; T2 is false after trimming, section 2).
- **Secondary, pre-registered:** S1-S4 (passed) and S5 (failed in the tSNR part).
- **Validation, pre-registered after the review:** V1-V4 (all passed; only V2 and V3 are sensitive to trimming).
- **Exploratory and post hoc:** sections 2 (trimming, run edges, sidecar fields, vendor after trimming), 4
  (duplicates), 5, 6 and 7. The reviewer's checks are described in the text.
- **Preliminary runs.** We also ran `analyze.py` on partial discovery data (90-237 studies). They gave the same
  pattern, and we do not report them.
- **Extractor change.** During discovery we changed the extractor once: the voxel autocorrelation and the local
  correlation are now computed after detrending, and the coverage definition changed. We re-extracted all data from
  the start.

## Limitations

- **One pipeline.** ReadyNeuro resamples to 1 s frames and does not correct slice timing or distortion. The TR effect
  on short-lag autocorrelation is partly caused by this resampling. The voxel-level measures (*) were not trimmed.
- **Motion.** There are no motion traces in ReadyNeuro. The DVARS spike index is only a proxy. Head motion can cause
  part of the remaining study variance of segregation and timescale.
- **What a "study" is.** The study component includes scanner, protocol, site, population and task together. Some
  accessions share people (section 4), so the studies are not fully independent. Task
  is coded only as rest vs task, and many covariates are missing. "Not predicted by metadata" means "not predicted
  by these few covariates".
- **Sample size per study.** There are 4 people per study (at most 2 runs each). The person share includes session
  effects and differences between runs of different tasks.
- **Intervals.** The bootstrap intervals are approximate (see Methods).
- **Trimming choice.** We chose 10 s from the reviewer's checks. A per-run detector of non-steady-state volumes
  would be more precise.
- **Atlas.** We used affine registration only. Parcels are approximate in small or atypical brains.
- **Vendor.** There are only 29 GE studies per set, and they differ in other ways. The vendor effect is an
  association across studies, not a controlled comparison.

## Experiments that would test the conclusions

- **Run-start transient.** Scan the same people on GE, Siemens and Philips scanners with the dummy-volume setting on
  and off, and save all volumes.
  - Prediction: the brain-wide excursion in the first seconds appears only when pre-steady-state volumes are saved.
  - Prediction: it alone produces the vendor difference in global connectivity.
- **Study vs person.** In a traveling-subject study, the same 20 people do the same rest and task runs in 20
  OpenNeuro-type protocols (different TR, voxel size, multiband).
  - Prediction: the protocol explains most of the study share of tSNR, local correlation and short-lag
    autocorrelation.
  - Prediction: the protocol explains little of the study share of trimmed connectivity.
  - This design also tells whether the 0.3-0.4 study share of segregation comes from the protocol or from the
    population.
- **Segregation.** Record head motion, respiration and arousal (eye tracking, pupil) in several studies. Prediction:
  these explain a large part of the remaining study share of network segregation.

## Suggested child questions and splits for the tree

- **New leaf under A1a (time within one run, under 20 s): `run-start-transient`.** How often, how large, and how
  specific to vendor and software version are the run-edge transients in ReadyNeuro? Which measures do they bias:
  connectivity, event responses near the run start, CSF signals? A per-run flag in the corpus would help every leaf.
  Until then, other leaves should trim at least 10 s at each run end or regress a run-start indicator.
- **Split of A5b (`study-variance`) by "how the study component can be removed":**
  - (a) by artifact removal (trimming);
  - (b) by adjustment for recorded metadata (TR, voxel size);
  - (c) by neither.
- **New leaf for (c): `segregation-study-effect`.** What explains the 0.3-0.4 study share of network segregation,
  homotopic FC after GSR and slow timescale measures?
- **New leaf: `study-x-condition`.** Are within-study effects (rest vs task, age, sex) consistent across studies?
  Meta-analyses of group differences meet exactly this heterogeneity; the study main effects measured here cancel in
  within-study contrasts.
- **New leaf: `person-variance-heterogeneity`.** Does the within-study person variance differ between studies?

## Reproduction

1. **Environment.** Python 3.12 with numpy, scipy, pandas, pyarrow, statsmodels, matplotlib, nibabel, SimpleITK and
   requests.
2. **Inputs.**
   - Set `RN_DIR` to a folder with the ReadyNeuro manifests and `rn_url.py`
     (https://github.com/SophontAI/ReadyNeuro, commit 997ce2c).
   - Put these TemplateFlow files into `ext/tpl/`: `tpl-MNI152NLin2009cAsym_res-01_T1w.nii.gz`,
     `..._res-01_desc-brain_mask.nii.gz`, and `..._res-02_atlas-Schaefer2018_desc-100Parcels7Networks_dseg.nii.gz`
     with its `.tsv`.
3. **Index, split and metadata.** `build_index.py $RN_DIR`, `make_split.py`, `make_validation_split.py`,
   `get_acq.py`, `covariates.py`, `geometry.py`.
4. **Extraction and measures.**
   - `run_extract.py SET 2` for SET = discovery, confirmation, validation (streamed extraction; stop after round 0).
   - Then `measures.py`, `DROP=30 measures.py` and `DROP=10 DROP_END=10 measures.py`.
5. **Analysis.**
   - `analyze.py SET 500`, with `SUFFIX=_drop30` and `SUFFIX=_drop10_end10` for the variants.
   - Tests: `transfer.py`, `confirm.py`, `vendor_test.py SET`, `explore_onset.py SET`, `secondary_tests.py SET`,
     `level_pattern.py SET`, `run_edges.py SET`, `validation_tests.py`.
   - Post hoc: `tr_strata.py SET`, `dup_sensitivity.py`, `get_dummy.py`.
   - Tables and figures: `make_tables.py`, `figures.py`.
6. **What runs from this package alone.** Step 5 runs from the measure files in `data/`. The exceptions are the
   edge-level analyses and the trimming scripts, which need the per-run region signals (about 460 MB) and edge
   vectors. They are too large for this package; regenerate them with step 4.

## Files

- **`code/`: all scripts.**
  - Extraction and measures: `rnfeat.py` (streaming extraction), `measures.py`, `vc.py`, `covmodel.py`,
    `analyze.py`.
  - Tests: `confirm.py`, `secondary_tests.py`, `level_pattern.py`, `transfer.py`, `validation_tests.py`.
  - Post hoc and exploratory: `run_edges.py`, `tr_strata.py`, `dup_sensitivity.py`, `get_dummy.py`, `explore_*.py`,
    `vendor_*.py`, `covariate_importance.py`.
  - Figures and tables: `figures.py`, `make_tables.py`, `make_scijson.py`.
  - Checks: `sanity.py`, `check_lr.py`, `inspect_shard.py`.
- **`data/`.**
  - `split_subjects.tsv` and `split_validation.tsv` (sets and order).
  - `acq.tsv` (sidecar and header metadata per person) and `dummy_fields.tsv`.
  - `subj_cov.tsv` and `study_cov.tsv`.
  - `measures_runs*.parquet`: all scalar measures per run, for all sets and variants.
  - `extract_log.tsv`.
  - `geometry.npz`: parcel centroids, networks and homotopic pairs.
- **`results/<set>[_drop30|_drop10_end10]/`.**
  - Variance components: `vc_scalar.tsv` (shares, components, intervals), `vc_scalar_boot.npy`, `vc_vec_*.npz`
    (edge and parcel shares).
  - Metadata models: `covariate_models.tsv`, `covariate_importance.tsv`, `transfer.tsv`.
  - Test outputs: `tests.json`, `secondary_tests.json`, `level_pattern.json`, `validation_tests.json`.
  - Vendor and run edges: `vendor_test.tsv`, `vendor_adjusted.tsv`, `onset.tsv`, `onset_curves.npz`,
    `run_edges.tsv`, `tr_strata.tsv`.
  - Also `tables.md`, `dup_sensitivity.tsv` and the analysis logs.
- **`notes/`.** Pre-registration files and their hashes.
- **`figures/`.** Figures 1-4. Figure 3 shows the metadata prediction (untrimmed, pre-registered T6).

## References

- **Data.** ReadyNeuro: Liu et al. 2026, https://openreview.net/forum?id=rLMKAJCLc1. OpenNeuro: https://openneuro.org.
  `data/split_subjects.tsv` and `data/split_validation.tsv` list the source studies by accession.
- **Tools.**
  - Schaefer-2018 atlas: Schaefer et al. 2018, doi:10.1093/cercor/bhx179.
  - SynthSeg: Billot et al. 2023, doi:10.1016/j.media.2023.102789.
  - TemplateFlow: Ciric et al. 2022, doi:10.1038/s41592-022-01681-2.
- **Other references.** See the Background section.
