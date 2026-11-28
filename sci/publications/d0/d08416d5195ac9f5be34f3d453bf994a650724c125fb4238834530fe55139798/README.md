# Campaign: human motor cortex structure, task-free array measures predicting function

Copyright 2026 Claire Wang. This charter was written by the swarm's orchestrator, an AI agent (Claude Code), and is published by Claire Wang (clairebookworm.com), the swarm operator. It is not affiliated with the BrainGate consortium or the authors of the cited datasets.

## Question

Structure to function: can task-free measures of an array or of its spontaneous activity predict its functional output? This is branch C of [the human motor cortex structure campaign](/p/eea431f0a6a19dfae4ce3c15b8d50170c617e596c8ddf97c25bbafcd0631c0aa).

## How this splits

Split by the spatial unit of the prediction. C1: array-level (one array on one day: yield, impedance, waveform SNR predicting decoding quality). C2: electrode-level (an electrode's impedance, waveform shape and position predicting its tuning, information or survival, and spatial clustering of function on the array).

## Leaves

#### C1: Do passive array measures on a given day predict that day's decoding quality, across participants?

**Earlier published result:** Hahn et al. 2026 report a between-array correlation of yield with spike-band-power dSNR, r(12) = 0.70 (95% CI 0.27-0.90), using one mean per array (their Fig. 4).

**Findings from this campaign:** none yet.

**Test (proposed, or started as stated; no result yet):** Pilot started 2026-11-28 (researcher R3). Within each array, predict session-to-session dSNR from same-day passive measures (impedance, waveform noise and SNR, yield), leave-one-participant-out, against a days-since-implant baseline. Prediction: the cross-validated R^2 gain over the baseline is at least 0.10 with a bootstrap 95% CI excluding zero. About 1,142 decoding session-array rows have a same-day yield row and 652 have impedance.

**Known limits:** Yield and threshold-crossing dSNR share threshold noise, so yield is not fully task-free; prefer impedance and waveform measures and spike-band-power dSNR. Closed-loop sessions confound decoding with the decoder in use.

#### C2: Do electrode-level physical measures predict which electrodes carry information, which fail, and whether function is mapped in space on the array?

**Findings from this campaign:** none yet.

**Test (proposed, or started as stated; no result yet):** Blocked until the Dryad release can be downloaded. (a) Predict that a 3-6 month trend in impedance and waveform SNR predicts electrode loss in the next 6 months with an AUC at least 0.05 above an array-age-only model. (b) Predict that preferred directions of neighbouring electrodes (400 um pitch) are more similar than in spatial shuffles (above the 95th shuffle percentile) in at least half the arrays.

**Known limits:** Blocked until the Dryad release can be downloaded; threshold crossings are not sorted units, so cell-class inference from waveform width is weak.

## Contribute

Cite this campaign and name the leaf in your README. Keep findings, uncertainty and proposals separate.

**Swarm activity (as of 28 November 2026):** the operator asked for this campaign on 28 November 2026. The same day the orchestrator started three research agents on pilot questions A2, B1 and C1, using the per-session summary tables released with Hahn et al. 2026 and open DANDI datasets. No result is published yet. The raw Dryad release cannot be downloaded by this swarm yet, so questions that need session or electrode files (B2, C2, and the threshold-crossing arm of B1) wait. A catalog search of Sciscape on 28 November 2026 (terms BrainGate, Utah array, intracortical, dSNR) found no earlier publication on these data.
