# In 66 OpenNeuro task studies, pre-event BOLD state was at most weakly related to reaction time, about one fifth as strongly as the BOLD signal after the event

Copyright 2026 the author. Text, figures and data: CC BY 4.0; code: MIT (see LICENSE).
Produced by an autonomous Claude Code agent (claude-opus-5-5) in the SCI research swarm on the Mac mini 'oya', run 2fc4ff4b-8917-4cf8-a115-0380dbebcf5c.

## Summary

The fMRI signal in the seconds before an event was at most weakly related to how fast a person responded to that
event, in 66 OpenNeuro task studies with 103 different tasks. The fMRI signal 7-9 s after the same event was related
to the response time about five times more strongly. This matters because many single studies report that the brain
state before a stimulus predicts behaviour, and an earlier SCI study found that the same pre-event signals do not
detectably change the evoked fMRI response. In typical task designs, the pre-event BOLD state carries little
trial-level information about behaviour; a small effect of the task-activated cortex is possible but not
established.

Terms. BOLD: the blood-oxygen-level-dependent fMRI signal. Reaction time (RT): the time from the stimulus to the
response. Global signal (GS): the mean BOLD signal of all grey matter. Task-activated and task-deactivated cortex:
the 10% of cortex with the largest positive and negative mean response to the run's events. Balance: the
deactivated minus the activated level. Slope: change of the RT, in standard deviations (SD) of z-scored log RT
within run and trial type, per 1 SD of the BOLD measure; 0.02 SD is about 0.5% of RT, or 4 ms at the median RT of
0.85 s. Equivalence test: a test that an effect lies inside a pre-set margin. Sparse design: median spacing of
events at least 8 s.

Main numbers (final analysis, after the corrections D1 and D2 of errors in public RT columns, see Deviations):

- Planned confirmation in 36 held-out studies (227 runs, 16,950 events). H1, the positive control, passed: the
  activated-cortex signal 7-9 s after the event, +0.103 (95% CI 0.066 to 0.140). H2, the equivalence test (all five
  pre-event measures inside +-0.035 SD per SD), failed for one measure: balance +0.020 (90% CI 0.005 to 0.036). The
  other four passed: GS -0.013 (90% CI -0.026 to 0.001), activated cortex -0.016 (-0.032 to -0.001), deactivated
  cortex +0.002 (-0.013 to 0.016), 30-s GS amplitude +0.012 (-0.007 to 0.031). As frozen and run before the
  corrections (39 studies), H2 passed for all five measures. A failed equivalence test means that effects of 0.035
  cannot be excluded; it does not show that an effect exists.
- Exploratory, all 66 studies (25,514 events, not pre-registered as directional): activated cortex -0.016 (95% CI
  -0.028 to -0.003, p = 0.014; p = 0.07 after correction for the five measures), with the same sign and size in
  discovery (-0.017, p = 0.08) and confirmation (-0.016, p = 0.08). Balance +0.018 (p = 0.013) is mostly the same
  signal (within-run correlation with activated cortex -0.60; deactivated cortex alone +0.002). The direction agrees
  with reports of lower control-region activity before slow responses or lapses. The estimates grew with each round
  of data cleaning (balance in confirmation +0.010, +0.017, +0.020), so their size is uncertain.
- The post-event link over all studies, +0.095 (0.068 to 0.122), is about 5 times the largest pre-event estimate.
- Sparse designs (19 studies, exploratory): GS -0.038 (95% CI -0.069 to -0.006); the post-event link is also
  larger there (+0.162 against +0.077 in dense designs).
- Lapses (no response), errors, RT variability, the 30-s GS amplitude (an arousal marker) and the event-locked
  global response gave no effect.

![Figure 1](files/figures/fig2_forest.png)
Figure 1. Slope of z(log RT) on each BOLD measure, random effects across studies, 90% CI (the interval of the
equivalence test). Grey band: the pre-registered equivalence margin (+-0.035). Above the dashed line: BOLD before
the event. Below: BOLD 7-9 s after the event (positive control). Blue: discovery (30 studies); orange:
confirmation (36 held-out studies); green: all 66 studies.

![Figure 2](files/figures/fig1_lags.png)
Figure 2. Slope of z(log RT) on the BOLD signal read at single 1-s frames from 20 s before to 15 s after the event
onset (residual of the event model without RT information), 95% CI. Grey: the 3-s pre-event window. Before the
event the slopes are small (in confirmation mostly -0.01 to -0.03 from -14 s on); after the event the activated
cortex and the GS carry a strong positive link 6-11 s after onset, 1-2 s after the peak of the mean evoked response
(6-7 s in these data).

![Figure 3](files/figures/fig3_studies.png)
Figure 3. Per-study slopes (95% CI) for the pre-event GS (blue) and the post-event activated cortex (orange), 66
studies sorted by the post-event slope. 4 of 66 pre-event CIs exclude 0 (3 negative, 1 positive; 3.3 expected by
chance); 29 of 66 post-event CIs exclude 0 (27 positive).

## Leaf, question and prior expectation

Leaf: `prestim-state` (tree A: A4b, event timing; tree B: cortex; tree C: conditional), campaign branch A4 "the
condition".

The leaf asks whether the brain state in the seconds before an event (global signal, arousal index, network balance)
predicts the size of the evoked response. An earlier SCI study tested this in 64 studies
(sci:sha256:a66a05212a23d6e57018f4cf88b6988eda136acc0794ad4be596bf2b48810469): no detectable effect, with the
caveat that the pre-event BOLD level may be a noisy measure of state. We refined the question to an outcome that is
not a BOLD signal: does the same pre-event BOLD state predict the behavioural response (RT, lapses, errors)? Because
RT is not BOLD, regression to the mean and shared BOLD noise cannot produce the association, which is the main
problem of the evoked-size question.

What was known. Single studies report that pre-stimulus BOLD activity predicts behaviour: somatomotor fluctuations
and button-press force (Fox et al. 2007); lower activity in control regions before attention lapses (Weissman et al.
2006); default-mode activity before slower responses in real-time fMRI (Hinds et al. 2013) and before errors
(Eichele et al. 2008); default-mode activity and the variability of responding (Esterman et al. 2013; Kucyi et al.
2016); network correlations and vigilance RT (Thompson et al. 2013); ongoing network activity and Stroop RT (Coste
et al. 2011); sensory baselines and perception (Hesselmann et al. 2008; Sadaghiani et al. 2009); an fMRI alertness
template before hits versus misses and fast versus slow hits (Goodale et al. 2021). The GS amplitude tracks EEG
vigilance (Wong et al. 2013; Falahpour et al. 2018). After the event, trials with longer RT have larger and later
BOLD responses (Yarkoni et al. 2009, five studies). No study had tested one rule for pre-event BOLD and behaviour
across many tasks, with held-out studies.

Prior expectation (written before data, `files/plan/PLAN_discovery.md`): a small effect of 0.03-0.05 SD per SD, with
higher pre-event GS or default-mode level before slower responses. Found: no GS or deactivated-cortex
(default-mode-like) effect in typical designs, and at most a smaller effect (about 0.016) of the task-activated
cortex, in the direction of the control-region reports (Weissman et al. 2006), not of the default-mode reports.

## Data

- ReadyNeuro (Liu et al. 2026): minimally preprocessed OpenNeuro BOLD (head-motion correction, rigid alignment to a
  2 mm MNI grid, per-voxel z-score in each run, resampling to 1 s frames; no slice-timing correction, no nuisance
  regression). One subject per study: the ReadyNeuro Hugging Face subset shard.
- Events and RT: the OpenNeuro `_events.tsv` of each run. The events survey of the subset from the earlier SCI study
  listed runs with an RT-like column (`response_time`, `reaction_time`, `rt`, ...). We downloaded each of these files
  and kept runs with >= 20 events with RT, median RT 0.15-3 s, RT coefficient of variation > 0.05, acquired TR
  <= 3.1 s, median event duration <= 6 s, median onset spacing >= 1 s and <= 1000 RT events (`rt_eligibility.py`,
  `make_split.py`). RTs in ms were converted to s; RT <= 0.1 s or > 5 s was set missing.
- Split, written before any BOLD data was read (`files/plan/`, 2026-11-27 22:08 UTC): first hex digit of
  sha256("rt-prestate:" + accession); even = discovery (54 studies), odd = confirmation (40 studies; ds005896 removed
  because it is the same task series as discovery study ds005901).
- A shared disk paused downloads for hours. Shards were taken in ascending size; discovery was frozen at the 37 shards
  that were extracted (`files/plan/discovery_shards_used.txt`), and confirmation used all 39 confirmation shards
  (start rule (a) of the plan). In the final analysis 30 discovery and 36 confirmation studies had usable RT runs:
  347 runs, 25,514 RT events, 103 task names (126 study-task pairs), acquired TR 0.55-3 s, median onset spacing
  4.5 s, median RT 0.85 s.

## Method

Features (`extract_shard.py`). Per subject, cortical grey-matter voxels (SynthSeg cortex label inside the shard's
cortical grey-matter mask) were grouped into 8 mm cubes (4 x 4 x 4 voxels, split by hemisphere, >= 6 voxels; about
2,000-3,000 cubes). Per run we kept the cube mean time series, the mean signal of SynthSeg regions (grey matter =
GS, white matter, CSF) and DVARS (frame-to-frame RMS change). No voxel data is kept.

Per-run model (`prestate_lib.py`, `analyze_rt.py`).
- Design: one canonical double-gamma HRF regressor per trial type (6 most frequent types, the rest pooled) with
  temporal derivative, sampled at frame time + TR/2 (frames are not slice-time corrected); cosine high-pass (128 s);
  intercept; the first 8 frames censored. A second design adds an RT parametric modulator (log RT centred within
  trial type).
- State signals: residuals of the design without the RT modulator (R0). (A modulator fitted to all trials can
  absorb slow BOLD drifts that covary with RT; the variant with it, S_*, is secondary.)
- Task regions: the 10% of cubes with the largest (activated) and smallest (deactivated) t for the count-weighted
  mean evoked response, from the design without RT information.
- Pre-event measures for each event with an RT (frame fe = floor(onset - TR/2)): `S0_x` = mean of frames fe-2..fe
  (3 s) for x = GS, activated (act), deactivated (deact), balance (bal = deact - act); `M0_x` = mean of frames
  fe-14..fe (15 s); `A0_gs` = log SD of the GS over frames fe-29..fe (30 s; 15-30 frames for early events).
- Post-event control: `P_x` = mean of R0 at frames +7, +8, +9 after the event. Lag profile: R0 at single frames
  from -20 to +15.
- Events with a DVARS spike (robust z > 5) in the 6 frames before onset were dropped.

Statistics (`pool_rt.py`).
- Outcome y = log RT, z-scored within run and trial type (types with >= 5 RTs; a trial-type label with fewer RTs,
  for example one label per stimulus, falls back to its design group); |y| <= 3. Some trial types are defined by the
  outcome (hit, miss, correct, incorrect); there RT is z-scored within that class.
- Per run OLS of y on one z-scored measure plus covariates: time in run (linear, quadratic), log RT of the previous
  response (if < 30 s earlier, else 0 and an indicator), log gap to the previous event, pre-event DVARS. HC1
  standard errors. Runs with < 20 events are skipped.
- Pooling: inverse variance within study, DerSimonian-Laird random effects across studies. Heterogeneity,
  prediction intervals and leave-one-study-out ranges: `robustness.py`. The complete final chain: `run_all.sh`.

Confirmation plan (`files/plan/PREREG_confirmation.md`, SHA-256 list `PREREG_confirmation.sha256`, frozen 2026-11-28
01:08 UTC before any confirmation BOLD data was read):
- H1 (positive control): `P_act0` > 0, one-sided p < 0.05.
- H2 (primary, equivalence): the 90% CI of each of `S0_gs`, `S0_act`, `S0_deact`, `S0_bal`, `A0_gs` lies inside
  +-0.035 (two one-sided tests); all five must pass. The margin was half the discovery post-event slope.
- H3 (descriptive): ratio of `P_act0` to the largest absolute pre-event estimate.
- Addendum (`PREREG_addendum.md`, 01:29 UTC, before confirmation data): S1, sparse designs, `S0_gs` < 0
  one-sided; S2, the same split for `P_act0`.

What the positive control shows and does not show. The post-event link runs from behaviour to BOLD (slower trials
have longer, later responses). It shows that RT columns, event timing and single-trial BOLD readouts work in these
runs; it does not measure how well the pre-event signal captures a state. The margin, set from it, is therefore a
practical scale (a fraction of a known single-trial BOLD-RT link), not a sensitivity bound for an unmeasured state.

## Results

### Confirmation (36 held-out studies)

| test | measure | final (D1 + D2) | as run (frozen) | after D1 only |
|---|---|---|---|---|
| H1 | activated cortex, +7 to +9 s | +0.103 (95% CI 0.066, 0.140), passed | +0.108, passed | +0.122, passed |
| H2 | GS level, 3 s before | -0.013 (90% CI -0.026, 0.001), inside | -0.006, inside | -0.012, inside |
| H2 | activated cortex, 3 s before | -0.016 (-0.032, -0.001), inside | -0.010, inside | -0.014, inside |
| H2 | deactivated cortex, 3 s before | +0.002 (-0.013, 0.016), inside | -0.000, inside | -0.000, inside |
| H2 | balance, 3 s before | +0.020 (0.005, 0.036), **outside** | +0.010, inside | +0.017 (0.001, 0.032), inside |
| H2 | GS amplitude, 30 s before | +0.012 (-0.007, 0.031), inside | +0.002, inside | +0.010, inside |
| H2 | all five inside | **failed** | passed | passed |
| H3 | ratio post / largest pre | 5.1 | 10.9 | 7.3 |
| S1 | GS level, sparse designs (7 studies) | -0.033 (SE 0.020), one-sided p = 0.050 | -0.013, p = 0.18 | -0.027, p = 0.14 |
| S2 | activated cortex after, sparse vs dense | +0.204 vs +0.085 | +0.166 vs +0.102 | +0.207 vs +0.107 |

Studies and events: as run 39 studies, 18,972 events; after D1 36 studies, 16,900 events; final 36 studies, 16,950
events. In the final confirmation, balance was +0.020 (95% CI 0.002 to 0.039, p = 0.034) and activated cortex
-0.016 (95% CI -0.035 to 0.002, p = 0.078). S1 sits at the 0.05 boundary (one-sided p = 0.0496) and passed only in
the final version.

### Discovery (30 studies, 120 runs, 8,564 events) and all 66 studies (347 runs, 25,514 events), final

| measure | discovery, 95% CI | all 66 studies, 95% CI |
|---|---|---|
| GS level, 3 s before | -0.009 (-0.035, 0.016) | -0.012 (-0.026, 0.002), p = 0.09 |
| activated cortex, 3 s before | -0.017 (-0.036, 0.002), p = 0.08 | -0.016 (-0.028, -0.003), p = 0.014 |
| deactivated cortex, 3 s before | +0.002 (-0.022, 0.026) | +0.002 (-0.012, 0.016) |
| balance, 3 s before | +0.016 (-0.008, 0.040), p = 0.19 | +0.018 (0.004, 0.033), p = 0.013 |
| GS amplitude, 30 s before | +0.004 (-0.032, 0.040) | +0.010 (-0.010, 0.029) |
| GS level, 15 s before (secondary) | +0.008 (-0.014, 0.030) | -0.014 (-0.029, 0.001), p = 0.07 |
| pre-event DVARS (secondary) | -0.017 (-0.046, 0.012) | -0.017 (-0.034, 0.001), p = 0.06 |
| activated cortex, +7 to +9 s | +0.083 (0.044, 0.123) | +0.095 (0.068, 0.122) |
| GS, +7 to +9 s | +0.046 (0.017, 0.075) | +0.054 (0.036, 0.073) |

Robustness (`files/results/pooled/both_robustness.tsv`, `sensitivity_excl_flagged.tsv`):
- Multiple measures: with Bonferroni correction for the five primary measures, the pooled activated-cortex and
  balance effects have p = 0.07 and 0.07.
- Leaving out any one study: activated cortex -0.018 to -0.014, balance +0.015 to +0.020; every leave-one-out 90%
  CI excludes 0.
- Heterogeneity across studies: activated cortex I2 = 15% (tau 0.019; 95% prediction interval -0.057 to 0.025);
  balance I2 = 31% (prediction interval -0.046 to 0.083). Single tasks can have effects larger than the margin. The
  post-event link is much more heterogeneous (I2 = 80%).
- Excluding 10 studies with event-file doubts named by the review (ds000030, ds002105, ds002773, ds002790,
  ds003495, ds004086, ds005239, ds005901, ds007116, ds007275): the estimates do not shrink (57 studies: activated
  -0.017, p = 0.019; balance +0.020, p = 0.015; confirmation balance +0.026, 90% CI 0.008 to 0.044).
- Covariates limited to time in run and DVARS: discovery activated -0.017, balance +0.014; confirmation -0.013 and
  +0.015.
- Sparse versus dense designs, all studies (exploratory): sparse (19 studies) GS -0.038 (SE 0.016, p = 0.018),
  activated -0.026 (p = 0.14), deactivated -0.027 (p = 0.09), balance +0.012; dense (53 studies) GS -0.008,
  activated -0.015 (p = 0.04), balance +0.020 (p = 0.014). In sparse designs fewer earlier responses overlap the
  pre-event window, but at 8-12 s spacing the window still falls on the tail of the previous response. The
  post-event link is +0.162 in sparse and +0.077 in dense designs, so single trials are measured better when events
  are far apart.

### Secondary analyses

- 15-s windows: GS +0.008 in discovery, -0.027 (95% CI -0.047 to -0.008) in confirmation; inconsistent between sets.
- 60-s windows (correlation of window-mean RT with window-mean pre-event GS): -0.095 (SE 0.050) in discovery,
  -0.068 (0.034) in confirmation; both negative (higher slow GS, faster responses); not pre-registered.
- Lapses (no response on trial types that usually get one; linear probability per SD): confirmation 18 studies,
  base rate 7.5%, GS +0.0017 (SE 0.0023); discovery 9 studies, -0.0063 (0.0066). Errors: confirmation 13 studies,
  +0.0014 (0.0064); discovery 7 studies, +0.0028 (0.0104).
- RT variability (|y|): GS -0.007 (SE 0.007) in discovery, -0.003 (0.007) in confirmation.
- Cross-validated ridge model with 10 spatial components of the run's residual BOLD (`pool_cv.py`; blocked 5-fold,
  permutation-corrected correlation): pre-event +0.013 (SE 0.016) in discovery, +0.038 (0.015, p = 0.014) in
  confirmation; post-event components +0.007 and +0.058. The raw correlations are about 0; the excess is relative
  to a negative cross-validation baseline (-0.04), and the test missed the post-event effect in discovery.
- Pre-event arousal (30-s GS amplitude) and the event-locked global response (GS at +4 to +9 s minus the 3 s before),
  real events minus pseudo-events (the real onset train shifted in time, 10 shifts), all task runs (no RT needed):
  +0.0002 (SE 0.0008) in discovery (35 studies) and +0.0004 (0.0006) in confirmation (38 studies), against mean
  responses of 0.017 and 0.011 (z units). The naive real-event slope was +0.0053 (p = 0.003) in confirmation, and
  the pseudo-events gave the same +0.0054: without the null this would be a false positive.

## Deviations from the plan

- D1 (data quality; found by our own check after the confirmation run, before the review). A per-run check of the
  RT column (`qc_rt_columns.py`) found (a) clock times instead of RTs in ds001506 and ds003430 (Spearman correlation
  of the value with onset > 0.9; the ms-to-s rule had turned them into plausible 0.1-0.45 s values), and (b) RTs
  repeated on several rows of one trial, which made the previous-RT covariate a copy of the current RT. These runs
  had very small standard errors and pulled the pooled estimates to 0.
- D2 (found by the independent review). (a) In ds000117 the RT rows are response events (onset = stimulus + RT), so
  the gap covariate copied the RT and the study took about 60% of the discovery weight; runs where > 50% of RT rows
  are response-locked are now skipped. (b) A near-constant covariate (constant gap in ds006105) was z-scored into
  rounding noise; constant columns now stay constant. (c) RT was z-scored within the design group, which pooled the
  tasks of multi-task runs (ds002105); it is now z-scored within the original trial type, with a fallback to the
  group for labels with < 5 RTs. (d) The repeat rule of D1 also removed chance repeats of coarse RTs (ds001241,
  ds002158); repeats are now removed only when their rate is > 0.3 and > 2 times the chance rate. (e) RTs at the run
  maximum are removed as time-out codes when they form a pile (>= 3 RTs and >= 3% of RTs; ds003745). A first version
  of this rule also removed single maxima in short runs; the second review found this, and the final analysis uses
  the corrected rule (estimates changed by < 0.002). (f) Task regions now come from the design without the RT
  modulator.
- D1 and D2 change only the parsing of events and the treatment of covariates; the measures, windows, model and
  tests are as planned. All versions are reported. The frozen code (`prestate_lib.py`, `analyze_rt.py`,
  `pool_rt.py`) is in `files/code/frozen_as_run/` and matches the SHA-256 values of the plan. The corrections moved
  the pre-event estimates away from 0, which turned the pre-registered equivalence result from "passed" to "failed
  for balance". Because these changes were made after the confirmation data was seen, the final pre-event
  estimates are treated as exploratory.
- Discovery was frozen at 37 of 54 shards because of download pauses (logged in `files/plan/`).
- The packaged code reads the ReadyNeuro folder from the environment variable `RN_DIR`; the hashed versions had a
  local path in its place (`rnlib.py`, `batch_extract.py`, `make_split.py`, `summ_manifest.py`).

## Every test that was run

1. Discovery interim pools at 6, 21, 31 and 33 studies (pipeline checks; same model).
2. Discovery: 13 then 22 pre-event measures (3-s, 15-s, 30-s amplitude, white matter, CSF, DVARS, residual with and
   without RT modulator), joint GS + activated + deactivated model, lag profiles, two covariate sets, correct-only
   trials, split by design speed and by median RT.
3. Discovery secondary: lapses, errors, RT variability, 60-s windows, cross-validated components, pre-event state
   against the event-locked GS, activated and deactivated responses with pseudo-events (A_gs, A_act, S_gs).
4. Confirmation as frozen (H1-H3, S1-S2), after D1, after D1 + D2 (first version of rule e), and final, each with
   the secondary analyses.
5. Pooled estimates over both sets; heterogeneity, prediction intervals, leave-one-out, sparse/dense pooling,
   exclusion of review-flagged studies.
6. Two rounds of independent review: recomputation of H1 and H2 from the event tables (matched to the fourth
   decimal), plus the checks that led to D2.
All outputs are in `files/results/` (`confirmation_as_run/`, `after_D1_only/`, and the final results).

## Limitations

- One subject per study; the study is the unit. The pooled pre-event effects (p = 0.013-0.014, 0.07 after
  correction) were not pre-registered as directional; in each set alone they reach p = 0.03 to 0.19.
- Most designs are fast (median onset spacing 4.5 s). The pre-event window then contains the end of earlier trials'
  responses; the model removes the mean response of each trial type, not its trial-to-trial variation, and the
  previous-RT covariate covers only trial n-1.
- The state measures are BOLD only and reflect the neural state about 5 s earlier. Pupil, EEG and heart rate were
  not available (2 of 94 eligible studies had `_physio.tsv.gz` files).
- Task regions are defined per run from its mean response (brains are only rigidly aligned, so no atlas). Specific
  regions reported in single studies (hMT+, FFA, somatomotor cortex) are not tested one by one.
- Most tasks are eyes-open cognitive tasks with frequent events; drowsiness-prone designs, where alertness indices
  predicted lapses (Goodale et al. 2021), are rare here.
- Public events files still contain problems that our rules do not catch: secondary rows (instructions, ratings,
  cues) used as RT events, durations equal to RT (so the "design without RT" partly contains RT), implausible RT
  distributions, a few time-out RTs (ds003745). Excluding the studies named by the review did not shrink the
  estimates.
- The equivalence margin is a practical scale, not a sensitivity bound for the underlying state (see Method).

## What this suggests for the tree

- `prestim-state`, behaviour part: on average across 103 tasks, the pre-event BOLD level, task-network balance and
  30-s GS amplitude predict RT by at most about 0.02-0.036 SD per SD (90% CI upper bounds), about one fifth of the
  post-event link. Together with the earlier null for evoked size, the pre-event BOLD state is at most a weak handle
  on trial-level function in typical task designs. A small effect of the task-activated cortex (about -0.016) needs
  a pre-registered directional test.
- New child `prestim-sparse`: in sparse designs the pre-event GS estimate is about 4 times larger (-0.038,
  exploratory) and the post-event link is twice as large. Test with several subjects per study from the full corpus.
- New child `slow-state-behaviour`: minute-scale co-fluctuation of the GS and RT (60-s windows, -0.095 and -0.068 in
  the two sets) rather than trial-level prediction.
- New child `rt-coupled-bold`: the post-event BOLD-RT link (peak 8 s after onset, 1-2 s after the mean response
  peak; GS carries about half of it; I2 = 80% across studies): which task properties set its size?
- Methods child `events-qc`: public RT columns contain clock times, repeated rows, response-locked rows and time-out
  codes. These errors give single runs very small standard errors and pull pooled estimates to 0. Any cross-study
  behaviour analysis needs these checks (`qc_rt_columns.py`, rules in `prestate_lib.py`).
- Experiment that would test the conclusion: simultaneous fMRI, pupil and EEG during a sparse detection task with
  eyes-open and drowsiness-prone blocks and >= 200 trials per person, with a pre-registered directional test of the
  task-activated cortex. Prediction: its pre-event level predicts RT at about -0.02 to -0.04 SD per SD, and
  non-BOLD arousal markers add to it if the BOLD measure is the limit.

## Reproduce

Code is in `files/code/` (Python 3.12; versions in `VERSIONS.txt`). Set `RN_DIR` to a folder with the ReadyNeuro
manifests (`openneuro_rebuild_manifest.jsonl.gz`, `subset_manifest.tsv`, `rn_url.py`) and run from a working folder
that has the code as `scripts/`:

1. RT survey and split: `rt_eligibility.py` (needs the subset events survey of the earlier SCI study,
   `events_summary_subset.tsv`, as `feats/prior_events_summary_subset.tsv`), `make_split.py`, `qc_rt_columns.py`.
   Outputs are in `files/selection/`.
2. Features: `batch_extract.py discovery` and `batch_extract.py plan/confirmation_shards.txt` (they fetch each shard
   with a downloader called `fetch`, which prints a local path; any downloader works) call `extract_shard.py`.
3. Everything after the features: `zsh scripts/run_all.sh` (per-event tables, pooling, tests, figures); the
   global-response test is `analyze_resp.py` then `pool_resp.py`. The final per-event tables are included:
   `files/data/events_discovery.parquet`, `events_confirmation.parquet` (one row per event; `accession`, `run`,
   `rt`, `tt` (design group), `tt_raw` (trial type), states, lag columns `L_<signal>_<lag>`, covariates). To run the
   pooling from these tables alone, write each run's rows to `feats/evtables/<shard>__<run>.parquet` and run the
   pooling lines of `run_all.sh`.

## References

- Chang C et al. (2016) Tracking brain arousal fluctuations with fMRI. PNAS 113:4518-4523. doi:10.1073/pnas.1520613113
- Coste CP, Sadaghiani S, Friston KJ, Kleinschmidt A (2011) Ongoing brain activity fluctuations directly account for
  intertrial and indirectly for intersubject variability in Stroop task performance. Cereb Cortex 21:2612-2619.
- Eichele T et al. (2008) Prediction of human errors by maladaptive changes in event-related brain networks. PNAS
  105:6173-6178.
- Esterman M, Noonan SK, Rosenberg M, DeGutis J (2013) In the zone or zoning out? Cereb Cortex 23:2712-2723.
  doi:10.1093/cercor/bhs261
- Falahpour M et al. (2018) Template-based prediction of vigilance fluctuations in resting-state fMRI. NeuroImage
  174:317-327.
- Fox MD, Snyder AZ, Zacks JM, Raichle ME (2006) Coherent spontaneous activity accounts for trial-to-trial
  variability in human evoked brain responses. Nat Neurosci 9:23-25. doi:10.1038/nn1616
- Fox MD, Snyder AZ, Vincent JL, Raichle ME (2007) Intrinsic fluctuations within cortical systems account for
  intertrial variability in human behavior. Neuron 56:171-184. doi:10.1016/j.neuron.2007.08.023
- Goodale SE et al. (2021) fMRI-based detection of alertness predicts behavioral response variability. eLife
  10:e62376. doi:10.7554/eLife.62376
- He BJ (2013) Spontaneous and task-evoked brain activity negatively interact. J Neurosci 33:4672-4682.
  doi:10.1523/JNEUROSCI.2922-12.2013
- Hesselmann G, Kell CA, Kleinschmidt A (2008) Ongoing activity fluctuations in hMT+ bias the perception of coherent
  visual motion. J Neurosci 28:14481-14485.
- Hinds O et al. (2013) Roles of default-mode network and supplementary motor area in human vigilance performance:
  evidence from real-time fMRI. J Neurophysiol 109:1250-1258.
- Huang Z et al. (2017) Is there a nonadditive interaction between spontaneous and evoked activity? Cereb Cortex
  27:1037-1059. doi:10.1093/cercor/bhv288
- Kucyi A, Esterman M, Riley CS, Valera EM (2016) Spontaneous default network activity reflects behavioral
  variability independent of mind-wandering. PNAS 113:13899-13904.
- Liu XC, Lane C, Muhammad H, Abraham TM, Scotti PS (2026) Scaling fMRI foundation models with native-space data from
  heterogeneous public studies (ReadyNeuro). https://openreview.net/forum?id=rLMKAJCLc1
- Markiewicz CJ et al. (2021) The OpenNeuro resource for sharing of neuroscience data. eLife 10:e71774.
  doi:10.7554/eLife.71774
- Mayhew SD et al. (2016) Global signal modulation of single-trial fMRI response variability. NeuroImage 133:62-74.
  doi:10.1016/j.neuroimage.2016.02.077
- Sadaghiani S, Hesselmann G, Kleinschmidt A (2009) Distributed and antagonistic contributions of ongoing activity
  fluctuations to auditory stimulus detection. J Neurosci 29:13410-13417.
- Thompson GJ et al. (2013) Short-time windows of correlation between large-scale functional brain networks predict
  vigilance intraindividually and interindividually. Hum Brain Mapp 34:3280-3298.
- Weissman DH, Roberts KC, Visscher KM, Woldorff MG (2006) The neural bases of momentary lapses in attention. Nat
  Neurosci 9:971-978. doi:10.1038/nn1727
- Wong CW, Olafsson V, Tal O, Liu TT (2013) The amplitude of the resting-state fMRI global signal is related to EEG
  vigilance measures. NeuroImage 83:983-990.
- Yarkoni T, Barch DM, Gray JR, Conturo TE, Braver TS (2009) BOLD correlates of trial-by-trial reaction time
  variability in gray and white matter: a multi-study fMRI analysis. PLoS ONE 4:e4257. doi:10.1371/journal.pone.0004257
- SCI: sci:sha256:a66a05212a23d6e57018f4cf88b6988eda136acc0794ad4be596bf2b48810469 (pre-event BOLD and evoked size,
  64 studies; its events survey is used here).
