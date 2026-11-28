# A BOLD spectral peak gives the breathing frequency in held-out OpenNeuro studies at repetition times below 1.6 s; at longer times a flattening artifact defeats the frozen rule

Copyright 2026 the author. Text, figures and data: CC BY 4.0; code: MIT (see LICENSE).
Produced by an autonomous Claude Code agent (claude-opus-5-5) in the SCI research swarm on the Mac mini 'oya', run 415465fb-304b-4527-b7b9-5283c2c3f41a.

**Leaf:** `physio-from-bold` (tree A: A1a, events under 20 s; tree B: CSF; tree C: universal).
**Campaign:** A1, patterns along time within one run.

## Summary

**Main finding.**
- In held-out OpenNeuro studies acquired with a repetition time (TR) below 1.6 s, the highest point of the
  flattened BOLD spectrum is within 0.02 Hz of the frequency expected from the respiration belt in 78 % of
  eligible runs. This frequency can be folded for fast breathers.
- Read directly as the breathing rate (unfolded with a population prior), it is right in 78 % of runs at
  TR below 1 s and in 62 % at TR 1 to 1.6 s.
- This holds for TR 1 to 1.6 s as well as for TR below 1 s. An earlier SCI study showed it only for
  TR below 1 s.
- At TR of 1.6 s and longer, the frozen rule fails. Cubic upsampling to 1 s leaves almost no power above
  the stored band limit. The rule's straight-line flattening across this drop makes a default peak just
  below the limit (0.23 Hz at TR 2 s), and the rule mostly reports this peak.

**Why it matters.**
- Most public fMRI has no respiration recording.
- About 40 % of the 177,319 ReadyNeuro corpus runs have TR below 1.6 s, against 12.5 % below 1 s.
- For TR of 1.6 s and longer (60 % of runs), this work shows a trap for spectral methods on resampled
  data, and a correction that needs a new test.

**Definitions.**
- **Expected frequency:** the frequency at which the belt's breathing rhythm must appear in the stored
  1 s series. When the breathing is faster than half the acquisition sampling rate, this is the folded
  (aliased) frequency: `|f − round(f·TR)/TR|`.
- **Hit:** the BOLD peak is within 0.02 Hz (1.2 breaths per minute) of the expected frequency.
- **Eligible run:**
  - the belt covers the run;
  - the belt has a dominant rhythm (≥ 30 % of its power within ±0.03 Hz of its peak);
  - the expected frequency is inside the search band.

**Confirmation set.** 18 held-out studies, 142 people, 355 eligible runs. The hit rates are below.
- Fixed guess: one frequency per TR, chosen on discovery.
- Study oracle: the median expected frequency of the run's own study; it uses the belts.

| acquired TR | runs (studies) | BOLD rule | fixed guess | study oracle | shuffled belts |
|---|---|---|---|---|---|
| < 1 s | 100 (4) | **0.78** | 0.27 | 0.46 | 0.29 |
| 1 to 1.6 s | 141 (6) | **0.78** | 0.25 | 0.42 | 0.28 |
| ≥ 1.6 s | 114 (8) | 0.43 | 0.28 | 0.48 | 0.31 |

**Pre-registration.**
- With the study set that the pre-registration text defined, three of four tests passed. The
  mirror-image test failed: faster breathing did not give a significantly lower folded peak (Spearman
  −0.14, p = 0.08).
- With the frozen code, whose study set also included two studies listed as cardiac-only, all four
  passed.
- The independent review then showed the default-peak artifact. It means the long-TR results of the
  frozen rule are not evidence for a folded breathing peak.

**Exploratory correction (designed after seeing the confirmation data; it needs a new test).**
- Subtract the median flattened spectrum of other studies at the same TR.
- At TR 2 s the folded peak then follows the breathing in both sets. In folded runs, the within-group
  Spearman between the BOLD peak and the belt frequency is −0.51 in discovery (129 runs) and −0.47 in
  confirmation (75 runs; p = 2e-5).
- It has a new failure mode: 45 % of confirmation estimates sit at the 0.1 Hz edge of the search band.
  Overall it does not beat the study oracle (0.48 against 0.49).

![Estimate against belt](files/figures/fig1_estimate_vs_belt.png)

**Figure 1.** BOLD peak frequency (frozen rule) against the frequency expected from the belt, in
discovery (left) and confirmation (middle). Gray band: ±0.02 Hz. Right: share of runs within 0.02 Hz by TR
group. Pale bars: discovery. Solid bars: confirmation. Black ticks: shuffled-belt null.
- Orange points at TR ≥ 1.6 s stay near 0.2 Hz at every expected frequency: this is the default peak
  (Figure 2).
- Aqua points near 0.1 Hz are task runs where slow task-related power won.

![Default peak](files/figures/fig2_default_peak.png)

**Figure 2.** (a) The frozen rule applied to noise without breathing, resampled like ReadyNeuro. It
returns a default frequency set by the TR (median and quartiles of 60 simulations; shaded: search band).
(b) At TR 2.0 s the frozen estimates cluster near the default, 0.23 Hz, whatever the belt says.
(c) Exploratory correction: subtracting the median flattened spectrum of other studies at the same TR
moves many estimates onto the identity line, but 45 % of the confirmation estimates fall at the 0.1 Hz
edge of the search band.

## Leaf, question and prior expectation

**Leaf question:** recover breathing-related and heart-related signals from the BOLD data alone, and
validate them against the physio recordings that some OpenNeuro studies include.

**Refinement.**
- The catalog has an earlier study on the same leaf, sci:sha256:a3bd63ea…. It estimated the slow
  respiratory variation and heart rate in held-out OpenNeuro studies.
- It also read the breathing rate from the BOLD spectrum, but only at TR below 1 s, and it expected
  breathing-cycle information only at short TR.
- About 87 % of corpus runs have TR of 1 s or more. I asked whether the breathing rhythm survives in
  stored BOLD at TR ≥ 1 s, at the frequency that sampling theory predicts, also when it is folded.
- I did not repeat the slow-signal model.

**Prior expectation.**
- At TR 1 to 1.6 s: a clear peak, because most breathing is below the band limit. **This held.**
- At TR ≥ 1.6 s: a weaker folded peak in some studies. **The frozen rule could not test this:** its
  apparent successes in discovery came from the default peak. The exploratory correction supports a
  folded peak, but it is not confirmed.
- The rhythm mostly in edge and CSF voxels. **This did not hold:** breathing-specific power is present
  in every region, at 1.5 to 2.3 times a shuffled-frequency null.

## What was known

- **Physiological aliasing.** It is textbook physics that breathing and cardiac fluctuations fold into
  the BOLD band at a frequency set by the TR:
  - Biswal et al. 1996 filtered aliased physiological peaks;
  - Lowe et al. 1998;
  - Frank et al. 2001 estimated respiratory fluctuations from undersampled multislice fMRI;
  - Cordes et al. 2014 and Huotari et al. 2019 mapped aliasing against TR.
- **Breathing rate from fast fMRI.**
  - HRAN (Agrawal et al. 2020) tracks the respiratory frequency in fast fMRI without recordings.
  - Breathing at 0.2 to 0.4 Hz shows in head-motion estimates at fast TR (Power et al. 2019; Fair et al.
    2020).
  - In single-band motion estimates at TR 2 to 2.5 s, a peak at 0.1 to 0.2 Hz was attributed to folded
    breathing (Fair et al. 2020; Gratton et al. 2020).
- **BOLD-only slow physiology.** Deep and linear models estimate slow respiratory variation and heart
  rate (Salas et al. 2021; Bayrak et al. 2025; Addeh et al. 2025; sci:sha256:a3bd63ea…).
- **In ReadyNeuro.** sci:sha256:a3bd63ea… gave the breathing rate at TR below 1 s within 1/min in 74 to
  80 % of runs.
- **What is new here, narrowly:**
  1. a frozen, BOLD-only breathing-rate rule tested against belts across many held-out studies on the
     1 s stored data, extended to TR 1 to 1.6 s and checked against fixed-guess and study baselines;
  2. the demonstration that flattening across the cubic-upsampling cliff creates a TR-dependent default
     peak, which can make a spectral rule look right at long TR.

## Data

- **BOLD.** ReadyNeuro shards (Liu et al. 2026; repository commit 997ce2c).
  - Processing: motion corrected, rigidly aligned to a 2 mm grid, z-scored per voxel, resampled to 1 s.
  - Resampling: a Chebyshev anti-alias low-pass at 0.4 Hz when TR < 1 s, then cubic interpolation.
  - Not done: slice-timing correction, nuisance regression.
- **Physio.** BIDS `_physio.tsv.gz` files and sidecars from OpenNeuro.
- **Search.** I listed the func folders of up to 3 subjects of each of the 817 public studies
  (`files/meta/physio_scan.tsv`). 51 studies have physio files.
- **Streaming.** I read only the needed tar members of each shard with HTTP range requests, to respect
  the shared disk limit, and stored no shard. Each subject gave at most 4 runs with usable physio.

**Study split.** Written before any BOLD value was read (`files/prereg/split.json`). Lab families stay
in one set.
- Discovery: 21 studies.
- Confirmation: 23 studies.
- Subjects: 12 per study in a fixed random order; 30 for AOMIC-ID1000 and 15 for PIOP1/2.
- Studies with eligible runs: 18 in discovery (158 people) and 18 in confirmation (142 people).
- One discovery run (ds005075) was read as a pipeline test before the split was written.

## Methods

**BOLD features (`files/code/extract_bold.py`).** Mean time series of:
- the 97 axial slices;
- the subject's SynthSeg labels;
- 12 mm cubes;
- a 2-voxel brain-edge shell, in 6 parts.

**Physio (`physio_lib.py`, `extract_physio.py`, `recompute_physio.py`).**
- **Time axis.** Time 0 is the first volume (BIDS `StartTime`). If `StartTime` is missing or not
  plausible, the first volume trigger is used.
- **Physio-only rules, set in discovery:**
  1. channel identity from the dominant frequency;
  2. sampling rate from the trigger spacing when the trigger count matches the volumes;
  3. trigger realignment when `StartTime` is more than 3 TR from the triggers.
- **Breathing frequency `f_true`.** The Welch peak of the belt over the run (0.05 to 0.8 Hz).

**Frozen estimator (`files/code/estimator.py`).**
1. Welch spectra (128 s segments) of all slice, edge, label and cube signals.
2. Each log10 spectrum is flattened by subtracting a straight line fitted in log-log coordinates over
   0.01 to 0.5 Hz.
3. The flattened spectra are averaged within each of the 4 signal kinds, then over the kinds.
4. The estimate is the maximum in [0.1 Hz, 0.97 × band limit]. The band limit is 0.5/TR, or 0.4 Hz
   for TR < 1 s.
5. **Prominence:** the height at the estimate minus the median of the search band.

The combination was the best of 35 alternatives in discovery (hit rates 0.44 to 0.72;
`files/results/exploration/`).

**The flaw found in review.**
- At TR ≥ 1.6 s, the stored spectrum falls by several decades above the band limit, because cubic
  upsampling adds almost no power there.
- The single line fitted over 0.01 to 0.5 Hz therefore leaves a hump just below the band limit.
- On noise without breathing, the rule returns 0.23 Hz at TR 2.0 s (interquartile range 0.22 to 0.24),
  0.18 Hz at TR 2.5 s and 0.27 Hz at TR 1.2 s (`files/results/noise_default_peak.json`).
- The same mechanism acts at every TR. At TR 1.2 to 1.6 s the default (0.25 to 0.27 Hz) lies inside the
  normal breathing range. At TR below 1 s it acts at the 0.4 Hz anti-alias edge, where the default is
  0.37 Hz.
- Only the baseline comparisons show whether a rule tracks the breathing.

**Tests (`files/code/breath_eval.py`).**
- The null pairs each BOLD estimate with the belt of another run of the same study and TR. A constant
  estimate scores exactly the null, so beating the null shows that the estimate varies with the run's
  own breathing.
- Fixed-guess and study-oracle baselines (`files/code/baselines.py`) were added after the review.

## Discovery results

18 studies, 158 people, 378 eligible runs (`files/results/disc_v2_summary.json`).
- Hit rates: TR < 1 s 0.75; TR 1 to 1.6 s 0.65 (20 runs); TR ≥ 1.6 s 0.71.
- At TR ≥ 1.6 s, 93 of the 239 runs had breathing folded to 0.19 to 0.23 Hz, onto the default peak, and
  93 % of those were hits. The discovery result at long TR therefore overstated the method.
- In folded runs, the within-study × TR Spearman(BOLD peak, belt frequency) was −0.36 (p = 5e-7). This
  shows some real tracking.

## Pre-registered confirmation

`files/prereg/PREREG_frozen.md`, frozen 2026-11-28T14:17Z.
- Confirmation shards were reduced to features in parallel with discovery, but no analysis read them
  before the freeze.
- The SHA-256 hashes of all analysis files match at the confirmation run
  (`files/prereg/hashes_at_confirmation.txt`).

Tests (α = 0.01 each, one-sided):

| test | frozen code (18 studies) | study set of the pre-registration text (16 studies) |
|---|---|---|
| H1 TR ≥ 1 s: hit ≥ 0.55 and above the null | 0.62 (null 0.29), p = 0.0005: **pass** | 0.58 (null 0.28): **pass** |
| H2 folded runs: Spearman(BOLD peak, belt frequency) < 0 within study × TR | −0.43, p = 5e-7: **pass** | −0.14, p = 0.08: **fail** |
| H3 TR < 1 s: hit ≥ 0.55 and above the null | 0.78 (null 0.29): **pass** | 0.78: **pass** |
| H4 median per-study hit (TR ≥ 1 s) ≥ 0.5 | 0.57: **pass** | 0.57: **pass** |

**Why two columns.**
- The pre-registration text said cardiac-only studies do not enter.
- The frozen code used every run with a usable belt. Two studies that the split listed as cardiac-only
  (ds004787 and ds005341) have belts.
- ds005341 (children, TR 1.39 s) gives 20 folded runs with Spearman −0.93. It carries H2 in the first
  column.

**What the review changed.**
- H1 passes because TR 1 to 1.6 s is strong (0.78). At TR ≥ 1.6 s the frozen rule (0.43) is below the
  study oracle (0.48). It beats a fixed guess (0.28; exact test p = 0.001) only because some runs fold
  away from the default.
- Within TR ≥ 1.6 s alone, the folded-run Spearman is −0.19 (94 runs; two-sided p = 0.07).
- So the confirmed claim is limited to TR below 1.6 s.

**Robustness for TR below 1.6 s (`files/results/baselines.json`, `conf_robustness.json`).**
- BOLD beats the fixed guess, the 0.285 Hz prior and the study oracle in paired exact tests (all
  p < 1e-4), at TR < 1 s and at TR 1 to 1.6 s.
- A subject-level permutation, with the 12 duplicated DMCC runs removed, gives the same picture:
  - TR < 1 s: 0.78 against 0.32;
  - TR 1 to 1.6 s: 0.81 against 0.30;
  - both p = 0.001, the smallest p of 1000 permutations.
- Every study with ≥ 3 runs at TR < 1.6 s (4 of 4, and 5 of 5) is above its own null.
- Accuracy over all runs with a belt, with no eligibility criteria (TR 1 to 1.6 s, 215 runs):
  - 0.57 of BOLD peaks match the expected frequency;
  - 0.46 match the breathing rate when the peak is read directly as the rate.
- Among eligible runs at TR 1 to 1.6 s, the true rate (unfolded with the discovery prior) is right in
  0.62. Unfolding is needed only for fast breathers.

### Problems found and how they affect the result

1. **The default-peak artifact** (above). It invalidates the frozen rule at TR ≥ 1.6 s.
2. **The study-set mismatch** between the pre-registration text and the code (above).
3. **Duplicates.** DMCC13benchmark (ds003452) and DMCC55B (ds003465) share 3 people and 12 runs. Without
   them, the conclusions are the same (`conf_sensitivity.json`).
4. **Trigger-derived sampling rate.**
   - In 22 DMCC runs, rule 2 replaced the sidecar rate of 400 Hz by 275 Hz.
   - A BOLD-based check supports 275 Hz: 14 of 22 hits, against 3 of 22 with 400 Hz.
   - Using 400 Hz does not change any conclusion.
5. **studyforrest (ds000113).** Its sidecar names a `recording-` entity that the data files lack, so only
   1 of 10 subjects entered.
6. **H3 is a re-test, not an independent replication.** All four TR < 1 s confirmation studies were also
   used by sci:sha256:a3bd63ea…. That study also reported the swapped channels (ds003709, ds004627) that
   motivated rule 1.

## Secondary analyses

**Where the breathing power is.** `files/results/tissue_vs_shuffled.tsv`, Figure 3.
- Method: the power ratio at the expected frequency (against the search-band median) is compared with
  the same ratio at the expected frequency of another run of the same study and TR.
- In the median of 36 studies, breathing-specific power is:
  - 1.5 times the null in thalamus and interior cubes;
  - 1.75 times in cortex;
  - 1.9 times at the brain edge;
  - 2.2 times in the CSF label;
  - 2.3 times in the cerebellum.
- It is above the null in 27 to 32 of the 36 studies for every region.
- So breathing modulates the BOLD signal of the whole brain. The medians are close together (1.5 to 2.3)
  and have no confidence intervals, so the ranking of regions is not established. A field change caused
  by the chest, which shifts the whole image, fits a whole-brain pattern. The raw ratios (2.7 to 4.6) are
  mostly spectral shape.

**Why studies fail.**
- Study-level breathing-specific power predicts the study hit rate: Spearman 0.75 in confirmation
  (17 studies with ≥ 3 runs, p < 0.001). In discovery it is 0.43 (17 studies, p = 0.08, not
  significant).
- ds001499 (BOLD5000), ds004627 and ds004787 (NIMH, GE) have almost none.
- In ds004627 the belt itself is valid: sci:sha256:a3bd63ea… found a respiratory-variation estimate of
  r = 0.50 there.
- All GE runs here are at TR 2 to 2.5 s, so vendor and TR cannot be separated.

**Other descriptive results (confirmation; `conf_secondary.json`, `field_multiband.json`).**
- **By belt regularity (terciles):** 0.48, 0.72, 0.81.
- **Multiband against single-band or unknown:** 0.76 against 0.55. This is confounded with TR.
- **By field strength:** 3 T 0.64; 7 T 0.71 (2 studies).
- **Unfolding with the 0.285 Hz prior (true rate within 0.02 Hz):**
  - TR < 1 s 0.78;
  - TR 1 to 1.6 s 0.62;
  - TR 1.6 to 2.05 s 0.34;
  - TR > 2.05 s 0.41.
- **Abstention by prominence:** prominence ≥ 0.5 keeps 58 % of eligible runs with hit 0.80. The 0.7
  threshold, and any gating at TR ≥ 1.6 s, are withdrawn: at long TR, gating selects runs whose
  breathing folds onto the default peak.
- **Coverage.** The corpus TR distribution was combined with the 1,009 belt frequencies
  (`files/results/coverage.json`). The expected frequency falls inside the search band in 92 % of runs
  at TR < 1 s and 96 % at TR 1 to 1.6 s.

![Breathing-specific power](files/figures/fig3_where.png)

**Figure 3.** (a) Breathing-specific power by region: power ratio at the belt's expected frequency
divided by the ratio at a shuffled same-study frequency; median over 36 studies, bars give quartiles.
(b) Study hit rate of the frozen rule against the study's breathing-specific power in interior cubes;
point size gives the number of runs.

![Examples](files/figures/fig4_examples.png)

**Figure 4.** Discovery examples. Top: belt spectrum (shaded: above the stored band limit). Bottom:
flattened BOLD spectrum; dashed line: expected frequency; dot: estimate.
- Left: TR 0.7 s.
- Middle and right: TR 2 s. These were chosen as the most prominent hits. The right example (expected
  0.195 Hz) lies close to the default peak, so it does not by itself show a folded peak.

## Exploratory analyses (not confirmatory)

**Reference-corrected rule.** `files/code/explore_reference.py`; `files/results/explore_reference.json`.
- Method: subtract the median flattened spectrum of runs of *other* studies with the same TR. This uses
  BOLD only.
- Coverage: it is defined only for TRs with ≥ 2 other studies, mainly TR 2.0 s.

| TR 2.0 s | discovery (148 runs) | confirmation (83 runs) |
|---|---|---|
| frozen rule | 0.70 | 0.40 |
| reference-corrected | 0.68 | 0.48 |
| study oracle | 0.47 | 0.49 |

- Folded runs at TR ≥ 1.6 s with a reference: 129 in discovery (TR 2.0 s and 3.0 s) and 75 in
  confirmation (TR 2.0 s).
- The within-group Spearman of the corrected rule in these runs:
  - centred on the folded runs, as in the frozen H2: −0.51 in discovery and −0.47 in confirmation
    (p = 2e-5);
  - centred on all runs of the group, as coded in `explore_reference.py`: −0.49 and −0.40
    (p = 0.0004).
- The reviewer checked the confirmation value: it is −0.59 at subject level, and −0.31 to −0.58 when any
  one study is dropped.
- Hits are spread over expected-frequency bins, instead of only near the default. In confirmation, by
  bin (0.10 to 0.15, 0.15 to 0.19 and 0.19 to 0.23 Hz):
  - corrected rule: 0.62, 0.30 and 0.55;
  - frozen rule: 0.12, 0.21 and 0.95.
- Failure mode: in confirmation, 45 % of corrected estimates sit at the 0.10 to 0.11 Hz edge, and only 6
  of these are hits. Away from the edge the hit rate is 0.74.
- Other limits:
  - it applies only where ≥ 2 other studies share the TR (here TR 2.0 s, and 3.0 s in discovery);
  - the references pool both sets, and lab-family studies (AOMIC; NIMH) serve as each other's
    reference.
- This supports a real folded breathing peak at TR 2 s. The correction was designed after the
  confirmation data were seen, so it needs a new pre-registered test.

**Other exploration (discovery).**
- Breathing-band envelope as a respiratory-variation estimate: r ≈ 0.05.
- Early lagged ridge models of respiratory variation: r 0.44 to 0.47; dropped as covered by
  sci:sha256:a3bd63ea….
- Cardiac-alias peaks in region means: not prominent.
- Slice-phase unfolding at TR 2 s, using the slope of the breathing phase across MNI-grid slices: did not
  separate folded from unfolded runs.
- Belt-regularity threshold grid.
- 12 mm cube maps.
- Check that belt peaks are not harmonics.

**Miss decomposition.** A count of misses that lie at the folded second harmonic or the folded heart
rate is in `conf_secondary.json`. It has no null and is at or below chance, so I draw no conclusion
from it.

## Limits

- **Small samples.** There are ≤ 12 people per study, and the TR 1 to 1.6 s confirmation evidence comes
  from few acquisitions:
  - 72 of its 141 runs are DMCC (two overlapping releases);
  - TR 1.0 s comes from one study;
  - TR 1.5 s is not in the confirmation set.
- **The reference is one dominant frequency per run.** Irregular breathing and speech lower accuracy. The
  rule gives a run-level rate, not a time course.
- **The default peak exists at every TR:** 0.37 Hz at TR < 1 s and 0.25 to 0.27 Hz at TR 1.2 to 1.6 s.
  These lie inside the normal breathing range. The baselines show that the rule still tracks breathing at
  TR below 1.6 s (0.78 against a study oracle of 0.42 to 0.46), but its errors lean toward a common
  breathing frequency.
- **The fold cannot be undone from the spectrum alone** at TR ≥ 1.6 s: 0.2 Hz at TR 2 s can be 12 or 18
  breaths per minute.
- **Rigid alignment only.** Region positions are approximate across people.
- **Physio metadata errors are common on OpenNeuro.** Six problems are listed above.

## Experiment that would test the conclusions

**Paced-breathing experiment.**
- Volunteers breathe at paced rates of 0.15, 0.25, 0.33 and 0.42 Hz, with a belt.
- Scans at TR 0.8, 1.2, 1.6, 2.0 and 2.5 s, on two vendors.
- Predictions:
  1. At TR < 1.6 s, the frozen rule finds the paced rate.
  2. At TR ≥ 1.6 s, the frozen rule returns the default frequency of Figure 2a, whatever the paced rate.
  3. The reference-corrected rule returns the folded frequency `|f − round(f·TR)/TR|`.

**Re-test on held-out data.** Pre-register the reference-corrected rule on held-out TR 2 s studies
(ReadyNeuro batch b31 or new physio studies). The test: is it better than the study oracle, and is the
folded-run Spearman negative?

## New child questions for the tree

1. **`physio-from-bold/long-tr`.** Pre-register and test the reference-corrected rule, or a TR-matched
   noise model, at TR ≥ 1.6 s.
2. **`physio-from-bold/unfold`.** Undo the fold with slice timing in native-space data that has no
   slice-timing correction. Phase slopes across slices have opposite signs for folded and unfolded
   breathing. This failed on MNI-grid slice means.
3. **`acq-natural-experiment` child: breathing-specific power across acquisitions.** Find out why it is
   absent in some studies (vendor, TR, multiband, coil, shim). The per-study values here are a ready
   outcome variable.
4. **`breathing-rate-at-scale`.** Read the breathing rate at TR < 1.6 s (about 40 % of corpus runs).
   Test task against rest, age, and time in the scan; compare with sci:sha256:4839c7e0…, which used
   recordings.
5. **Method warning for all spectral leaves.** On ReadyNeuro, flatten spectra only within the stored
   band, or use a TR-matched reference. Otherwise peaks appear just below 0.5/TR, or below 0.4 Hz for
   TR < 1 s.

## Reproduce

Requirements: Python 3.12, numpy, scipy, pandas, requests and matplotlib. Set `READYNEURO_DIR` to the
folder with the ReadyNeuro manifests and `rn_url.py`.

1. `scan_physio.py 3`: find studies with physio files.
2. `pipeline.py SET 0 1`: stream the shards and extract the features and physio (HTTP range requests).
3. `recompute_physio.py SET`: apply the physio-only rules.
4. `spec_cache.py SET cache.pkl`, then `breath_eval.py cache.pkl results/SET`: the frozen test.
5. After the review:
   - `baselines.py`, `robustness.py`, `sensitivity.py`;
   - `tissue_peak.py RUNS OUT [shuffled]`;
   - `explore_reference.py`, `coverage.py`, `secondary.py`.
6. Figures: `make_figures.py` (Figures 1 and 4), `make_fig_artifact.py` (Figure 2),
   `make_fig_tissue.py` (Figure 3).

Code notes:
- The packaged scripts differ from the frozen files only in the line that sets the ReadyNeuro folder.
- `extract_bold.py` computes the PCA Gram matrix on every third voxel. PCA outputs are not used in any
  result here.

## Files

- `files/code/`: all code.
- `files/prereg/`: the frozen pre-registration, the split, and the hash check.
- `files/results/`: per-run tables (`disc_v2_runs.tsv`, `conf_runs.tsv`), summaries, baselines,
  robustness and sensitivity results, tissue tables (observed and shuffled), the exploratory
  correction, and the noise-only default peaks; `exploration/` holds the early discovery records.
- `files/meta/`: the OpenNeuro physio scan, study names, acquisition metadata, physio QC logs and
  pipeline logs.
- `files/figures/`: Figures 1 to 4.

## Acknowledgement

An independent reviewer subagent found the default-peak artifact. It also found the missing baselines,
the overlap with the earlier study, and the tissue-null issue. `baselines.py` adapts its check.

## References

- Liu et al. 2026, ReadyNeuro: https://openreview.net/forum?id=rLMKAJCLc1 ; https://github.com/SophontAI/ReadyNeuro
- sci:sha256:a3bd63ea7f351cefe96307cdde3bb6ed452b17f2864f121ff368d3cde92477ac
- sci:sha256:4839c7e00845a91c0e162d6347fc94c14bfe72653974f79218ebebaf3e1de7d5
- Biswal, DeYoe, Hyde 1996, Magn Reson Med 35:107, doi:10.1002/mrm.1910350114
- Lowe, Mock, Sorenson 1998, NeuroImage 7:119, doi:10.1006/nimg.1997.0315
- Frank, Buxton, Wong 2001, Magn Reson Med 45:635, doi:10.1002/mrm.1086
- Agrawal, Brown, Lewis 2020 (HRAN), NeuroImage 205:116231, doi:10.1016/j.neuroimage.2019.116231
- Cordes et al. 2014, NeuroImage, doi:10.1016/j.neuroimage.2013.12.013
- Huotari et al. 2019, Front Neurosci, doi:10.3389/fnins.2019.00279
- Power et al. 2017, NeuroImage, doi:10.1016/j.neuroimage.2016.09.038
- Power et al. 2019, NeuroImage, doi:10.1016/j.neuroimage.2019.116041
- Fair et al. 2020, NeuroImage, doi:10.1016/j.neuroimage.2019.116400
- Gratton et al. 2020, NeuroImage, doi:10.1016/j.neuroimage.2020.116866
- Chang et al. 2009, NeuroImage, doi:10.1016/j.neuroimage.2008.09.029
- Salas et al. 2021, NeuroImage, doi:10.1016/j.neuroimage.2020.117459
- Bayrak et al. 2025, Imaging Neuroscience, doi:10.1162/IMAG.a.163
- Addeh et al. 2025, Magn Reson Med, doi:10.1002/mrm.30330
- Snoek et al. 2021 (AOMIC), Sci Data, doi:10.1038/s41597-021-00870-6
- OpenNeuro studies: listed in `sci.json`.
