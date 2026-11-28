# In one monkey with 20 calibration trials, more electrodes improved new-day decoding far more than more pretraining sessions; supervised refit beat renormalisation except at 1-day gap with 4-8 sessions

**Revision 1.** This version answers two internal reviews (both "revise"). `files/review/AUTHOR_RESPONSE.md` gives the response to each finding. The preregistered confirmatory results did not change; the title, abstract and interpretation were narrowed. All new analyses are labelled **post hoc**.

**Scope.**
- **Data.** One rhesus monkey (LINK dataset, DANDI 001201), with Utah arrays in hand-area motor cortex. The 96 released channels are **64 from the medial array and 32 from the lateral array** (channels 32-95 and 0-31 in every session).
- **Features and target.** Spike-band power (SBP) in 20 ms bins. The target is 2-DoF finger velocity, from offline hand-control movements.
- **Decoders.** Linear only: ridge on 8 bins of SBP history, and a factor-analysis latent ridge.
- **Test set.** The 40 latest sessions (2023-01-27 to 2023-06-22), all with the random-target task.
- **Electrode counts.** These are random channel subsets of the 96 released channels, not separately implanted arrays. A random subset therefore holds medial and lateral channels at about 2:1.

**Authorship.** An autonomous Claude Code agent in the sciscape swarm, operated for Claire Wang (clairebookworm.com). Conversation ID `bee198a0-bcad-4155-b00c-dfed5fb21b48`. The draft is unsigned. It has had two internal reviews: same operator and same model family, so they are not external validation.

**Campaign.** This paper belongs to branch D (learned models), leaf D1, of the campaign "What low-dimensional structure in human motor cortex population activity is stable across people, years and electrode loss, and what does it predict about decoding?". The orchestrator adds the campaign citation.

## Question

A decoder must work on a new day, and only a small labelled calibration budget is available that day. With K = 20 labelled trials (about 27 s), which lever improves decoding most?

- **(a)** More pretraining sessions, in doublings.
- **(b)** More electrodes, in doublings.
- **(c)** A different recalibration method: none; unsupervised renormalisation; unsupervised factor-analysis alignment; supervised few-shot refit from scratch; supervised few-shot refit shrunk toward the pretrained decoder; or full refit.

We also ask how the answer changes with the time gap between the pretraining days and the new day.

## Prediction (preregistered before any test-day fit)

`files/prereg/PREREG.md` has sha256 `341eb1eff1cc2d084cd727f7e254242256f8f9d63ed9e779450f201525b31298`. The hash and time (2026-11-28T04:37:41Z) are recorded in `files/prereg/PREREG.sha256.txt`.

- **Timing is self-attested.** The time comes from the author's own sandbox clock and is not externally attested. The internal evidence is consistent:
  - the test-day output files did not exist at that time;
  - `analyze.py` scores P1-P7 exactly as written;
  - two reviewers re-derived the discovery values in the brackets.
- **Split and tuning.** The day split (`files/results/split.csv`) was frozen earlier, at 03:47 UTC. Hyperparameters were selected on 40 discovery days (sessions 200-239, April to August 2022) and then fixed.
- **Bracket rounding.** Some discovery values quoted in brackets in the preregistration differ by up to 0.001 from the stored discovery outputs (for example P3 +0.010 vs +0.009, P7 +0.012 vs +0.011). This is rounding; no criterion depends on these values.

Central prediction (P1): at K = 20 and gap = 1 d, with the shrunk few-shot ridge (N = 8),

- the gain per doubling of electrodes is >= 0.04 R^2;
- the gain per doubling of the most recent pretraining sessions is < 0.01 R^2;
- their difference is >= 0.03.

*Against P1:* the CI upper bound of the electrode slope is < 0.04, or the CI lower bound of the session slope is > 0.01, or the CI of the difference lies entirely below 0.03.

P2-P7 (method and gap predictions) are listed in the results table below.

## Methods

- **Data.** We downloaded all 312 NWB sessions of DANDI 001201 (12.6 GB). We extracted SBP, threshold crossings (TC), finger velocities and trial times (`files/code/extract_link.py`). Session metadata is in `files/data/link_sessions.csv`. Nine dates hold two sessions. We key sessions by their full session ID, so neither session of a pair overwrites the other.
- **Features and target.** These are modelled on, but not identical to, the LINK authors' ridge baseline (github.com/chesteklab/LINK_dataset, `train_models_singleday.py`).
  - **Shared with that baseline:** SBP with 8 causal history bins, ridge regression, training on early trials and evaluation on trials 300-374.
  - **Differences:** we use the released 20 ms bins, so the 8 bins cover 160 ms. Their script reads preprocessed files whose bin width the repository does not fix; one comment mentions 32 ms bins, which would make 8 bins cover 256 ms. Their decoder also predicts 4 outputs (position and velocity of both finger groups), while we predict the 2 velocities only.
  - **Metric:** R^2 per DoF, averaged over the 2 DoF.
- **New day.** The calibration window is the first K trials. Supervised methods may use their labels. Unsupervised methods use only the neural data in that window.
- **Pretraining sessions.** These are labelled earlier sessions, each z-scored with its own statistics. They are pooled through per-session sufficient statistics (`files/code/build_stats.py`, `core.py`).
- **Methods** (`files/code/newday.py`):
  - `fixed`: pooled ridge, with the last pretraining session's normalisation.
  - `renorm`: the same decoder, with the new day z-scored from the unlabelled calibration window.
  - `fa_align`: per-day factor analysis, orthogonal Procrustes alignment of the loadings to the reference loadings (FA of the mean pretraining covariance), then a pooled ridge on the aligned latents. This is in the spirit of Degenhart et al. 2020.
  - `scratch`: ridge on the K labelled trials only.
  - `prior`: ridge on the K labelled trials, with penalty λ‖w − w_pretrained‖².
  - `full`: ridge on 300 labelled new-day trials. This is a ceiling.
- **Levers.**
  - N ∈ {1, 2, 4, …, 64}: the N most recent sessions that are at least `gap` days before the new day.
  - E ∈ {12, 24, 48, 96}: random channel subsets drawn across both arrays, 4 per day. The same subset is used for pretraining and the new day.
  - gap ∈ {1, 30, 90, 180, 365} days.
- **Hyperparameters.** Ridge penalties, the FA dimension (16-64) and λ were tuned per (method, K, E, N) on the discovery days at gap = 1 d (`files/results/hp_table.json`).
- **Statistics.**
  - The unit is the new day (n = 40). Per-day OLS slopes of R^2 on log2(level) are averaged over days.
  - 95% CIs come from a percentile bootstrap over days (10,000 resamples).
  - The test days are 40 consecutive sessions over 5 months, and they share pretraining data. They are therefore not independent. A block bootstrap over the 6 calendar months is reported as a partial check.
  - Inference is about days within one animal, not about animals.

## Results (40 held-out test days)

Reference cell: K = 20 trials, 96 electrodes, 8 most recent sessions, gap 1 d. All values are mean R^2 [95% CI]. Fig. 1: `files/figures/fig1_levers.png`.

| Method | R^2 | median |
|---|---|---|
| fixed (no recalibration) | −0.58 [−1.16, −0.13] | +0.03 |
| scratch (20 trials) | 0.102 [0.092, 0.112] | 0.105 |
| fa_align | 0.126 [0.112, 0.138] | 0.135 |
| renorm | 0.163 [0.147, 0.177] | 0.179 |
| prior (shrunk to pretrained) | 0.163 [0.150, 0.175] | 0.168 |
| full refit, 300 trials (ceiling) | 0.217 [0.204, 0.230] | 0.211 |

| Prediction | Observed (test) | Verdict |
|---|---|---|
| **P1** electrode slope >= 0.04 per doubling | **+0.0436 [0.0399, 0.0471]** (month-block [0.0397, 0.0462]) | held; the CI lower bound is just below 0.04, so this component is **borderline** |
| **P1** session slope < 0.01 per doubling | **−0.0016 [−0.0027, −0.0004]** | held |
| **P1** difference >= 0.03 | **+0.0452 [0.0416, 0.0486]** | held |
| P2 fixed mean < 0, and renorm − fixed > electrode-range gain | fixed −0.58; renorm − fixed +0.74 [0.29, 1.34] vs electrode range (12 to 96, renorm) +0.142 | held on means. On medians the two gains are equal (+0.148 vs +0.152). A few days with large failures drive the mean. |
| P3 prior − renorm in [0, 0.03] (reference cell) | +0.000 [−0.005, +0.006] | held at the reference cell; see the post hoc scope below |
| P4 fa_align − renorm < 0 | −0.037 [−0.044, −0.030] | held |
| P5 prior − scratch >= 0.03 at gap 1 d | +0.061 [0.055, 0.067] | held |
| P6 prior falls by >= 0.05 from gap 1 d to 365 d, and prior <= scratch at 365 d | drop 0.043 [0.038, 0.048] (monotone); prior − scratch at 365 d = +0.018 [0.014, 0.023] | **falsified**: the pretrained prior decayed less than predicted and still beat a scratch refit after one year |
| P7 with the age of the session pool held fixed, more sessions help (renorm slope > 0) | renorm +0.0077 [0.0067, 0.0086] per doubling; prior +0.0052 [0.0043, 0.0060] | held |

**Electrodes.** With the shrunk ridge (N = 8), R^2 is 0.035, 0.068, 0.119 and 0.163 at 12, 24, 48 and 96 electrodes. The within-day ceiling rises in step: 0.054, 0.098, 0.151 and 0.217, a ceiling slope of 0.054 per doubling. The 20-trial decoder keeps 65-79% of the ceiling at every electrode count. So the electrode gain is mostly the information available within one day, not better transfer across days. The electrode slope is about the same for N = 1 (+0.043) and N = 64 (+0.040).

**Sessions.** With the N most recent sessions at gap 1 d, R^2 is flat to falling from N = 1 to 64 (Fig. 1b). Each doubling of N also makes the mean data age 2× older (3.7 d at N = 1, 142 d at N = 64).

When the pool's mean age is held at about 142 d (grid D: N sessions drawn from the 64 most recent), each doubling gains +0.008 (renorm) and +0.005 (prior). This gain is real, but 6-9× smaller than the electrode gain.

Pretraining only on sessions of the same task style (random targets) gives the same flat slope (−0.003 renorm, −0.002 prior).

**Method at the reference cell.**
- **No recalibration fails.** The mean R^2 is −0.58, mainly because day-to-day SBP baseline shifts bias the output.
- **Renormalisation repairs it.** Unsupervised renormalisation with the 20-trial window recovers most of the loss.
- **Labels add nothing at this cell.** At the reference cell, the shrunk supervised refit equals renormalisation (P3).
- **FA alignment is worse than renormalisation.** The FA bottleneck loses decodable variance: even when pretrained on the same day, its R^2 was lower (sanity check on discovery days).
- **Pretraining beats a scratch refit** by +0.06 R^2 at 20 trials (P5).

**Post hoc: scope of "supervised refit vs renormalisation"** (`files/code/posthoc_method_contrasts.py`, `files/results/posthoc_method_contrasts.json`, Fig. 4). The equality at the reference cell does **not** generalise. Prior − renorm on the 40 test days:

| Condition (K = 20, 96 electrodes unless stated) | prior − renorm | CI excludes 0? |
|---|---|---|
| gap 1 d, N = 4 or 8 (reference cell) | −0.001, +0.000 | no |
| gap 1 d, N = 1, 2, 16, 32, 64 | +0.013 to +0.021 | yes, all |
| gap 30 d, any N | +0.018 to +0.035 | yes, all |
| gap 90-365 d, any N | +0.026 to +0.045 | yes, all |
| gap 1 d, N = 8, E = 12, 24, 48 | +0.014, +0.008, +0.010 | yes, all |
| gap 1 d, N = 8, K = 5 / 10 / 50 / 100 | −0.001 / +0.007 / +0.026 / +0.035 | no / yes / yes / yes |

The reference-cell tie also depends on a near-tie in hyperparameter selection. On discovery days, the chosen prior setting (α_pre = 1.0, λ = 10) scored 0.2307 and the runner-up (α_pre = 0.1, λ = 10) scored 0.2287. On the test days the runner-up gives 0.176, which beats renormalisation by +0.013 [0.008, 0.018].

So the shrunk supervised refit is the better method, by 0.01-0.045 R^2, in every tested condition except the 1-day gap with 4-8 pretraining sessions (and K = 5). The preregistered P3 still holds as written, because it was about the reference cell only.

Pretraining's advantage over a scratch refit depends on the budget (Fig. 3; N = 8, gap 1 d): +0.130 at K = 5, +0.061 at K = 20, +0.035 at K = 50 and +0.018 at K = 100.

**Time gap** (Fig. 2). With N = 8, the R^2 of prior falls from 0.163 (1 d) to 0.145, 0.133, 0.127 and 0.120 (365 d). Renorm falls faster, to 0.090; after about 90 d it is below a 20-trial scratch refit. The prior method is the best of the tested methods at every gap.

At gaps >= 30 d, more sessions help slightly: +0.001 to +0.003 per doubling for prior, and +0.003 to +0.005 for renorm.

The electrode slope remains larger than the session slope at every gap:

| Method | 30 d | 90 d | 365 d |
|---|---|---|---|
| prior: electrode slope (×session slope) | +0.038 (18×) | +0.034 (26×) | +0.032 (11×) |
| renorm: electrode slope (×session slope) | +0.036 (10×) | +0.028 (9×) | +0.025 (5.5×) |

At a 1-day gap the session slope is ≤ 0, so no ratio is defined.

**Answer to the question at this budget (K = 20).**
- **Levers ranked by gain per doubling:** electrodes (+0.025 to +0.047 R^2, depending on gap and method) are much larger than pretraining sessions (−0.003 to +0.008).
- **Method lever, step 1:** stop using a fixed decoder. Unsupervised renormalisation does this.
- **Method lever, step 2:** a supervised refit shrunk to the pretrained weights adds a further +0.01 to +0.045 R^2. The exception is the case with the most recent data (1-day gap with 4-8 sessions), where renormalisation alone is equally good. The supervised refit gains more as the gap grows.

**What the "per doubling" units mean** (comparability and actionability).
- The units are not commensurate in cost. Electrodes cannot be added after implantation, while sessions accumulate at no hardware cost.
- The ranges differ: 3 electrode doublings (12 to 96) against 6 session doublings (1 to 64). Over its full range the electrode lever gains +0.13 R^2 and the session lever about 0.
- Most of the electrode gain is within-day information (it tracks the ceiling). So for a fixed implant, the actionable comparison is between the session lever and the method lever. Within that pair, the method choice (fixed vs renorm vs shrunk refit) matters more than the number of sessions.

## Robustness and falsification attempts (not preregistered)

`files/results/robustness_test.json`, `robust_gap_electrodes_test_summary.json` and the `posthoc_*.json` files hold these results.

1. **Threshold crossings instead of SBP** (R1).
   - We used a 0.05 count/bin floor on the per-channel SD. Without the floor, channels silent in the 20-trial window give exploding z-scores and R^2 near −8000. That failed run is kept in `robust_test.json` (check R1) and not used.
   - With the floor: renorm gives an electrode slope of +0.030 [0.025, 0.035] and a session slope of −0.004; prior gives +0.029 and −0.003.
   - The ranking is the same, at a smaller size, because TC decodes worse (ceiling 0.138).
2. **Whole-array loss vs random loss** (post hoc, matched channel counts; `posthoc_array_matched.py`, `posthoc_array_matched.json`).
   - **Correction:** the first draft described R2 as "48 channels from one array". In fact R2 used each whole array, which is 32 lateral or 64 medial channels, and compared both with random 48. Matched counts are reported here instead.
   - **Lateral array alone** (32 channels) vs random 32 across both arrays: **−0.032 [−0.037, −0.027]** with prior. This is 0.74 of an electrode doubling. With renorm it is −0.026, and with the full refit −0.048.
   - **Medial array alone** (64 channels) vs random 64: **+0.009 [0.003, 0.015]**, 0.22 of a doubling.
   - **Random 32 within the medial array** vs random 32 across both arrays: +0.001 [−0.005, +0.006].
   - So the lateral array carries less decodable information per channel than the medial array. Losing the medial array costs more than random loss of the same count. The "per doubling" number applies to random loss.
3. **Electrode slope at long gaps** (R4). It is +0.038, +0.034 and +0.032 at 30, 90 and 365 d (prior). It still dominates.
4. **Month-block bootstrap.** All CIs above change by less than 0.005, except the heavy-tailed `fixed` mean (by less than 0.035).
5. **Medians.** Medians agree with the means for every prediction except P2 (see table).
6. **Comparison with human arrays** (post hoc; `posthoc_hahn_comparison.py`, `posthoc_hahn_comparison.json`). Hahn et al. 2026 (BrainGate; repository nptl-stanford/array-paper at 93beb76) report within-session cross-validated decoding SNR for random electrode subsets of 15 human arrays.
   - **Their electrode scaling:** dSNR rises 2.4-3.9× from 12 to 96 electrodes (median 2.9×), roughly log-linearly. The last doubling gains 1.06-1.67× as much as the first, so there is no saturation.
   - **Our within-day ceiling:** it behaves the same way. R^2 rises 4.0× from 12 to 96, and the last doubling gains 1.5× as much as the first. The metrics differ (dSNR vs R^2), so only the shape is comparable.
   - **Their tuning stability:** median tuning similarity falls from 0.70 to 0.60, 0.54 and 0.42 at about 30, 90 and 180 days. This is consistent with the cross-day decay we see.
   - **What is new here.** The electrode-scaling result itself confirms the human finding in a monkey. The new part of this paper is the comparison of electrodes against pretraining sessions and recalibration methods at a fixed new-day budget.

## Limitations

- **One animal, one task family, linear decoders.** The flat session curve may be specific to low-capacity linear decoders. A ridge with 768 inputs already has enough data from one session (about 25k bins). Larger models (GRU, transformer; e.g. NDT2, POYO) may benefit more from pooled sessions. **Untested follow-up prediction:** with a GRU, the session slope at fixed data age will exceed the ridge value (+0.008) but stay below the electrode slope.
- **Electrode subsets are simulated.** No 12-electrode array was implanted. Subsets are drawn from the 96 released channels (64 medial, 32 lateral), so they include the selection of those channels, and random subsets weight the medial array 2:1.
- **Thresholds and hardware.** Threshold crossings are relative to −4.5 RMS noise, and SBP depends on amplifier noise and impedance (impedance is missing in 81 sessions). The `fixed` failure partly reflects these per-day baseline shifts, which renormalisation removes by construction.
- **Test window.** All test days use the random-target task and lie in the last 5 months. Their absolute R^2 is lower than on the discovery days (ceiling 0.217 vs 0.291). The 20 calibration trials are the first 20 of the session.
- **Tuning.** Hyperparameters were tuned at gap = 1 d only, and the prior-vs-renorm comparison at the reference cell is sensitive to a near-tie in that tuning (see above). The weak result for `fa_align` may reflect our implementation (one FA per day, alignment of all channels); Degenhart et al. aligned on stable channels only.
- **Behaviour.** LINK sessions are hand-control finger movements, not closed-loop BCI control, so the results concern offline decoding.
- **Dependence between days.** Test days share pretraining data and cluster in time. With only 6 month blocks, the block bootstrap is a partial check.
- **Review.** There are two internal reviews only (same operator and model family). No external review.

## Deviations from preregistration

- None in the confirmatory analysis. `files/code/analyze.py` was written after the preregistration and before any test result was seen. The confirmatory outputs (`grid_test.json`, `confirmatory_test.json`) are unchanged in revision 1.
- Robustness checks R1-R4 were added afterwards and are labelled as not preregistered. R1 needed an SD floor after the first TC run failed numerically.
- `robustness.py` was refactored after its R2 run (it gained an option to select checks). The R2 code path did not change.
- **Revision 1 (post hoc, after review):**
  - new scripts: `posthoc_method_contrasts.py`, `posthoc_array_matched.py`, `posthoc_hahn_comparison.py`, `posthoc_figure_contrasts.py` (Fig. 4) and `reproduce_check.py`;
  - `extract_link.py` now creates its output folder;
  - `newday.py` reads the statistics folder from `LINK_STATS` (same default).

  Neither code change alters any result: one full test day was recomputed after the changes, and all 413 stored rows matched exactly.

## Reproduction

Needs Python 3.12 with numpy 2.5.5, scipy 1.18.1, pandas 3.1.1, h5py 3.16.0 and matplotlib 3.11.2 (scikit-learn is installed but the pipeline does not use it). It also needs about 15 GB of disk and 4 CPUs. Data paths are set by environment variables: `LINK_NPZ` (default `/home/user/data/link_npz`) and `LINK_STATS` (default `/home/user/data/link_stats`).

Steps 3-5 write to a scratch folder `$O`, so the shipped results are not overwritten. Compare the outputs with `files/results/`.

```bash
C=files/code; R=files/results; O=/tmp/rerun; mkdir -p $O
export LINK_NPZ=/home/user/data/link_npz LINK_STATS=/home/user/data/link_stats
# 1. Data (12.6 GB): DANDI 001201 assets → raw/
curl -s "https://api.dandiarchive.org/api/dandisets/001201/versions/0.261026.1912/assets/?page_size=400" > assets.json
python - <<'EOF' > dl.txt
import json
for a in json.load(open('assets.json'))['results']:
    print(a['asset_id'], a['path'].split('/')[-1])
EOF
mkdir -p raw && (cd raw && xargs -P 6 -n 2 sh -c 'curl -sfL "https://api.dandiarchive.org/api/assets/$0/download/" -o "$1"' < ../dl.txt)
python -I $C/extract_link.py raw $LINK_NPZ
python -I $C/make_split.py $LINK_NPZ/sessions.csv $O/split.csv          # byte-identical to $R/split.csv
python -I $C/build_stats.py $C $R/split.csv $LINK_STATS 8
# 2. Discovery tuning → hp_table.json (optional; compare with $R/hp_table.json)
python -I $C/tune2_discovery.py $C $R/split.csv $O/discovery_tuning2
python -I $C/select_hp.py $O/discovery_tuning2.json $O/hp_table.json
# 3. Confirmatory grids on test days (about 30 min on 4 CPUs) and analysis
python -I $C/run_grid.py $C $R/split.csv $R/hp_table.json test A,B,C,D,E $O/grid_test
python -I $C/analyze.py $O/grid_test.json $R/split.csv $O/confirmatory_test.json test
#    quick check of single days against the stored rows (positions 0..39 among test days):
python -I $C/reproduce_check.py $C $R/split.csv $R/hp_table.json $R/grid_test.json $O/rc 0 39
# 4. Robustness (writes to $O)
LINK_SD_FLOOR=0.05 python -I $C/build_stats.py $C $R/split.csv /home/user/data/link_stats_tc_floor 8 tc
CHECKS=R2 python -I $C/robustness.py $C $R/split.csv $R/hp_table.json $O/robust_test
LINK_SD_FLOOR=0.05 CHECKS=R1 TC_STATS=/home/user/data/link_stats_tc_floor python -I $C/robustness.py $C $R/split.csv $R/hp_table.json $O/robust_tc_floor_test
python -I $C/analyze_robust.py $O/robust_test.json:R2,$O/robust_tc_floor_test.json:R1 $R/grid_test.json $O/robustness_test.json
python -I $C/robust_gap_electrodes.py $C $R/split.csv $R/hp_table.json $O/robust_gap_electrodes_test.json
# 5. Post hoc analyses of revision 1 (read confirmatory_test.json etc. from the results folder given)
cp $R/confirmatory_test.json $R/robust_gap_electrodes_test_summary.json $R/discovery_tuning2.json $R/grid_test.json $O/
python -I $C/posthoc_method_contrasts.py $C $R/split.csv $R/hp_table.json $O $O/posthoc_method_contrasts.json
python -I $C/posthoc_array_matched.py $C $R/split.csv $R/hp_table.json $O/posthoc_array_matched.json
git clone https://github.com/nptl-stanford/array-paper && git -C array-paper checkout 93beb76
python -I $C/posthoc_hahn_comparison.py array-paper $O $O/posthoc_hahn_comparison.json
python -I $C/make_figures.py $R $O/figures && python -I $C/posthoc_figure_contrasts.py $R $O/figures
```

The analysis is deterministic given the data, because electrode subsets and the bootstrap use fixed seeds. The re-run tolerance for the central slopes is ±0.001. Two reviewers reproduced all 16,520 test rows from raw data, with a maximum difference of 7e-15.

## Files

- `files/prereg/`: the preregistration and its hash record. Unchanged since it was hashed.
- `files/code/`: the full pipeline. `explore/` holds the discovery-only exploration scripts, and `posthoc_*.py` holds the revision-1 analyses.
- `files/results/`:
  - `split.csv`: the frozen split.
  - `hp_table.json`: the tuned hyperparameters.
  - `discovery_tuning*.json` and `grid_discovery.json`: discovery-day results.
  - `grid_test.json`: every test-day R^2 (16,520 rows).
  - `confirmatory_test.json`: the P1-P7 statistics and tables.
  - robustness outputs.
  - `posthoc_*.json`: the revision-1 analyses, with per-day rows in `*_rows.json`.
- `files/figures/`: Figs. 1-3 (confirmatory) and Fig. 4 (post hoc prior − renorm by gap and N).
- `files/data/link_sessions.csv`: per-session summary derived from DANDI 001201.
- `files/review/AUTHOR_RESPONSE.md`: the response to the round-1 reviews.
- `files/HANDOFF.md`: the handoff note.

## References

- Temmar H. et al. LINK: Long-term Intracortical Neural activity and Kinematics. NeurIPS 2025 Datasets and Benchmarks (openreview.net/forum?id=TAdeh1dLzu).
  - Data: DANDI 001201 version 0.261026.1912, doi:10.48324/dandi.001201/0.261026.1912 (CC-BY-4.0). We downloaded the draft version on 2026-11-28; reviewers confirmed that its 312 assets are identical to the published version.
  - Code: github.com/chesteklab/LINK_dataset.
- Hahn et al. 2026, Nature Medicine, doi:10.1038/s41591-026-04530-3. BrainGate 20-year array study: random-subset electrode scaling and tuning stability in 15 human arrays; code and summary data at github.com/nptl-stanford/array-paper.
- Degenhart A. D. et al. 2020, Nature Biomedical Engineering 4:672, doi:10.1038/s41551-020-0542-9 (latent-factor stabilisation).
- Gallego J. A. et al. 2020, Nature Neuroscience 23:260, doi:10.1038/s41593-019-0555-4 (stable latent dynamics, CCA alignment).
- Nason S. R. et al. 2020, Nature Biomedical Engineering 4:973, doi:10.1038/s41551-020-0591-0 (spiking-band power).
- Ye J. et al. 2023, Neural Data Transformer 2: multi-context pretraining for neural spiking activity, NeurIPS 2023; bioRxiv doi:10.1101/2023.09.18.558113.
- Azabou M. et al. 2023, POYO, NeurIPS 2023, arXiv:2310.16046 (multi-session pretraining).
