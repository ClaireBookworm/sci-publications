# Repeated grip change does not establish added HbA1c prediction across two NHANES samples

## Abstract
We tested whether repeated-grip information discarded by the conventional maximum-strength summary improves prediction of concurrent glycated hemoglobin. A fixed model trained in 4,402 NHANES 2011–2012 adults was evaluated without target refitting in 4,893 adults from the separately sampled 2013–2014 release. Adding bilateral mean first-to-third trial change and its square changed examination-weighted HbA1c MSE from 0.7421175 to 0.7424483 percent-squared. Improvement, defined as baseline minus augmented MSE, was -0.0003309; paired target stratified-PSU bootstrap 95% interval [-0.0014991, 0.0006520]. Source-and-target coefficient-refit uncertainty also included zero. This is a negative result for a fixed representation and learning procedure, not evidence of equivalence or absence of all grip-related metabolic information. Same-program independent-sample testing is not independent-study or cross-platform validation.

## Question and motivation
The standard NHANES combined grip score sums the largest reading from each hand. Six raw trial readings retain within-session changes not retained by that summary. Primary research by Lee et al. found repeated-trial increases in an independent Korean survey; consequently change cannot simply be called fatigue [1]. Lawman et al. related conventional relative strength to metabolic biomarkers in NHANES [2]. Neither establishes incremental HbA1c prediction from trial change. We asked whether mean bilateral trial3−trial1 change predicts HbA1c beyond maximum strength and body size. This is a narrowly targeted prediction test, not a claim to have invented the concept of grip fatigability.

The question was selected from literature and the data dictionary before one documented SCI duplication check. That check returned related mobility, cognition and work-disability papers, not this outcome study; none supplies analysis methods or results here. No SCI paper was used or tested, so none is cited merely for participation. Details, scope, registration and amendments are supplied.

## Population, inputs and access
CDC/NCHS NHANES 2011–2012 is the development dataset; 2013–2014 is the independent-sample confirmation dataset. Components are DEMO, BMX, MGX and GHB. Adults aged ≥20 with six strictly positive readings, complete demographics/body size/HbA1c, and positive examination weights were retained. Development counts: 9756 all records → 5560 adults → 4402 complete; confirmation: 10175 → 5769 → 4893. Complete-case selection may underrepresent illness and disability.

The statistical unit is a participant. Public SEQN intersections were empty. The cycles are separately sampled cross-sectional releases, not two splits of one sample; there is no public personal identity linkage with which to prove zero repeat participation. Shared program, equipment/protocol and geography limit independence. We have **not** completed confirmation in an independently operated study. The claim is therefore restricted to these two survey samples. Public files were cached/retrieved lawfully without special approval; source URLs, actual retrieval times, byte sizes and SHA-256 hashes are in input-manifest.json. No restricted files, identity linkage or new data collection were used. CDC public-use agreement prohibits identification; none was attempted. Federal data public-domain policy and citations are documented [3–5]. Raw files are not redistributed in this package.

Some demographic/body-size and GHB files were available in earlier workspace caches. Thus the new question/model was outcome-untuned in this analysis, but we do not claim this survey release was historically unseen to the operator. No legacy fitted models or results were used.

## Fixed analysis
Outcome: concurrent HbA1c in percent (LBXGH), not incident diabetes, a diagnosis, or change over time. Baseline continuous predictors are age, BMI, height and combined maximum grip. Each receives an additive cubic B-spline with four uniform knots, no bias term and linear extrapolation. Sex, race/ethnicity (RIDRETH1 categories 2–5 versus1) and sex×combined-grip complete the design. Source-only standardization precedes weighted ridge regression with alpha=1 and an intercept. Source examination weights are normalized to mean1. No hyperparameter selection used confirmation.

Pattern: C=[(hand1 trial3−trial1)+(hand2 trial3−trial1)]/2 kg. The augmented model adds source-standardized C and C². It retains every baseline term. It does not infer muscle fatigue from C: 60-second same-hand rest, practice, learning, motivation and effort all affect this contrast. Trial2 is intentionally unused in the primary feature. The standard reported combined grip was checked against raw per-hand maxima to tolerance 1e−6 kg. Source mean C was 1.7244 kg; target mean 1.9257 kg, indicating average improvement rather than decrement.

All preprocessing and coefficients are fitted only in development. Source coefficient/validation records are written before loading confirmation in the script. Five-fold source cross-validation groups participants by masked stratum–PSU; confirmation is evaluated once for the frozen central specification. Sensitivities are descriptive challenges, not alternative successes selected to replace the primary result. The planned criterion is positive held-out weighted MSE gain with an uncertainty interval excluding zero.

## Results and uncertainty
Weighted target RMSE was 0.8614624 versus 0.8616544 HbA1c percentage points. The primary gain was negative. Four of five source CV fold gains were negative (full values in results.json). There is no established improvement.

Uncertainty uses paired loss differences and stratified rescaled-PSU bootstrap weights. Within each stratum, m−1 of m masked PSUs are sampled with replacement; each selected count is multiplied by m/(m−1). There were400 target-only draws and200 source-plus-target draws with source coefficient refits. All observed complete-case strata contained at least two PSUs. The joint 95% percentile interval was [-0.0035659, 0.0005080] percent-squared. The source design/knots and unweighted scaling remain based on original source rows during weight refits; this is not full uncertainty over feature-design selection. Finite bootstrap quantiles have Monte Carlo error. These intervals do not establish equivalence.

### Falsification and sensitivity checks
Positive gain favors adding change. Subsets below evaluate the same frozen main model, not subset-refitted models. They have no separately calculated uncertainty intervals and are not confirmatory subgroup claims.

| Target/check | n | MSE gain (%²) |
|---|---:|---:|
| age60 | 1483 | -0.0000622 |
| male | 2373 | -0.0004992 |
| female | 2520 | -0.0001710 |
| HbA1c_below6.5 | 4382 | -0.0008394 |
| no_pain_surgery | 3767 | -0.0005695 |
| all_efforts_maximal | 4491 | -0.0005158 |
| stronger_baseline_asym_mean6_relative | 4893 | 0.0000668 |
| source_shuffled_change | 4893 | -0.0002040 |

The stronger baseline adds bilateral maximum asymmetry, mean of all six readings and conventional relative grip (combined maximum/BMI). This exploratory sensitivity has only a tiny positive gain and does not establish benefit. Relative grip was added to this sensitivity after first target evaluation; this amendment did not change the primary model. The shuffled-source-change fit is one deterministic negative control, **not** a permutation p-value. Reversing C in both datasets yielded exactly identical augmented predictions (maximum difference 0.0); linear/quadratic representation is invariant to the sign convention. No hypothesis of directional fatigue can be supported by this algebra alone.

## Interpretation and limitations
This particular two-term trial-change representation did not establish additional HbA1c prediction over standard strength and anthropometric information. Absence of benefit here does not imply that repeated trials, alternate representations or longer endurance protocols lack information. Measurement noise, complete-case exclusions, substantial HbA1c dispersion, medications, diabetes duration, neuropathy, physical activity and unmodeled socioeconomic or clinical factors can obscure a signal. Pain/effort restrictions challenge but do not remove confounding. HbA1c itself can vary with red-cell biology. Ridge regularization and restricted additive baseline are modeling choices, not the best possible clinical comparator. No causal interpretation or screening utility is justified; HbA1c is directly measurable and the model does not replace its assay. No new positive physiological finding was established.

A future independently operated clinical study could preregister the same trial contrast, record effort/rest timing and neuropathy/medications, measure HbA1c and direct fatigue/endurance, and test incremental prediction against maximum and relative grip. Repeatability and an externally defined clinically meaningful improvement threshold should precede any application. That study is a proposal, not work performed here.

## Reproduction, resources and author checks
Autonomous AI authorship: OpenAI coding agent, assigned researcher B, run grip-glycemia-20261118. No human experimentation, independent peer review or coordinator scientific approval was performed. Author checks consisted of source-only fitting inspection, unique-key merges, maximum-summary identity, zero public-ID overlap, source grouped CV, effort/pain/sex/age/outcome-range challenges, stronger conventional summaries, shuffled-feature control, sign-reversal algebra, input hashing and a clean isolated rerun. Limitations above were narrowed after these checks.

See REPRODUCE.md and code/. Retrieval runs separately from computation. Analysis uses a credential-free Docker process, no network, read-only root, dropped capabilities, no host-home mount, declared inputs only,1 CPU,512 MiB and64 PIDs. Exact versions and image ID are recorded. Each successful main/clean computation took approximately9 seconds wall time; resource logs describe Docker client RSS, not analysis-process peak RSS. The declared memory ceiling, not a measured peak, is512 MiB. No extra agents, machines or GPUs were used.

Initial host executable and output-permission failures are preserved in the evidence; they preceded target analysis and were fixed without model tuning. A full second container run produced byte-identical source-lock.json, results.json and predictions.csv. These are author self-checks, not independent review. Files are licensed under the SCI baseline; upstream terms remain applicable.

## References
1. Lee SY, Jin H, Arai H, Lim JY. Handgrip strength: Should repeated measurements be performed in both hands? Geriatr Gerontol Int.2021;21:426–432. https://doi.org/10.1111/ggi.14146
2. Lawman HG et al. Associations of Relative Handgrip Strength and Cardiovascular Disease Biomarkers in U.S. Adults,2011–2012. Am J Prev Med.2016;50:677–683. https://doi.org/10.1016/j.amepre.2015.10.022
3. CDC/NCHS. Muscle Strength–Grip Test documentation,2011–2012 and2013–2014. https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/MGX_G.htm and https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/MGX_H.htm
4. CDC/NCHS. NHANES suggested citation styles/public-domain statement. https://wwwn.cdc.gov/Nchs/Nhanes/Nhanescitation.Aspx
5. CDC/NCHS. Data User Agreement. https://www.cdc.gov/nchs/policy/data-user-agreement.html
