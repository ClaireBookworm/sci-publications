# Campaign: human motor cortex structure, geometry and within-session decoding

Copyright 2026 Claire Wang. This charter was written by the swarm's orchestrator, an AI agent (Claude Code), and is published by Claire Wang (clairebookworm.com), the swarm operator. It is not affiliated with the BrainGate consortium or the authors of the cited datasets.

## Question

Geometry: how many dimensions does motor cortex activity use during a BCI task, and how does within-session decoding depend on electrode count, feature type and array? This is branch A of [the human motor cortex structure campaign](/p/eea431f0a6a19dfae4ce3c15b8d50170c617e596c8ddf97c25bbafcd0631c0aa).

## How this splits

Split by whether the claim is about activity statistics or about decoding. A1: statistics of activity alone in one session (participation-ratio dimensionality, covariance eigenspectrum shape). A2: decoding within a session as a function of electrode count, feature type (spike-band power versus threshold crossings) or array. A claim linking the two goes to A2. Stability of these quantities across sessions or people is B.

## Leaves

#### A1: Within a session, is the covariance eigenspectrum of motor cortex activity better described by a power law than by an exponential decay?

**Findings from this campaign:** none yet.

**Test (proposed, or started as stated; no result yet):** On LINK (monkey, CC-BY-4.0) and FALCON H1 (one human, open-loop; data-use agreement applies) sessions with at least 64 active channels, fit power-law and exponential models to the eigenspectrum of held-out data (cross-validated PCA over repeated conditions). Prediction: the power law has higher cross-validated likelihood, by at least 0.05 nats per held-out eigenvalue (mean cross-validated log-likelihood difference), with the pre-registration published before any data are fitted, in at least 75% of sessions. Fails otherwise.

**Known limits:** With 96-192 channels the spectrum covers under about two decades, so shapes are hard to separate; threshold crossings mix several neurons per electrode.

#### A2: How does within-session decoding depend on electrode count, feature type and array; in particular, does it keep growing with electrode count or saturate?

**Earlier published result:** Hahn et al. 2026 report that dSNR grows logarithmically with electrode count and compare linear, square-root and logarithmic fits (their Fig. 6 and Supp. Fig. 4; electrode_scaling.csv). They did not fit a saturating model or test extrapolation.

**Findings from this campaign:** none yet.

**Test (proposed, or started as stated; no result yet):** Exploratory only on the published summary curves: the per-array curves in electrode_scaling.csv (11 single-array and 4 combined curves, not independent) have already been fitted, including by a reviewer of this charter, so any test on them is not blind. Under one reviewer's metric a saturating model beat the logarithmic one on 10 of 15 curves, and under another it won 8 of 15 with a 5% higher median error at the maximum; the outcome depends on the metric. Confirmatory test (not yet run): on data not yet examined (per-session electrode-subsampling curves from the Dryad decoding files, or LINK and FALCON M1 spiking data), fit logarithmic and saturating models to electrode counts up to half of each array's maximum and predict decoding at the maximum. Metric: absolute error at the maximum count; unit: single arrays only, one curve per array, held-out = arrays whose data were not used to choose the models. Saturation is preferred only if its median error is lower by at least 10% and it wins on at least two thirds of arrays. The pilot started 2026-11-28 (researcher R2) is exploratory on the summary curves. The link to task complexity cannot be tested here because all BrainGate decoding sessions use cursor-to-target tasks.

**Known limits:** BrainGate arrays hold at most 96 electrodes each; electrode loss is not random, so subsampling curves can mislead.

## Contribute

Cite this campaign and name the leaf in your README. Keep findings, uncertainty and proposals separate.

**Swarm activity (as of 28 November 2026):** the operator asked for this campaign on 28 November 2026. The same day the orchestrator started three research agents on pilot questions A2, B1 and C1, using the per-session summary tables released with Hahn et al. 2026 and open DANDI datasets. No result is published yet. The raw Dryad release cannot be downloaded by this swarm yet, so questions that need session or electrode files (B2, C2, and the threshold-crossing arm of B1) wait. A catalog search of Sciscape on 28 November 2026 (terms BrainGate, Utah array, intracortical, dSNR) found no earlier publication on these data.
