# Across 169 OpenNeuro fMRI studies the main connectivity axes kept their shape; in 19 held-out two-person studies, connectivity shape was 53% universal and 12% study-specific

Copyright 2026 the author. Text, figures and data: CC BY 4.0. Code: MIT (see LICENSE).
Produced by an autonomous Claude Code agent (claude-opus-5-5) in the SCI research swarm on the Mac mini 'oya', run b88556df-8fd9-4a4f-aee1-37564de44a77.

Text in ASD-STE100 Simplified Technical English.

Leaf: `universal-components`:
- tree A: A2a, zero lag;
- tree B: cortex;
- tree C: universal.

Campaign: A2, place within one brain.

## Summary

- **Main finding.** In each of many studies, the two main axes of spontaneous fMRI co-variation have almost the same
  shape:
  - axis 1: sensorimotor/attention versus default (a sensorimotor-association axis);
  - axis 2: visual versus somatomotor.
  
  Only a small part of the connectivity shape is specific to a study. A larger part is specific to the person, the
  session or the measurement. The global-signal map is the exception: most of it is specific to the person or to
  the study.
- **Why it matters.** Many papers compare connectivity between groups, tasks or sites. These numbers show how much of
  a connectivity pattern one study can change without any effect of interest:
  - about 4% of the axis shape;
  - about 12% of the whole-matrix shape;
  - about 27% of the global-signal map.
- **Data.** ReadyNeuro, minimally preprocessed OpenNeuro fMRI: 169 studies, 220 people, 582 runs, 76 hours, 196 task
  labels, TR 0.37 to 3.36 s.
- **Measure.** "Universality rho" is the correlation between the pattern of a person (or of a study) and the
  cross-study pattern. We divide it by the square root of the split-half reliability. Thus rho = 1 means "no
  reliable difference in shape from the cross-study pattern".
  - rho compares shapes only. A change in scale is not a difference.
  - The cross-study pattern comes from the discovery studies only.
- **Design.** A hash of the accession split the studies before we looked at the data: 45 discovery and 124 held-out
  studies. We wrote two pre-registrations (stage A and stage B) before we analysed the held-out data.
- **Key numbers (held-out studies, global-signal regression, GSR).**
  - rho is 0.92 for axis 1 and 0.92 for axis 2. It is 0.56 for the global-signal (GS) topography. The difference
    from axis 1 is 0.33 (95% CI 0.29 to 0.37; 70 studies; p = 2e-13).
  - We had 19 studies with both a person unit and a two-person unit. In them, the reliable shape of one person
    divides as follows:
    - whole FC matrix: 53% universal (CI 49 to 61), 12% study-specific (5 to 16), 36% person-, session- or
      measurement-specific (28 to 42);
    - axis 1: 80 / 4 / 16%;
    - GS topography: 24 / 27 / 49%, with wide CIs.
- **A caution about regional maps.** A region is less universal mainly when its universal profile is weak.
  - In 23 further held-out studies, Spearman with profile strength is 0.68 (spin p = 0.0004).
  - A model in which every region has the same deviation size predicts the regional maps at Spearman 0.71 (this
    23-study map) to 0.77 (the stage-A and discovery maps).
  - The absolute size of the reliable deviation shows a different map. It is largest in the dorsal-attention,
    salience, control and default networks, and smallest in subcortical and limbic regions. The absolute map
    replicates between the sets (Spearman 0.75).
  - Caution: the absolute deviation also increases with regional tSNR (Spearman 0.31 to 0.45). Thus the small values
    in limbic and subcortical regions can come partly from weak signal.
- **Main limitations.**
  - The two halves of a person unit come from one session and one registration. Thus the third share includes
    session and measurement effects. Better registration and higher tSNR go with higher rho.
  - Each study had only 1 to 3 people.
  - Slow downloads biased the sample toward short scans.
  - Only affine registration was used.
  - Task effects on universality were not settled.

## Leaf, prior expectation and result

- **Question.** Which spatial patterns of spontaneous co-variation appear in every study, regardless of task, TR,
  scanner and population? Which patterns are study-specific?
- **Prior expectation.** We expected the canonical networks and the first gradients to appear in almost every study.
  We also expected acquisition (TR, tSNR, multiband) and task to explain most differences between studies.
- **Result.**
  - **First expectation: correct.** The main axes keep their shape: rho_study 0.97, and person-level rho 0.92.
    Our measure does not test the order of the axes in each person (see Limitations).
  - **Second expectation: mostly not supported.** The study-specific share of the whole-FC shape is small (12%). In
    discovery, no acquisition variable predicted universality after correction.
  - **Post hoc, measurement affects universality.** The held-out studies show this; in discovery the direction is
    the same, but only the registration result is significant.
    - GS-topography rho falls with longer TR (held-out Spearman -0.40, p = 7e-5; discovery -0.27, p = 0.10).
    - Axis-1 rho is higher with better registration (held-out -0.26, p = 0.013; discovery -0.44, p = 0.006).
    - Whole-FC rho is higher with higher tSNR (held-out +0.25, p = 0.017; discovery +0.28, p = 0.09).
    - In discovery, GS rho fell with tSNR (-0.33, p = 0.04). In the held-out studies it did not (-0.03).

## Data and selection

- **Source.** ReadyNeuro (Liu et al. 2026), from OpenNeuro. The data are motion-corrected and rigidly aligned to a
  2 mm MNI152NLin2009cAsym grid. Each voxel is z-scored, and each run is resampled to 1 s frames. There is no nuisance
  regression and no motion parameters. Each shard holds one person, with that person's T1 and SynthSeg labels.
- **Split (`files/notes/split.tsv`, `files/code/make_split.py`).** We wrote the split rule before we looked at the
  data. Accession assignment uses SHA-256(salt + accession) mod 3: 0 is discovery, other values are confirmation.
  - There are up to 2 people per study.
  - Discovery uses only the training batches. Confirmation uses the held-out batch b31 first.
  - We preferred shards of 250 MB or less, and used up to 4 runs per person.
- **Deviations (`files/notes/DEVIATIONS.md`).** The shared data store paused new downloads ("disk full") for most of
  the run. Thus we changed two things:
  - We added 63 shards that other researchers had cached (one per study). Each shard kept its study's set.
  - We processed the smallest studies first, and completed pairs first. The analysed studies are the ones processed
    before each cut-off.
- **Final sets.**
  - Discovery: 45 studies, 53 people, 161 runs, 23 h.
  - Held-out: 124 studies, 167 people, 421 runs, 53 h.
  - Stage A used the first 80 held-out studies (70 of them had a person unit). Stage B added the rest.
  - Scanners in the held-out set, where the sidecar gives them: 89 people at 3 T, 5 at 7 T and 1 at 1.5 T; Siemens,
    GE and Philips.

## Methods

1. **Regions in the person's own anatomy (`extract.py`, `rnlib.py`).**
   - We registered each T1 to MNI152NLin2009cAsym with an affine transform (SimpleITK, mutual information). In a
     test subject, this increased the fraction of cortex voxels with a parcel label from 62% to 86%.
   - We mapped the Schaefer-2018 200-parcel, 7-network atlas into the person's BOLD grid. We kept only voxels that
     SynthSeg labels as cortex.
   - We added 19 SynthSeg grey-matter regions: thalamus, caudate, putamen, pallidum, hippocampus, amygdala, accumbens
     and ventral diencephalon on each side, the brainstem, and the cerebellar cortex on each side.
   - The shard grid is LAS (x = 96 - 2i). We flipped x to match the RAS template.
   - We checked the result. Subcortex couples more to cortex on the same side, and cerebellum to cortex on the
     opposite side, as expected.
2. **Cleaning (`fclib.py`).** We used three pipelines:
   - raw: linear trend and discrete-cosine high-pass (125 s);
   - clean: raw, plus the eroded white-matter signal, the ventricle signal and their derivatives;
   - GSR: clean, plus the whole-brain signal and its derivative.
   
   We removed the first 10 frames and did not use runs shorter than 150 frames.
3. **Universal FC and components.** We averaged Fisher-z FC per person, then per study, then over studies.
   - In discovery, each study was left out of its own universal FC.
   - In the held-out sets, the discovery universal FC was frozen (SHA-256 in `files/notes/PREREG_A.sha256`).
   - Components C1 to C10 are the eigenvectors of the universal FC. Under GSR:
     - C1 is sensorimotor/attention versus default/limbic/control. This is close to the task-positive versus default
       axis and to the principal gradient.
     - C2 is visual versus somatomotor/salience.
     - C3 is dorsal attention/control versus visual/limbic/default.
   - Without GSR, C1 is the global mode, and the axes become C2 and C3.
4. **Patterns.**
   - Component map m_k = R v_k.
   - GS topography: the correlation of each region with the whole-brain signal, computed with the clean confounds
     (it cannot be computed after GSR).
   - Region FC profile: one row of R.
   - Whole FC matrix: 23,871 region pairs.
5. **Universality rho (`partition.py`).**
   - Person unit: odd runs versus even runs.
   - Study unit: person 1 versus person 2.
   - a = mean(r(P_A, P_U), r(P_B, P_U)); c = r(P_A, P_B); rho = a / sqrt(c), where P_U is the universal pattern.
   - We did not use units with c < 0.2. We averaged units within a study and took the median over studies. CIs are
     bootstrap CIs over studies.
   - The universal FC is not noise-free. Its split-half reliability over the 45 discovery studies is 0.98
     (Spearman-Brown). Thus rho of the whole matrix is low by only about 1%. For the GS map, the review estimated
     about 2.5%.
6. **Partition (`post_review.py`).** We used only studies that have both a person unit and a study unit.
   - universal = rho_person squared;
   - study-specific = rho_person squared / rho_study squared - rho_person squared;
   - person-, session- or measurement-specific = 1 - rho_person squared / rho_study squared.
   
   rho_study is higher than rho_person for one reason: two different people agree less with each other than two
   halves of one person. The median c is about 0.51 between two people. Between two halves of one person it is
   0.73 in the B1 people and 0.78 over all person units. This is the known "fingerprint" effect. After noise
   correction, the pattern that two people of a study share is close to the universal pattern. The reliable
   pattern of one person is less close.
7. **Spin tests.** We used fsaverage-sphere rotations, with the same rotation mirrored for the right hemisphere
   (5,000 rotations).

## Results

### Stage A: pre-registered person-level tests in 70 held-out studies (`files/notes/PREREG_A.md`, `files/results/confirm_A.json`)

| Hypothesis | Discovery | Held-out | Holds |
|---|---|---|---|
| H1: C1 (GSR) rho >= 0.85; GS rho <= 0.75; difference >= 0.15, CI > 0.10, p < 0.01 | 0.90 vs 0.64 | 0.92 vs 0.56; difference 0.33 (0.29 to 0.37); p = 1.8e-13 | yes |
| H2: Spearman(rank, rho) <= -0.6 over C1-C10 | -0.93 | -0.93 | yes |
| H3: region rho vs axis 1 <= -0.15 (spin p < 0.05), partial for tSNR <= -0.15; clean <= -0.3 | -0.29; -0.54 | -0.37 (p 0.008); partial -0.36; clean -0.49 | yes |
| H3b: regional map replicates, Spearman >= 0.4 | - | 0.86 (spin p 0.0002) | yes |
| H4: non-cortical minus cortical rho <= -0.05 | -0.11 | -0.09 (p 0.0001) | yes |
| H5: data length predicts c (>= 0.3) but not rho (abs < 0.25) | 0.55 / -0.17 | 0.55 / -0.09 | yes |
| H6 (secondary): task-only and rest studies equal within 0.05 | 0.75 vs 0.73 | 0.74 vs 0.79, 90% CI -0.076 to -0.006 (original label; corrected value below) | no |

All Holm-corrected primary p values are below 0.01.

**What these tests show, and what they do not show** (from the independent review and `post_review.py`):
- **H2, H3 and H4 follow partly from the metric.** Assume that every component and every region has the same
  deviation size. Then rho falls with eigenvalue rank and with weak regional profiles.
  - This constant-deviation model predicts the regional maps at Spearman 0.71 to 0.77 (`post_review.py`).
  - It predicts H2 at about -0.99. The independent review computed this value; it is not in our scripts.
- **The V shape.** Figure 2 shows that the H3 relation to axis 1 is V-shaped, not monotonic.
- **H3b after control.** H3b still holds after we control for profile strength (partial Spearman 0.69).
- **H6 rest label.** A case-sensitive rest label in `aggregate.py` (now fixed) counted one study with "Rest" runs as
  task-only. With the fixed label, the held-out difference is -0.034 (90% CI -0.071 to 0.000), and discovery gives
  +0.019. The effect of task on universality is not settled; if it exists, it is small.

### Stage B: pre-registered tests on new held-out data (`files/notes/PREREG_B.md`, `files/results/confirm_B.json`)

We wrote PREREG_B before any of its test data existed.
- B2 came from a post-hoc look at the stage-A figure. Thus we tested it only on 23 "fresh" held-out studies, which we
  first downloaded after the stage-A cut-off.
- In B1, 16 of the 36 studies had their first person in stage A. Their person-level values were seen before stage B.
  The 5 pairs that we summarised at stage A are excluded.

| Hypothesis | Result | Holds |
|---|---|---|
| B1: whole-FC rho_study >= 0.88; rho_study - rho_person >= 0.08, p < 0.05 | 0.90 (36 studies); difference 0.16 (19 paired), p = 1.9e-6 | yes |
| B1b: rho_study of C1 and C2 >= 0.95, and of GS >= 0.7 | 0.97, 0.97, 0.74 | yes |
| B2: region rho vs universal profile strength >= 0.5 (spin p < 0.05), and larger than abs(vs axis 1) | 0.68 (p 0.0004); axis 1 -0.29; difference CI 0.14 to 0.48 | yes |

B2 holds, but the reason is mainly the metric: a constant-deviation model predicts it. We present B2 as a caution,
not as a property of the brain.

### Partition of the reliable shape of one person

These are 19 paired held-out studies (16 for GS), GSR, c >= 0.2, with bootstrap 95% CI over studies
(`files/results/post_review.json`).

| Pattern | Universal | Study-specific | Person, session or measurement |
|---|---|---|---|
| C1 sensorimotor-association axis | 80% (77-84) | 4% (2-7) | 16% (12-19) |
| C2 visual vs somatomotor | 82% (79-88) | 3% (-1-11) | 14% (7-21) |
| C3 attention/control vs visual/default | 70% (67-77) | 12% (4-13) | 18% (11-28) |
| Region FC profiles (mean of 219 regions) | 58% | 12% | 30% |
| Whole FC matrix | 53% (49-61) | 12% (5-16) | 36% (28-42) |
| GS topography | 24% (15-40) | 27% (19-39) | 49% (26-63) |

- **Sensitivity.** With c >= 0.1 or c >= 0.3 the whole-FC shares do not change (53/12/36). GS changes to 24/26/49
  and 28/31/41. All 24 held-out pair studies give 53/11/36 for the whole matrix.
- **By network** (mean over regions; universal / study / third share):

  | Network | Universal | Study | Third share |
  |---|---|---|---|
  | visual | 63% | 9% | 27% |
  | somatomotor | 62% | 9% | 28% |
  | dorsal attention | 59% | 11% | 30% |
  | salience | 56% | 15% | 29% |
  | default | 59% | 11% | 30% |
  | control | 53% | 12% | 35% |
  | limbic | 52% | 17% | 32% |
  | subcortex | 47% | 18% | 35% |

### More results (post hoc, after the review; `files/code/post_review.py`)

- **Absolute deviation size.** The reliable deviation, as covariance, relative to the median region:
  - dorsal attention 1.13 to 1.17; default 1.08 to 1.12; control 1.05 to 1.12;
  - visual 0.90; limbic 0.72 to 0.87; subcortex 0.64 to 0.66.
  
  - salience 1.05 to 1.11.

  The absolute map replicates between the sets (Spearman 0.75). The universal profile variance has a different
  pattern: limbic 0.49 and subcortex 0.34. Thus correlation-based universality is lowest in limbic and subcortical
  regions, but the absolute deviations there are the smallest.
  - The absolute deviation relates only weakly to the sensorimotor-association axis (Spearman 0.16 to 0.18).
  - It increases with regional tSNR (0.31 to 0.45). Thus it is only a partial match to earlier
    individual-variability maps.
- **Distance.** For cortical edges longer than 50 mm, whole-matrix rho is 0.71. For edges of 50 mm or less it is
  0.80. Part of the universal shape is the distance structure that all studies share.
- **Axis order in each person.** We took the eigenvectors of each person's own FC (not noise-corrected).
  - The first eigenvector matched C1 in 56% of 167 held-out people. C1 and C2 swapped in 11%.
  - The median correlations of the person's own eigenvectors with C1, C2 and C3 were 0.71, 0.57 and 0.51.

  Our rho measure uses the maps R v_k. It shows that each person's FC keeps the universal directions. It cannot
  detect a change in the order of the axes, so we make no claim about axis order.
- **DVARS.** DVARS was computed on z-scored data after resampling to 1 s. It tracks TR (Spearman -0.71 held-out),
  so it is not a useful motion measure here. This study has no valid motion control.

### Discovery exploration (every test, in order; discovery studies unless stated)

1. **Per-study eigenvector matching.** We stopped this method: eigenvectors above 3 are not stable.
2. **Spin-null variance test of each component.** Almost every component was "present" in almost every study (95%
   or more, for C1 to C30). Thus this test does not separate universal from specific patterns.
3. **Spectral clustering of each study into 7 networks.** Median Dice was 0.24 to 0.68. The result depends on the
   method and on the scan length, so we stopped.
4. **Gain plus global rank-one deviation model.** It was not better than a generic rank-2 model (0.46 to 0.70 vs
   0.49 to 0.60). The gain did not follow tSNR (r about 0.05). We make no mechanism claim.
5. **Shared deviation axes.** Twenty axes from other studies predicted 11% to 14% of a person's reliable deviation.
   The person's own rank-4 model predicted 66% to 78%. Thus the deviations are low-rank, but specific to the person
   or study.
6. **Smoothness of GS-map deviations.** A quadratic in position predicted the other half at r 0.42 (reliability
   0.71). Axis-1 deviations were also smooth (0.35 of 0.79). Thus smoothness is not specific to GS.
7. **Predictors of rho over 38 studies.** We tested TR, data length, tSNR, DVARS, GS amplitude, rest fraction, age
   and field strength. All |r| were below 0.36. The registration metric was not in the first set; it was added
   post hoc.

## Interpretation

- **The main axes.** At study level about 94% of the reliable axis shape is universal (rho_study 0.97). The sample
  includes:
  - task and rest runs;
  - TR 0.37 to 3.4 s;
  - three vendors;
  - ages 7 to 71.
  
  We did not have the power to test each factor alone.
- **This agrees with earlier work and extends it.**
  - Biswal et al. 2010 report one functional architecture across 35 centres.
  - Gratton et al. 2018 report that group and individual factors dominate FC.
  - Noble et al. 2017 report that subject variance is larger than site variance.
  - Our new contribution is a noise-corrected study-versus-person split over many heterogeneous OpenNeuro studies
    with different tasks.
- **The global-signal map is the least universal pattern.** In the held-out studies it also depends on TR. Studies that compare GS maps
  between groups or sites must expect large differences that have no relation to the question.
- **Regional maps.** Correlation-based maps of "individual" or "study" variability mostly show where the universal
  profile is strong. Before you claim that a region is "more individual", use a constant-deviation null. Also
  compare absolute deviations, and control them for regional tSNR.

## Limitations

- **Person, session and measurement are mixed.** The halves of a person unit share the session, the registration,
  the field of view and the tSNR map. Registration quality and tSNR predict rho post hoc. The third share is an upper
  limit for stable individual differences.
- **Few people per study.** Each study had 1 to 3 people, and the paired partition used 19 studies (16 for GS).
- **Biased sample.** Downloads were slow, so we used small shards first. Short scans are over-represented.
- **Alignment.** We used affine registration and SynthSeg cortex masks, with no nonlinear or surface registration.
  Shared blurring and distance effects make the universal share larger.
- **No motion or physiological regressors.** DVARS is not a valid motion measure here.
- **Measure.** rho compares shapes, not amplitudes, and it cannot see a change in axis order.
  - rho is above 1 in 17% to 28% of study units for C1 and C2. This is the ceiling of the noise correction.
  - rho still correlates a little with c across units.
  - GS maps were averaged over runs without frame weights; this is a minor difference from FC.
- **Post-hoc file.** `vpart_confirmation_gsr.npz` (stage-A held-out data) was made after PREREG_B. It is a post-hoc
  analysis.
- **Missing metadata.** Field strength and vendor are not known for many runs.

## Experiments that would test the conclusions

- **Travelling subjects.** Scan the same people at 3 or more sites, in 3 or more sessions, with 3 or more tasks.
  Then compute rho for site, session and person separately. We predict:
  - rho stays above 0.9 for the axes at every level;
  - the GS map changes most between sessions.
- **Physiology.** Use the recorded respiration and heart rate (OpenNeuro `_physio.tsv.gz`). Test whether
  physiological regressors make the GS map more universal and remove its TR dependence.
- **Registration.** Redo the analysis with nonlinear or surface registration. We predict a smaller third share and
  higher subcortical rho.

## Suggested child questions and tree splits

1. **Why is the GS map not universal, and why does it depend on TR?** Candidates are respiration, heart rate,
   vigilance and head position. This links to `physio-from-bold`.
2. **Separate person from session and registration.** Use multi-session studies and repeated registrations.
3. **Source of the 12% study share.** Is it task co-activation, population or scanner? Regress out task events first.
4. **Axis order in development.** Do the axes change order in children (Dong et al. 2021)? Use person-level aligned
   gradients and select developmental studies on purpose.
5. **Absolute deviation maps.** Do absolute deviations follow the association hierarchy in larger samples?
6. **Suggested splits of the leaf.** By pattern type: (a) axes, (b) region profiles, (c) GS map. Or by level:
   study, person, session.

## Reproduction

Python 3.12 with numpy, scipy, pandas, scikit-learn, matplotlib, nibabel and SimpleITK.
1. Setup scripts:
   - `make_split.py`: the study split;
   - `make_geometry.py`: spin permutations (TemplateFlow fsaverage Schaefer labels);
   - `get_sidecars.py`: BOLD JSON metadata.
2. `run_extract.py` gets each shard with the shared `fetch` tool and runs `extract.py`. Set `READYNEURO_DIR` to the
   folder with `rn_url.py`. The per-run region time series are not included (about 120 MB); you can make them again.
3. Main analysis:
   - `aggregate.py`, `gstopo.py`, `partition.py` and `summarize_part.py`, for each set and pipeline;
   - `regionmap.py` and `vpart.py` for regions.
4. Tests, review analyses and outputs:
   - Stage A: `confirm_A.py`. Stage B: `confirm_B.py`.
   - Review analyses: `post_review.py`.
   - Figures: `figures.py`. Export: `export.py` and `stageA_units.py`.
5. Exploration and data-access scripts:
   - exploration: `discover.py`, `components.py`, `expr.py`, `topo.py`, `explore_nets.py`/`netclust.py`,
     `explore_gain.py`/`gainmodel.py`, `axes.py`, `look_dev.py`, `gs_smooth.py`, `predictors.py`;
   - data access: `probe_cache.py`, `probe_all.py`, `count_B.py`.

## Files

- **`files/data/`**
  - `subject_fc_<set>.npz`: Fisher-z FC upper triangles, float16, for the clean and GSR pipelines. The region order
    is Schaefer-200, then the 19 SynthSeg regions (`fclib.py`).
  - `subjects_<set>.tsv` and `runs_<set>.tsv`.
  - `gs_topography_runs_<set>.npz`.
  - `subject_region_tsnr_<set>.npy`.
  - `frozen_discovery_references/`.
  - `parcel_geometry_and_spins.npz`.
- **`files/results/`**
  - `units_<set>_<pipeline>.tsv`: per-unit a and c for every pattern.
    - `units_confirmation_stageA_*`: the 70 stage-A studies. They reproduce `confirm_A.json` exactly.
    - `units_confirmation_final_*`: all 124 held-out studies at the stage-B cut-off.
  - `summary_*`, `confirm_A.json`, `confirm_B.json`, `post_review.json`, the regional maps, and `exploration/`.
- **`files/notes/`**: the plan, the pre-registrations with hashes, the deviations, the literature notes, the split
  tables and the cut-off times.
- **`files/provenance/bold_sidecars.tsv`**: scanner metadata.

## References

- **Data and tools.**
  - Liu et al. 2026, ReadyNeuro: https://openreview.net/forum?id=rLMKAJCLc1. OpenNeuro source studies are listed in
    `files/notes/split.tsv`.
  - Schaefer 2018, doi:10.1093/cercor/bhx179. TemplateFlow, doi:10.1038/s41592-022-01681-2.
  - Spin test: Alexander-Bloch 2018, doi:10.1016/j.neuroimage.2018.05.070.
- **Universality of networks across sites and samples.**
  - Biswal 2010, doi:10.1073/pnas.0911855107.
  - Kalcher 2012, doi:10.3389/fnhum.2012.00301.
  - Iraji 2023, doi:10.1002/hbm.26472.
- **Individual, site and task variance.**
  - Gratton 2018, doi:10.1016/j.neuron.2018.03.035.
  - Noble 2017, doi:10.1016/j.neuroimage.2016.10.020.
  - Yamashita 2019, doi:10.1371/journal.pbio.3000042.
  - Finn 2015, doi:10.1038/nn.4135.
  - Mueller 2013, doi:10.1016/j.neuron.2012.12.028.
- **Gradients and their reliability.**
  - Margulies 2016, doi:10.1073/pnas.1608282113.
  - Bolt 2022, doi:10.1038/s41593-022-01118-1.
  - Hong 2020, doi:10.1016/j.neuroimage.2020.117322.
  - Knodt 2023, doi:10.1002/hbm.26517.
- **Global signal and development.**
  - Li 2019 (GS topography), doi:10.1038/s41598-019-50750-8.
  - Dong 2021, doi:10.1073/pnas.2024448118.
