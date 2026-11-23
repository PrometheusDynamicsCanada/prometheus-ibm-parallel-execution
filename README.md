# Parallel Multi-Circuit Execution on IBM Heron Hardware

*Portfolio Hub: [Prometheus Quantum Overview](https://github.com/PrometheusDynamicsCanada/prometheus-quantum-overview) · [Detailed Technical Report](DETAILED_TECHNICAL_REPORT.md) · [Public Disclosure](PUBLIC_DISCLOSURE.md)*

**Co-scheduling up to 8 independent circuits simultaneously with idle-qubit buffers on 156-qubit IBM Heron delivered 2.85x qualifying workloads per billed second.**

## At a Glance

- **IBM Marrakesh (`16` heterogeneous `4Q-12Q` workloads):** Simultaneous `8-way` execution delivered **`3.67` accepted circuits per billed second** vs. **`1.29/s`** sequential Qiskit SABRE (**`2.85x`**), cutting billed runtime by **`-57.1%`** (`3.0 s` vs. `7.0 s`).
- **IBM Kingston (`12` uniform `6Q-10Q` workloads):** Simultaneous `6-way` packing delivered **`3.00` accepted/billed second** vs. **`1.17/s`** sequential SABRE (**`2.57x`**, `-50.0%` billed time).
- **Minimal crosstalk:** Neighboring circuits remained within `-0.0012` to `+0.0035` overlap of isolated same-qubit controls.
- **Hardware scope:** `19` live IBM Heron jobs, `586` PUBs, `1,140` evaluations, `655,360` physical shots, and `220.0` billed seconds across `ibm_kingston`, `ibm_marrakesh`, and `ibm_fez`.

![Accepted Workloads per Billed Second on 156-Qubit IBM Marrakesh](assets/fig0_opening_parallel_throughput.png)
*Accepted workloads per billed second on `ibm_marrakesh` across 16 heterogeneous `4Q-12Q` circuits (`3.67/s` vs. `1.29/s` SABRE = `2.85x`).*

---

## Executive Summary (`16` Heterogeneous `4Q-12Q` Workloads on `ibm_marrakesh`, `N = 6` Timing Jobs, `41,984` Shots)

*Hardware IDs:* `dbk2gr1kdqps7391kq90` .. `dbkaflhkdqps739235hg` ([`ALL_19_IBM_HERON_JOBS_LEDGER.csv`](data/ALL_19_IBM_HERON_JOBS_LEDGER.csv)).

| Execution Arm (`ibm_marrakesh`) | Billed `s` `(↓)` | PUBs / Shots `(↓)` | Overlap $F_B$ `*(mean / med)*` `(↑)` | Hellinger $F_H$ `*(mean)*` `(↑)` | `TVD` `*(mean)*` `(↓)` | Pass `F_B >= 0.90` `(↑)` | Pass / Billed `s` `(↑)` |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **`T1`: SABRE O3 Sequential (`1w`)** | `7.0 s` | `16` / `16,384` | `0.8593` / `0.9062` | `0.7557` | `0.2945` | `9 / 16` | `1.29 / s` (`1.00x`) |
| **`T2`: Prometheus Sequential (`1w`)** | `7.0 s` | `16` / `16,384` | **`0.8840`** / `0.9280` | **`0.7986`** | **`0.2489`** | **`11 / 16`** | `1.57 / s` (`1.22x`) |
| **`T4`: Prometheus `8-Way` Packed (`2` PUBs)** | **`3.0 s`** (`-57.1%`) | **`2` / `2,048`** | `0.8837` / **`0.9396`** | `0.7976` | `0.2560` | **`11 / 16`** | **`3.67 / s` (`2.85x`)** |
| **`T5`: TKET `8-Way` Packed (`2` PUBs)** | **`3.0 s`** (`-57.1%`) | **`2` / `2,048`** | `0.8542` / `0.9302` | `0.7548` | `0.2938` | `10 / 16` | `3.33 / s` (`2.59x`) |
| **`T6`: Prom Meta-Selector (`2` PUBs)** | **`3.0 s`** (`-57.1%`) | **`2` / `2,048`** | `0.8634` / `0.9282` | `0.7686` | `0.2801` | **`11 / 16`** | **`3.67 / s` (`2.85x`)** |

*Metric Key:* $F_B = \sum_x \sqrt{P_{\text{ideal}}(x)P_{\text{meas}}(x)}$ (Studio `"State Fidelity"`); $F_H = F_B^2$.

- **Offline `numpy` raw-histogram & SHA-256 commitment verifier:** `python scripts/verify_spatial_multiplexing_campaigns.py`
- **Start a supported pilot:** [william@prometheusdynamics.ca](mailto:william@prometheusdynamics.ca) | [prometheusdynamics.ca](https://prometheusdynamics.ca)

---

## Full Campaign 1 `ibm_kingston` Table, Commitments & Interference Studies

<details>
<summary><strong>Expand Campaign 1 (ibm_kingston, 12 Workloads), T3 Arm, and Pre-Execution SHA-256 Commitments - Click to expand</strong></summary>

### Campaign 1 Timing-Isolation Benchmark (`ibm_kingston`, `12` Uniform `6Q-10Q` Workloads, `N = 4` Jobs, `29,696` Shots)
| Arm (`ibm_kingston`) | IBM Job ID | Billed `s` `(↓)` | PUBs / Shots `(↓)` | $F_B$ `*(mean / med)*` `(↑)` | $F_H$ `*(mean)*` `(↑)` | `TVD` `*(mean)*` `(↓)` | Pass `>= 0.90` `(↑)` | Pass / Billed `s` `(↑)` |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **`T1`: SABRE Sequential** | `dbk1i3l6p51s73clp22g` | `6.0 s` | `12` / `12,288` | `0.8940` / `0.9203` | `0.8067` | `0.2880` | `7 / 12` | `1.17 / s` (`1.00x`) |
| **`T2`: Prom Sequential** | `dbk1i6t6p51s73clp290` | `5.0 s` | `12` / `12,288` | **`0.9179`** / `0.9380` | **`0.8467`** | **`0.2348`** | `8 / 12` | `1.60 / s` (`1.37x`) |
| **`T3`: Prom `4-Way` (`3` PUBs)** | `dbk1ia1kdqps7391ikgg` | **`3.0 s`** | `3` / `3,072` | `0.9108` / **`0.9443`** | `0.8352` | `0.2520` | `8 / 12` | `2.67 / s` (`2.29x`) |
| **`T4`: Prom `6-Way` (`2` PUBs)** | `dbk1ic9ffo6c73baki30` | **`3.0 s`** | **`2` / `2,048`** | `0.9157` / `0.9419` | `0.8434` | `0.2428` | **`9 / 12`** | **`3.00 / s` (`2.57x`)** |

*(Marrakesh `T3` `3-PUB` packed arm `dbk2hkl6p51s73clr60g`: `3.0 s`, `3` PUBs / `3,072` shots, $F_B = 0.8838 / 0.9365$, $F_H = 0.7981$, `TVD = 0.2494`, `11/16` pass = `3.67/s` [`2.85x`]. Pre-execution SHA-256 commitments: C1 `f0610583..`, C2 `cfb875e6..`, C3 `9e3d96c6..`; full 3-QPU interference and `Arm 6` selector tables are in [`DETAILED_TECHNICAL_REPORT.md`](DETAILED_TECHNICAL_REPORT.md).)*

</details>
