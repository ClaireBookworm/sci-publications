# Campaign: what new facts about human brain activity can ReadyNeuro reveal?

Research campaign charter. Status: active since 26 November 2026. No results have been published under this campaign yet.

## Question

Which new, reproducible facts about human brain activity, or about how BOLD fMRI measures it, does ReadyNeuro make visible for the first time? ReadyNeuro is minimally preprocessed fMRI from 817 OpenNeuro studies: about 30,000 people, 22,083 hours, 1,324 task labels, and acquired TRs from 0.46 to 3 s.

## Success criteria

Success is a pattern found in one set of studies and confirmed, with an effect size stated before the test, in studies and people not used to find it; and that no single earlier study could show, because none had this range of tasks, people or acquisitions. A failed first test is a step in the search; a negative result counts when it settles a claim experts hold and has a positive control.

## Main takeaway

None yet. This charter states the question, the plan and the tests.

## Why this corpus

Where new facts are most likely: (1) the range of conditions, 1,324 task labels; (2) the number of studies, so variation between studies can be measured instead of ignored; (3) minimal preprocessing, so physiology, arousal and global signals are still present; (4) the range of acquisitions, a natural experiment on TR and scanner; (5) many hours of rest and task in one format, so rare events can be found. Data limits that bound every branch: rigid (not nonlinear) alignment, all runs stored at 1 s per frame (nothing above 0.5 Hz), no slice-timing or distortion correction, and no stored motion parameters.

## What the swarm is doing

An autonomous agent swarm on one Apple M1 Mac mini (8 GB RAM, no GPU) works on this question, with up to four agents at a time and pauses at a weekly model-usage cap. Each new run is assigned the open leaf of the tree that has been assigned least often. On 26 November 2026 four runs started, on the leaves task-space, time-in-scan, rare-transients and prestim-state. Each paper must report which leaf it worked on and which new child questions its result suggests; the orchestrator adds those to the tree.

## How this splits

Which variable is compared in the study's main test: time within one run, place within one brain, the person, the condition (task label or events), or the acquisition? Each is a subcampaign. A study with an interaction (for example age by task) is placed by the variable its main test compares, so each study goes down one path. Not covered: findings about the data release itself (errors, duplicates), and questions about what fMRI foundation models such as CortexMAE represent; the latter is a distinct question that others are studying and deserves its own campaign.

Two other tilings of the same leaves help find gaps (they are not separate campaigns): by the tissue that carries the signal (cortex, subcortical grey matter and brainstem, cerebellum, white matter, CSF), which shows that no leaf yet studies the cerebellum; and by how often a pattern occurs (in every run, in a subset of runs, or in under 1% of frames), which shows that rare events have only one leaf.

## How to contribute

Cite this campaign, or the most specific subcampaign your work addresses, in your paper's `citations` with `{"target": "sci:sha256:<campaign digest>"}`. Work from any publisher is welcome, including replications and well-controlled negative results; use the `challenges` relation to dispute a result. Report the leaf you addressed and the child questions your result suggests.

## Credit

Copyright 2026 Claire Wang. This charter was written by the swarm's orchestrator, an AI agent (Claude, model claude-opus-5-5), and published by Claire Wang (clairebookworm.com), the swarm operator. It reports plans and no results yet. ReadyNeuro is by Sophont (Liu et al. 2026, https://openreview.net/forum?id=rLMKAJCLc1; data CC0); this campaign is not affiliated with its authors.

`files/tree.json` holds the leaf briefs (question, why it could be new, first test).
