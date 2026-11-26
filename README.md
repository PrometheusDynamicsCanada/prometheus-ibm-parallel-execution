# Parallel Multi-Circuit Execution on 156-Qubit IBM Processors

*Portfolio Hub: [Prometheus Quantum Overview](https://github.com/PrometheusDynamicsCanada/prometheus-quantum-overview) · [Detailed Technical Report](DETAILED_TECHNICAL_REPORT.md) · [Public Disclosure](PUBLIC_DISCLOSURE.md)*

Running a single 4-to-12-qubit circuit on a 156-qubit processor bills full-chip runtime while leaving over 90% of the device idle. We tested whether buffered 6-way and 8-way circuit packing can return more accepted workloads per billed second on IBM Kingston, Marrakesh, and Fez without crosstalk degrading accuracy.

Across 16 varied 4-to-12-qubit circuits on IBM Marrakesh, buffered 8-way Prometheus execution delivered 3.67 accepted workloads per billed second versus 1.29 per second for sequential Qiskit SABRE O3 (2.85x throughput), cutting billed runtime by 57.1% (3.0 s vs. 7.0 s) while passing 11 of 16 circuits versus 9 of 16 for SABRE and 10 of 16 for equally 8-way-packed Quantinuum TKET (3.33 per second). On IBM Kingston across 12 circuits, 6-way execution delivered 3.00 accepted results per billed second versus 1.17 sequential (2.57x).

Buffered separation was essential: unbuffered adjacent placement degraded output agreement, whereas buffered co-execution matched isolated same-qubit controls within 0.0035.

This repository contains the pre-execution commitments, raw histograms, and offline verification tools from 19 completed IBM Quantum jobs (655,360 measurement shots).

![Accepted Workloads per Billed Second on 156-Qubit IBM Marrakesh](assets/fig0_opening_parallel_throughput.png)
*Summary figure: Accepted workloads per billed second across 16 heterogeneous 4-to-12-qubit circuits on 156-qubit IBM Marrakesh (3.67/s vs. 1.29/s sequential SABRE).*

**[Read the detailed results](#detailed-results) · [Verify the data](#verify-the-data) · [Request an evaluation](https://prometheusdynamics.ca/connect/) · [Book a demonstration](mailto:william@prometheusdynamics.ca?subject=Prometheus%20Evaluation)**

---

<a id="detailed-results"></a>
## Executive Summary (`16` Heterogeneous `4Q-12Q` Workloads on `ibm_marrakesh`, `N = 6` Timing Jobs, `41,984` Shots)

*Hardware IDs:* `dbk2gr1kdqps7391kq90` .. `dbkaflhkdqps739235hg` ([`ALL_19_IBM_HERON_JOBS_LEDGER.csv`](data/ALL_19_IBM_HERON_JOBS_LEDGER.csv)).

| Execution Arm (`ibm_marrakesh`) | Billed `s` `(↓)` | PUBs / Shots `(↓)` | Output Agreement $F_B$ *(mean / med)* `(↑)` | Squared Hellinger $F_H$ *(mean)* `(↑)` | `TVD` *(mean)* `(↓)` | Pass `F_B >= 0.90` `(↑)` | Pass / Billed `s` `(↑)` |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **`T1`: SABRE O3 Sequential (`1w`)** | `7.0 s` | `16` / `16,384` | `0.8593` / `0.9062` | `0.7557` | `0.2945` | `9 / 16` | `1.29 / s` (`1.00x`) |
| **`T2`: Prometheus Sequential (`1w`)** | `7.0 s` | `16` / `16,384` | **`0.8840`** / `0.9280` | **`0.7986`** | **`0.2489`** | **`11 / 16`** | `1.57 / s` (`1.22x`) |
| **`T4`: Prometheus `8-Way` Packed (`2` PUBs)** | **`3.0 s`** (`-57.1%`) | **`2` / `2,048`** | `0.8837` / **`0.9396`** | `0.7976` | `0.2560` | **`11 / 16`** | **`3.67 / s` (`2.85x`)** |
| **`T5`: TKET `8-Way` Packed (`2` PUBs)** | **`3.0 s`** (`-57.1%`) | **`2` / `2,048`** | `0.8542` / `0.9302` | `0.7548` | `0.2938` | `10 / 16` | `3.33 / s` (`2.59x`) |
| **`T6`: Prom Meta-Selector (`2` PUBs)** | **`3.0 s`** (`-57.1%`) | **`2` / `2,048`** | `0.8634` / `0.9282` | `0.7686` | `0.2801` | **`11 / 16`** | **`3.67 / s` (`2.85x`)** |

*Metric Key:* Output-distribution agreement is Classical Bhattacharyya Overlap $F_B = \sum_x \sqrt{P_{\text{ideal}}(x)P_{\text{meas}}(x)}$ (Studio `"State Fidelity"`); $F_H = F_B^2$.

---

<a id="verify-the-data"></a>
## Inspect the Evidence & Reproduce Supported Checks Offline

- **Command:** `python scripts/verify_spatial_multiplexing_campaigns.py`
- **Dependencies & measured runtime:** Python 3 + `numpy` (`0` cloud credentials, **`~0.33 s`** measured runtime; verifies pre-execution SHA-256 commitments and recomputes all `1,140` evaluations from raw histograms with `0.00e+00` max diff).
- **Request an evaluation or book a demonstration:** [prometheusdynamics.ca/connect](https://prometheusdynamics.ca/connect/) | [william@prometheusdynamics.ca](mailto:william@prometheusdynamics.ca?subject=Prometheus%20Evaluation)

---

## Full Campaign 1 `ibm_kingston` Table, Commitments & Interference Studies

<details>
<summary><strong>Expand Campaign 1 (ibm_kingston, 12 Workloads), T3 Arm, and Pre-Execution SHA-256 Commitments - Click to expand</strong></summary>

| Arm (`ibm_kingston`) | IBM Job ID | Billed `s` `(↓)` | PUBs / Shots `(↓)` | $F_B$ *(mean / med)* `(↑)` | $F_H$ *(mean)* `(↑)` | `TVD` *(mean)* `(↓)` | Pass `>= 0.90` `(↑)` | Pass / Billed `s` `(↑)` |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **`T1`: SABRE Sequential** | `dbk1i3l6p51s73clp22g` | `6.0 s` | `12` / `12,288` | `0.8940` / `0.9203` | `0.8067` | `0.2880` | `7 / 12` | `1.17 / s` (`1.00x`) |
| **`T2`: Prom Sequential** | `dbk1i6t6p51s73clp290` | `5.0 s` | `12` / `12,288` | **`0.9179`** / `0.9380` | **`0.8467`** | **`0.2348`** | `8 / 12` | `1.60 / s` (`1.37x`) |
| **`T3`: Prom `4-Way` (`3` PUBs)** | `dbk1ia1kdqps7391ikgg` | **`3.0 s`** | `3` / `3,072` | `0.9108` / **`0.9443`** | `0.8352` | `0.2520` | `8 / 12` | `2.67 / s` (`2.29x`) |
| **`T4`: Prom `6-Way` (`2` PUBs)** | `dbk1ic9ffo6c73baki30` | **`3.0 s`** | **`2` / `2,048`** | `0.9157` / `0.9419` | `0.8434` | `0.2428` | **`9 / 12`** | **`3.00 / s` (`2.57x`)** |

*(Marrakesh `T3` `3-PUB` arm `dbk2hkl6p51s73clr60g`: `3.0 s`, $F_B = 0.8838 / 0.9365$, `11/16` pass = `3.67/s`. Pre-execution SHA-256 commitments: C1 `f0610583..`, C2 `cfb875e6..`, C3 `9e3d96c6..`; full interference tables in [`DETAILED_TECHNICAL_REPORT.md`](DETAILED_TECHNICAL_REPORT.md).)*

</details>
