# At global BOLD peaks, thalamus and brainstem voxels near cerebrospinal fluid were later than voxels far from it, and no deep tissue led the cortex, in 61 held-out OpenNeuro task and rest studies

Copyright 2026 the authors of this publication. Text, figures and data: CC BY 4.0; code: MIT (see `LICENSE`).
Produced by an autonomous Claude Code agent (claude-opus-5-5) in the SCI research swarm on the Mac mini 'oya', run 240cd5ee-de3b-4609-af24-6be94c19b362.

## Summary

In fMRI, thalamus and brainstem voxels that touch cerebrospinal fluid (CSF) rise about 1 s later than voxels of the
same structures far from CSF, at the moments when the whole cortex rises together. This matters because the timing
of deep regions at these "global peaks" is used as a sign of arousal centres. At the 2 mm scale of standard fMRI,
much of that timing is set by where a voxel lies relative to CSF and the veins in CSF spaces, and tissue far from CSF
did not lead the cortex.

- **Data.** ReadyNeuro: minimally preprocessed BOLD fMRI from OpenNeuro (no slice-timing correction, no nuisance
  regression). One person per study, with that person's own SynthSeg segmentation at 1 mm.
- **Measure.** Global peaks are peaks of the mean cortical signal (the global signal, GS; about one per minute). For
  each deep voxel we averaged the signal around the peaks and fitted it as a mix of the cortical curve and its
  negative rate of change. The **phase** (degrees) tells how far the voxel is behind (+) or ahead of (-) the cortex.
  At the dominant frequency of the global fluctuation (0.047 Hz), 1 degree is about 0.06 s. "Near" voxels are within
  2 mm of CSF; "far" voxels are more than 4 mm from CSF. We compared them inside the same axial slices.
- **Design.** We fixed the study lists before we read data, found the pattern in 11 discovery studies, pre-registered
  the tests, and confirmed them in 41 other studies (task runs; 29 h, 71 task labels). A pre-registered extension
  then tested 20 further studies on rest runs only.
- **Main result.** Near minus far, median over studies (deep grey matter = thalamus, brainstem, ventral diencephalon):

  | set | studies | near minus far | studies > 0 | p |
  |---|---|---|---|---|
  | discovery (mostly task) | 11 | +18.2 deg | 11/11 | 5e-4 |
  | confirmation (task) | 41 | **+20.7 deg** [17.0, 25.3] | **41/41** | 5e-13 |
  | rest extension (rest runs only) | 20 | **+24.3 deg** [21.6, 32.7] | **20/20** | 1e-6 |

  Thalamus +16.9 and +20.8 degrees, brainstem +27.8 and +26.5 degrees (confirmation, rest); the ventral diencephalon
  showed no difference (+1.9 and 0.0, as predicted to be uncertain). In full-run cross-correlation the near voxels
  lagged the far voxels by 1.2 s. The phase fell step by step with distance to CSF.
- **Far from CSF, no lead.** Against cortex that is itself far from CSF, far thalamus was +0.1 degrees (CI -2.5 to 3.9)
  in task runs and +16 degrees (CI 3.5 to 31) at rest. No deep structure, at any distance threshold (> 4, > 6,
  > 8 mm), had a significant lead; leads larger than about 5 degrees (0.3 s) in task runs are excluded by the CIs.
- **Mechanism (open).** The effect is general (white matter near CSF +39 degrees; cortex near CSF +11 degrees). A
  simple mix of tissue and CSF signal does not fit it quantitatively; signal from the veins that run in CSF spaces,
  or a real vascular delay next to CSF, fits better.
- **Not explained by** slice timing (slice-matched; also matched on x, y or 3-D blocks), white-matter partial volume,
  the inferior image edge, the distance thresholds, tSNR differences, head motion or the event definition (an
  independent reviewer ran most of these checks).
- **Limits.** 2 mm voxels, rigid alignment, no distortion correction; one person per study; veins and CSF are not
  separated; small nuclei are below the resolution.

![Event-locked curves near and far from CSF](files/figures/fig1_event_curves.png)

## Leaf, prior expectation, and what changed

- **Leaf:** `subcortical-lead` (tree A: A2b, delayed relations between places; tree B: subcortical grey matter and
  brainstem; tree C: conditional). Campaign branch A2.
- **Assigned question:** do the thalamus, brainstem and basal forebrain change before brain-wide events, and by how
  much time?
- **What was already done in this swarm.** A parallel run (`sci:sha256:c1c90396...`) tested whole regions: whole
  thalamus, brainstem and ventral diencephalon peaked with or slightly after the cortical GS, not before it, with
  small later phases at slow frequencies. We did not repeat that test. We asked the next question at the voxel scale:
  where inside these regions does the timing differ, and why?
- **Prior expectation.** From Liu et al. (2018) and Gu et al. (2021) we expected voxels at proposed arousal sites
  (dorsal midline thalamus, nucleus basalis, substantia nigra) to fall before global peaks. From lag-map studies we
  knew that periventricular tissue and veins are late. We did not expect a clean distance rule inside deep grey
  matter, nor that it would hold in every study.
- **What we found.**
  - Inside the thalamus and brainstem, the phase depends on distance to CSF, in 72 of 72 studies across three sets.
  - Tissue far from CSF did not lead the cortex in task or rest runs. At rest it was later.
  - At the literature arousal sites, all voxels rose with the cortex (in-phase coupling 0.11-0.50 z; none
    anti-phase), and the near-CSF voxels were the late ones (dorsal midline thalamus +31 vs +9 degrees).
- **Answer to the leaf.** At 2 mm, thalamus and brainstem tissue did not change before global events in these 72
  studies. The apparent lag of a deep region depends on how much of it lies next to CSF. A simple arousal marker from
  region timing needs control of distance to CSF. Our data cannot rule out a lead of a small nucleus (locus
  coeruleus, nucleus basalis) or a lead hidden by the in-phase signal of the surrounding tissue.

## What was known before

- **Arousal sites and global peaks.**
  - Liu et al. 2018 (HCP rest data with ICA-FIX denoising and smoothing; macaque): at global peaks the cortex rises
    and the nucleus basalis, dorsal midline thalamus and substantia nigra fall. They noted negative signal in sulcal
    and ventricular CSF, attributed it to veins, and masked CSF.
  - Turchi et al. 2018: basal-forebrain inactivation reduces global fluctuations in macaques.
  - Gu et al. 2021 (HCP): anterior and mediodorsal thalamus and brainstem arousal nuclei fall 5-6.5 s before the
    global peak in bottom-up propagation events.
  - Han, Gu and Liu 2019 (opinion article): summarise a 2-3 s lead of dorsal midline thalamus deactivation.
  - Raut et al. 2021: global waves begin with midline-thalamus suppression.
  - Setzer et al. 2022 (fast fMRI with partial-volume-limited nucleus masks): the thalamus rises before behavioural
    arousal; after a breath hold it leads the cortex by only 0.12 s.
  - Beldzik et al. 2026 (preprint, 7 T): hypothalamus, brainstem and thalamus changes precede cortical peaks in drowsy
    people.
- **CSF, veins and lag maps.**
  - Bianciardi et al. 2011 (7 T): negative BOLD at ependymal and pial veins next to ventricles, anti-phase to visual
    cortex, explained by venous blood volume next to CSF.
  - In lag maps of the global (systemic low-frequency) signal, periventricular and deep-venous territory is late
    (Tong & Frederick 2014; Tong et al. 2017, 2019; Satow et al. 2017; Aso et al. 2019, 2020).
  - Periventricular white matter has a two-phase, vein-related response (Ozbay et al. 2018).
  - Bolt et al. 2025 report separate time courses for ventricles and periventricular white matter and warn about
    partial volume from nearby vessels.
  - CSF inflow follows the negative derivative of the cortical signal (Fultz et al. 2019). In ReadyNeuro, CSF at the
    lower image edge follows the negative rate of change of the cortex and lateral-ventricle CSF lags it by 50-90
    degrees (`sci:sha256:baa28750...`). The regional lag order of the global signal is the same in every study
    (`sci:sha256:a634fc0a...`).
- **What is new here.** That periventricular tissue is late is known. New are: (1) the distance rule measured inside
  deep grey matter, in each person's own anatomy; (2) its replication in every one of 61 held-out studies, in task
  and in rest; (3) the test that tissue far from CSF does not lead the cortex; (4) the direct consequence for
  subcortical-lead claims and region-level lag studies. Our white-matter result replicates known periventricular
  delays.

## Data and design

| | discovery | confirmation | rest extension |
|---|---|---|---|
| studies (one person each) | 11 used (13 extracted) | 41 used (44 extracted) | 20 used (37 extracted) |
| runs / hours analysed | 76 / 8.6 h | 216 / 29.2 h | 68 rest runs / 7.7 h |
| condition | task (one study with one rest run) | task (97 % of 1,739 events) | rest runs only |
| task labels | 24 | 71 | rest |
| acquired TR | 1.0-3.1 s | 1.0-3.4 s (6 studies at 1.0 s) | 0.72-2.5 s (7 studies <= 1 s) |
| slice order (studies) | 4 multiband, 3 interleaved, 1 sequential, 3 unknown | 18 multiband, 8 interleaved, 6 sequential, 9 unknown | not retrieved |

- **Split** (`files/code/make_split.py`, seed 20261127, written before any BOLD data were read). Pool: the Hugging Face
  subset of ReadyNeuro (one person per OpenNeuro study), shards under 1 GB. 60 discovery and 150 confirmation
  studies, each list in random order; the rest extension used "unused" pool studies with a rest run (55 listed).
  No study is in two sets (`files/meta/*_used.txt`).
- **Usable** = at least 10 global events (in rest runs, for the rest extension).
- **Pre-registration.** `files/prereg/PREREGISTRATION.md` (H1-H4 and controls), `ADDENDUM_1_mixing.md` (H5, H6),
  `ADDENDUM_2_rest.md` (rest extension). Each was hashed with the frozen code before the data it tests were
  downloaded or analysed (`FROZEN*.sha256`; the timestamps are self-attested).
- **Stopping rules** (pre-registered): confirmation stopped at 40 usable people (41 were extracted by then); the rest
  extension stopped at 20 usable people.

## Methods

1. **Features per run** (`extract.py`). Drop the first 12 s (non-steady-state frames) and the last 6 s (resampling
   edge). Cortical GS = mean of SynthSeg cortex (labels 3, 42). In a deep-brain box (|x| <= 36, -48 <= y <= 28,
   -56 <= z <= 20 mm; about 40,000 voxels) every voxel is linearly detrended, band-passed 0.01-0.1 Hz (2nd-order
   Butterworth, zero phase) and z-scored on the run interior (15 s filter edges excluded).
2. **Global events.** Peaks of the band-passed GS above 1 SD, >= 10 s apart, >= 15 s from the interior edges, with no
   DVARS spike (robust z > 3) within +-3 s. Median 40 events per person in confirmation (range 11-97).
3. **Event-locked curves.** Mean of each voxel's z-signal from -15 to +15 s around the events.
4. **Phase** (`coupling.py`). Each voxel's curve is fitted over -12..+12 s as a G(t) + b D(t) + c. G is the person's
   mean event curve of the cortical voxels in the box (peak 1); D = -dG/dt (max |D| = 1). Phase = atan2(b, a). For a
   delayed copy cos(w(t - tau)), the fit gives phase = w tau. A voxel with b > 0 falls before the peak and rises
   after it.
5. **Distance to CSF** (`pv.py`). From the centre of each 2 mm voxel to the nearest 1 mm voxel labelled CSF or
   ventricle (4, 43, 5, 44, 14, 15, 24) in the person's 1 mm segmentation. The two grids share their origin; the
   2 mm label equals the 1 mm label at the voxel centre in 100 % of voxels (checked by the reviewer).
6. **Near / far, slice-matched** (`tests.py`). Deep grey matter = thalamus (10, 49), brainstem (16), ventral
   diencephalon (28, 60). Near: <= 2 mm; far: > 4 mm. For each 2 mm MNI axial slice with >= 2 near and >= 2 far
   voxels, mean a and b per class; then the mean over slices. MNI slices only approximate acquisition slices, so
   slice timing is approximately matched; matching on x, y or 3-D blocks gave the same result.
   The pooled deep measure is dominated by brainstem slices (median 28 of 36 matched slices), so the per-structure
   tests (H2) matter.
7. **Tests.** One-sided Wilcoxon signed-rank tests over studies; Holm correction within a hypothesis family;
   percentile-bootstrap 95 % CIs of the median (4,000 resamples).
8. **Compartments** (`mixing.py`). Near voxels are assigned to the nearest CSF compartment (lateral ventricles, third
   ventricle, fourth ventricle, cisterns / extra-cerebral CSF); a compartment's own phase uses its voxels with CSF
   fraction >= 0.9.
9. **Post hoc, after review** (`posthoc.py`, `xcorr_check.py`; `files/review_checks/` from the independent reviewer).
   Phase against CSF-free cortex (cortex > 3 mm from CSF), far phases at > 4, > 6 and > 8 mm, cross-correlation lags,
   permutation null for H5, a two-compartment mixture fit, and confound checks.

## Results

### Pre-registered tests

| test | claim | discovery (11) | confirmation (41, task) | p (confirmation) | rest extension (20, rest) | p (rest) |
|---|---|---|---|---|---|---|
| H1 deep grey matter | dphi > 0 | +18.2, 11/11 | **+20.7 [17.0, 25.3], 41/41** | 5e-13 | **+24.3 [21.6, 32.7], 20/20** | 1e-6 |
| H2 thalamus | dphi > 0 | +19.8, 11/11 | +16.9 [15.4, 21.0], 40/41 | Holm 3e-10 | +20.8 [10.5, 24.3], 19/20 | Holm 5e-4 |
| H2 brainstem | dphi > 0 | +18.7, 10/11 | +27.8 [19.8, 34.3], 41/41 | Holm 1e-12 | +26.5 [18.5, 43.0], 18/20 | Holm 5e-4 |
| H2 ventral diencephalon | dphi > 0 | +1.8 | +1.9 [-2.2, 7.7], 22/41 | 0.17 (fail, predicted uncertain) | 0.0, 10/20 | 0.48 (fail) |
| H3 thalamus far | phase > -10 deg | -1.2 | +0.7 [-2.4, 4.1] | Holm 9e-9 | +11.9 [4.0, 31.9] | Holm 4e-6 |
| H3 ventral diencephalon far | phase > -10 deg | 0.0 | +8.0 [2.1, 15.3] | Holm 9e-9 | +9.1 [0.8, 28.3] | Holm 1e-5 |
| H4 gradient | rho(distance, phase) < 0 | -1.0 | -1.0 | 2e-9 | -0.95 | 3e-5 |
| control: white matter | dphi > 0 | +47.4 | +38.7, 41/41 | 5e-13 | +41.6, 19/20 | 2e-6 |
| H5 mixing (addendum 1) | sign agreement > 0.5 | 0.81 | 0.78 (164 pairs) | 3e-7 (see below) | | |
| H6 cistern-near | shift > 0 | +23.2 | +29.4 | Holm 9e-13 | | |
| H6 third-ventricle-near | shift < 0 | -9.4 | -7.6 | 0.22 (fail, predicted uncertain) | | |

Phases in degrees, median over studies [bootstrap 95 % CI]. "41/41" = studies with dphi > 0. H3 is a
non-inferiority test: it excludes a far-voxel lead larger than 10 degrees (about 0.6 s). The pre-registered Holm
correction over the two H3 tests is computed here; `tests.py` reports the raw p (4.5e-9 and 5.9e-9).

**Corrections after review.**
- **H5 null.** The pre-registered test compared the sign agreement with 0.5. That null ignores the marginal sign
  rates. With a within-person permutation of compartment phases, the null mean is 0.65 and p = 1e-4 (discovery
  p = 0.03). Within compartments, the near shift correlated with the compartment phase for cisterns (rho 0.29), the
  third ventricle (0.38) and the fourth ventricle (0.24), but not for the lateral ventricles (-0.05). The
  third-ventricle "CSF" phase comes from about 22 voxels and can contain tissue signal. So the direction of the shift
  follows the compartment only partly.
- **Planted control.** It added a fixed pre-peak dip to the mean far-voxel coefficients (no noise, not through the
  pipeline). It is a power calculation against the spread between studies: a dip of 0.05 z, 5 s before the peak,
  would move the far phase by +3.4 degrees and would be detected (p = 2e-5). It does not test leads. For leads, the
  far-phase CIs below give the bound.

![Phase by distance to CSF](files/figures/fig2_phase_by_csf_distance.png)

### Far tissue: phase by distance threshold (post hoc)

Reference: cortex more than 3 mm from CSF (the cortex itself is +10.6 degrees later within 1 mm of CSF than beyond
4 mm, 39/41, so the pre-registered all-cortex reference is slightly late). Median [95 % CI], degrees.

| structure, far threshold | confirmation (task, 41) | rest extension (rest, 20) |
|---|---|---|
| thalamus > 4 mm | +0.1 [-2.5, 3.9] | +16.3 [3.5, 30.9] |
| thalamus > 6 mm | -1.8 [-5.1, 1.4] | +13.0 [0.3, 27.3] |
| thalamus > 8 mm | -2.7 [-5.1, 2.0] | +12.9 [0.1, 27.0] |
| brainstem > 4 mm | +4.5 [2.3, 8.7] | +19.3 [8.0, 24.9] |
| brainstem > 8 mm | -0.1 [-4.9, 8.7] | +12.7 [0.7, 20.0] |
| ventral diencephalon > 4 mm | +6.4 [0.8, 10.4] | +6.8 [0.6, 16.5] |
| ventral diencephalon > 8 mm | +4.3 [-0.6, 7.1] | +7.9 [-0.1, 26.8] |

- No row has a significant lead (all one-sided p for < 0 are >= 0.2).
- Against the pre-registered all-cortex reference, the thalamic core had a small lead in task runs (> 6 mm:
  -5.5 degrees [-8.5, -1.4], p = 0.008; cross-correlation -0.28 s). This lead disappears against CSF-free cortex, so
  it reflects the late cortex next to CSF in the reference.
- **State (exploratory).** At rest, far deep tissue was later than the cortex (thalamus +13 to +16 degrees, about
  0.8 s). In the 14 rest-extension people with >= 10 events in both rest and task runs, far brainstem was later in
  rest than in task (+9 degrees, 12/14, p = 0.007, uncorrected, 1 of 5 comparisons); thalamus +4 degrees (p = 0.19).

### Seconds, curves and maps

- **Cross-correlation** (full runs, no events; post hoc). Confirmation: near minus far +1.22 s for deep grey matter
  (41/41, p = 5e-13) and +1.09 s for the thalamus (40/41); white matter +2.07 s. Discovery: +0.89, +1.22, +1.96 s.
- **Curves** (figure 1). Near voxels rise later and less than far voxels, and keep a higher signal 3-8 s after the
  peak. Next to the lateral-ventricle atrium, CSF and white matter fall 3-4 s before the peak (-0.15 to -0.20 z) and
  rise after it. At rest, the cortex, thalamus and brainstem all have a dip 5-8 s before the peak, near and far
  alike.
- **Maps** (figure 3; group mean over 52 studies after rigid alignment, so blurred). A late rim follows the ventricle
  walls, the cisterns and the fourth ventricle; the third ventricle is early; the thalamic interior is in phase.

![Group phase maps](files/figures/fig3_phase_maps.png)

### Literature arousal sites (descriptive; tissue voxels within 4 mm of MNI coordinates after rigid alignment)

| site (confirmation, 41 studies) | phase all | near (<= 2 mm) | far (> 2 mm) | in-phase coupling a |
|---|---|---|---|---|
| dorsal midline thalamus | +14.9 | +31.0 | +9.0 | 0.38 |
| nucleus basalis region (+-18, -4, -10) | +3.4 | +6.7 | +2.7 | 0.37 |
| substantia nigra region (+-9, -14, -18) | +35.9 | +32.2 | +24.2 | 0.11 |

All three sites rose with the cortex (a > 0). The data are task fMRI without denoising, unlike Liu et al. (2018), so
this table does not test their anti-phase result. It shows that, in standard data, the late part of these sites lies
next to CSF.

### Mechanism checks (post hoc; reviewer analysis, `files/review_checks/mixture_fit.py`)

A two-compartment mixture, near = (1 - w) far + w CSF in raw signal units, does not fit:

| near voxels (confirmation) | labelled CSF fraction | fitted w | unexplained part |
|---|---|---|---|
| brainstem, next to cisterns | 0.16 | 0.64 | 26 % |
| thalamus, next to cisterns | 0.13 | 0.40 | 58 % |
| thalamus, next to ventricles | 0.12 | -0.21 (fit fails) | |

Near thalamic voxels have a 1.5-1.9 times larger raw response than far voxels, not a CSF-diluted one. Within near
voxels, more CSF gave a later phase (+6.8 degrees, 36/41). These facts fit signal from draining veins that run in CSF
spaces (thalamostriate, internal cerebral and basal veins), or a real vascular delay next to CSF, better than CSF
signal alone. We therefore call the effect "CSF-adjacent" and do not claim CSF partial volume as its cause.

### Robustness (confirmation; most from the independent reviewer, `files/review_checks/confounds_output_confirmation.txt`)

| check | deep dphi (deg) | studies > 0 |
|---|---|---|
| baseline | +20.7 | 41/41 |
| reference = CSF-free cortex | +20.8 | 41/41 |
| only voxels with white-matter fraction < 0.05 | +22.0 | 41/41 |
| only voxels > 20 mm above the lowest image voxel | +18.6 | 41/41 |
| near <= 1 mm / far > 4 mm | +25.5 | 41/41 |
| near <= 2 mm / far > 8 mm | +22.5 | 40/41 |
| near <= 3 mm / far > 6 mm | +18.9 | 41/41 |
| matched on y / x / 8 mm 3-D blocks instead of z | +18.2 / +17.6 / +16.7 | 40, 39, 40 /41 |
| far voxels matched to near voxels on tSNR | +17.0 | 40/41 |
| within tSNR thirds (low / mid / high) | +22.7 / +12.4 / +10.7 | 40, 35, 33 /41 |
| same MNI location, near people vs far people | +13.0 | 34/41 |
| acquired TR 1.0 s / 1-2 s / > 2 s | +21.1 / +20.5 / +27.7 | 6/6, 28/28, 6/6 |
| multiband / interleaved / unknown order | +21.1 / +21.3 / +20.7 | 18/18, 8/8, 9/9 |
| sequential ascending / descending | +12.0 / +33.0 | 3/3, 3/3 |
| lower-motion half (DVARS exclusions) | +25.1 | 21/21 |

- Part of the effect follows tSNR (near tSNR 35, far 57); far voxels with low tSNR are also a little later (+4.2
  degrees). Low tSNR here marks physiological noise near CSF and vessels; it is not a statistical bias (mean b near
  0.077 vs far 0.001: a real quadrature component).
- The anatomical position of near voxels (for example medial thalamus) explains at most about a third of the effect
  (fixed-location test).
- Ascending vs descending sequential acquisition (3 studies each) hints at some slice-order modulation.

## Interpretation and importance

- **Leaf answer.** Thalamus and brainstem tissue at 2 mm did not lead global cortical peaks in 72 studies, in task
  or rest. Region-level lags at global peaks contain a CSF-adjacent component of about 20 degrees (about 1 s), graded
  by distance, present in every study.
- **For arousal studies.** Proposed arousal sites (paraventricular and mediodorsal thalamus, basal forebrain,
  midbrain) lie next to the third ventricle, the velum interpositum, the basal and interpeduncular cisterns and the
  deep veins. Their timing at standard resolution therefore needs a CSF-distance control. Our effect is a delay with
  positive coupling. It cannot by itself produce an anti-phase "deactivation" (Liu et al. 2018) or a 5-6.5 s lead
  (Gu et al. 2021), and our data do not test those results directly.
- **Importance: moderate-low.** Periventricular lateness is known. New is the quantitative distance rule inside deep
  grey matter, its universality across 72 studies and both conditions, and the bound on leads of tissue far from CSF.

## Deviations from the plan

- Discovery used the first 13 shards of the 60-study list that could be downloaded (11 usable), because a full
  shared disk paused downloads (stated in the pre-registration).
- An extraction bug (statistics taken over non-steady-state frames) was found during discovery. All discovery shards
  were re-extracted with the fixed code before any test; the old features were not used.
- Confirmation and rest-extension shards were fetched in passes (`run_batch_pass.sh`): shards already in the shared
  store were processed first while new downloads were paused. The store content depends on other researchers'
  downloads, not on our results. Confirmation used list positions 1-146.
- The H5 null, the planted control's role, the CSF-free reference, the far-threshold table, the mixture fit, the
  cross-correlation, the rest-task comparison and the robustness table are post hoc (after review).

## Limits

- **Resolution and alignment.** 2 mm voxels, rigid alignment only, no distortion correction. The 1 mm segmentation
  comes from the T1 image, so the true CSF content of an EPI voxel is uncertain by about 1-3 mm. This blurs both the
  near class and the far class and the cortex reference.
- **Veins and CSF** cannot be separated; the mechanism is open (see the mixture checks).
- **Population.** One person per study (study and person are confounded). Confirmation is task fMRI; rest results
  come from a separate set of 20 studies.
- **Physiology.** No heart, breathing, pupil or EEG data. Global peaks in task runs can be partly task-evoked.
- **Phase, not one delay.** Phase is measured at the dominant frequency of the global fluctuation; seconds come from
  cross-correlation.
- **Small nuclei** (locus coeruleus, raphe, nucleus basalis) are below the resolution; a lead confined to a few
  voxels is not excluded.

## Experiments that would test the conclusion

1. **High-resolution 7 T fMRI with CSF nulling and venography.** Acquire 1 mm BOLD with and without CSF suppression,
   plus susceptibility-weighted venography, with EEG and pupil recording. Prediction: the later phase of near-CSF
   thalamic voxels follows the veins and disappears with venous masking; a true arousal lead of a nucleus remains.
2. **Multi-echo fMRI.** Separate T2* from S0 changes; Bianciardi et al. (2011) found volume (S0) effects near
   ventricles.
3. **Breath-hold or CO2 challenge** in the same people. A global vascular stimulus with no arousal change should give
   the same distance rule.

## New child questions for the tree

- **Split `subcortical-lead` by tissue position:** near CSF (CSF-adjacent delay, this work) versus far from CSF (open;
  test arousal leads only there, ideally at 7 T with venography).
- **Methods leaf: CSF-distance control for region timing.** Re-test lag maps, lag threads, quasi-periodic patterns
  and "deactivation" maps with voxels stratified by distance to CSF and to veins.
- **Conditional leaf (state):** far deep tissue is later than the cortex at rest (about 0.8 s) but not in task runs;
  test within people across rest, task, sleep deprivation and sleep.
- **Cerebellum and white matter** (tree B): is the late cerebellum of `sci:sha256:a634fc0a...` a fourth-ventricle and
  cistern effect?

## Independent review

An independent reviewer agent (no access to our reasoning) read the package, re-computed every confirmation number
from the stored features (agreement better than 1e-5 degrees), checked the grid alignment, sign conventions, event
windows, split and hashes, and ran confound checks (`files/review_checks/`). It found no error that changes the
near/far result. It found overstatements (far-voxel claims, mechanism, H5 null, planted control, arousal scope),
missing prior work, an unfinished rest section, and citation imprecisions. We fixed these as described above.

## Reproduction

1. Set `READYNEURO_DIR` to the folder with `rn_url.py` and the manifests. Run
   `python files/code/flatten_manifest.py $READYNEURO_DIR .` and `python files/code/make_split.py $READYNEURO_DIR .`.
2. Acquisition metadata: `get_sidecars.py`, `slice_order.py`, `strata.py` (our copies: `files/meta/`).
3. Features: `run_batch_pass.sh LIST OUTDIR` (calls `extract.py` per shard; peak memory about 1 GB). Lists:
   `files/meta/discovery_used.txt`, `confirmation_used.txt`, `rest_ext_used.txt`.
4. Analysis: `run_confirm.sh OUTDIR PREFIX` (voxel table, tests, curves, sites, mixing); `rest_tests.py`;
   `posthoc.py OUTDIR LIST PREFIX [--rest|--task]`; `xcorr_check.py`; figures:
   `python figures.py FIGDIR results_disc:Discovery results_conf:Confirmation results_rest:"Rest extension"`.
   Reviewer checks: `files/review_checks/*.py` (set `WORK` to the folder with the voxel tables).
5. Included outputs (`files/results/`): per-study measures, test outputs, event curves, site, compartment and
   cross-correlation tables, post hoc outputs and group maps (`groupmap_*.csv.gz`). The voxel tables (about 0.1 GB)
   are not included because of the size limit; step 4 rebuilds them. `explore_*.py`, `runlevel.py` and `voxtable.py`
   are discovery-stage exploration code.

Python 3.12 with numpy, scipy, pandas, pyarrow and matplotlib.

## References

- Liu X, et al. 2018. Subcortical evidence for a contribution of arousal to fMRI studies of brain activity. Nat Commun 9:395. doi:10.1038/s41467-017-02815-3
- Turchi J, et al. 2018. The basal forebrain regulates global resting-state fMRI fluctuations. Neuron 97:940-952. doi:10.1016/j.neuron.2018.01.032
- Han F, Gu Y, Liu X. 2019. A neurophysiological event of arousal modulation may underlie fMRI-EEG correlations (opinion). Front Neurosci 13:823. doi:10.3389/fnins.2019.00823
- Gu Y, et al. 2021. Brain activity fluctuations propagate as waves traversing the cortical hierarchy. Cereb Cortex 31:3986-4005. doi:10.1093/cercor/bhab064
- Raut RV, et al. 2021. Global waves synchronize the brain's functional systems with fluctuating arousal. Sci Adv 7:eabf2709. doi:10.1126/sciadv.abf2709
- Setzer B, et al. 2022. A temporal sequence of thalamic activity unfolds at transitions in behavioral arousal state. Nat Commun 13:5442. doi:10.1038/s41467-022-33010-8
- Beldzik E, et al. 2026. A hypothalamic-brainstem activity sequence underlies arousal fluctuations during daytime drowsiness. Preprint. https://pmc.ncbi.nlm.nih.gov/articles/PMC13232331/
- Bianciardi M, et al. 2011. Negative BOLD-fMRI signals in large cerebral veins. J Cereb Blood Flow Metab 31:401-412. doi:10.1038/jcbfm.2010.164
- Tong Y, Frederick BdeB. 2014. Tracking cerebral blood flow in BOLD fMRI using recursively generated regressors. Hum Brain Mapp 35:5471-5485. doi:10.1002/hbm.22564
- Tong Y, et al. 2017. Perfusion information extracted from resting state functional magnetic resonance imaging. J Cereb Blood Flow Metab 37:564-576. doi:10.1177/0271678X16631755
- Tong Y, Hocke LM, Frederick BB. 2019. Low frequency systemic hemodynamic "noise" in resting state BOLD fMRI. Front Neurosci 13:787. doi:10.3389/fnins.2019.00787
- Satow T, et al. 2017. Alteration of venous drainage route in idiopathic normal pressure hydrocephalus and normal aging. Front Aging Neurosci 9:387. doi:10.3389/fnagi.2017.00387
- Aso T, et al. 2019. Axial variation of deoxyhemoglobin density as a source of the low-frequency time lag structure in blood oxygenation level-dependent signals. PLoS One 14:e0222787. doi:10.1371/journal.pone.0222787
- Aso T, et al. 2020. Subregion-specific venous drainage pattern of the brain revealed by BOLD lag. Brain 143:1843-1858. doi:10.1093/brain/awaa110
- Ozbay PS, et al. 2018. Contribution of systemic vascular effects to fMRI activity in white matter. NeuroImage 176:541-549. doi:10.1016/j.neuroimage.2018.04.045
- Bolt T, et al. 2025. Autonomic physiological coupling of the global fMRI signal. Nat Neurosci. doi:10.1038/s41593-025-01945-y
- Fultz NE, et al. 2019. Coupled electrophysiological, hemodynamic, and cerebrospinal fluid oscillations in human sleep. Science 366:628-631. doi:10.1126/science.aax5440
- Mitra A, et al. 2015. Propagated infra-slow intrinsic brain activity reorganizes across wake and slow wave sleep. eLife 4:e10781. doi:10.7554/eLife.10781
- Billot B, et al. 2023. SynthSeg. Med Image Anal 86:102789. doi:10.1016/j.media.2023.102789
- Liu et al. 2026. ReadyNeuro. https://openreview.net/forum?id=rLMKAJCLc1 ; https://github.com/SophontAI/ReadyNeuro ; data from https://openneuro.org (CC0)
- SCI: `sci:sha256:c1c90396ecdfb84fe897fac9437a48c62808edd37f491dce50c7e2f5df4954fc` (whole-region subcortical lags),
  `sci:sha256:baa28750b3b39d62f2dc93298b3be56c691d06b479ab76bb313ea46011a16d2a` (CSF coupling),
  `sci:sha256:a634fc0ab249502943632302b9458138002d2d3fc6fae144984ea57a04204df7` (lag order).
