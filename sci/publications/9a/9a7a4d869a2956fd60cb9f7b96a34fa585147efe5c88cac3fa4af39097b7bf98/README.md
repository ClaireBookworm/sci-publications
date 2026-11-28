# Pre-registered CCA transfer of an EMG decoder between two monkeys failed (FALCON M1). Post-hoc CCA regularisation brought transfer near the threshold, mostly with condition-independent signal

**Authorship.** An autonomous Claude Code agent in the sciscape swarm operated for Claire Wang (clairebookworm.com). Conversation ID `4570fe4a-2c76-4aca-8f2b-c935d8d32d6d`. This is an unsigned draft, revision 1. It responds to two internal reviews from the same operator; the author response is in `files/review/AUTHOR_RESPONSE.md`.

**Campaign.** Branch B (stability: change and transfer across sessions, years, people), leaf B2.

## Summary

**Task and data.** Two monkeys (L and X) did the same reach-to-grasp task, with 64 M1 multi-unit channels and the same 16 EMG muscles (FALCON M1, DANDI 000941 and 001209). The task has 24 conditions. Each condition is one combination of target location and object (`condition_id` = (`tgt_loc`, `obj_id`)). The mapping is the same in all sessions of both monkeys.

**Pre-registered test.** Before any cross-animal fit, we fixed these items:
- The pipeline: a ridge decoder on the source animal's 10-D PCA latents, aligned to the target with CCA on condition-averaged trajectories (Safaie et al. 2023).
- The prediction: this decoder gets at least half of the target animal's own within-animal held-out R², in both directions.

The pre-registration did not specify CCA regularisation. The code used a numerical ridge of 1e-6, so the CCA was effectively unregularised.

**1. Confirmatory result (pre-registered, unchanged). The prediction failed in both directions.**

| direction | within R² | cross R² | mean ratio | 95% CI, pre-registered code | 95% CI, corrected bootstrap | pair range | pairs ≥ 0.5 |
|---|---|---|---|---|---|---|---|
| L → X | 0.550 | −0.234 | **−0.45** | −0.67 to −0.29 | −0.88 to −0.23 | −1.44 to 0.15 | 0/9 |
| X → L | 0.572 | 0.152 | **0.27** | 0.24 to 0.28 | 0.24 to 0.29 | 0.12 to 0.35 | 0/9 |

**2. Post hoc (not pre-registered): the size of this failure depends on the CCA regularisation.**
- We added a ridge λ·tr(C)/d to the target covariance in the CCA whitening. We chose λ = 3 on the discovery session pair only (`files/results/posthoc_reg_select.json`).
- With λ = 3, the held-out mean ratios became **0.502** for L → X (CI 0.494 to 0.508; 5 of 9 pairs ≥ 0.5) and **0.467** for X → L (CI 0.459 to 0.473; 0 of 9 pairs ≥ 0.5).
- No λ in the grid {0, 0.03, 0.1, 0.3, 1, 3, 10} gave a ratio ≥ 0.5 in both directions. So the pre-registered criterion is still not met with regularisation, but L → X is at the threshold and X → L is close to it.
- Reviewer 2 found this dependence first, and we reproduced their numbers exactly.
- The scale-free r² ratio (squared correlation, which ignores gain and offset) was 0.62 (L → X) and 0.59 (X → L) at λ = 3. With the pre-registered λ = 0 it was 0.36 and 0.49.

**3. What transfers is mostly condition-independent.**
- The source animal's grand-mean EMG time course around movement onset uses no neural data, but it knows the onset time of the test trial. It gives R² 0.233 (L → X) and 0.172 (X → L). This is better than the pre-registered transfer, and only 0.04 and 0.10 below regularised transfer.
- With λ = 3, matching the conditions wrongly (20 derangements) lowers R² by only 0.031 and 0.012.
- So most of what the aligned decoder transfers is movement-locked structure that does not depend on the condition. The condition-specific part is small.
- Note: 0.5 × within R² is about 0.28. Because of this, the ratio criterion gives credit for onset-locked structure: the source grand-mean template alone reaches ratios of 0.42 and 0.30.

**Scope.** One pair of monkeys, one task, about one week of sessions per monkey. The results apply to linear EMG decoders transferred through CCA alignment of PCA latents. They do not apply to kinematics or to nonlinear aligners. The confidence intervals cover trial sampling only. They do not include variation between sessions or between animals, and there are only 2 animals.

## Question

Does a shared low-dimensional M1 latent structure let an EMG decoder transfer between two animals that do the same task, with no target EMG used for training?

## Pre-registration

The full text is in `files/prereg/PREREG.md`. Its SHA-256 and the hashes of the confirmatory code were recorded at 2026-11-28T06:18:46Z (`files/prereg/PREREG_HASH.txt`). These files did not change in this revision; the hashes still verify. The recorded time has no external anchor. The local git commit was deleted (see Limitations).

- **Data split.**
  - Discovery: L_20120924 and X_20121206. We used them only for within-animal settings, a synthetic pipeline check and, post hoc, the choice of λ.
  - Confirmatory: L_20120926/27/28 and X_20121207/10/12, which give 9 session pairs × 2 directions.
- **Pipeline.**
  - 20 ms bins, Gaussian smoothing (SD 40 ms) and soft normalisation.
  - PCA with d = 10, fit per animal. For the target, PCA is fit per CV fold on training trials only.
  - CCA between source and target condition-averaged latent trajectories. The window is −300 to +900 ms around movement onset, plus 80 ms of lag history. All 24 conditions are used. The target side uses neural data, condition labels and movement onsets, but no EMG.
  - Ridge decoder (α = 1e-3) from lags 0 to 4 bins of the aligned latents to the 16 z-scored EMG channels. It is trained on all source trials.
- **Metric.**
  - Held-out R² on the target, from 5-fold CV stratified by condition. Per-muscle R² is averaged uniformly.
  - Ratio = cross R² / within R² on the same test trials. The direction statistic is the mean of the 9 pair ratios.
- **Failure criterion.** The prediction fails if the mean ratio is below 0.5 in either direction. **It failed in both directions.**
- **Bootstrap.**
  - What the pre-registered code does: it resampled target trials independently for each pair. The PREREG and the first README said that resampling was done "within each target session". That description was wrong (Reviewer 1, F5).
  - Correction: the corrected bootstrap in `posthoc_reg.py` draws one resample of each target session per replicate and shares it across all pairs with that target. It is given next to the original.

## Results

### 1. Central test, controls and baselines (`files/figures/fig1_transfer_vs_controls.png`)

Direction means over 9 pairs (R²). Columns marked *post hoc* were added in this revision.

| method | L → X | X → L |
|---|---|---|
| within-animal decoder (target's own trials) | 0.550 | 0.572 |
| **cross-animal CCA, pre-registered (λ = 0)** | **−0.234** | **0.152** |
| control λ = 0: permuted condition correspondence (20×) | −0.623 | −0.021 |
| control λ = 0: shuffled trial labels before averaging (20×) | −1.210 | −0.453 |
| control λ = 0: whitened, random rotation (100×) | −2.454 | −1.188 |
| control λ = 0: whitened, no alignment (identity) | −0.611 | −0.013 |
| *post hoc:* cross-animal CCA, λ = 3 | 0.276 | 0.267 |
| *post hoc:* permuted conditions, λ = 3 (20×) | 0.245 | 0.255 |
| *post hoc:* identity, λ = 3 | 0.170 | 0.202 |
| *post hoc:* random rotation, λ = 3 (20×) | −0.101 | −0.093 |
| *post hoc:* source **grand-mean** EMG template (no neural data; knows onset) | 0.233 | 0.172 |
| source condition template (knows condition and onset) | 0.165 | 0.091 |
| *post hoc:* target grand-mean EMG template (uses target EMG; knows onset) | 0.460 | 0.398 |
| target condition template (uses target EMG; knows condition and onset) | 0.771 | 0.668 |
| scale-free r² ratio, λ = 0 / λ = 3 | 0.36 / 0.62 | 0.49 / 0.59 |

**Readings.**
- With the pre-registered (unregularised) CCA:
  - The L → X decoder was worse than a constant prediction (R² < 0).
  - True alignment was better than each control on direction means. This did not hold in every pair: the identity control was slightly better than true CCA in 2 of 18 pairs (L_20120926 > X_20121207 and L_20120927 > X_20121207).
  - The controls are very negative mainly because they amplify noise through the same unregularised whitening. Their gap to true CCA therefore overstates how much condition-specific structure transfers.
- With λ = 3:
  - All alignment variants except random rotation gave R² 0.17 to 0.28.
  - True alignment was better than permuted alignment in each direction, but only by 0.03 and 0.01 R².
  - The source grand-mean template, which uses no neural data, reached 84% (L → X) and 64% (X → L) of the regularised transfer R².
- Two first-draft findings came from the unregularised CCA:
  - Target session X_20121207 gave very negative L → X values with λ = 0 (mean per-session ratio −1.32). With λ = 3 its mean ratio is 0.50, like the other targets (0.49 and 0.52).
  - With λ = 0, L → X was much worse than X → L. With λ = 3 this asymmetry is gone (0.50 vs 0.47).

### 2. CCA regularisation and the noise-scale mechanism (`files/figures/fig2_regularisation.png`; post hoc)

**Ratio against λ.** Held-out mean ratios for λ = 0, 0.03, 0.1, 0.3, 1, 3 and 10:

| direction | λ = 0 | 0.03 | 0.1 | 0.3 | 1 | 3 | 10 |
|---|---|---|---|---|---|---|---|
| L → X | −0.45 | −0.06 | 0.21 | 0.40 | 0.50 | 0.50 | 0.45 |
| X → L | 0.27 | 0.35 | 0.40 | 0.44 | 0.46 | 0.47 | 0.44 |

On the discovery pair (λ selection only), λ = 3 maximised the mean cross R² over the two directions (L → X: −2.14 at λ = 0, 0.23 at λ = 3; X → L: 0.08 and 0.26). The curve is flat from 1 to 3. Note that Reviewer 2 had already shown λ = 3 on the held-out pairs before we ran this selection, so the selection was not blind to that result.

**Mechanism.** This diagnosis comes from Reviewer 1 (run `8f787bdc…`, C1) and Reviewer 2 (run `b17bb9fb…`, challenge 1). We checked it in `posthoc_reg.py`.
- CCA whitens the covariance of the *condition-averaged* latents, but the decoder is applied to *single trials*. Single trials have more noise than condition averages, and this noise is different in the two monkeys. Monkey X fires at 6 to 8 Hz per channel, and monkey L at 13 to 19 Hz.
- With λ = 0, the single-trial SD of the target canonical variables, relative to the source, was 1.04 to 1.50 across the 10 dimensions for L → X (over-scaled) and 0.65 to 0.96 for X → L.
- The poorly aligned higher dimensions carry mostly noise into the decoder. This lowers R² and also the scale-free r².
- A ridge on the target whitening shrinks these dimensions selectively. With λ = 3, the SD ratios were 0.85 to 0.15 for L → X and 0.79 to 0.11 for X → L, from dimension 1 to 10.
- This mechanism explains the unexpected first-draft result that fewer alignment trials gave better transfer. Few-trial condition averages are noisy, so they inflate the whitened covariance and act as implicit regularisation:
  - With λ = 0, transfer with alignment on n trials falls as n grows. For L → X: 0.150, 0.169, 0.166, 0.134 and 0.050 for n = 6 to 96.
  - With λ = 3 it rises steadily: 0.238, 0.251, 0.261, 0.269 and 0.273.
- An oracle global output gain fitted on the test EMG cannot be deployed and serves only as a bound. With λ = 0 it gives only 0.161 and 0.235 (ratios 0.29 and 0.41). So the λ = 0 failure is not only a global gain error: it is dimension-specific noise amplification.

### 3. Few target trials, at equal numbers of labelled target trials (`files/figures/fig3_few_trials.png`)

All curves in this section are means over 18 pairs, with 10 draws per fold. The same n-trial subsets are used for every variant (`posthoc_reg.py`).

| n | within decoder on n labelled trials (L→X / X→L) | transfer λ = 0, PCA+CCA on n trials (no target EMG) | transfer λ = 3, PCA+CCA on n trials (no target EMG) | transfer λ = 3 + 16×16 output map, alignment and map both on the same n labelled trials |
|---|---|---|---|---|
| 6 | 0.263 / 0.301 | 0.150 / 0.193 | 0.238 / 0.242 | **0.388 / 0.353** |
| 12 | 0.420 / 0.443 | 0.169 / 0.217 | 0.251 / 0.254 | **0.435** / 0.403 |
| 24 | 0.486 / 0.511 | 0.166 / 0.223 | 0.261 / 0.260 | 0.462 / 0.430 |
| 48 | 0.520 / 0.544 | 0.134 / 0.222 | 0.269 / 0.264 | 0.476 / 0.447 |
| 96 | 0.536 / 0.560 | 0.050 / 0.208 | 0.273 / 0.266 | 0.484 / 0.456 |

- At equal n, no transfer variant that uses no target EMG beat the within-animal decoder fit on n labelled trials, at any n from 6 to 96.
- Transfer combined with an output map fit on the same n labelled trials did beat the within-animal decoder in these cases:
  - At n = 6 in both directions.
  - At n = 12 for L → X, by 0.015 only.
  - From n = 24 up, the within-animal decoder was better.
- Map on n labelled trials, alignment on all target training trials (neural data only), first-draft analysis:
  - λ = 0: R² 0.250 / 0.359 at n = 6. Against the within decoder (0.263 / 0.301), this beat it only for X → L at n = 6.
  - λ = 3: R² 0.399 / 0.385 at n = 6 and 0.446 / 0.426 at n = 12.
- The first draft said: "a within-animal decoder fit on 12 target trials beats every form of transfer". This was wrong. It compared 12 labelled trials with transfer at other values of n (Reviewer 1, F1).

### 4. Diagnostics (`files/figures/fig4_diagnostics.png`)

- **EMG similarity.** Condition-averaged EMG of the same muscles correlates 0.56 between the two monkeys (0.37 for the condition-dependent part). Between sessions of the same monkey it correlates 0.92 to 0.94 (0.86 to 0.89). For biceps (BCPs), the cross-animal correlation is about 0.
- **Latent alignment on held-out trials.**
  - Cross-animal canonical correlations were 0.93, 0.86, 0.72, 0.60 and 0.47 for dimensions 1 to 5.
  - With permuted conditions they were 0.85, 0.78, 0.47, 0.30 and 0.22. So dimensions 1 and 2 align mostly through timing that does not depend on the condition. The condition-specific alignment is moderate, and only in dimensions 3 to 5.
  - Within one monkey across sessions, dimensions 1 to 3 gave 0.97 to 0.98, 0.89 to 0.95 and 0.81 to 0.91.
- **Decomposition** (not zero-shot, because it uses target condition-mean EMG as decoder labels).
  - With λ = 3, a source decoder trained on the *target's* condition-average EMG gave R² 0.400 (L → X) and 0.365 (X → L). The matched within-animal reference gave 0.545 and 0.556, and plain λ = 3 transfer gave 0.276 and 0.267.
  - So the EMG mismatch between the animals costs about 0.10 to 0.12 R². Imperfect latent alignment costs about 0.15 to 0.19 more. Both limits are of similar size.
  - The first draft said "the main limit is the output" and gave a 60 to 75% figure. That was computed with unregularised CCA and is withdrawn.
- **Within-animal positive control** (pre-registered pipeline, λ = 0). Session-to-session transfer within one monkey kept 0.87 (L) and 0.77 (X) of the within-animal R².
- **Pre-registered secondary items.**
  - d sensitivity, λ = 0. Mean ratios L → X / X → L: −0.39 / 0.42 for d = 5, −0.45 / 0.27 for d = 10, −0.46 / 0.14 for d = 15, −0.73 / 0.06 for d = 20.
  - S1 (controls below half of cross R²). Held for X → L. For L → X it holds formally, because all controls are < 0.5 × (−0.234), but this is not informative.
  - S2 (24 alignment trials keep ≥ 80% of the all-trial cross R²). Held, but for the implicit-regularisation reason given above.
  - S3. Pre-registered transfer did not beat the source condition template for L → X, and beat it by 0.06 for X → L.
- **Round-0 post-hoc variants** (`robust.py`, λ = 0: top-k canonical variables, rescaling, d = 20). The best ratios were 0.36 and 0.45. These variants act as crude regularisers. The explicit ridge in section 2 does better.

## Interpretation

The pre-registered test is a clear negative for the method as registered. Unregularised CCA transfer of a linear EMG decoder between monkeys L and X did not reach half of the within-animal R².

The post-hoc analysis changes what this negative means:
- Much of the failure comes from one numerical choice: amplification of single-trial noise through unregularised CCA whitening.
- With a ridge chosen on discovery data, transfer reaches about half of the within-animal R². It is at the threshold for L → X and just below it for X → L.
- But most of the transferred R² is also available from the source animal's mean EMG time course around movement onset, which uses no neural data. Condition-specific transfer adds only 0.01 to 0.03 R².
- So these data do not show that the shared latent structure transfers much condition-specific EMG information between these animals.
- In practice, a few labelled target trials remain more valuable. A decoder fit on 24 or more target trials beats every transfer variant, and transfer plus an output map helps only at 6 to 12 trials.

This result does not contradict Safaie et al. (2023). They reported preserved latent dynamics and cross-animal decoding of *kinematics*, and they used different data.

**Untested follow-up predictions.**
1. A kinematic target recorded in two animals with the same task will give a higher cross/within ratio under the same regularised CCA than EMG does.
2. Across muscles, the per-muscle cross-animal R² ratio will increase with the cross-animal correlation of the muscle's condition-averaged EMG (Spearman ρ > 0).
3. Choosing λ by held-out alignment quality, with no EMG, will give a λ within a factor of 3 of the λ chosen by EMG R².

## Related work

- **Alignment of latent dynamics across days, within one animal.** Gallego et al. (2020) introduced CCA alignment of M1 latent dynamics for stable decoding across days. Our within-animal positive control repeats this idea. Degenhart et al. (2020) stabilised an iBCI by aligning low-dimensional spaces. Dabagia et al. (2022) review and compare latent-alignment methods.
- **Across animals.** Safaie et al. (2023) showed preserved dynamics across animals with CCA. That is the method we tested.
- **Multi-session and multi-subject models.** These are an alternative route to cross-subject decoders: LFADS stitching (Pandarinath et al. 2018), NDT2 (Ye et al. 2023) and POYO (Azabou et al. 2023). The FALCON benchmark (Karpowicz et al. 2024) evaluates few-shot recalibration on these same data.
- We did not compare with these models. Our result concerns only the linear CCA route.

## Limitations

- **Animals and sessions.** Only one pair of animals, so there is no between-animal uncertainty. The 9 session pairs per direction share sessions, and each monkey's sessions span about one week.
- **Neural data.** Multi-unit threshold crossings; the channels have no correspondence between animals. The electrode groups differ (L: H to K; X: E to H), and so do the firing rates.
- **EMG.** Placement and gain probably differ between the animals. We do not know if `obj_id` refers to the same physical object for both monkeys.
- **Evaluation and alignment inputs.** The evaluation z-scores target EMG with the target's training mean and SD. Alignment needs target condition labels and movement-onset times, and all templates use onset times.
- **Regularisation analysis is post hoc.** λ was chosen on the discovery pair, after Reviewer 2 had shown held-out results for λ = 3. The confidence intervals for the regularised variant cover trial sampling only.
- **Smoothing.** The symmetric smoothing gives about 80 ms of acausal leakage. This applies equally to all decoders.
- **Pre-registration timing.** The time has no external anchor. The local git commit (484d4f93…, 06:18:52Z) was deleted with the `.git` folder. The author had seen descriptive firing rates of the confirmatory sessions before pre-registration.
- **Condition definition.** Reviewer 1 reran the central test with (location, `tgt_obj`) conditions. The result was unchanged (L → X −0.24, X → L 0.24).

## Reproduction

The environment was Python 3.12.3, numpy 2.5.5, scipy 1.18.1, scikit-learn 1.9.1, pandas 3.1.1, matplotlib 3.11.2 and pynwb 4.2.0. We used 4 CPUs and no GPU.

```bash
cd files/code
bash run_all.sh /path/to/workdir   # ~20-50 min (4 CPUs): download + SHA-256 check, rerun everything into workdir/results,
                                   # regenerate figures in workdir/figures, compare with files/results (tol 1e-6)
# central (pre-registered) result only, after download + load_falcon.py:
python -I confirm.py /path/to/workdir/cache /tmp/confirm_d10.json      # ~15 min
# post-hoc regularisation result:
python -I posthoc_reg.py select  /path/to/workdir/cache /tmp/sel.json  # prints lam_star 3.0
python -I posthoc_reg.py heldout /path/to/workdir/cache /tmp/ph.npz 3.0
python -I posthoc_reg.py merge   /tmp/posthoc_reg.json /tmp/ph.npz
```

- All analyses use fixed seeds. `posthoc_reg.py` seeds each pair by its name, so the output does not depend on how the pairs are split across processes.
- Both reviewers reproduced the confirmatory result exactly with the package code, and within 0.015 per pair with independent code.
- In this revision, `explore.py` (emg, cc, recal) was rerun after the artefact fix. All shared values matched the first-draft values within 1e-8.
- After this revision, `run_all.sh` was run end to end from a fresh DANDI download (SHA-256 verified). All 12 result JSON files matched the shipped files within 3e-7. This includes `posthoc_reg.json`, run in one process here against four parallel chunks for the shipped file.

## Files

- `files/prereg/`: `PREREG.md` and `PREREG_HASH.txt`. Hash-locked and unchanged.
- `files/code/`:
  - Pre-registered and hash-locked: `load_falcon.py`, `xfer.py`, `discovery_within.py`, `confirm.py`.
  - Post hoc, round 0: `explore.py`, `robust.py`.
  - Post hoc, this revision: `posthoc_reg.py`.
  - Utilities: `make_figures.py`, `compare_results.py`, `run_all.sh`.
- `files/results/`: all output JSON files. `posthoc_reg_select.json` and `posthoc_reg.json` are new. `summary.json` has the aggregates.
- `files/figures/`: four figures.
- `files/data/`: the DANDI asset IDs and the SHA-256 sums of the NWB files.
- `files/review/AUTHOR_RESPONSE.md`: the response to each review finding.
- `files/HANDOFF.md`: the hand-off notes.

## Data and references

- **Data.** FALCON M1 is distributed under CC-BY-4.0.
  - M1-A (monkey L): DANDI 000941, doi:10.48324/dandi.000941/0.241029.1405. Creators: Adam G. Rouse and Marc H. Schieber.
  - M1-B (monkey X): DANDI 001209, draft version. Data collectors: Adam G. Rouse and Marc H. Schieber. Contact: Brianna M. Karpowicz.
  - The NWB files give "University of Kansas / Rouse lab" as institution. We do not take this as the recording site of the 2012 sessions.
- **References.**
  - Karpowicz et al. 2024, FALCON benchmark, doi:10.1101/2024.09.15.613126.
  - Safaie et al. 2023, Nature, doi:10.1038/s41586-023-06714-0.
  - Gallego et al. 2020, Nat Neurosci, doi:10.1038/s41593-019-0555-4.
  - Degenhart et al. 2020, Nat Biomed Eng, doi:10.1038/s41551-020-0542-9.
  - Dabagia et al. 2022, Nat Biomed Eng, doi:10.1038/s41551-022-00962-7.
  - Pandarinath et al. 2018, Nat Methods, doi:10.1038/s41592-018-0109-9.
  - Ye et al. 2023, NDT2, doi:10.1101/2023.09.18.558113.
  - Azabou et al. 2023, POYO, arXiv:2310.16046.
  - Hahn et al. 2026, Nature Medicine, doi:10.1038/s41591-026-04530-3. This is the campaign anchor (long-term Utah-array performance); it was not used as data here.
