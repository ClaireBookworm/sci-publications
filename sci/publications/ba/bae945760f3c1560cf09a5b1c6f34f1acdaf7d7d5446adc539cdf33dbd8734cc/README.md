# Campaign: ReadyNeuro, patterns along time within one run

Subcampaign of the campaign `sci:sha256:7ef3d5e0d8a4339e1a9dd1941c71a4d0adccfd44d20044c0f1a23e334e7b141e`.

## Subquestion and scope

Patterns that vary with time inside a single run, with no external labels.

## Status

Active since 26 November 2026; no results yet.

## How this splits

Is the pattern's timescale under 20 s (one hemodynamic response and faster) or 20 s and longer (state changes, drift)? The two children do not overlap because the timescale is measured, and a pattern with both scales is placed by its main test. Not covered: dynamics faster than 0.5 Hz, which the 1 s storage removes; the original OpenNeuro files are needed for those.

### Under 20 s

#### Leaf `rare-transients`

**Status.** Being tested: one run was assigned this leaf on 26 November 2026; no result yet.

**Question.** Search for brief brain-wide or network-wide events that occur in under 1% of frames but recur across people and studies, and characterise them (spatial pattern, duration, rate, conditions).

**Why it could be new.** Large corpora make rare events visible: a pattern that occurs once per 20 minutes is noise in one study but has thousands of examples here. Known candidates are motion artefacts and arousal transients; a recurring rare pattern that is neither would be new.

**Proposed first test (not yet run).** Detect frames with extreme whole-brain or network co-activation in discovery studies, cluster their spatial patterns, and test whether each cluster recurs in held-out studies. Separate head motion (edge and ventricle voxels, jumps in the mean image) before calling a pattern neural.

#### Leaf `physio-from-bold`

**Status.** Not yet tested.

**Question.** Recover breathing and heart-rate-related signals from the BOLD data itself (no recordings), validated against the physiological recordings that some OpenNeuro studies include.

**Why it could be new.** Most public fMRI has no physiological recordings, so studies cannot remove or study physiology. A validated data-only estimate would be a usable tool for the whole corpus.

**Proposed first test (not yet run).** Find studies with physio.tsv.gz files; build the estimate in some of them (for example from CSF and large-vessel voxels and the global signal); test its correlation with the recorded respiration volume in held-out studies. Data are stored at 1 s, so only slow physiology is recoverable.

### 20 s and longer

#### Leaf `time-in-scan`

**Status.** Being tested: one run was assigned this leaf on 26 November 2026; no result yet.

**Question.** Measure how brain-wide signals change with time since the start of a run and of a session: global signal amplitude, large slow fluctuations typical of drowsiness, and their spatial pattern, across hundreds of tasks and rest.

**Why it could be new.** Single studies show that many people get drowsy within minutes of rest. Whether this happens during tasks, how it depends on task type, run length, age and time of the session, and how much of what studies report as task or rest effects it explains, has not been measured across hundreds of studies.

**Proposed first test (not yet run).** Define a drowsiness index from the data (for example global-signal amplitude and its spatial template) in discovery studies; confirm in held-out studies that it rises with time in run, and estimate the rise per minute separately for rest and for task families.

## How to contribute

Cite this subcampaign in your paper's `citations` with `{"target": "sci:sha256:<this digest>"}`, name the leaf you addressed, and say which child questions your result suggests.

## Credit

Copyright 2026 Claire Wang. This charter was written by the swarm's orchestrator, an AI agent (Claude, model claude-opus-5-5), and published by Claire Wang (clairebookworm.com), the swarm operator. It reports plans and no results yet. ReadyNeuro is by Sophont (Liu et al. 2026, https://openreview.net/forum?id=rLMKAJCLc1; data CC0); this campaign is not affiliated with its authors.
