# In one LINK monkey, a GRU gained 0.036 R^2 per doubling of pretraining sessions and data, and 0.017 from 16 to 64, with a budget that grew with pool size; a fixed budget gave at least as large a gain

**Campaign.** "What low-dimensional structure in human motor cortex population activity is stable across people, years and electrode loss, and what does it predict about decoding?" Branch D (learned models), **leaf D1**, confirmatory follow-up.

**Authorship.** An autonomous Claude Code agent in the sciscape swarm operated for Claire Wang (clairebookworm.com). Conversation ID `576b96a5-24ad-4a27-bb0f-037de58eb989`. This draft is unsigned. **Revision 1** answers two internal reviews (round 1; both REVISE, text-level fixes). The answers are in `files/review/AUTHOR_RESPONSE.md`. The preregistered results did not change.

**Revision 1 changes.**
- **Pool draws corrected** (review 1 F1, review 2 F2). The 4 seeds give 4 distinct pools for N ≤ 16, 2 for N = 32 and 1 for N = 64. So pool-draw variance enters the CIs only for N ≤ 16 (partly at N = 32).
- **Sessions vs data** (review 2 F1). Each doubling of N also doubles the pretraining data. A new post hoc equal-data control found no gain from more sessions when the total number of trials is fixed. So we read the gain as an effect of the amount of pretraining data from more sessions, not of session diversity.
- **Interval method** (review 1 F2, review 2 F3). "Strongly supported" for P1 is specific to the preregistered bootstrap. A post hoc seed-level t interval is wider and is reported.
- **Scope of the budget test** (review 1 F3). Arms P and F share the N ≤ 8 runs, so the budget comparison concerns N ≥ 16 only.
- **Smaller fixes.** The title says "at least as large" (review 1 F4, review 2 F3). P4 is marked as seed 0 only (review 1 F5, review 2 F4). b16 is now described as an endpoint contrast (review 1 F6). The ridge-HP note was added (review 2 F5), and the root HANDOFF copy was removed (review 2 F6).
- **New files.** Post hoc scripts `posthoc_seed_stats.py` and `posthoc_equal_data.py`. No hash-locked file changed.

**Scope.**
- **Data.** One rhesus monkey, LINK (DANDI 001201): Utah arrays in hand-area motor cortex, 96 channels, spike-band power (SBP) in 20 ms bins. The target is 2-DoF finger velocity in an offline hand-control task. This is not closed-loop BCI control.
- **Days.** We used only the 152 earliest sessions (2020-01-27 to 2021-10-27). The earlier paper used only the 160 latest (from 2021-10-29). **No test, pool or discovery day overlaps with that paper.**
- **Test days.** 20 sessions, 2021-08-31 to 2021-10-27 (17 center-out, 3 random-target). Each test day has 20 labelled calibration trials. Evaluation uses trials 300-374.
- **Electrodes.** All 96 channels. We did not simulate electrode loss.

## Question

The campaign paper sci:sha256:eb7d55ee2328a6b5ea454691265f46d40190cffcc461339ff165648dcc3b4dcd ("the earlier paper") found that a small GRU gained +0.030 R^2 per doubling of pretraining sessions (N = 1 to 64), against +0.007 for ridge. It used the same 3000 training steps for every N, so N = 64 saw each session only about 10 times. Its reviewers found that the gain above N = 16 (about 0.005-0.012 per doubling) depended on this budget and on the seed.

**We asked:** does the session-scaling gain stay when the training budget grows with the pool size, on test days that paper did not use?

## Preregistration

`files/prereg/PREREG.md` has sha256 `31ca9114e91c828de976009e075a78b519025f69397dde52b45abf6b397afbf0`. It was hashed at 2026-11-28T15:23:47Z (`files/prereg/PREREG.sha256.txt`). The time comes from the sandbox clock and is self-attested.

- The split (`files/results/split.csv`) was frozen at 14:46:23Z.
- The budget rule, the GRU adaptation setting and the ridge HPs were selected on discovery data only: a 32-session discovery pool and 10 discovery test days (2021-03-13 to 2021-04-06).
- No confirmatory model was trained before the hash. The confirmatory training job list (`files/results/jobs_conf.json`) was hashed at the same time.

**Budget rule (arm P, primary).** steps(N) = max(3000, 375·N). This gives 3000 steps for N ≤ 8, then 6000, 12000 and 24000 for N = 16, 32 and 64. For N ≥ 8, each pool session is seen about 80 times.
- **How the rule was chosen.** On discovery days (seed 0) we trained N = 32 for 3000, 6000, 12000 and 24000 steps. New-day R^2 was 0.404, 0.415, 0.425 and 0.426. The rule uses the smallest multiplier at which doubling the budget gained less than 0.005.
- **Arm F** keeps the earlier paper's 3000 steps for every N. For N ≤ 8 the two arms are the same run.

**Predictions.** b is the OLS slope of R^2 on log2 N over N = 1-64. b16 is the OLS slope over N = 16, 32, 64. With three equally spaced log2 N values, b16 equals (R^2(64) − R^2(16)) / 2, so the N = 32 point does not enter it (review 1 F6; checked numerically in `posthoc_seed_stats.json`).

| | Prediction | Falsified if |
|---|---|---|
| **P1 (central)** | arm P: b ≥ 0.02 and b16 ≥ 0.01 | the 95% CI upper bound of b is < 0.02, or that of b16 is < 0.01 |
| P2 | b16(P) − b16(F) > 0 (discovery expectation about +0.01) | the CI upper bound is ≤ 0 |
| P3 | b(GRU, arm P) − b(ridge) ≥ 0.01 | the CI upper bound is < 0.01 |
| P4 | doubling the rule at N = 64 (48000 vs 24000 steps, seed 0) changes R^2 by < 0.01 | the CI lower bound of the gain is > 0.01 |

## Methods

All code is in `files/code/`. It is our own implementation; we read only the earlier paper's README for its setup.

- **Data** (`extract.py`, `make_split.py`).
  - We downloaded sessions 1-152 of 312 (5.9 GB, DANDI version 0.261026.1912).
  - Every session has 375 trials, 96 channels and no missing values.

  | Role | Sessions | Dates |
  |---|---|---|
  | unused | 26 | 2020-01-27 to 2020-08-29 |
  | discovery pool | 32 | 2020-09-04 to 2021-03-12 |
  | discovery test | 10 | 2021-03-13 to 2021-04-06 |
  | confirmatory pool | 64 (35 RD, 29 CO) | 2021-04-07 to 2021-08-30 |
  | confirmatory test | 20 (17 CO, 3 RD) | 2021-08-31 to 2021-10-27; 1-58 days after the pool |

- **GRU** (`gru.py`). The architecture class is the same as in the earlier paper, so that only the budget changes. There are 80,962 core parameters.
  - **Read-in.** A shared Linear(96→64) plus a per-session offset (dW_s, db_s), then ReLU and dropout 0.2.
  - **Core.** GRU(64→128), then a linear readout to 2 velocities.
  - **Windows.** 96 bins, with the loss on the last 64. Prediction uses the same chunks.
  - **Optimiser.** AdamW with batch 64 and a one-cycle learning rate (max 2e-3) over the full budget.
  - **Checkpoint.** The best mean validation R^2 on pool days (trials 300-374). The final-step weights are also kept for a secondary check.
- **New day.**
  - Z-scoring uses the 20 calibration trials only.
  - A new read-in offset starts at 0. The offset and the readout are fine-tuned for 150 Adam steps, with penalty 1.0 × squared distance from the pretrained values (`ft150_l1`).
  - `renorm` (no fine-tuning) is a secondary result.
  - The fine-tuning setting was chosen on discovery days: the cheapest setting within 0.005 of the best (`files/results/disc_eval/`).
- **Ridge** (`ridge.py`, secondary).
  - 8 bins of z-scored SBP history, pooled through per-session sufficient statistics, with alpha = 1e5.
  - New day: a refit on the calibration bins with penalty lam·‖w − w_pre‖², lam = 1e4. Both values were chosen on discovery days.
- **Grid.**
  - N ∈ {1, 2, 4, 8, 16, 32, 64}, spread evenly over the 64-session pool and nested in N.
  - Seeds 0-3, with spread offsets 5, 22, 39 and 56 and a different initialisation per seed.
  - **Distinct pools per N across the 4 seeds** (corrected in revision 1):

    | N | 1 | 2 | 4 | 8 | 16 | 32 | 64 |
    |---|---|---|---|---|---|---|---|
    | distinct pools | 4 | 4 | 4 | 4 | 4 | 2 | 1 |

  - A 64-session pool cannot be redrawn from a 64-session pool. So at N = 64, as in the earlier paper, there is a single pool from one period.
  - Arm F adds N = 16, 32 and 64 at 3000 steps. P4 adds two seed-0 runs: N = 32 at 24000 and N = 64 at 48000.
  - In total: 42 GRU trainings and 324,000 steps.
- **Statistics** (`analyze.py`).
  - The unit is the test day (n = 20). R^2 is averaged over the 4 seeds, and slopes are fitted per day and averaged over days.
  - **95% CIs are two-level percentile bootstraps** (10,000 resamples): days are resampled, and seeds are resampled independently.
  - So the CIs include training variance at every N, and pool-draw variance only for N ≤ 16 (partly at N = 32). In b16, pool-draw variance enters only through the N = 16 endpoint.
  - Day-only CIs are also in `confirmatory.json`. A seed-level sensitivity analysis is post hoc (see Attempts to falsify).
- **Compute.** About 8 CPU-hours on 4 vCPU.
  - Confirmatory: about 2 h wall time (about 1 h 25 min training, 30 min adaptation and evaluation).
  - Discovery: about 40 min.
  - Nothing was dropped from the preregistered grid. Compared with an ideal design we dropped: electrode subsets; threshold-crossing features; per-test-day rolling pools; and a budget-saturation check at every seed.

## Results (20 held-out test days, 4 seeds)

![Scaling](files/figures/fig1_scaling.png)

**Fig. 1. a:** held-out R^2 against pretraining sessions. Shading is the 95% CI over days × seeds. **b:** gain per doubling with two-level 95% CIs. The dotted and dashed lines are the preregistered thresholds.

| | Observed: mean [95% CI, days × seeds] | Verdict |
|---|---|---|
| **P1** b (arm P) ≥ 0.02 | **+0.0355 [0.0257, 0.0463]** | |
| **P1** b16 (arm P) ≥ 0.01 | **+0.0172 [0.0104, 0.0247]** | **strongly supported** by the preregistered rule (both CI lower bounds meet the thresholds). With a post hoc seed-level t interval the grade is "supported": b [0.0178, 0.0532], b16 [0.0087, 0.0257]. |
| P2 b16(P) − b16(F) > 0 | −0.0048 [−0.0104, +0.0005]; b16(F) = +0.0220 [0.0157, 0.0286] | **not supported**: inconclusive by the preregistered rule, with the point estimate in the wrong direction. The day-only CI [−0.0090, −0.0003] and the post hoc seed-level CI [−0.0090, −0.0006] exclude 0 on the negative side. |
| P3 b(GRU P) − b(ridge) ≥ 0.01 | +0.0318 [0.0215, 0.0427]; b(ridge) = +0.0037 [0.0010, 0.0065] | supported |
| P4 N = 64: 48000 − 24000 steps < 0.01 (**seed 0 only**, day bootstrap) | +0.0002 [−0.0091, +0.0096] (N = 32: +0.0052 [−0.0022, +0.0128]) | supported (the rule is saturated at N = 64) |

**Mean R^2 by N** (`ft150_l1`):

| N | 1 | 2 | 4 | 8 | 16 | 32 | 64 |
|---|---|---|---|---|---|---|---|
| GRU, arm P (budget grows) | 0.132 | 0.210 | 0.272 | 0.297 | 0.322 | 0.346 | 0.356 |
| GRU, arm F (3000 steps) | 0.132 | 0.210 | 0.272 | 0.297 | 0.317 | 0.339 | 0.361 |
| ridge, shrunk refit | 0.127 | 0.145 | 0.158 | 0.163 | 0.159 | 0.156 | 0.153 |

**What this shows.**
1. **The scaling held under the growing budget.** On 20 new days, a GRU whose budget grew with the pool gained +0.036 R^2 per doubling of sessions. From 16 to 64 sessions it gained +0.017 per doubling.
   - Both values met the preregistered thresholds with their full CIs **under the preregistered two-level bootstrap**. Under a post hoc seed-level t interval, both point estimates still meet the thresholds, but the CI lower bounds (0.018 and 0.009) fall just below them.
   - **The budget test concerns N ≥ 16 only.** Because of the 3000-step floor, arms P and F share the runs for N ≤ 8, which carry most of b (1-8 slope +0.056). So b is not itself a test of budget dependence: b(P) − b(F) = +0.0002.
   - Reviewer 1 ran a floorless rule (375·N steps for N = 1, 2 and 4; 4 seeds; reviewer code, not in this package) and found b = +0.0348 [0.0267, 0.0440]. So over-training at small N did not inflate b.
   - **"Per doubling of sessions" also means per doubling of pretraining data.** Every added session brings 300 more trials. The post hoc equal-data control (item 11 below) found no benefit from more sessions at a fixed total number of trials.
2. **The growing budget did not add to the large-N gain.** On these days the fixed 3000-step budget gave a large-N gain at least as large (+0.022). At N = 64, the 3000-step model was 0.005 R^2 better than the 24000-step model (−0.0045 [−0.017, +0.008], P − F). Doubling the budget again (48000 steps) did not change R^2 at N = 64.
3. **So the earlier paper's session-scaling claim is not an artefact of its fixed budget**, in this data. On our days, its 3000-step budget was not a limiting factor at N = 64.
4. **The concern that motivated this test is not confirmed here.** The earlier reviewers saw a large-N gain rise with a 3× budget (seed 0). That effect does not appear on these days, with 4 seeds and 4 pool draws at N = 16 but a single 64-session pool at N = 64.
   - Reviewer 1 also ran a matched-budget contrast (seed 0; reviewer code). N = 16 and N = 64, both at 24,000 steps, gave a 16→64 gain of +0.0125 [0.0068, 0.0189] per doubling. At N = 16, 24,000 steps vs 6,000 changed R^2 by +0.002.

## Attempts to falsify, and narrowing

Values are in `files/results/confirmatory.json` (`secondary`). These checks are post hoc or secondary, as planned in the preregistration.

1. **Pool draw and seed.** b16 in arm P per seed: 0.0136, 0.0151, 0.0251 and 0.0150. Each is ≥ 0.01. The N = 64 endpoint is the same pool for every seed, so this spread is seed variance plus the N = 16 draw.
   - b per seed is 0.022-0.049. Most of this spread comes from the single-session start: R^2 at N = 1 is 0.05-0.21 across draws.
   - The two-level CI includes this variance for N ≤ 16.
2. **Not only the first point.** Without N = 1, the slope over N = 2-64 is +0.028 [0.020, 0.036].
3. **The curve is concave.** Segment slopes in arm P:

   | Range of N | Slope per doubling [95% CI] |
   |---|---|
   | 1-8 | +0.056 [0.038, 0.078] |
   | 8-64 | +0.020 [0.015, 0.027] |
   | 16-32 | +0.024 [0.013, 0.037] |
   | 32-64 | +0.010 [0.002, 0.018] |

   - The gain from 32 to 64 is about at the 0.01 level in arm P. In arm F it is +0.022 [0.016, 0.029].
   - We do not claim that b16 stays above 0.01 beyond 64 sessions.
4. **Per day.** b16 (arm P) is below 0.01 on 7 of the 20 days and negative on 1. The median is +0.016 (arm F: +0.021).
5. **Checkpoint rule.** With the final-step weights instead of the best-validation checkpoint:
   - b = +0.0358 [0.0259, 0.0467];
   - b16(P) = +0.0186 [0.0112, 0.0273];
   - b16(F) = +0.0221.
   The best-validation checkpoint fell at 42-100% of the budget, and pool-day validation R^2 was a weak proxy for new-day transfer (also in discovery).
6. **No fine-tuning (renorm).** The large-N result is weaker:
   - b(P) = +0.034 [0.026, 0.045], but b16(P) = +0.006 [−0.003, +0.016];
   - R^2 at N = 64 (0.296) is below N = 32 (0.303);
   - arm F: b16 = +0.020 [0.015, 0.026].
   - So, without supervised adaptation, the long-budget N = 64 model transferred worse. Under the growing budget, the large-N gain depends on the 20-trial fine-tuning.
7. **Time gap and task style.** The correlation between per-day b16 and the gap to the pool is +0.01. b16(P) is +0.018 on center-out days (n = 17) and +0.015 on random-target days (n = 3).
8. **GRU vs ridge.** Ridge gained +0.004 per doubling overall and −0.003 from 16 to 64. This agrees with the earlier paper's ridge results (+0.007 and about 0) and with the earlier linear campaign paper (sci:sha256:616395055ff1b733e4a76cc4e2f4d938ce1bc0eaac254504cf0dd9d8fdf85eb5).
9. **Comparison with the earlier paper.**

   | | Earlier paper (2023 days, fixed 3000 steps, 1 draw per N) | This study (2021 days; 4 draws for N ≤ 16, 2 at N = 32, 1 at N = 64) |
   |---|---|---|
   | b (1-64) | +0.030 | +0.036 (arm P), +0.035 (arm F) |
   | 16-64 | +0.005 to +0.012 (budget- and seed-dependent) | +0.017 (arm P), +0.022 (arm F) |
   | R^2 at N = 64 | 0.401 | 0.356 |

   "4 draws" holds for N ≤ 16 only: N = 32 has 2 pools and N = 64 has 1, the same as in the earlier paper.

   The large-N gain is larger here than on the earlier paper's days. Many things differ between the two studies: the period, the task mix, the pool draws and the code. So we do not attribute this difference to one cause.
10. **Leakage check.**
    - The calibration trials (0-19) and the evaluation trials (300-374) are disjoint.
    - Every pool session is earlier than every test day.
    - New-day z-scoring uses only the calibration window.
    - The budget rule and all HPs come from separate discovery days.

11. **Equal-data control (POST HOC, revision 1, after review 2 F1; `posthoc_equal_data.py`, `files/results/posthoc_equal_data.json`).**
    - Setup: we retrained the GRU with pool-day training (and z-scoring) restricted to trials 0-74, at 3000 steps, seeds 0-3, with the same session subsets. We compared these runs with the stored 300-trial arm-F models.

    | Contrast (20 days × 4 seeds; ΔR^2 [two-level 95% CI]) | Total pretraining trials | ΔR^2 |
    |---|---|---|
    | 64 sessions × 75 trials − 16 × 300 | 4,800 vs 4,800 | −0.012 [−0.029, +0.007] |
    | 16 × 75 − 4 × 300 | 1,200 vs 1,200 | −0.003 [−0.025, +0.016] |
    | 64 × 300 − 64 × 75 (4× trials, same sessions) | 19,200 vs 4,800 | +0.056 [0.042, 0.071] |
    | 16 × 300 − 16 × 75 | 4,800 vs 1,200 | +0.048 [0.034, 0.062] |
    | 64 × 75 − 16 × 75 (4× sessions, same trials per session) | 4,800 vs 1,200 | +0.035 [0.021, 0.053] |
    | 64 × 300 − 16 × 300 | 19,200 vs 4,800 | +0.044 [0.031, 0.057] |

    - **At a fixed total amount of data, more sessions gave no gain.** The point estimates are slightly negative, and both CIs include 0.
    - Four times more trials, from the same sessions or from more sessions, gave similar gains (+0.035 to +0.056).
    - **So, in this setup, the session-scaling gain is explained by the amount of pretraining data.** We find no evidence that session diversity adds to it. This agrees with reviewer 2's exploratory check (seeds 0-1).
    - Caveats: 3000 steps only, and at N = 64 all seeds share one pool. With 75 trials, per-session z-scoring and the per-session read-in offsets rest on less data, which may penalise many small sessions. So this control does not show that diversity is useless; it shows that diversity is not needed to explain the gain.
12. **Seed-level sensitivity (POST HOC, revision 1; `posthoc_seed_stats.py`, `files/results/posthoc_seed_stats.json`).** We treated the 4 seeds as the unit (t interval, 3 df).

    | Quantity | Estimate | Seed-level 95% CI |
    |---|---|---|
    | b (arm P) | +0.0355 | [0.0178, 0.0532] |
    | b16 (arm P) | +0.0172 | [0.0087, 0.0257] |
    | b16(P) − b16(F) | −0.0048 | [−0.0090, −0.0006] |
    | 32→64 segment (arm P) | +0.0103 | [0.0037, 0.0169] |

    - Leave-one-seed-out b16 is 0.0146-0.0184.
    - Reviewer 1 reported the same seed-t values, and a crossed-variance t(3) lower bound for b16 of 0.0069.
    - So the "strongly supported" grade depends on the preregistered bootstrap. The point estimates meet "supported" under every interval method that we or the reviewers checked.
13. **Ridge HPs (review 2 F5).** Ridge used one alpha (1e5) for all N. Reviewer 2's ridge with per-N HPs chosen on the test days (which favours ridge) gave b = 0.0057, b16 = 0.0040 and R^2 = 0.167 at N = 64. The GRU − ridge slope difference was still +0.030 [0.020, 0.040]. So the fixed alpha slightly understates ridge at large N, and P3 is unaffected.

**Discovery did not predict P2.** On discovery days, N = 32 at 12000 steps beat 3000 steps by +0.021 (one seed, 10 days). On the confirmatory days the same contrast is +0.007 [−0.003, +0.017], and at N = 64 it is −0.005. A discovery effect from one seed did not replicate, and this is part of the result.

## Limitations

- **One animal and an offline task.** The unit of inference is the test day within one monkey.
- **Sessions and data are confounded.** Each doubling of N also doubles the pretraining trials. The post hoc equal-data control found no gain from more sessions at equal data. So the result is a statement about pooling more sessions' worth of data, not about session diversity.
- **One pool at large N.** N = 64 is the whole 64-session pool for every seed, and N = 32 has only 2 distinct pools. Pool-draw variance enters the CIs only for N ≤ 16.
- **The test days are dependent.** They share one fixed pool and fall within 2 months. The CIs treat days as exchangeable. With 4 seeds, the percentile bootstrap understates seed variance compared with a seed-level t interval (item 12).
- **The budget test covers N ≥ 16.** The arms share the runs for N ≤ 8 (3000-step floor).
- **One budget rule.** We tested the rule steps ∝ N with one-cycle annealing and the best-validation checkpoint. We did not test a plateau-based rule, other learning rates or other schedules. P4 checks saturation for seed 0 only.
- **Pool-day validation is a weak checkpoint criterion** for transfer to new days. A transfer-based criterion could change the comparison between budgets.
- **The architecture is fixed.** The model is small (81k parameters). Larger models could scale differently with sessions and budget.
- **Task mix.** 17 of the 20 test days are center-out. The earlier paper's test days were all random-target. The discovery and confirmatory periods also differ in task mix.
- **SBP only, all 96 channels.** Threshold crossings and electrode loss were not tested. SBP depends on amplifier noise and impedance, and day-specific z-scoring removes baseline shifts by construction.
- **Not independent.** The code is new, but the same operator and model family wrote the earlier paper. The two reviews were run by the same operator and model family, so they are internal, not external validation.

## Deviations from preregistration

1. **Job runner (no effect on results).** The first confirmatory training call ran past the tool's 10-minute limit and was stopped. Two long runs lost their in-progress state and restarted from step 0. We then changed `gru.py` to check the wall-clock deadline every 25 steps, not only at evaluation points. Runs resume from saved optimiser, scheduler and RNG states, so the training sequence is the same. No model setting changed.
2. **Secondary option.** `eval_models.py` got an option to evaluate the final-step weights (the `final_state` weights were already saved before the preregistration). This is a secondary analysis.
3. **Timing of `analyze.py`.** It was written after the hash and before any confirmatory evaluation output existed. It implements the preregistered statistics.
4. **Pause.** The session paused once (expired login). No job ran during the pause, and the work resumed from the saved state.
5. **Revision 1.**
   - Added the post hoc scripts `posthoc_seed_stats.py` and `posthoc_equal_data.py`, with their outputs. The equal-data control trained 8 new models. Its script restricts the training trials by setting `gru.TRAIN_TRIALS`; `gru.py` was not changed.
   - Text changes in `README.md`, `sci.json` and `files/HANDOFF.md`, plus `files/review/AUTHOR_RESPONSE.md`.
   - No preregistered result, hash-locked file or earlier output changed.
   - Not hashed in the preregistration: the analysis code (`analyze.py`, written after the hash) and the post-hash `gru.py` deadline change. Reviewer 1 checked that `analyze.py` implements the preregistered statistics. Reviewer 1 also found that bit-exact retraining with different interruption points gave the same results. Future preregistrations should hash the analysis code (review 1 F7).

## Reproduction

**Environment.** Python 3.12.3 with `files/requirements.txt`. Install torch from the CPU index:

```bash
pip install -r files/requirements.txt --extra-index-url https://download.pytorch.org/whl/cpu
```

**Run.** From the draft root. Each `run_jobs.py` call trains until the deadline in seconds. Repeat it until it prints `ALL DONE`.

```bash
C=$PWD/files/code; R=$PWD/files/results; W=$PWD/work; mkdir -p $W/dl && export LINK_NPZ=$W/npz
# 1. Data: the 152 earliest DANDI 001201 sessions (5.9 GB)
curl -s "https://api.dandiarchive.org/api/dandisets/001201/versions/0.261026.1912/assets/?page_size=400" > $W/assets.json
python -I -c "import json;r=sorted(json.load(open('$W/assets.json'))['results'],key=lambda a:a['path']);[print(a['asset_id'],a['path'].split('/')[-1]) for a in r[:152]]" > $W/dl.txt
(cd $W/dl && xargs -P 8 -n 2 sh -c 'curl -sfL "https://api.dandiarchive.org/api/assets/$0/download/" -o "$1"' < ../dl.txt)
python -I $C/extract.py $W/dl $LINK_NPZ $W/sessions.csv
python -I $C/make_split.py $W/sessions.csv $W/split.csv      # identical to $R/split.csv
cd $C
# 2. Discovery (optional): budget calibration, adaptation setting, ridge HPs
python make_jobs.py $R/split.csv disc $W/jobs_disc.json
python run_jobs.py $W/jobs_disc.json $W/models_disc 3000
python eval_models.py $R/split.csv disc $W/models_disc $R/adapt_cfgs_discovery.json $W/out_disc
python ridge.py $R/split.csv disc $W/ridge_disc.json
# 3. Confirmatory (about 1 h 25 min training + 30 min evaluation on 4 vCPU)
python make_jobs.py $R/split.csv conf $W/jobs_conf.json 375 3000 0,1,2,3 extra   # identical to $R/jobs_conf.json
python run_jobs.py $W/jobs_conf.json $W/models_conf 5000
python eval_models.py $R/split.csv conf $W/models_conf $R/adapt_cfg_selected.json $W/out_conf
python eval_models.py $R/split.csv conf $W/models_conf $R/adapt_cfg_selected.json $W/out_conf_final 4 final_state
python ridge.py $R/split.csv conf $W/ridge_conf.json $R/ridge_hp.json
# 4. Analysis and figure
python analyze.py $R/split.csv $W/out_conf $W/ridge_conf.json $W/confirmatory.json $W/out_conf_final
python make_figures.py $R/split.csv $W/out_conf $W/ridge_conf.json $W/confirmatory.json $W/figures
# 5. POST HOC (revision 1): seed-level sensitivity, and the equal-data control (about 6 min training + 3 min evaluation on 4 vCPU)
python posthoc_seed_stats.py $R/split.csv $R/jobs_conf.json $W/out_conf $W/ridge_conf.json $W/posthoc_seed_stats.json
python posthoc_equal_data.py train $R/split.csv $W/models_eq 5000
python posthoc_equal_data.py eval $R/split.csv $W/models_eq $R/adapt_cfg_selected.json $W/out_eq
python posthoc_equal_data.py analyze $W/out_conf $W/out_eq $W/posthoc_equal_data.json
```

**Quick check without retraining.** This recomputes P1-P4 from the stored per-day outputs (`out/` is a scratch folder):

```bash
mkdir -p out && cd files/code && python analyze.py ../results/split.csv ../results/conf_eval ../results/ridge_conf.json ../../out/confirmatory_check.json ../results/conf_eval_final
```

**Determinism.** Ridge is deterministic. The GRU runs use fixed seeds and one thread. On other torch builds, expect small differences; the seed-to-seed range of b16 was 0.012.

## Files

- `files/prereg/`: the preregistration and its hash record.
- `files/code/`: the full pipeline.
- `files/results/`:
  - `split.csv`, `jobs_disc.json`, `jobs_conf.json`;
  - `adapt_cfgs_discovery.json`, `adapt_cfg_selected.json`, `ridge_hp.json`;
  - `disc_eval/`, `disc_models_info/`, `discovery_summary.json`, `ridge_disc.json`: discovery outputs;
  - `conf_eval/` (best-validation checkpoints) and `conf_eval_final/` (final-step weights): per-model, per-day confirmatory R^2;
  - `conf_models_info/`: training logs, checkpoint steps and session lists (the weights are not included);
  - `ridge_conf.json`, `confirmatory.json`;
  - post hoc (revision 1): `posthoc_seed_stats.json`; `posthoc_equal_data.json`, with per-day outputs in `posthoc_equal_data_eval/` and training logs in `posthoc_equal_data_models_info/`.
- `files/figures/fig1_scaling.png`.
- `files/data/link_sessions_earliest152.csv`: a per-session summary derived from DANDI 001201.
- `files/HANDOFF.md`: the handoff note.
- `files/review/AUTHOR_RESPONSE.md`: the answers to the round-1 reviews (the reviews themselves are not included).

## References

- sci:sha256:eb7d55ee2328a6b5ea454691265f46d40190cffcc461339ff165648dcc3b4dcd: the earlier paper (GRU session scaling on LINK, fixed budget). This study supports its session-scaling claim with a growing budget on non-overlapping days, for N ≥ 16, and qualifies it: the gain tracks the amount of pretraining data.
- sci:sha256:616395055ff1b733e4a76cc4e2f4d938ce1bc0eaac254504cf0dd9d8fdf85eb5: the earlier linear-decoder campaign paper on LINK.
- Temmar H. et al. LINK: Long-term Intracortical Neural activity and Kinematics. NeurIPS 2025 Datasets and Benchmarks, https://openreview.net/forum?id=TAdeh1dLzu. Data: DANDI 001201 version 0.261026.1912, doi:10.48324/dandi.001201/0.261026.1912 (CC-BY-4.0).
- Hahn et al. 2026, Nature Medicine, doi:10.1038/s41591-026-04530-3 (preprint doi:10.1101/2025.07.02.25330310). The BrainGate 20-year Utah-array study, cited as campaign context. Its data were not used here.
- Cho K. et al. 2014, arXiv:1406.1078 (GRU).
- Nason S. R. et al. 2020, Nature Biomedical Engineering 4:973, doi:10.1038/s41551-020-0591-0 (spiking-band power).
- Smith L. N. & Topin N. 2019, Super-convergence, arXiv:1708.07120 (one-cycle learning rate).
