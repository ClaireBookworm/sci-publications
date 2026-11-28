# Campaign: human motor cortex structure, what learned models add

Copyright 2026 Claire Wang. This charter was written by the swarm's orchestrator, an AI agent (Claude Code), and is published by Claire Wang (clairebookworm.com), the swarm operator. It is not affiliated with the BrainGate consortium or the authors of the cited datasets.

## Question

Learned models: what do models trained across sessions and people learn that fixed analyses miss, and how does that scale? This is branch D of [the human motor cortex structure campaign](/p/eea431f0a6a19dfae4ce3c15b8d50170c617e596c8ddf97c25bbafcd0631c0aa).

## How this splits

Split by whether the claim is about what improves a model's decoding or about what its representation contains. D1: what improves learned decoding: pretraining data (sessions, hours, people), model size or architecture, and recalibration or alignment methods, compared at a stated budget. D2: what a trained model's representation contains (probing latents for structure found in A and B).

## Leaves

#### D1: What improves learned decoding most: more pretraining sessions or people, larger or different models, better recalibration, or more electrodes?

**Findings from this campaign:** none yet.

**Test (proposed, or started as stated; no result yet):** Idea, not yet a test: train small CPU-scale latent models on increasing numbers of LINK sessions and compare few-shot decoding gains per doubling of sessions against the electrode-count curve from A2. A criterion will be pre-registered before any run.

**Known limits:** No GPUs in this swarm; only small models are feasible.

#### D2: Do learned latents recover the stable structure found by fixed analyses, or something else?

**Findings from this campaign:** none yet.

**Test (proposed, or started as stated; no result yet):** Idea, not yet a test: after A and B report, probe latents of a small pretrained model for the persistent component found in B1.

**Known limits:** Depends on results from A and B.

## Contribute

Cite this campaign and name the leaf in your README. Keep findings, uncertainty and proposals separate.

**Swarm activity (as of 28 November 2026):** the operator asked for this campaign on 28 November 2026. The same day the orchestrator started three research agents on pilot questions A2, B1 and C1, using the per-session summary tables released with Hahn et al. 2026 and open DANDI datasets. No result is published yet. The raw Dryad release cannot be downloaded by this swarm yet, so questions that need session or electrode files (B2, C2, and the threshold-crossing arm of B1) wait. A catalog search of Sciscape on 28 November 2026 (terms BrainGate, Utah array, intracortical, dSNR) found no earlier publication on these data.
