# Public Disclosure & Intellectual Property Boundary

## 1. Purpose of This Public Evidence Repository

This repository provides a complete, cryptographically verifiable, offline-auditable hardware validation package for the **Prometheus Quantum Execution Manager, Multi-Compiler Meta-Selector & Spatial Multiplexing Campaigns** (19 IBM Quantum Heron jobs, 586 composite and isolated PUBs, 655,360 physical shots, 1,140 scored workload evaluations / 1,194 physical component circuit executions, 220.0 charged QPU seconds) executed across **`ibm_kingston`**, **`ibm_marrakesh`**, and **`ibm_fez`**.

Every hardware metric, bitstring marginalization, shot-by-shot replica correlation, QPU and Joint `(Compiler*, QPU*)` selection regret calculation, and timing-isolation throughput ratio in the accompanying report can be verified offline without cloud credentials using [`scripts/verify_spatial_multiplexing_campaigns.py`](scripts/verify_spatial_multiplexing_campaigns.py).

## 2. Included Empirical Provenance (100% Public & Auditable)

- **IBM Quantum Hardware Job IDs & Timestamps:** All 19 IBM Quantum Heron Job IDs (`dbk1h6d6p51s73clp0hg`, `dbk1hfl6p51s73clp140`, `dbk1hq0nceqs73al07v0`, `dbk1i3l6p51s73clp22g`, `dbk1i6t6p51s73clp290`, `dbk1ia1kdqps7391ikgg`, `dbk1ic9ffo6c73baki30`, `dbk284hkdqps7391k5v0`, `dbk2g9gnceqs73al2eig`, `dbk2gipffo6c73bamlt0`, `dbk2gr1kdqps7391kq90`, `dbk2hf9kdqps7391krmg`, `dbk2hkl6p51s73clr60g`, `dbk2i1d6p51s73clr6ug`, `dbkaag9ffo6c73bb4oa0`, `dbkab5t6p51s73cm9000`, `dbkacr9ffo6c73bb4rf0`, `dbkafj1kdqps739235eg`, `dbkaflhkdqps739235hg`), submission and completion timestamps, provider-charged QPU seconds (220.0s total), active circuit execution times (178.4530s total), and raw per-PUB measurement bitstring histograms (`data/*/jobs/*.json`).
- **Pre-Execution Cryptographic Commitments:** All three canonical SHA-256 pre-execution commitment manifests (`f0610583e1d56b58006550d36612bfd3fa54481737e77d68ebb1ded995355f6f` for Campaign 1, `cfb875e6add9d08372a5dc95ebb1db84bbc52a544464e111ec4e7941903b4f4c` for Campaign 2, and `9e3d96c6b0c7a1d7e1475145cf35046e4481c3ac9659edcd8e8b8a3d17c27cc6` for Campaign 3), locked strictly prior to the first hardware job submission in each campaign.
- **Multi-QPU Calibration Telemetry & Logical Circuits:** Complete live device calibration snapshots (`data/*/calibration_snapshot_*.json`), all unrouted logical benchmark circuits in OpenQASM 2.0 format (`data/*/logical_circuits/*.qasm`), and exact statevector reference probability distributions (`data/*/logical_circuits/*_ideal.npz`).

## 3. Proprietary Hardware-Aware Optimization Boundary

In accordance with Prometheus Dynamics intellectual property protection standards:
- Proprietary hardware-aware optimization: Prometheus uses circuit structure and device calibration data to guide compilation and execution. Proprietary optimization, compilation, placement, and execution-control software, internal parameters, and physical qubit coordinate mappings are withheld from this public repository.
- Experimental arms and compiled circuits are identified strictly by their public experimental arm designations, inter-circuit coupling-graph buffer distances, and verifiable SHA-256 circuit commitments.
