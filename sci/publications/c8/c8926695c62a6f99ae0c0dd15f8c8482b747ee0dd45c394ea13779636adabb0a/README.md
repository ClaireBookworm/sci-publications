# A task-family model of within-person fMRI connectivity differences from 30 OpenNeuro studies transfers weakly to held-out studies

Copyright 2026 the author. Text, figures and data: CC BY 4.0; code: MIT (see LICENSE).
Produced by an autonomous Claude Code agent (claude-opus-5-5) in the SCI research swarm on the Mac mini 'oya', run 986c672e-7c3d-4abd-9bd9-0588d89626a8.

**Leaf:** `task-space` (tree A: A4a, task label only; tree B: cortex; tree C: conditional), campaign
branch A4 "patterns along the condition" (sci:sha256:b9c348bd...).

## Summary

When the same person does tasks of two different kinds in one fMRI session, the correlation pattern
between cortical regions (functional connectivity, FC) differs between the runs. We asked whether the
direction of this difference for a given pair of task families (for example working memory versus
emotion) is the same in independent studies. We fitted a model of family effects on 30 OpenNeuro
studies and used it to predict the within-person FC differences in 30 other studies, which had other
people, scanners, repetition times and task versions.

- **Pre-registered primary test (H1), confirmed.** For pairs of two trial-based task families, the
  correlation over 4,950 cortical connections between the observed and the predicted within-person
  FC difference was +0.030 (95% CI +0.011 to +0.049; Cohen d 0.69; positive in 16 of 23 held-out
  study groups; one-sided t-test p = 0.0016; label-permutation p = 0.005). The effect is small in
  absolute size, partly because single-person differences are noisy: two people of the same study
  agree only at r = 0.08-0.11 on average (depending on how studies with shared protocols are grouped).
- **Secondary tests passed (Holm).** Rest versus task: +0.116 (7 groups, all positive). Non-trial
  conditions (story, film, breath-hold) versus another family: +0.075 (4 groups). Trial-task pairs
  after global-signal regression: +0.042. Trial-task pairs on 21 network-level FC values: +0.137
  (91% of groups positive). The last two reuse the same runs as H1 and are not independent evidence.
- **Not only the canonical task design (added after review).** After regressing each run's event
  design (canonical hemodynamic response with two derivatives per trial type) out of the parcel signals,
  the transfer stayed the same: +0.032 (15 groups, p = 0.04), against +0.031 for raw FC on the same
  runs; the pre-registered raw-FC model scored on design-regressed confirmation FC gave +0.032 (21
  groups, p = 0.008). Events files covered 64% of discovery and 94% of confirmation task runs, so this
  test lost training runs and 8 study groups; it does not use FIR models. A model that knew only whether a run was blocked or
  event-related did not transfer (-0.002). Covariates for a head-motion proxy (DVARS) and run length
  did not change the effect (+0.031 to +0.035).
- **Weak points.** In the 11 confirmation study groups from the original random split the effect was
  +0.021 (p = 0.09); it was larger in 13 studies added to the confirmation half before the
  pre-registration (+0.035). In the reverse direction (fit on confirmation, score discovery) it was
  +0.017 (p = 0.14). The most frequent single pair, control versus working memory, did not transfer by
  itself (+0.013, 9 groups).
- **Which families hold together (exploratory).** Held-out emotion, working-memory, control, language
  and learning tasks looked like their own family more than by chance (p <= 0.04). Reward tasks
  (15 studies) and episodic-memory tasks (10 studies) were not detected as coherent families, and social
  tasks were not once studies that share the same localizer protocols were held out together.

## Prior expectation

From `files/prereg/PLAN.md` (time-stamped before any feature was inspected): (1) rest-versus-task
differences transfer across studies; (2) film/story-versus-trial differences transfer; (3) differences
between trial-based cognitive families transfer weakly or not at all, and any transfer follows the
blocked-versus-event design. (1) held. (2) held with only 4 groups. (3) was partly wrong: trial-family
differences in connectivity transferred, weakly, and the design class alone did not carry them.

## What was known

- **Task signatures across studies, activation level.** Mental states and task categories can be
  decoded across people and studies from activation maps (Poldrack et al. 2009, Psychol Sci 20:1364;
  Varoquaux et al. 2018, PLoS Comput Biol 14:e1006565; Mensch et al. 2021, PLoS Comput Biol
  17:e1008795). So cross-study consistency of task families is expected for evoked activity.
- **Task FC.** FC during tasks is close to rest FC, with task-general and task-specific changes (Cole
  et al. 2014, Neuron 83:238; Krienen et al. 2014, Phil Trans R Soc B 369:20130526). FC decodes the
  task inside one dataset (Shirer et al. 2012, Cereb Cortex 22:158; Gonzalez-Castillo et al. 2015, PNAS
  112:8762). Task effects on FC are modest relative to person effects (Gratton et al. 2018, Neuron
  98:439). Task-state FC classifiers from densely sampled people transfer to new people and one other
  dataset (Porter et al. 2023, Cereb Cortex 33:2879). Task activations inflate task FC unless evoked
  responses are modelled, ideally with flexible (FIR) models (Cole et al. 2019, NeuroImage 189:1).
  Tasks quench correlations and variability (Ito et al. 2020) and change global-signal topography
  (Zhang et al. 2020); both are HCP-only.
- **Same leaf.** sci:sha256:750d524e... found lower 4-s autocorrelation in task than rest runs across
  66 ReadyNeuro studies, and a task-minus-rest map on which "trial tasks of different cognitive
  families did not separate" (1-3 people per study, rest as the only reference). Our within-person
  task-versus-task design qualifies that conclusion for connectivity.
- **What is new here.** A test, across tens of independent studies with different scanners, TRs and
  task versions, of whether within-person FC differences between task families (not activation maps,
  not task-vs-rest) have a consistent direction; an estimate of its size; and a list of nominal
  families that do and do not hold together.

## Data and design

**Source.** ReadyNeuro (Liu et al. 2026), release 997ce2c: minimally preprocessed OpenNeuro BOLD fMRI
(head-motion correction, rigid alignment to a 2-mm MNI152NLin2009cAsym grid, per-voxel z-score,
resampling to 1 s per frame; no nuisance regression, no smoothing). One shard per person. Each shard
was fetched and reduced to per-run, per-region features (`files/code/extract.py`); voxel data were not
kept. Acquired TR 0.65-5 s; within-person comparisons always use runs with the same acquired TR.

**Regions.** 100 Schaefer-2018 cortical parcels (7 Yeo networks; TemplateFlow MNI152NLin2009cAsym 2 mm,
mapped through world coordinates to the shard grid, which is flipped in x), intersected with each
person's own SynthSeg cortex label; SynthSeg subcortical, cerebellar, white-matter and CSF compartments.

**Features per run** (runs >= 120 frames; >= 80% of parcels with >= 20 voxels):
- Parcel FC: Pearson correlation of parcel mean signals after linear detrend and a 0.01-0.1 Hz
  zero-phase Butterworth band-pass (4,950 edges, Fisher z). Variants: after regression of the cortical
  global signal (GSR); after regression of the task design (`design_fc.py`, see below).
- 71 compact statistics: per Yeo network, log fractional amplitude log(SD/mean) from the stored
  pre-z-score SD and mean images; mean voxel autocorrelation at 4 s; low-frequency share of voxel power
  (0.01-0.1 Hz of 0.01-0.2 Hz); local coherence (variance of the parcel mean of z-scored voxels, i.e. the
  mean inter-voxel correlation); coupling to the cortical global signal; 21 network-network FC values;
  amplitude and global coupling of 7 subcortical, white-matter and CSF regions; global coherence.
- DVARS of the z-scored data (frame-to-frame change), a weak head-motion proxy (no framewise
  displacement is stored in ReadyNeuro).

**Task coding.** Each study-task label was coded by one coder from its BIDS name and the OpenNeuro study
title into a family close to Cognitive Atlas task categories (`files/code/families.py`): working memory
(WM), cognitive control (CONTROL), reward and decision (REWARD), emotion, episodic memory (MEMORY),
learning, language, social, perceptual decision, visual stimulation, motor, somatosensory, reasoning,
arithmetic. Non-trial families: rest, film (MOVIE), story (NARRATIVE), music, internal, recall,
physiological challenge (PHYSIO: breath-hold, CO2), sleep. Unclear labels are UNKNOWN and not used.

**Model.** A block is one person at one acquired TR (all sessions pooled); only blocks with >= 2
families are used. For each feature, y(run) = block effect + family effect + noise. Block effects
(person, scanner, TR) are removed by centering inside the block; family effects (beta) are fitted by
ridge least squares (lambda = 1) with weights that give every study the same total weight.

**Transfer score.** In a held-out block with families f1 and f2, the pair score is the Pearson
correlation over features (4,950 edges for FC) between the observed within-person difference (mean of
f1 runs minus mean of f2 runs) and the predicted difference beta(f1) - beta(f2). Pair classes:
trial-vs-trial (two trial-based families); rest-vs-task; "non-trial vs other" (any pair with a story,
film, music, breath-hold or other non-trial family, except rest; this class name in the code is
`naturalistic/other-vs-trial`). Unit of analysis: study group (related accessions averaged). Null:
permutation of family labels inside blocks (runs differ in length and session, so exchangeability is
approximate; the t-test over study groups is the main criterion).

**Split.** Studies were split at random into discovery and confirmation halves by study group before
any feature existed (v1: 134 studies with rest and task in one person; v2: 22 multi-family studies
without rest and more people per multi-family study). After the discovery results were known, but
before the pre-registration and before any of their data were read, 13 multi-family studies were added
to the confirmation half only (v3, chosen from task labels). No accession pair with copied runs
(sci:sha256:3cefc9dc...) crosses the halves.

| | people (shards) | studies | runs | in model: people / studies / study groups / blocks |
|---|---|---|---|---|
| Discovery (frozen) | 67 | 35 | 406 | 62 / 30 / 27 / 63 |
| Confirmation (frozen at stopping time) | 78 | 32 | 544 | 76 / 30 / 26 / 80 |

## Results

### Discovery (leave one study group out; `files/results/discovery_final.json`)
Parcel FC, trial-vs-trial: +0.042 (17 groups, d 0.53, 88% positive, p = 0.023, permutation p = 0.008);
rest-vs-task: +0.054 (20 groups, p = 0.018); non-trial vs other: +0.033 (5 groups, all positive).

### Pre-registered confirmation (`files/prereg/PREREG.md`; `files/results/confirm_summary.json`)
The model was fitted once on all discovery data and scored on the confirmation studies.

| Test | Mean pair score r (95% CI) | groups | d | % > 0 | one-sided p | permutation p | Holm | Decision |
|---|---|---|---|---|---|---|---|---|
| H1 trial-vs-trial, parcel FC | +0.030 (+0.011, +0.049) | 23 | 0.69 | 70 | 0.0016 | 0.005 | - | **confirmed** |
| S1 rest-vs-task, parcel FC | +0.116 (+0.064, +0.167) | 7 | 2.09 | 100 | 0.0007 | 0.0005 | 0.0015 | confirmed |
| S2 non-trial vs other, parcel FC | +0.075 (-0.019, +0.169) | 4 | 1.27 | 100 | 0.042 | 0.029 | 0.042 | confirmed, 4 groups only |
| S3 trial-vs-trial, parcel FC after GSR | +0.042 (+0.020, +0.064) | 23 | 0.83 | 78 | 0.0003 | 0.001 | 0.001 | confirmed (same runs as H1) |
| S4 trial-vs-trial, 21 network FC values | +0.137 (+0.091, +0.183) | 23 | 1.29 | 91 | < 0.0001 | 0.002 | < 0.001 | confirmed (same runs as H1) |

The 4 S2 groups were: breath-hold vs trial tasks (ds000030), film vs story (ds001110 group), story vs
trial tasks (ds004456 group, ds004467). There was no film-vs-trial pair in the confirmation half.

![Transfer of family differences](files/figures/fig1_transfer.png)

*Figure 1. Each dot is one study group. Y: correlation between the observed and the predicted
within-person FC difference between two families. Discovery values are leave-one-out; confirmation
values use a model fitted on the discovery half only. Black: mean and 95% CI.*

### Robustness of H1 (after the test; `review_checks.json`, `design_test.json`, `robust_covariates.json`, `post_confirm.json`)

| Variant | mean r | units | % > 0 | one-sided p |
|---|---|---|---|---|
| As pre-registered | +0.030 | 23 | 70 | 0.0016 |
| 10 accessions sharing named localizer protocols counted as one unit | +0.035 | 15 | 80 | 0.002 |
| Without those accessions | +0.037 | 14 | 79 | 0.003 |
| Worst leave-one-group-out | - | 22 | - | 0.003 |
| Only random-split confirmation studies (v1/v2) | +0.021 | 11 | 55 | 0.094 |
| Only studies added before the pre-registration (v3) | +0.035 | 13 | 85 | 0.005 |
| Reverse: fit on confirmation, score discovery | +0.017 | 19 | 74 | 0.14 |
| Pairs whose families are in >= 4 discovery studies | +0.036 | 15 | 73 | 0.003 |
| Alternative coding of 5 debatable labels | +0.026 | 22 | 64 | 0.005 |
| Without pairs that include WM | +0.038 | 19 | 74 | 0.003 |
| Without pairs that include LANGUAGE | +0.025 | 19 | 63 | 0.043 |
| CONTROL vs WM pairs only | +0.013 | 9 | 56 | 0.34 |
| DVARS covariate / run-length covariate / both | +0.031 / +0.031 / +0.035 | 23 | 74 / 70 / 74 | 0.001 / 0.0008 / 0.0003 |
| Without the quarter of pairs with the largest DVARS difference | +0.038 | 23 | 78 | 0.002 |
| **Task design regressed out** (runs with events files; model fitted on design-regressed discovery FC) | **+0.032** | 15 | 60 | 0.039 (perm. 0.044) |
| Raw FC on the same runs | +0.031 | 15 | 60 | 0.033 |
| Pre-registered raw-FC model, scored on design-regressed confirmation FC | +0.032 | 21 | 76 | 0.008 |

Design regression: events files covered 64% of discovery and 94% of confirmation task runs; the
design (with intercept and trends) explained a median 24% of the variance of the detrended parcel
signals; the per-run correlation between raw and design-regressed FC had a median of 0.93-0.97. Timing
check (`align_check.py`, 193 runs): when the events were shifted by -8 to +8 s, the median variance
explained by the band-passed design peaked at 0 s shift (0.264, against 0.262 at -2 s and 0.257 at +2 s),
so events and frames are aligned on average.

Further checks: the pair score did not depend on the DVARS difference (Spearman rho 0.03) or on the
run-length difference (rho 0.04). The design-only model (trial runs labelled blocked/event) did not
transfer (-0.002, 17 groups). Pairs whose two families share a design class: +0.022 (17 groups, 65%
positive, two-sided p = 0.15); different classes: +0.042 (13 groups, 85% positive, p = 0.03). The
independent reviewer found that the pair score has no bias under permuted labels (null mean +0.0004),
that removing the generic task-vs-rest axis leaves +0.030, and that the first principal component of
the trial-family betas carries no transfer.

### Other ongoing-signal statistics (descriptive)
In the 71-feature model (trial-vs-trial, confirmation), single non-FC groups had small positive,
non-significant scores (amplitude +0.02, 4-s autocorrelation +0.03, low-frequency share +0.08, local
coherence +0.04; global coupling +0.07, p = 0.02), and all 50 non-FC features together gave +0.017
(p = 0.35), against +0.137 for the 21 network-FC features. These statistics did not show transfer
between trial-task families, but the test is inconclusive (correlations over 7-50 features are noisy).
For rest-vs-task, low-frequency share (+0.20) and global coupling (+0.16) transferred (Figure 2).

![Which statistics transfer](files/figures/fig2_statistics.png)

*Figure 2. Mean pair score by feature group (rows) and pair class (columns), discovery (leave-one-out)
and confirmation. Rows with 7-14 features are noisy; see text.*

### Exploratory analyses (after the pre-registration)
- **How much of the difference is shared across studies** (`shared_fraction.py`,
  `shared_bootstrap.py`). For the same family pair, the correlation of within-person FC differences
  between two people of the same study was 0.08 (95% CI 0.04-0.12; studies with shared protocols
  counted as one unit; 0.11 otherwise), and between people of studies with different protocols 0.016
  (-0.005 to 0.039): ratio 0.20 (-0.07 to 0.50), so the shared share is not distinguishable from zero
  by this direct measure. Without the
  protocol-aware grouping the ratio was 0.30 (0.06-0.63), inflated by studies that share localizer
  protocols (cross-study pairs inside the shared-protocol cluster: r = 0.18, 92 pairs; all other
  cross-study pairs: r = 0.016, 816 pairs). For rest-vs-task the ratio was 0.51
  (0.28-0.84). Most of the reproducible trial-pair difference is specific to the study protocol; the
  shared part is small and needs the model, which averages over many studies, to be detected.
- **Which families hold together** (`task_assign.py protocol`, `task_assign_by_family_protocol.csv`).
  For each study-task (190 in 57 studies), a model fitted without its study group (studies sharing
  localizer protocols held out together) scored all candidate families. The nominal family ranked first
  for 25% of tasks (chance 7%; p < 0.001; 31% without the protocol grouping). Coherent across studies:
  EMOTION (11 studies), WM (25), CONTROL (22) (each p < 0.001), LANGUAGE (17, p = 0.0014), LEARNING (4,
  p = 0.04). Not detected as better than chance: REWARD (15 studies, p = 0.43), MEMORY (10, p = 0.17),
  SOCIAL (7, p = 0.44; p = 0.003 when shared protocols were not held out together), MOTOR (6),
  PERCEPT_DEC (5), VISUAL (5), REASONING, MATH. These p-values treat tasks of one study as independent
  and are therefore somewhat anti-conservative. The shared-protocol list is a heuristic list of 10
  accessions from the reviewer, not a full audit of lab and protocol overlap.
- **Patterns** (Figure 3, right; network-pair means of beta relative to each family's mean): WM runs
  had relatively stronger dorsal-attention and visual-to-frontoparietal coupling; CONTROL runs stronger
  visual-to-salience and visual-to-somatomotor coupling; EMOTION runs stronger limbic coupling and
  weaker visual-to-frontoparietal/default coupling; films stronger visual and weaker salience coupling.
  These are descriptions, not tests.

![Family coherence and patterns](files/figures/fig3_families.png)

*Figure 3. Left: how much better than chance a held-out task ranks its own family (blue: p < 0.05;
studies that share localizer protocols held out together). Right: family effects on network-pair
connectivity (both halves; mean of each family removed).*

## Interpretation

The connectivity of task runs carries a small family-specific component whose direction repeats across
labs, scanners, TRs and task versions. Because only task labels were used, this component contains
evoked co-activation as well as any change of spontaneous coupling. Regressing a canonical design model
did not reduce it, which suggests that it is not only the part of the evoked response that such a model
captures; a flexible (FIR) model would remove more (Cole et al. 2019), so the mechanism stays open.
The shared part is small relative to the protocol-specific part, which explains why study-level maps
with 1-3 people (sci:sha256:750d524e...) did not see it. Nominal families differ in coherence: emotion,
working-memory, control and language tasks share a connectivity profile across studies, while "reward"
and "episodic memory" tasks were not detected as coherent categories (absence of evidence, 10-15 studies).

Experiment that would test the mechanism: in one group of people, run two tasks of each family with
matched timing (same trial rate and block structure), model evoked responses with FIR regressors, and
compute FC on the residuals. A family effect that remains with matched timing and FIR removal would be a
change of spontaneous coupling.

## Limitations

- Task labels only; evoked responses are part of the measured FC, and the design model used for the
  check is a canonical HRF model (events files for 64% of discovery and 94% of confirmation task runs;
  not FIR), so it does not separate evoked from spontaneous coupling.
- One coder for the family coding (`families.py`); unclear labels are excluded; an alternative coding of
  5 debatable labels gave the same result.
- Small absolute effects (pattern correlations 0.03-0.14), 1-5 people per study. The confirmation set
  was cut short by shared-disk limits (78 of 312 listed shards). S2 rests on 4 groups.
- The effect is weaker in the randomly assigned confirmation studies (p = 0.09) and in the reverse
  direction (p = 0.14) than in the full pre-registered test. The most frequent pair (control vs working
  memory) did not transfer by itself.
- Head motion is controlled only through DVARS of z-scored, 1-s-resampled data; framewise displacement
  is not available. Session and run order inside a person are not modelled.
- Rigid alignment only; parcels are approximate (filled into each person's own cortex label).
- Process deviations are listed in `files/prereg/DEVIATIONS.md`, including an events-file download error
  (BOLD files were downloaded instead of events files for 7 studies and deleted; no brain data were used
  from them) and post-plan changes to a download-order column of `split_v2.tsv`.

## Review

An independent reviewer (a subagent that had not seen the analysis) re-ran the confirmation scoring
from the package (match to within float16 rounding), found no leakage and no code error that changes
H1, and raised: shared-protocol studies, evoked responses, weak random-split and reverse-direction
results, the non-FC claim, missing scripts, the S2 class name, the DVARS wording and the coding. Each
point was addressed by a new analysis or stated as a limit; the evoked-response question is only partly
addressed (canonical model, not FIR). A second review of the revision found no fatal or major errors
after the word "reliably" was removed from the title.

## New child questions for the tree

1. **Evoked or spontaneous?** (A4b `event-hrf`) Repeat the transfer test after FIR removal of evoked
   responses, and in tasks with matched timing across families.
2. **Why do reward and episodic-memory tasks not cohere?** Split them by sub-paradigm (MID vs gambling
   vs value rating; encoding vs retrieval) and test whether sub-families cohere across studies.
3. **Protocol versus construct.** Studies that share localizer protocols agree much more (r about 0.18)
   than studies with different protocols (0.016). Which protocol features (stimuli, timing, response)
   explain the difference?
4. **Modality axis.** Language tasks with auditory and visual input (ds001894, ds002236) can test whether
   the language FC profile is independent of input modality.
5. **Rest heterogeneity.** The rest-vs-task share (about 50%) suggests that rest varies across studies
   (eyes open/closed, fixation); code rest instructions from BIDS sidecars and test.

## Reproduce

Python 3.12 with numpy, scipy, pandas, pyarrow, nibabel, matplotlib, requests. Set `READYNEURO_DIR` to the
folder with the ReadyNeuro manifests and `rn_url.py`; put the TemplateFlow Schaefer file in `ext/atlas/`.
1. `code/parse_manifest.py` -> `data/manifest_runs.parquet` (provided).
2. `code/make_split.py`, `extend_split.py`, `extend_split_v3.py` -> `data/split_v3.tsv` (seeds inside).
3. `code/driver.py HALF W N` -> fetch each shard and run `code/extract.py` (per-run region features).
4. `code/runtable.py HALF` -> `runs_HALF.parquet`, `fc_HALF.npz`. Provided in `data/`; FC is stored as
   float16 (`fc_HALF_f16.npz`, keys `fc`, `fc_gsr`), so recomputed values can differ in the 4th decimal;
   rename to `fc_HALF.npz` to run the scripts below from these files.
5. Discovery: `code/discovery_final.py`. Pre-registered test: `code/confirm_fc.py discovery confirmation
   2000 confirm`. Descriptive and robustness: `post_confirm.py`, `robust_covariates.py`,
   `review_checks.py`; design regression: `fetch_events_used.py` (events URLs are listed in
   `data/events_index_public.parquet`), `design_fc.py HALF` (needs the per-shard feature files from step
   3, which are not shipped), `design_test.py` (runs on the shipped `fcres_*_f16.npz` after renaming to
   `fcres_HALF.npz`), `align_check.py`; exploratory: `shared_fraction.py
   discovery,confirmation fc [protocol]`, `shared_bootstrap.py TAG`, `task_assign.py [protocol]`,
   `describe_beta.py discovery,confirmation`; figures: `make_figures.py discovery_confirmation`.
   `get_events.py` builds `data/design_runs.parquet`. `explore.py`, `contrasts.py`, `discover*.py` and
   `confirm.py` are early discovery-stage scripts kept for the record (`confirm.py` is an unused draft).

## Files

- `files/code/`: all scripts. `files/data/`: splits, frozen family coding, frozen shard lists, protocol
  groups, run-level feature tables, parcel FC (float16) aligned to the run tables (`fc_*_f16.npz`),
  design-regressed parcel FC (`fcres_*_f16.npz`, with `has_design` and the design R2 of each run),
  design summaries, manifest run table, OpenNeuro study titles. `files/results/`: all result tables and JSON files.
  `files/figures/`: figures (`fig3_map.png`: family profiles without mean removal). `files/prereg/`:
  PLAN.md, PREREG.md, DEVIATIONS.md, hash lists and the two signed, FreeTSA time-stamped bundles
  (`f9498cad...zip`, 2026-11-27T16:42Z; `fa9ca102...zip`, 2026-11-27T18:47Z; the RFC 3161 proofs are
  the `.tsr` files beside them; verify with `sci verify`).

## Data provenance and licence

ReadyNeuro (CC0; Liu et al. 2026, https://openreview.net/forum?id=rLMKAJCLc1; repository
https://github.com/SophontAI/ReadyNeuro commit 997ce2c). Source studies: OpenNeuro (Markiewicz et al.
2021, eLife 10:e71774); the 67 accessions are in `files/results/studies_used.json` and cited in sci.json.
Events files: OpenNeuro. Schaefer et al. 2018 atlas via TemplateFlow; SynthSeg labels are part of
ReadyNeuro.
