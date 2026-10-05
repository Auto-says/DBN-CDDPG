# DBN-CDDPG Reproducibility Repository

This repository accompanies the manuscript:

**Dynamic Bayesian Inference-Driven Safe Human-Robot Collaborative Disassembly Planning for Retired Power Batteries**

The repository is organized to make the simulation protocol, seed-level outputs, checkpoint sources, disturbance records, ablations, and manuscript results traceable. This README follows the **final revised manuscript numbering**: the main text contains **Tables 3–17**, and Appendix C contains **Tables C1–C6**.

---

## 1. Repository contents

The released materials are organized around the following packages:

```text
DBN-CDDPG repository
├── README.md
├── DBN_CDDPG_Source_Code.zip
├── Supplementary_Data_and_Results.zip
├── nominal_completed_seed_checkpoints.zip
├── ablation_completed_seed_checkpoints.zip
└── matched_full_m1_HE1_HE4_evaluation.zip
```

### `DBN_CDDPG_Source_Code.zip`

Contains the implementation used for the event-driven DBN-CDDPG framework and the benchmark/diagnostic experiments. The implementation covers:

- event-state refresh and task-frontier updates;
- Stage-I task typing and the locked Type-II boundary;
- event-wise AHD and fatigue updates;
- DBN posterior updating for currently flexible Type-III tasks;
- continuous task-mode-executor preference scoring;
- hard feasible-domain projection;
- four-critic cost/performance learning and common-descent coordination;
- benchmark baselines and experiment-level evaluation utilities.

Principal files in the current source archive are:

```text
DBN_CDDPG_Source_Code/
├── dbn_cddpg_main_engineering_dynamicAHD_retyping_final.py
├── experiment_common.py
├── run_fourpanel_performance_10000.py
├── run_table7_formal.py
├── run_table8_formal.py
├── run_static_vs_event_HE1_HE4.py
├── run_projection_diagnosis_2000.py
├── train_full_m1.py
├── evaluate_full_m1.py
├── analyze_full_m1.py
├── full_m1_support.py
├── check_environment.py
├── requirements.txt
├── requirements-matched-evaluation.txt
├── SOURCE_PROVENANCE.json
├── MATCHED_FULL_M1_PROVENANCE.json
├── RECORDED_EVALUATION_ENVIRONMENT.json
├── UPSTREAM_SHA256.json
├── CHECKSUMS.sha256
├── data/
│   └── case2_22tasks_dynamicAHD_retyping.xlsx
├── commands/
│   ├── run_nominal_10000.sh / .bat
│   ├── run_disturbance_10000.sh / .bat
│   ├── run_ablation_10000.sh / .bat
│   ├── run_projection_diagnosis_2000.sh / .bat
│   ├── run_static_vs_event_10000.sh / .bat
│   ├── run_matched_full_m1_train_10000.sh / .bat
│   ├── run_matched_full_m1_HE1_HE4_eval.sh / .bat
│   ├── run_matched_full_m1_analysis.sh / .bat
│   └── run_smoke_cpu.sh / .bat
└── validation/
    └── verify_source.py
```

The shell/batch files in `commands/` are the recommended entry points for the experiment families represented by the core source archive.

**Important:** the physical scheduling action is discrete even when a DDPG-family actor produces continuous preference scores. Actor output is projected onto the current legal task-mode-binding domain before execution.

### `Supplementary_Data_and_Results.zip`

Contains task-level inputs, protocol/configuration records, seed-level outputs, disturbance records, robustness-analysis results, event traces, and statistical outputs used to construct the manuscript tables and figures.

The supplementary results are the main evidence location for:

- Case 1 and Case 2 task inputs;
- processing-time provenance records;
- AHD robustness analysis;
- DBN posterior perturbation analysis;
- nominal seed-level summaries;
- HE-1–HE-4 disturbance evaluation;
- static-versus-event-driven matched records;
- mechanism-ablation outputs;
- representative Case-1 projection traces;
- projection-integrity diagnostic outputs.

The matched Full/M1 extension is released separately in `matched_full_m1_HE1_HE4_evaluation.zip`. The final source package also includes dedicated Full/M1 training, saved-actor evaluation, and analysis runners for this extension.

### `nominal_completed_seed_checkpoints.zip`

Contains completed-seed checkpoint/result records for the nominal six-method benchmark:

- Joint masked DDQN;
- DBN-guided DDQN;
- Projected DDPG;
- Lagrangian DDPG;
- PPO-Lagrangian;
- DBN-CDDPG.

These records trace the nominal Case-2 benchmark in **Tables 8–9** and **Figs. 7–8**. They are JSON checkpoint/result records rather than reloadable PyTorch actor-weight files.

### `ablation_completed_seed_checkpoints.zip`

Contains completed-seed checkpoint/result records for the independent mechanism-ablation suite:

- Full DBN-CDDPG;
- M1: No DBN inference;
- M2: No DBN guidance;
- M3: No dynamic AHD;
- M4: Fixed penalty;
- M5: Reward mixing.

This suite uses its own internally matched Full reference and supports **Table 14** and **Fig. 10(a)–(b)**. The archive contains JSON checkpoint/result records rather than reloadable PyTorch actor-weight files.

For Section 5.6, the final source package provides `train_full_m1.py` to generate reloadable Full/M1 actor weights under the matched training protocol, `evaluate_full_m1.py` to evaluate those saved actors under nominal, HE-1, and HE-4 conditions, and `analyze_full_m1.py` to reproduce the paired inference and same-state diagnostic summaries. The released `matched_full_m1_HE1_HE4_evaluation.zip` contains the archived evaluation outputs. The 20 nominal evaluations reproduce the mechanism-suite seed-level outcomes and are **not additional independent nominal samples**.

The **P0/P1 projection-integrity diagnosis** is a separate dataset and must not be pooled with the complete-schedule ablation suite.


### `matched_full_m1_HE1_HE4_evaluation.zip`

Contains the archived outputs from the matched Full/M1 evaluation reported in Section 5.6:

- 10 matched seeds for Full and M1;
- nominal, HE-1, and HE-4 evaluation for each saved actor;
- **60 complete trajectories** in total;
- `evaluation_manifest.json`;
- `evaluation_runs.csv`;
- `evaluation_summary.csv`;
- per-trajectory `schedule.csv`, `dbn_trace.csv`, `dynamic_ahd_trace.csv`, `task_type_trace.csv`, `event_decisions.csv`, `result.json`, and `metrics_raw.json`.

These outputs support **Tables 15–17** and **Appendix Table C6**. The evaluation introduces no learning, parameter tuning, or checkpoint reselection. The corresponding evaluation and analysis logic is provided in `evaluate_full_m1.py` and `analyze_full_m1.py` in the final source package.


---

## 2. Final manuscript table and figure mapping

| Manuscript item | Content | Repository evidence |
|---|---|---|
| Table 3 | Definitions of metrics and experimental configurations used in Section 5 | Manuscript definitions; evaluation/protocol files |
| Table 4 | Algorithm operating parameters | Source-code configuration and protocol records |
| Table 5 | Robustness of dynamic AHD ranking under input perturbation | AHD Monte-Carlo robustness outputs in supplementary results |
| Table 6 | Initial task-type distribution and Type-III subset used for DBN inference | Case-1/Case-2 task inputs |
| Table 7 | Robustness of DBN posterior beliefs under joint parameter perturbation | DBN perturbation outputs; Appendix Table C4 provides the family-specific detail |
| Table 8 | Nominal Case-2 scheduling and cumulative-budget performance | `run_fourpanel_performance_10000.py`; `commands/run_nominal_10000.*`; nominal checkpoints and seed-level outcomes |
| Table 9 | Paired statistical comparison between baselines and DBN-CDDPG | Seed-level nominal outcomes from the Table-8 benchmark |
| Table 10 | DBN-CDDPG safety response and cumulative-budget feasibility under HE-1–HE-4 | `run_table7_formal.py`; `commands/run_disturbance_10000.*`; disturbance records |
| Table 11 | Nominal static-versus-event-driven execution and event-update activation | `run_static_vs_event_HE1_HE4.py`; nominal matched records |
| Table 12 | Static-versus-event-driven disturbance outcomes under HE-1 and HE-4 | `run_static_vs_event_HE1_HE4.py`; matched HE-1/HE-4 records |
| Table 13 | Event-wise task type, raw mode preference, feasible modes, and executed mode | Controlled Case-1 representative projection trace |
| Table 14 | Mechanism-ablation outcomes under the matched protocol | `run_table8_formal.py`; `commands/run_ablation_10000.*`; ablation checkpoints and seed-level outcomes |
| Table 15 | Matched Full/M1 full-run engineering outcomes under nominal, HE-1, and HE-4 | `evaluate_full_m1.py`; `matched_full_m1_HE1_HE4_evaluation.zip` |
| Table 16 | Matched Full/M1 completion and cumulative-budget feasibility under nominal, HE-1, and HE-4 | Same matched Full/M1 evaluation archive |
| Table 17 | All explicit-guidance action changes in the matched Full trajectories | `event_decisions.csv` generated by `evaluate_full_m1.py` |
| Fig. 7 | Learning dynamics and constraint-feasibility evolution of DBN-CDDPG | Nominal training-history/log outputs |
| Fig. 8 | Nominal scheduling performance and cumulative-budget feasibility | Same nominal results as Table 8 |
| Fig. 9 | Prescribed AHD and mode-belief inputs for the controlled Case-1 projection scenario | Controlled Case-1 representative trace; prescribed belief inputs |
| Fig. 10(a)–(b) | Mechanism-ablation effects and matched-seed paired effects | Same independent ablation suite as Table 14 |
| Fig. 10(c) | P0/P1 projection-integrity diagnosis | `run_projection_diagnosis_2000.py`; separate 2,000-episode diagnostic dataset |
| Appendix Table C4 | Structured DBN posterior perturbation results | DBN parameter-perturbation outputs |
| Appendix Table C5 | Data-learned cross-check of the engineering-prior DBN mode tendency | Learned cross-check outputs |
| Appendix Table C6 | Paired Full–M1 HE-1/HE-4 engineering comparisons with one eight-test Holm family | `analyze_full_m1.py`; matched evaluation archive |

`run_table7_formal.py` and `run_table8_formal.py` retain legacy filenames for source provenance. They map to the **current Table 10** and **current Table 14**, respectively.

---

## 3. Experiment protocols

### 3.1 Nominal six-method benchmark

- Case: Case 2, 22 tasks.
- Algorithms: six methods listed above.
- Training budget: **10,000 episodes** for trainable matched-seed experiments.
- Evaluation: **10 matched random seeds**.
- Checkpoint evaluation: every **50 episodes**.
- Checkpoint selection: **feasibility first**, then primarily makespan-oriented.
- Common hard executable boundary: applied to all six benchmark algorithms.
- Cumulative feasibility limits:
  - Energy: **E ≤ 15.7**
  - AHD: **AHD ≤ 85**
  - Fatigue: **F ≤ 6.0**
- Makespan `Cmax` is optimized and reported directly; it is **not** a feasibility threshold.

The nominal benchmark is the source for **Tables 8–9** and **Figs. 7–8**.

### 3.2 Original four-scenario disturbance evaluation

The trained DBN-CDDPG policies are evaluated under four scenarios:

- **HE-1:** AHD escalation;
- **HE-2:** fatigue shock;
- **HE-3:** robot delay;
- **HE-4:** combined disturbance.

Each of ten trained seeds is evaluated once per scenario, giving **40 complete disturbance trajectories**.

The final manuscript **Table 10** reports:

- post-event AHD;
- relative `Cmax` change;
- Dynamic ABFR.

Overall Dynamic ABFR is **52.5%**. HE-1 is **0%**, HE-2 is **90%**, HE-3 is **100%**, and HE-4 is **20%**. Completion of every local action through the hard feasible domain does not imply trajectory-level cumulative-budget feasibility.

### 3.3 Static-versus-event-driven comparison

The static/event-driven analysis contains:

1. a nominal matched comparison used to confirm whether the event-conditioned interface is activated; and
2. matched disturbance comparisons for **HE-1** and **HE-4**.

This analysis corresponds to **Table 11** (nominal activation) and **Table 12** (HE-1/HE-4 disturbance outcomes).

The results are intentionally interpreted as **scenario-dependent**:

- under HE-1, the static controller has better reported cumulative-budget and makespan outcomes;
- under HE-4, event-driven execution improves post-event AHD, Dynamic ABFR, and NVS, but has higher mean makespan.

### 3.4 Mechanism-ablation suite

The complete-schedule mechanism suite contains six configurations:

- Full;
- M1;
- M2;
- M3;
- M4;
- M5.

Each configuration uses **10 matched seeds** and a **10,000-episode** training budget, producing an independent 60-run package.

This suite corresponds to **Table 14** and **Fig. 10(a)–(b)**.

### 3.5 Projection-integrity diagnosis

P0/P1 is a separate execution-integrity experiment:

- **P0:** hard projection retained;
- **P1:** hard projection removed; invalid raw proposals are rejected/penalized.

This diagnosis uses a separate **2,000-episode** dataset and is **not pooled** with the 10,000-episode nominal benchmark or complete-schedule mechanism-ablation suites.

It corresponds to **Fig. 10(c)**.

The raw pre-projection invalid-proposal rate measures **projection load**, not executed unsafe actions. Incomplete P1 runs are not used for complete-schedule `Cmax` or AHD comparisons.

### 3.6 Matched Full/M1 extension

Section 5.6 evaluates the internally matched Full and M1 actors from the mechanism suite.

Training/checkpoint source:

- seeds: `42,52,62,72,82,92,102,112,122,132`;
- training budget: **10,000 episodes**;
- checkpoint evaluation: every **50 episodes**;
- checkpoint rule: the same nominal feasibility-first selection rule.

Each selected actor is evaluated once under:

1. nominal conditions;
2. HE-1;
3. HE-4.

This gives **60 complete trajectories**:

- 20 nominal evaluations;
- 20 HE-1 evaluations;
- 20 HE-4 evaluations.

The 20 nominal evaluations reproduce the corresponding mechanism-suite seed-level engineering outcomes and selected checkpoint episodes at the recorded precision; they are **not additional independent samples** for the nominal tests.

Full and M1 retain:

- dynamic AHD;
- event-wise retyping;
- the Type-III AHD-threshold gate;
- the same hard task-mode-executor rules;
- hard feasible-domain projection.

M1 disables:

- DBN state input;
- explicit DBN mode-ranking bias.

The matched extension supports:

- **Table 15:** full-run engineering outcomes;
- **Table 16:** completion and cumulative-budget feasibility;
- **Table 17:** explicit-guidance action changes;
- **Appendix Table C6:** paired Full–M1 statistical comparisons.

The extension does **not** establish a consistent disturbance-specific cumulative-performance advantage for the DBN. Full has slightly lower mean makespan but higher mean AHD and energy under both HE-1 and HE-4, without improving Dynamic ABFR.

---

## 4. Section-5 metrics and configuration labels

### ABFR

**All-budget feasible-run rate.**

Percentage of complete nominal runs satisfying all three cumulative limits simultaneously:

```text
E ≤ 15.7
AHD ≤ 85
F ≤ 6.0
```

In the nominal benchmark, energy and fatigue remain comfortably below their limits for all methods, so variation in ABFR is driven primarily by the AHD budget.

### NVS

**Normalized violation severity.**

Sum of normalized positive budget violations. `NVS = 0` indicates no cumulative-budget violation.

### UAPR, SMSR, and SCMR

These are **local disturbance-response diagnostics**, not trajectory-level feasibility measures:

- **UAPR:** unsafe top-ranked raw proposals at affected disturbance decisions;
- **SMSR:** safe-mode responses at affected disturbance decisions;
- **SCMR:** hazard-escalation decisions that do not increase manual-mode preference.

UAPR is **not** the same as the overall pre-projection invalid-proposal rate used in the P0/P1 diagnosis.

### Dynamic ABFR

A **trajectory-level disturbance outcome**: percentage of complete disturbance runs satisfying all three cumulative budgets.

### M1–M5

- **M1:** removes DBN state input and explicit DBN mode-ranking guidance; event-driven updates and hard projection remain.
- **M2:** removes explicit DBN guidance while retaining the remaining configuration.
- **M3:** disables event-wise dynamic-AHD updating **and the associated Type-III AHD-threshold gate**. It is therefore a joint ablation of dynamic AHD updating and AHD-based mode gating, not an isolated temporal-AHD test under an identical hard domain.
- **M4:** fixes the initial constraint-penalty coefficients.
- **M5:** replaces the four-critic/common-descent update with scalar reward mixing.

### P0/P1

- **P0:** hard projection retained.
- **P1:** hard projection removed; invalid raw proposals are rejected/penalized.

---

## 5. Processing-time provenance

For all 22 Case-2 tasks, the base processing times for:

- human execution;
- robot execution;
- HRC execution

are adopted directly from **Ref. [11]** in the manuscript.

These 66 task-mode values are literature-derived simulation inputs. They are **not** independently measured or predicted processing times, and agreement with the source is **not** treated as an independent timing-validation result.

---

## 6. Dynamic-AHD and DBN implementation notes

### Dynamic AHD

The multiplicative formulation in Eqs. (4)–(6) is the reference risk-priority construction.

The scheduling experiments initialize task-level baseline AHD from the supplied case-study inputs. For unfinished non-Type-II tasks in Full, the nominal online update is a bounded engineering modulation of those baseline values as specified in Eqs. (7)–(8). The online routine does **not** recompute the discrete likelihood level from Eqs. (5)–(6) at every event.

Type-II tasks retain their baseline AHD and the Stage-I Type-II boundary remains fixed.

### DBN input scaling

For the Type-III DBN evidence scores:

- AHD is divided by **625** and clipped to `[0,1]`;
- hazard is normalized as defined by the implementation;
- the fatigue component is the **unscaled mean cumulative human-fatigue state**.

The fatigue input is **not divided by 6.0** before entering the DBN scores. The value `6.0` is the cumulative fatigue budget and the denominator used for fatigue-cost normalization.

### DBN legality boundary

DBN posterior beliefs provide **soft ranking guidance** only. Engineering, safety, precedence, resource, and binding constraints determine the legal task-mode-binding domain independently.

---

## 7. Cross-protocol reproducibility note

The nominal-benchmark DBN-CDDPG run and the Full reference used by the independent mechanism suite were trained and checkpointed separately under the same substantive Case-2 setup, the same ten matched seeds, and the same 10,000-episode budget.

Their reported mean cumulative AHD values are:

- nominal benchmark DBN-CDDPG: **71.08**;
- independent Full reference: **66.35**;
- difference: approximately **4.72 AHD units**, or about **7.1%** relative to the latter value.

Seed-level inspection shows that this is not a uniform shift in the AHD calculation; most of the aggregate difference is concentrated in a small subset of seeds.

Checkpoint selection is feasibility-first and then primarily makespan-oriented. Once all three budgets are satisfied, AHD is not independently minimized among already feasible checkpoints. Independently trained policies can therefore select different feasible schedules.

The original experiments fixed Python, NumPy, PyTorch, and CUDA pseudorandom seeds, but **fully deterministic CUDA execution was not enforced**, and the original dependency specification did not pin every package version.

For this reason, the approximately **4.72-AHD-unit difference is treated as an empirical cross-protocol reproducibility scale**. Smaller effects are interpreted cautiously when they compare independently trained suites.

The later matched Full/M1 evaluation does not remove or reinterpret this cross-protocol scale; it is a separate saved-actor evaluation.

---

## 8. Statistical analysis conventions

### 8.1 Nominal benchmark: Table 9

The nominal paired comparisons use:

- 10 matched seeds;
- two-sided paired Wilcoxon signed-rank tests;
- SciPy's default **Wilcox** zero-difference convention;
- Hedges' `gav` effect sizes;
- 95% t-based intervals for seed means where reported.

For each named baseline-versus-DBN-CDDPG contrast, Holm adjustment is applied across the four engineering outcomes:

- `Cmax`;
- AHD;
- fatigue;
- energy.

This is a **contrast-specific family of four tests**, not one omnibus family across all baselines.

### 8.2 Mechanism-ablation multiplicity sensitivity

The mechanism analysis retains the originally released within-contrast correction and additionally reports an experiment-wide Holm sensitivity correction across:

```text
5 ablations × 4 engineering metrics = 20 tests
```

Under the 20-test correction, the M3 fatigue comparison is not treated as a statistically robust experiment-wide effect.

### 8.3 Matched Full/M1 HE-1/HE-4 inference: Appendix Table C6

Table C6 pairs Full and M1 by training seed for:

- `Cmax`;
- full-run AHD;
- execution-fatigue cost;
- energy

under HE-1 and HE-4.

The paired-test convention is:

- differences are `Full − M1`;
- lower values are preferable for all four engineering outcomes;
- negative differences therefore favor Full, and positive differences favor M1;
- exported engineering outcomes are rounded to four decimal places before ranking;
- zero differences are removed from the signed-rank calculation;
- tied absolute differences receive average ranks;
- two-sided exact Wilcoxon signed-rank probabilities are obtained by enumerating all sign assignments of the nonzero pairs;
- Holm adjustment is applied once across the complete **eight-test family**;
- the two-sided 95% Student's t confidence intervals describe paired mean differences using **all ten matched pairs** and are **not simultaneous confidence intervals**.

ABFR, NVS, and event counts are reported descriptively and are not additional significance claims.

Across the eight engineering comparisons, **none is significant after Holm correction** (`pHolm ≥ 0.75`).

Non-significance is not interpreted as equivalence in any experiment suite.

---

## 9. Hardware and execution environments

### 9.1 Original trainable matched-seed experiments

The manuscript reports the following workstation for the trainable matched-seed experiments:

- CPU: **Intel Core i5-14600KF @ 3.50 GHz**
- GPU: **NVIDIA GeForce RTX 5060 Ti, 16 GB VRAM**
- RAM: **32 GB @ 5600 MT/s**
- system-reported CUDA environment: **13.2**
- Python: **3.13**

The original runs were seed-controlled but not fully deterministic at the CUDA execution level, and the original dependency specification did not pin every package version.

### 9.2 Peer-review source-package requirements

The current source archive pins the following environment for repository reruns:

```text
numpy==2.1.2
pandas==2.3.1
torch==2.7.1+cu128
scipy==1.16.0
openpyxl==3.1.5
```

These versions document the **released rerun environment**. They do not retroactively identify the exact package versions used in every original training execution.

The `+cu128` suffix is the CUDA build of the PyTorch wheel and should not be rewritten as `+cu132`.

### 9.3 Matched Full/M1 evaluation manifest

The Section-5.6 saved-actor evaluation records:

- OS: **Windows 11**
- Python: **3.13.5**
- PyTorch: **2.8.0+cu129** (CUDA build 12.9)
- NumPy: **2.1.3**
- pandas: **2.2.3**
- SciPy: **1.15.3**
- openpyxl: **3.1.5**
- GPU: **NVIDIA GeForce RTX 5060 Ti**
- PyTorch intra-operation threads: **1**
- deterministic algorithms: **disabled**

This manifest identifies the later matched evaluation environment separately from the historical training environment. Evaluation introduced **no learning, parameter tuning, or checkpoint reselection**.

---

## 10. Key implementation settings

| Setting | Value |
|---|---:|
| Maximum training episodes | 10,000 |
| Replay-buffer capacity | 50,000 |
| Mini-batch size | 64 |
| Hidden dimension | 128 |
| Actor learning rate | 1×10^-4 |
| Critic learning rate | 3×10^-4 |
| Discount factor `γ` | 1.0 |
| Soft-update coefficient `τ` | 0.01 |
| Network-update interval | 4 interactions |
| Exploration noise | 0.30 → 0.05 over first 80% of training |
| Projection temperature | 0.25 |
| Actor backward temperature | 1.0 |
| Posterior-guidance coefficient `κβ` | 0.35 |
| Initial constraint coefficients | `λE = 0.20`, `λA = 1.00`, `λF = 0.50` |
| Common-descent parameters | `p = 1.5`, `ηmin = 0.10`, `ρ = 0.20`, `vsev = 0.25` |

Additional DBN, safety, projection, and common-descent parameters are reported in Appendix Table C3.

---

## 11. Interpretation boundaries

The released evidence supports the framework as an **event-driven, traceable, and executable decision architecture**. It does not establish universal numerical superiority of DBN-CDDPG.

In particular:

- Joint masked DDQN has lower nominal means for `Cmax`, AHD, fatigue, and energy in the nominal benchmark.
- The absence of Holm-adjusted significance with ten seeds is not treated as equivalence.
- The nominal M1 ablation does not demonstrate a scalar-performance advantage attributable to the DBN.
- Under HE-1, the static controller has better reported cumulative-budget and makespan outcomes than the event-driven controller.
- Under HE-4, event-driven execution improves post-event AHD, Dynamic ABFR, and NVS, but has higher mean makespan.
- In the matched Full/M1 extension, Full has slightly lower mean makespan but higher mean AHD and energy under both HE-1 and HE-4 and does not improve Dynamic ABFR.
- None of the eight matched Full/M1 engineering comparisons is significant after the eight-test Holm correction.
- Same-state decoding identifies only **2/52** explicit-guidance action changes across disturbed multi-mode Type-III opportunities, both from seed 82; no downstream counterfactual benefit is claimed.
- Hard feasible-domain projection is independent of the DBN and remains essential for executable completion in the projection-integrity diagnosis.
- AHD is a case-study risk-priority indicator, not a universally calibrated accident probability or industrial safety standard.
- Validation remains simulation-based; physical deployment requires application-specific sensing, calibration, expert validation, and hardware testing.

---

## 12. Runner-file mapping

The filenames below are the literal runner names in the current `DBN_CDDPG_Source_Code.zip`. Some retain legacy table numbering.

| Experiment family | Current source runner / command | Final manuscript output |
|---|---|---|
| Nominal six-method benchmark | `run_fourpanel_performance_10000.py`; `commands/run_nominal_10000.*` | Tables 8–9; Figs. 7–8 |
| Original HE-1–HE-4 DBN-CDDPG disturbance evaluation | `run_table7_formal.py`; `commands/run_disturbance_10000.*` | Table 10 |
| Static-versus-event-driven evaluation | `run_static_vs_event_HE1_HE4.py`; `commands/run_static_vs_event_10000.*` | Tables 11–12 |
| Controlled Case-1 representative trace | released trace/protocol records | Table 13; Fig. 9 |
| Full/M1–M5 mechanism ablation | `run_table8_formal.py`; `commands/run_ablation_10000.*` | Table 14; Fig. 10(a)–(b) |
| P0/P1 projection-integrity diagnosis | `run_projection_diagnosis_2000.py`; `commands/run_projection_diagnosis_2000.*` | Fig. 10(c) |
| Matched Full/M1 training, saved-actor evaluation, and analysis | `train_full_m1.py`; `evaluate_full_m1.py`; `analyze_full_m1.py`; `commands/run_matched_full_m1_*` | Tables 15–17; Appendix Table C6 |
| AHD robustness analysis | released numerical outputs; no dedicated runner in the six-file core source set | Table 5 |
| DBN posterior perturbation analysis | released numerical outputs; no dedicated runner in the six-file core source set | Table 7; Appendix Table C4 |
| Learned DBN tendency cross-check | released analysis outputs; no dedicated runner in the six-file core source set | Appendix Table C5 |

`SOURCE_PROVENANCE.json` documents source-file provenance, formal protocols, common budgets, and analyses supplied through supplementary outputs rather than dedicated core runners.

---

## 13. Reproduction workflow

A recommended workflow is:

1. Extract `DBN_CDDPG_Source_Code.zip`.
2. Enter the extracted `DBN_CDDPG_Source_Code/` directory.
3. Confirm that `data/case2_22tasks_dynamicAHD_retyping.xlsx` is present.
4. Create an environment matching the packaged `requirements.txt` for core-runner reruns.
5. Run the source-integrity check:

   ```bash
   python validation/verify_source.py
   ```

6. Use the formal command wrappers for the required core experiment family.

   **Linux/macOS**
   ```bash
   bash commands/run_nominal_10000.sh
   bash commands/run_disturbance_10000.sh
   bash commands/run_static_vs_event_10000.sh
   bash commands/run_ablation_10000.sh
   bash commands/run_projection_diagnosis_2000.sh
   ```

   **Windows**
   ```bat
   commands\run_nominal_10000.bat
   commands\run_disturbance_10000.bat
   commands\run_static_vs_event_10000.bat
   commands\run_ablation_10000.bat
   commands\run_projection_diagnosis_2000.bat
   ```

   **Matched Full/M1 extension**

   Linux/macOS:
   ```bash
   bash commands/run_matched_full_m1_train_10000.sh
   bash commands/run_matched_full_m1_HE1_HE4_eval.sh
   bash commands/run_matched_full_m1_analysis.sh
   ```

   Windows:
   ```bat
   commands\run_matched_full_m1_train_10000.bat
   commands\run_matched_full_m1_HE1_HE4_eval.bat
   commands\run_matched_full_m1_analysis.bat
   ```

7. Use the released completed-seed record archives for traceability:
   - `nominal_completed_seed_checkpoints.zip` for the six-method nominal benchmark;
   - `ablation_completed_seed_checkpoints.zip` for the internally matched Full/M1–M5 mechanism suite.
   These archives contain JSON checkpoint/result records, not reloadable PyTorch actor weights.
8. For a fresh reproduction of Section 5.6, use the dedicated Full/M1 workflow in the final source package:
   - train and save reloadable Full/M1 actors with `train_full_m1.py`;
   - evaluate the saved actors under nominal, HE-1, and HE-4 with `evaluate_full_m1.py`;
   - reproduce the paired inference and same-state diagnostic summaries with `analyze_full_m1.py`.
   The archived reference outputs are in `matched_full_m1_HE1_HE4_evaluation.zip`.
9. Keep the original four-scenario disturbance dataset, the static/event-driven dataset, the mechanism suite, and the later matched Full/M1 evaluation as **separate protocol datasets** unless the manuscript explicitly combines them.
10. Retain seed-level outputs before aggregation.
11. Recompute manuscript statistics from the released seed-level/evaluation records using the statistical convention for the relevant experiment family.
12. Compare outputs with the final manuscript mapping in Section 2 of this README.
13. Keep the separate 2,000-episode P0/P1 projection-integrity diagnosis distinct from all 10,000-episode complete-schedule suites.

### Core command settings

- nominal benchmark: 10,000 episodes, evaluation every 50 episodes, seeds `42,52,62,72,82,92,102,112,122,132`;
- original disturbance evaluation: DBN-CDDPG only, one evaluation per seed per disturbance scenario;
- mechanism ablation: Full + M1–M5, 10,000 episodes, evaluation every 50 episodes, seeds `42,52,62,72,82,92,102,112,122,132`;
- projection-integrity diagnosis: P0/P1 only, 2,000 episodes, evaluation every 20 episodes, the same ten mechanism seeds;
- static-versus-event-driven analysis: 10,000 episodes, evaluation every 50 episodes, seeds `2,3,4,5,6,7,8,9,10,11`.

Formal command wrappers should be preferred over legacy default values inside individual runner scripts.

---

## 14. Package integrity

The source archive includes:

- `SOURCE_PROVENANCE.json` for the core source-file provenance and formal protocol metadata;
- `MATCHED_FULL_M1_PROVENANCE.json` for the Section-5.6 Full/M1 extension;
- `RECORDED_EVALUATION_ENVIRONMENT.json` for the matched-evaluation software manifest;
- `UPSTREAM_SHA256.json` for hashes of source/input files used by the matched Full/M1 workflow;
- `CHECKSUMS.sha256` for final package-integrity verification;
- `validation/verify_source.py` for source-hash and compilation checks.

If any packaged file is changed, including replacement of the README or `requirements.txt`, regenerate `CHECKSUMS.sha256` so that the checksum manifest describes the final archive exactly.

---

## 15. Data availability

The manuscript reports the public repository at:

`https://github.com/Auto-says/DBN-CDDPG.git`

The repository is intended to provide the task inputs, source code, protocol information, completed-seed records, seed-level outcomes, the dedicated matched Full/M1 evaluation archive, and event-decision logs needed to trace the manuscript results.

---

## 16. Citation

If you use this repository, please cite the associated manuscript:

**Dynamic Bayesian Inference-Driven Safe Human-Robot Collaborative Disassembly Planning for Retired Power Batteries**

Bibliographic details should be updated after publication.
