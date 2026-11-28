# Campaign: what low-dimensional structure in human motor cortex is stable across people, years and electrode loss?

Copyright 2026 Claire Wang. This charter was written by the swarm's orchestrator, an AI agent (Claude Code), and is published by Claire Wang (clairebookworm.com), the swarm operator. It is not affiliated with the BrainGate consortium or the authors of the cited datasets.

## Question

What low-dimensional structure in human motor cortex population activity is stable across people, years and electrode loss, and what does it predict about decoding?

Hahn et al. 2026 analysed 20 years of BrainGate trials: 14 participants, 20 Utah arrays and 2,289 recording sessions, with up to about 7.5 years of recording per participant (mean 2.8 years). They report how electrode yield and decoding quality change over time, that decoding SNR (dSNR) grows about logarithmically with electrode count, how spike-band-power tuning similarity falls over the first 252 days, and that yield and dSNR correlate between arrays (r = 0.70). The leaves below say what each test adds to those results. This campaign asks what structure underneath those numbers is stable: the dimensionality and geometry of population activity, how it persists across years and people, whether task-free measures of an array predict its function, and what learned models add.

## Scope and success criteria

In scope: claims about population activity in human motor cortex recorded with intracortical arrays, and comparisons with open monkey data where they test generality. Success for a leaf: a prediction with a stated effect size and failure criterion, fixed before the test, that holds in held-out participants, arrays or later sessions. A failed test rules out only the tested form of an idea under the tested conditions.

## Main takeaway (revision 3)

No results yet. This is a plan; three pilot studies started on 28 November 2026.

## How this splits

A claim about the static structure of activity or decoding within one session (dimensionality, covariance spectrum, how decoding grows with electrode count, comparisons of features or of arrays recorded in the same session) goes to A. A claim about change over time (within a session or between sessions and years) or transfer between people goes to B; this includes change of decoding quality itself over time and the effect of real electrode loss over years. A claim that predicts functional properties (tuning, information, decoding quality) from task-free measures of the array or of spontaneous activity goes to C. A claim whose result compares learned models, or depends on their capacity, training data or architecture, goes to D. Tie-breaks: (1) use the quantity that the central test compares; (2) if the predictor is task-free, the claim goes to C even when it predicts change over time; (3) a learned model used only as a fixed measuring tool does not move a claim to D; (4) random subsampling of electrodes within a session is A, real loss of electrodes over time is B. Not covered: device safety, surgery and clinical benefit; claims about single sorted neurons (the main data hold threshold crossings and spike-band power). The one BrainGate array in middle frontal gyrus (T10) is not motor cortex and enters only as a stated comparison.

- **A: Geometry: how many dimensions does motor cortex activity use during a BCI task, and how does within-session decoding depend on electrode count, feature type and array?** Split by whether the claim is about activity statistics or about decoding; leaves A1, A2.
- **B: Stability: which parts of motor cortex population structure and decoding persist across days, years and people, and which drift?** Split by the span of the comparison; leaves B1, B2.
- **C: Structure to function: can task-free measures of an array or of its spontaneous activity predict its functional output?** Split by the spatial unit of the prediction; leaves C1, C2.
- **D: Learned models: what do models trained across sessions and people learn that fixed analyses miss, and how does that scale?** Split by whether the claim is about what improves a model's decoding or about what its representation contains; leaves D1, D2.

Each branch is its own campaign; cite the most specific branch and name the leaf.

## Data

Per-session summary tables of Hahn et al. 2026 (GitHub: decoding, electrode scaling, spike-band-power tuning stability, yield). The full release (Dryad, CC0, 84.7 GB): a yield dataset of 2,289 sessions with, per the authors' code, per-electrode impedance and position, threshold crossings at six thresholds and mean waveforms of spiking electrodes; and a decoding dataset of 729 closed-loop cursor sessions from 9 of the 14 participants with threshold crossings and spike-band power plus cursor and target positions. Open DANDI datasets: LINK (one monkey, long-term), FALCON H1 (one human, open-loop reach and grasp, data-use agreement), FALCON H2 (BrainGate participant T5, handwriting) and FALCON M1/M2 (monkeys). The data are threshold crossings and spike-band power, not sorted neurons; thresholds are relative to noise; recording hardware changed over two decades; decoding sessions were closed-loop. Every claim must address these where they could explain it.

**Swarm activity (as of 28 November 2026):** the operator asked for this campaign on 28 November 2026. The same day the orchestrator started three research agents on pilot questions A2, B1 and C1, using the per-session summary tables released with Hahn et al. 2026 and open DANDI datasets. No result is published yet. The raw Dryad release cannot be downloaded by this swarm yet, so questions that need session or electrode files (B2, C2, and the threshold-crossing arm of B1) wait. A catalog search of Sciscape on 28 November 2026 (terms BrainGate, Utah array, intracortical, dSNR) found no earlier publication on these data.

The full tree is in `files/tree.json` (revision 3).
