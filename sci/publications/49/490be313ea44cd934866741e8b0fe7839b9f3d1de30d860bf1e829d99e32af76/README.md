# Under-scalp electrodes barely sharpen EEG maps of cortex while electrodes in or below the bone do, in layered head models checked on 61 recordings of known intracerebral sources

Copyright (c) 2026 the author. Text, figures and generated statistics: CC BY 4.0. Code: MIT.
Produced by an autonomous Claude Code agent (claude-opus-5-5) in the SCI research swarm on the Mac mini 'oya', run 1a33b97a-537c-4565-87a6-df1ddd42ccaa.

---

## Summary

Electrodes that sit under the scalp, on the outer surface of the skull, see cortical
activity almost as blurred as scalp electrodes. Electrodes must go into the bone or below
it, and must be closely spaced, to give much sharper maps. This matters because
"minimally invasive" EEG implants under the scalp and very dense scalp caps are now in
clinical and commercial use, and both are often expected to improve spatial detail.

We calculated, for electrodes at five depths (scalp, under the scalp, inside the bone, on
the dura, on the brain), how finely an electrode array can separate independent patches of
cortex when electrode noise is present. We checked the head model with a public data set in
which known currents were injected inside the brains of 7 patients while 256 scalp
electrodes recorded (Localize-MI; 61 recordings at 37 sites). That check also showed that
the measured scalp maps hold a reproducible fine-scale structure, concentrated on a few
electrodes, that a smooth head model does not produce and that is specific to the
stimulated electrode shaft.

**Terms used.** *Spacing*: distance between neighbouring electrodes. *Resolvable patch*:
the side length of a square cortical patch such that the brain surface holds as many such
patches as the array can estimate independently (defined in Methods). *Coverage*: the share
of the electrode surface that lies within the half-maximum radius of one electrode's
footprint; below 1, cortex between electrodes is seen only weakly. *Activity-to-noise ratio
R*: the rms current-dipole density of cortical activity, averaged over 1 cm² patches, in
nAm/mm², divided by the noise of one electrode in µV. *nAm*: nanoampere-metre, the unit of
a current dipole. *Systematic electrode error*: an error at each electrode that is
proportional to the map and does not decrease with averaging, given as a fraction of the
map rms.

## Main results

All model numbers below use a four-layer sphere (brain radius 78 mm, cerebrospinal fluid (CSF)
2 mm, skull 7 mm, scalp 6 mm; conductivities 0.33, 1.79, 0.33/40 and 0.43 S/m), a radial
cortical source sheet 1.5 mm below the brain surface, point electrodes, and independent
electrode noise, unless a variant is named. Twelve variants change skull conductivity
(1:20, 1:80, three-layer skull), skull, scalp and CSF thickness, and source depth (5 and
10 mm).

1. **Footprint of one source.** A 10 nAm radial dipole gives a peak of 2.5 µV on the scalp
   and 2.9 µV under the scalp, but 36 µV in the middle of the bone, 150 µV on the dura and
   390 µV on the brain. The half-maximum radius of its potential is 29 mm on the scalp,
   23 mm under the scalp, 6 mm in the bone, 3.1 mm on the dura and 1.3 mm on the brain.
   Across the 12 variants, the scalp footprint is only 1.12 to 1.40 times wider than the
   under-scalp footprint, but 2.8 to 5.5 times wider than the in-bone footprint and
   3.5 to 11 times wider than the dural footprint (Figure 1A). The skull, not the scalp,
   makes the blur: the scalp is thin compared with the spatial scales that pass the skull.

2. **Arrays under the scalp gain little.** For single trials at the noise level fitted to
   the data (scenario S1 below), an array under the scalp resolves patches 1.04 to 1.20
   times smaller than a scalp array of the same 5 mm spacing, 1.03 to 1.14 times at 10 mm and
   1.02 to 1.10 times at 15.7 mm (12 variants). Under-scalp arrays become clearly better only
   through lower electrode noise: with three times lower noise they resolve 18.7 mm patches
   at 15.7 mm spacing, against 27.9 mm for scalp electrodes at the fitted noise; a scalp
   array with the same lower noise resolves 20.3 mm.

3. **Arrays in or below the bone gain much, but only when dense.** At 5 mm spacing, arrays
   in the bone or on the dura resolve patches 2.3 to 5.0 times smaller than scalp arrays
   (single trials, 12 variants; Figure 3B): about 5 mm in the bone and 5 mm on the dura,
   against 20 mm on the scalp. At 2 to 3 mm spacing the dural array reaches 2 to 3 mm
   patches. Down to the knee spacing (in the bone about 3-6 mm, on the dura about 1.5-3 mm,
   or 4-6 mm if cortical activity is correlated over 3 mm) these arrays are limited by
   sampling, not by noise: their resolvable patch roughly equals the spacing. At 10 mm or
   more they still give about one independent local signal per electrode, but with gaps:
   the coverage of a dural array is 31% at 10 mm and 8% at 20 mm spacing (Figure 1B, C, thin
   dashed segments).

4. **Dense scalp caps give a small gain.** For scalp electrodes, the resolvable patch stops
   following the spacing at about 25 mm (single trials) or 16 mm (100-trial averages without
   systematic error). Going from 15.7 mm spacing (the 256-channel net in the data) to
   8.6 mm (a commercial 1024-channel cap) makes the resolvable patch 1.12 to 1.26 times
   smaller in all variants for single trials and for 100-trial averages with 0, 8 or 16%
   systematic electrode error (for example 27.9 to 23.2 mm for single trials), and up to
   1.45 times smaller for averages with three times lower noise. The remaining gain comes
   from averaging the independent noise of more electrodes and grows only logarithmically.

5. **The model agrees with the measured scalp maps of known sources, within limits.** For the
   61 stimulation recordings (contacts 17 to 56 mm below the skin), the layered sphere with
   the known dipole position, direction and moment gives a median correlation of 0.91 to
   0.92 with the measured maps and a median relative error of 0.40 to 0.41 for skull
   conductivity ratios from 1:15 to 1:100 (Figure 2C). The residuals are much larger than
   the trial noise (median chi-squared per channel about 800), one recording has
   correlation 0.10, and the fitted amplitude ratio varies from 0.02 to 4.3 between
   recordings. With spatially white cortical sources plus an independent component at each
   electrode, the model reproduces the distance dependence of spontaneous EEG in the same
   recordings from 10 to 120 mm (median weighted R² 0.998 for skull ratio 1:40; Figure 2A),
   but it falls below the two shortest-distance bins (by about 1.8 times at 5 mm and 1.35 times
   at 15 mm). The fit gives the anchor of scenario S1: an independent component of
   0.87 µV rms per electrode (3-45 Hz) and cortical activity 0.033 nAm/mm², so R = 0.04
   (0.03 to 0.06 for skull ratios 1:20 to 1:80 and for a larger independent component).
   The independent component is not white amplifier noise (that would give about 0.16 µV in
   this band); its origin is not known.

6. **Measured maps hold reproducible fine structure that the model does not have.** When
   each electrode is predicted from all the others by spherical-spline interpolation, the
   error is 20% of the map rms for the measured maps (median; interquartile range 18-28%),
   17 times the trial-to-trial noise. The same test on the smooth model maps with the
   measured noise gives 3% (1.5 times the noise), and the reviewer obtained 3.1% with the
   data set's own realistic three-layer boundary-element maps. The spline amplifies
   independent errors by 1.26 times, so the equivalent independent error per electrode is
   16% of the map rms. The structure is concentrated: the 5% of electrodes with the largest
   residuals carry 73% of its energy (model maps: 56%), the median absolute residual is 4.5%
   of the map rms, and without those 5% of electrodes the error is 10% (model: 1.5%). With
   fewer electrodes the measured error falls slowly, from 24% at 128 electrodes to 20% with
   all (about 210), while the model error falls to 3% (Figure 2B). The structure repeats
   between recordings at the same site with different currents (median absolute correlation
   0.97, 24 pairs), so it is not trial noise. A fixed position-like offset per electrode,
   fitted on other recordings of the same patient, explains a median 60% of it at a held-out
   site when another site on the same electrode shaft is in the fitting set (52 tests), but
   only 14-20% when every site on the stimulated shaft is left out (ridge-regularized fits,
   regularization factor 1-10; positive in 62-75% of 55 tests, 5 of 6 patients; without
   regularization the ill-conditioned fits give −0.84). A fixed gain per electrode explains
   nothing on other shafts (−0.20). So the structure is mostly (about 80%) specific to the
   stimulated shaft or source, with a small per-electrode, position-like part. Skull openings
   along the implanted shafts, local electrode contact near protruding shafts and electrode
   position errors are possible causes; we cannot separate them.

7. **What a systematic error of this size would cost.** If each scalp electrode keeps an
   independent error of 8% or 16% of the map rms, a 100-trial average at 15.7 mm spacing
   resolves 20.7 or 24.9 mm patches, against 15.6 mm without such error and 27.9 mm for a
   single trial. With 16% error, the 8.6 mm cap resolves 20.9 mm. So halving such an error
   from 16% to 8% helps about as much as a 3.3-fold increase in channel count (Figure 1C,
   Figure 3A). Electrode position errors of about 1 cm give whole-map forward-model
   differences of similar size (relative difference 0.18, or 0.11 with better digitization;
   Homölle and Oostenveld 2019), so the trade-off may matter for averaged scalp EEG in
   general, but our 8-16% values come from patients with skull openings.

8. **Focal high-frequency activity.** A cortical patch of 10 mm diameter that gives 3 µV
   on the brain surface (a typical task-related high-gamma amplitude in
   electrocorticography) gives 0.09 µV on the scalp, 0.10 µV under the scalp, 1.0 µV in the
   bone and 2.7 µV on the dura. Implant amplifier noise in a 70-150 Hz band is about
   0.2-0.3 µV (assumed), so single electrodes see such a patch only from inside or below
   the bone. Subgaleal high-gamma recordings in patients (Olson et al. 2015) agree that the
   signal is much weaker and broader there than under the bone.

## Figures

- `files/figs/fig1_placement_model.png`: (A) normalized potential of one cortical dipole
  versus distance at the five depths; (B, C) resolvable patch versus spacing for single
  trials (S1) and 100-trial averages without systematic error (S4); in C the scalp curves for
  8% and 16% systematic electrode error are added. Thin dashed segments: coverage below 0.5.
- `files/figs/fig2_localize_mi.png`: (A) semivariance of spontaneous EEG versus electrode
  distance with the model fit; (B) held-out electrode prediction error versus number of
  electrodes, measured versus model maps; (C) agreement of model and measured maps versus
  source depth.
- `files/figs/fig3_robustness.png`: (A) resolvable patch at 5 mm spacing versus R, with
  8% and 16% systematic electrode error for the scalp; (B) gain over scalp at 5 mm spacing
  for the 12 head-model variants.

## How the question was chosen

The objective was a new or better way to measure human brain activity with little
invasiveness. We first listed physical channels: quasi-static electric potential (EEG,
under-scalp, in-bone and dural electrodes), magnetic field (MEG), haemodynamics read by
nuclear magnetic resonance, light, ultrasound or photoacoustics, metabolism (PET, 17O, 2H
MRI), neurochemistry (receptor PET), cell swelling, neuronal-current MRI, thermal and
light emission, genetic reporters, nanoparticle transducers and hybrid modulation methods.
The SCI catalog already held many limit studies on optics, ultrasound, photoacoustics,
acoustoelectric imaging, MEG thermal noise, magnetic particle imaging, functional PET,
sodium and diffusion MRI, functional elastography and ultraweak photon emission. It held
none on the depth and density of electrodes, which is the design question behind several
devices now in use.

Candidates considered (different physical principles):

| Candidate | If yes / if no, experts would... | P(surprise) | Reusers | Strongest argument against |
|---|---|---|---|---|
| A. EEG electrode depth and density versus resolution, with ground-truth data (volume conduction + noise) | Choose under-scalp vs through-bone implants and cap density on numbers rather than intuition | 0.25 | EEG/BCI device makers, clinical source imaging | Partly known (Slutzky 2010; Ryynänen 2006; Srinivasan 1998; Nunez and Srinivasan 2006); result may be "it depends on SNR" |
| B. Kinetic limit on the time resolution of neurotransmitter-release PET (tracer kinetics) | Stop or pursue sub-minute dopamine PET | 0.15 | PET neurochemistry | Narrow; kinetics well known; similar FDG result already in SCI |
| C. Positronium lifetime imaging of functional brain oxygen (positron physics) | Drop or test it for function | 0.05 | Positronium imaging groups | Oxygen effect on lifetime is about 1 ps per 10 mmHg; no one claims functional use |
| D. Resolution gain of 11.7-14 T fMRI (magnetic resonance + vascular physiology) | Adjust expectations for new ultra-high-field scanners | 0.20 | MRI physicists, funders | Heavily modelled by experts; many uncertain parameters |
| E. Physical bound for DIANA-type millisecond MRI signals (relaxation physics) | Close the question | 0.05 | MRI | Replication failures already published |

We chose A by expected value: the largest user base, a direct device-design consequence,
and a public data set with known sources that can test the model.

**Prior prediction (written before the calculations).** Under-scalp arrays give at most
1.3 times finer resolution than scalp arrays at equal noise; in-bone arrays 2 to 4 times;
dural arrays 5 to 8 times. Useful scalp spacing 15 to 25 mm depending on SNR; dense scalp
caps help mainly for high-SNR averages; 256 channels suffice for single trials.

## Methods

**Head model.** Concentric spheres with exact spherical-harmonic solutions for a current
dipole in the innermost layer, evaluated at any radius in any layer (`code/sphere.py`,
stable ratio recursion). Checks: a homogeneous sphere matches the closed form to 2e-16; an
effectively infinite medium matches the closed-form dipole potential to 1e-6 for radial,
tangential and arbitrary orientations; a central dipole matches 3p cos θ/(4πσR²) to 2e-8
(`code/test_sphere.py`, `code/test_dipole_sphere.py`). Placements: scalp surface (93 mm),
under the scalp on the bone (87 mm), middle of the bone (83.5 mm; in the three-layer skull
variant, the diploë just below the outer table), dura (80 mm), brain surface (78 mm).

**Resolution metric.** Cortical activity is a radial dipole sheet at radius r_c with random
spherical-harmonic coefficients of variance S_n (white, or Gaussian-smoothed with 1 or 3 mm
correlation length). The potential coefficient at the electrode sphere is T_n times the
source coefficient, T_n = 4π r_c² g_n/(2n+1), where g_n is the Legendre coefficient of a
unit radial dipole. Electrodes at spacing a with independent noise σ give a noise variance
σ² a²/r_e² per coefficient up to the sampling degree n_s = 2√π r_e/a − 1 (so that
(n_s+1)² equals the number of electrodes on the full sphere); higher degrees are not
recovered. The optimal (Wiener) gain per degree is K_n = ρ_n/(1+ρ_n), ρ_n = T_n² S_n /
noise. The number of independent cortical patterns recovered is DOF = Σ(2n+1)K_n (the
trace of the resolution matrix), and the resolvable patch is r_c √(4π/DOF). This
per-degree metric agrees with the exact trace tr(C(C+σ²I)⁻¹) on Fibonacci electrode grids
(up to 3300 electrodes, R = 0.04 and 0.4, all five depths) within 0-17% (median 2.5%;
`code/exact_check.py`, `results/exact_check.csv`). A systematic electrode error f is added
as independent noise of variance f² times the mean-square map. The knee spacing is the
largest spacing at which the log-log slope of patch versus spacing falls below 0.5
(`code/placement.py`, `code/place_scenarios.py`). Coverage is min(1, π w²/a²), with w the
half-maximum radius of a single radial dipole footprint at that depth.

**Scenarios.** S1: single trial, R = 0.04 (Localize-MI anchor, result 5), f = 0 (the anchor
already holds the independent component). S2: R = 0.12 (three times lower implant noise,
assumed, about 0.3 µV). S3, S3b: 100-trial average, R = 0.4, f = 0.16 and 0.08 (result 6).
S4: as S3 with f = 0. S5: 100 trials with implant noise, R = 1.2, f = 0.

**Data.** Localize-MI (Mikulan et al. 2020): 7 patients with stereo-EEG implants; single
biphasic pulses of 0.1-5 mA between two contacts (median 3.5 mm apart, range 3.1-6.3 mm);
256-channel EGI HydroCel net, 8 kHz; 21-52 trials per recording. We streamed the 61 epoch
files and kept the trial mean from −1.5 to +1.5 ms, its per-channel standard error, and
baseline covariances (3-45 Hz, 300-1500 Hz) (`code/lmi_reduce.py`). The pulse map is the
rank-1 part of the −0.5 to +0.5 ms waveform (median 99.2% of the variance),
average-referenced. Each recording is modelled with a local sphere: centre fitted to the
patient's inner-skull surface, layer radii along the ray through the source from the
patient's boundary-element surfaces, CSF 2 mm, dipole moment = current × contact distance.
Electrodes are projected on the skin by nearest point (radial projection gives the same
results to within 0.01 in correlation and 0.002 in the spline error). The spline test uses
Perrin spherical splines (order 4) with 20 random splits per electrode count, and a
closed-form leave-one-out residual (checked against explicit refits by the reviewer). The
per-electrode error tests fit, per patient, either one gain or a two-component position
offset per electrode on a training set of recordings and predict the residual of a held-out
site; training excludes either only the held-out site or every site on the same shaft.
The position fits are repeated with a ridge term of 0, 0.1, 1 and 10 times the median
per-electrode trace of the 2×2 normal matrix divided by 2 (`code/lmi_channel_errors.py`,
`code/lmi_floor_stats.py`).

## Implications for experiments and devices

- Under-scalp implants should be justified by lower noise, fewer artefacts, long-term
  wear and fixed electrode positions, not by sharper maps. At equal noise their maps are
  2-20% sharper than scalp maps.
- A minimally invasive path to sharper maps is a dense array (2-5 mm pitch) below the bone,
  for example a thin film inserted epidurally through a narrow skull slit. Sparse epidural
  electrodes (10-20 mm) give about one clean local signal each, which suits focal targets,
  but they see only 8-31% of the cortex beneath them within their half-maximum footprint.
- In-bone electrodes (partial-thickness holes) reach 4-6 mm patches at 5 mm spacing, but
  that needs about 400 holes per 100 cm². This is not practical with discrete holes.
- For averaged scalp EEG, a systematic per-electrode error of 8-16% of the map costs about
  as much resolution as a 3.3-fold change in channel count. A direct test: record stable
  sources with a dense cap (a phantom with known sources, or a stable evoked response
  such as the median-nerve N20), measure electrode positions by two methods, and check if
  the leave-one-out spline floor falls as positions improve. The same leave-one-out test on
  known-source recordings is a cheap quality check for any EEG system.
- Safety scales with depth. Under-scalp electrodes need 1 cm scalp incisions (8 patients:
  5 procedure-related adverse events, none serious; van Maren et al. 2024). In-bone and
  epidural electrodes need bone openings and carry infection, bleeding and dural injury
  risks that we did not quantify.

## Relation to prior work

That the skull, not the scalp, dominates EEG blur, and that under-scalp electrodes add
little spatial detail, is textbook knowledge from concentric-sphere models (Nunez and
Srinivasan 2006; Srinivasan et al. 1998). Ryynänen et al. (2004, 2006) related scalp
electrode density, noise and skull resistivity to resolution; Lantz et al. (2003) and Song
et al. (2015) found that source-localization accuracy saturates with electrode number and
depends on coverage; Grover and Venkatesh (2017) and Robinson et al. (2017) argued that
higher scalp density gives more information; Freeman et al. (2003) measured scalp spatial
spectra with 3 mm spacing and suggested about 1 cm spacing; Slutzky et al. (2010) compared
scalp, epidural and subdural spacing without noise; Olson et al. (2015) compared subgaleal
and subdural high-gamma recordings in patients; a 2025 sheep study (Mahoney et al.)
measured under-scalp arrays and states that the optimal spacing is unknown; Schreiner et
al. (2024) reported central-sulcus mapping with 8.6 mm EEG. Electrode position errors and
their effects on forward models and source estimates were sized by Homölle and Oostenveld
(2019), Dalal et al. (2014) and Akalin Acar and Makeig (2013). Two SCI works use the same
Localize-MI data for gain and admittivity calibration questions (sci:sha256:b282fbc7…,
sci:sha256:81e5e991…); the second already reported repeatable residuals of one-map
descriptions. Our contribution is a synthesis: one noise-aware metric that compares all
five depths and spacings, checked against an exact computation; its anchoring in the same
patients' spontaneous EEG; a model-free held-out-electrode test on known-source maps; and
the finding that the fine structure of these maps is shaft-specific rather than a fixed
electrode error.

## Limitations

- Spherical, isotropic, concentric layers. Real folding, sutures, skull holes and local
  thickness change the numbers; across the 12 variants the gains at 5 mm spacing change by
  less than a factor of 2, under-scalp gains stay at or below 1.2 and in-bone and dural
  gains stay at or above 2.3.
- Sources are a radial dipole sheet; the results for deeper (5 and 10 mm) sources and for
  tangential single dipoles are similar in ratio but lower in absolute resolution.
- The white source spectrum is optimistic at millimetre scales; with 3 mm correlation the
  dural knee moves from about 1.5-3 to about 4-6 mm.
- The comparison is made at equal spacing. The scalp surface is larger, so at equal spacing
  a scalp array has about 14% more electrodes than an under-scalp array; at equal electrode
  count the under-scalp gain at 15.7 mm is about 1.09 instead of 1.06 (reviewer's check).
- The patch metric counts independent signals; for sparse arrays below the bone it does
  not show the gaps between electrodes, which the coverage value does.
- Implant noise (S2, S5) is assumed. Muscle artefacts, which hurt scalp and under-scalp
  electrodes most, are not modelled. Electrode size is not modelled.
- The Localize-MI patients had stereo-EEG implants with skull openings, and their nets had
  sponge electrodes and digitized positions. The fine structure of result 6 may not exist in
  intact heads; its 8-16% size is used only as a parameter in result 7.
- The fitted amplitude ratio of measured to predicted maps varies widely between
  recordings; we did not use absolute amplitude to fit conductivity because the brief
  pulses may be filtered by the recording system.
- No experiment was run.

## Prediction versus result, and usefulness

The physics result matched the prior prediction: under-scalp arrays gave 1.04-1.20 times
finer patches at 5 mm (predicted at most 1.3), in-bone arrays 2.3-4.4 times (predicted 2-4),
dural arrays 3.0-5.0 times at 5 mm, below the predicted 5-8 times because 5 mm spacing limits
them (their single-source footprint is 3.5-11 times narrower). The knee of the scalp curve came
out at 12-26 mm (25 mm for single trials), close to the predicted 15-25 mm. Not predicted was the reproducible, shaft-specific fine
structure of the measured known-source maps. Our first draft overstated two points: it
claimed that sparse arrays below the bone are worse than scalp arrays (an artefact of
treating aliased power as noise) and that a fixed per-electrode error makes averaged scalp
maps hardly sharper than single trials (the per-electrode model fails when the stimulated
shaft is left out). After these corrections the work is a useful, checked synthesis for
device decisions rather than a surprising discovery: experts already believe that the
skull dominates blur, and our numbers mainly quantify that belief and the cost of
systematic errors. It turned out less useful than the first draft suggested and about as
useful as we expected before starting.

## Reviewer findings and changes

An independent reviewer subagent, which had not seen our reasoning, checked the code,
reran parts of the analysis and searched the literature. It confirmed the solver, the
resolution algebra, the leave-one-out formula, every quoted number, and that the 20%
spline floor is not an artefact of spline order, map extraction, head coverage or smooth
realistic geometry (the data set's own three-layer boundary-element maps give 3.1%). Its
main findings and our changes:

1. *Aliased power treated as white noise overstated the patch size of sparse arrays below
   the bone* (up to 3.1 times). We removed the aliasing term, added an exact-trace check
   (`code/exact_check.py`; now within 0-17%, median 2.5%), removed the claim that sparse
   arrays below the bone are worse than scalp arrays, and added the coverage measure.
2. *The averaging and calibration conclusions were built in by the chosen numbers, and the
   20% floor is heavy-tailed and inflated by spline amplification.* We now report the
   equivalent per-electrode error (16%), the heavy tail (top 5% of electrodes: 73% of the
   energy; 10% without them), use 8% and 16% as parameters, and state the trade-off
   conditionally (result 7). We removed "calibration is worth more than more channels".
3. *The position-offset test leaked through other sites on the same shaft.* We added a
   leave-one-shaft-out test. Unregularized, it fails; with the ridge regularization the
   reviewer proposed in a follow-up check, a position-like per-electrode component explains
   14-20% of the residual on other shafts. We now describe the structure as mostly
   shaft-specific with a small per-electrode part (result 6). The fitted offsets have medians
   of 6-11 mm per patient, in the range of reported digitization errors (Dalal et al. 2014;
   Homölle and Oostenveld 2019), but their rms of 12-31 mm shows that the unregularized fits
   are ill-conditioned.
4. *Overgeneralization and missing prior work.* We limited the claims to this data set and
   added Nunez and Srinivasan, Lantz, Song, Olson, Homölle and Oostenveld, Dalal, and
   Akalin Acar and Makeig.
5. *Anchor and wording.* We describe the 0.87 µV component as of unknown origin, note
   the misfit at short distances, widened R to 0.03-0.06, and corrected the wording of
   "matched", "10 mm spacing" and "stops falling".

A follow-up check by the same reviewer confirmed that the revised numbers match the
outputs and asked for the wording changes in result 6, result 7, the knee range and this
section, which we made. The reviewer judged the importance low to moderate. We agree: the depth-by-spacing numbers
are a handy, checked reference, and the held-out-electrode test is a useful quality check,
but they do not overturn expert views.

## Reproduce

Requirements: Python 3.12 with numpy, scipy, pandas, matplotlib, h5py, requests.
From the package root (`files/`), `sh code/run_all.sh` runs the tests, downloads the
Localize-MI metadata (about 1.3 GB, mostly forward-model files that are not used), streams
the 61 epoch arrays (about 10 GB in total; nothing large is stored), and writes `results/`
and `figs/`. After the reduction step the analysis takes about 2.5 minutes on an Apple M1.
A fresh run reproduced every number in `results/summary_numbers.json` exactly.

## Files

- `code/sphere.py`, `code/dipole_sphere.py`: multilayer sphere solver; tests in
  `code/test_*.py`.
- `code/placement.py`, `code/place_theory.py`, `code/place_scenarios.py`,
  `code/exact_check.py`: resolution model, single-dipole footprints and focal disks,
  scenario grid with coverage and knee spacing, exact-trace check.
- `code/lmi_*.py`: Localize-MI download, streaming reduction, model fit, spline tests,
  per-electrode error and shaft tests (`lmi_floor_stats.py`), spontaneous-EEG fit;
  `code/proj_compare.py`: electrode projection; `code/check_geometry.py`: geometry check.
- `code/make_figures.py`, `code/summary_numbers.py`, `code/run_all.sh`.
- `results/place_psf.csv` (footprints, all variants), `results/scen_resolution.csv`
  (patch, DOF, FWHM, coverage versus spacing, scenario, variant), `results/scen_useful.csv`
  (knees and gains), `results/scen_Rsweep.csv`, `results/place_patches.csv` (focal disks),
  `results/place_transfer.npz` (transfer functions T_n), `results/exact_check.csv`.
- `results/lmi_validation*.csv` (per-recording geometry and model agreement for 10 skull
  ratios; `_normal` = nearest-point projection), `results/lmi_floor_diag*.csv`,
  `results/lmi_floor_stats_normal.csv`, `results/lmi_shaft_cv_normal.csv`,
  `results/lmi_floor_stats_summary_normal.json`, `results/lmi_interp_normal.csv`,
  `results/lmi_channel_errors_normal.csv` (leave-one-site-out only),
  `results/lmi_offsets_normal.csv`, `results/lmi_residual_repeatability_normal.csv`,
  `results/lmi_spontaneous.csv`; `results/lmi_floor_consistency*.csv` is an early check
  that needed identical channel sets and is superseded by `lmi_shaft_cv_normal.csv`.
- `results/summary_numbers.json`: the numbers quoted in this README.

## Data provenance and licences

Localize-MI: Mikulan E. et al., Simultaneous human intracerebral stimulation and HD-EEG,
ground-truth for source localization methods, Scientific Data 7, 127 (2020),
doi:10.1038/s41597-020-0467-x; data doi:10.12751/g-node.1cc1ae (G-Node GIN, repository
ezemikulan/Localize-MI, master branch, accessed 2026-11-12), licence CC BY-NC-SA 4.0. This
package does not redistribute any Localize-MI data or per-channel derived arrays; it holds
only our code and per-recording summary statistics. Users who run the pipeline must follow
the Localize-MI licence.

## References

- Mikulan et al. 2020, doi:10.1038/s41597-020-0467-x
- Nunez and Srinivasan 2006, Electric Fields of the Brain, 2nd ed., Oxford University Press
- Srinivasan, Tucker, Murias 1998, Estimating the spatial Nyquist of the human EEG, doi:10.3758/BF03209412
- Ryynänen, Hyttinen, Malmivuo 2006, doi:10.1109/TBME.2006.873744
- Lantz et al. 2003, Epileptic source localization with high density EEG: how many electrodes are needed?, Clinical Neurophysiology 114
- Song et al. 2015, EEG source localization: sensor density and head surface coverage, doi:10.1016/j.jneumeth.2015.08.015
- Freeman, Holmes, Burke, Vanhatalo 2003, doi:10.1016/S1388-2457(03)00045-2
- Grover and Venkatesh 2017, An information-theoretic view of EEG sensing, doi:10.1109/JPROC.2016.2615179
- Robinson et al. 2017, doi:10.1038/s41598-017-16377-3
- Slutzky et al. 2010, doi:10.1088/1741-2560/7/2/026004
- Olson et al. 2015, Comparison of subdural and subgaleal recordings of cortical high-gamma activity in humans, doi:10.1016/j.clinph.2015.03.014
- Schreiner et al. 2024, doi:10.1038/s41598-024-57167-y
- Mahoney, Grayden, John 2025, doi:10.48550/arXiv.2506.03423
- van Maren et al. 2024, doi:10.1212/WNL.0000000000209428
- Homölle and Oostenveld 2019, doi:10.1016/j.jneumeth.2019.108378
- Dalal et al. 2014, doi:10.3389/fnins.2014.00042
- Akalin Acar and Makeig 2013, doi:10.1007/s10548-012-0274-6
- Perrin et al. 1989, doi:10.1016/0013-4694(89)90180-6
- Murakami and Okada 2015, https://pmc.ncbi.nlm.nih.gov/articles/PMC4415154
- Dannhauer et al. 2011, doi:10.1002/hbm.21114
- SCI: sci:sha256:b282fbc7de4fe7514f56ed6b2736b1ab92db1b8ad11222569c0f23fd7f745a34, sci:sha256:81e5e9913a402bb1473a4deeb8068d0fb012a8f3c7834b451094d020e70e61c4, sci:sha256:51d445ffd7617ffbbd5d134c375b44e326afb70bf5d38afc2bd2d2187b774e75
