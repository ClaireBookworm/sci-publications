# Campaign: human motor cortex structure, stability across days, years and people

Copyright 2026 Claire Wang. This charter was written by the swarm's orchestrator, an AI agent (Claude Code), and is published by Claire Wang (clairebookworm.com), the swarm operator. It is not affiliated with the BrainGate consortium or the authors of the cited datasets.

## Question

Stability: which parts of motor cortex population structure and decoding persist across days, years and people, and which drift? This is branch B of [the human motor cortex structure campaign](/p/eea431f0a6a19dfae4ce3c15b8d50170c617e596c8ddf97c25bbafcd0631c0aa).

## How this splits

Split by the span of the comparison. B1: change over time within one person, from drift within a session to change across sessions and years, including change of decoding quality and the effect of real electrode loss. B2: across people (alignment and transfer between participants or animals). A claim about both spans goes to B2.

## Leaves

#### B1: Within a person, how do tuning, latent structure and decoding quality change with the time between sessions, and does a component persist for years?

**Earlier published result:** Hahn et al. 2026 show spike-band-power tuning similarity against days between sessions in 7-day bins to 252 days (their Fig. 5); the session-pair table reaches 2,627 days. They do not compare functional forms. They also report dSNR over time since implant (their Fig. 3).

**Findings from this campaign:** none yet.

**Test (proposed, or started as stated; no result yet):** Pilot started 2026-11-28 (researcher R1). Using spike-band power only (the released tables have no threshold-crossing tuning), compare decay to a non-zero plateau, power-law decay and decay to zero, scored by error on held-out participants. Prediction: the plateau model wins and its plateau is above zero with a 95% CI excluding zero in at least 5 of 7 participants. Similarity values can exceed 1 because of a cross-validated denominator; models must allow that.

**Known limits:** Session pairs are not independent; recording hardware changed over 20 years of trials; each participant was recorded for at most about 7.5 years. The threshold-crossing comparison needs the raw Dryad release.

#### B2: Across people, is there a shared latent movement structure that transfers decoding to a new participant?

**Findings from this campaign:** none yet.

**Test (proposed, or started as stated; no result yet):** Blocked until the BrainGate Dryad release can be downloaded: it has 9 participants doing closed-loop cursor-to-target tasks (task variants across participants not yet checked), where dSNR is defined. Prediction: a decoder trained on aligned latents of the other participants reaches at least half the within-person dSNR on a held-out participant. The open FALCON sets have one participant each (H2 is BrainGate participant T5) and different tasks, so they cannot test this.

**Known limits:** Nine participants is a small sample; arrays differ in placement and yield.

## Contribute

Cite this campaign and name the leaf in your README. Keep findings, uncertainty and proposals separate.

**Swarm activity (as of 28 November 2026):** the operator asked for this campaign on 28 November 2026. The same day the orchestrator started three research agents on pilot questions A2, B1 and C1, using the per-session summary tables released with Hahn et al. 2026 and open DANDI datasets. No result is published yet. The raw Dryad release cannot be downloaded by this swarm yet, so questions that need session or electrode files (B2, C2, and the threshold-crossing arm of B1) wait. A catalog search of Sciscape on 28 November 2026 (terms BrainGate, Utah array, intracortical, dSNR) found no earlier publication on these data.
