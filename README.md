# QCCG: Quantum Column-and-Cut Generation for Maximum Independent Set

**Authors:** Shivaram Subramanian et al. — IBM Research

## Overview

QCCG is a hybrid quantum-classical optimization framework for the Maximum
Independent Set (MIS) problem. It uses column-and-cut generation with
quantum circuit simulation (VQE) as the main pricing oracle to generate
high-quality columns for the LP relaxation.

## Algorithm

### Column-and-Cut Generation

The master problem is a set packing LP relaxation solved with CPLEX.
Columns (independent sets) and cuts (clique inequalities) are added
dynamically.

### Pricing Subproblem (Quantum Simulation)

The pricing problem — a Maximum Weight Independent Set (MWIS) instance
using dual variables as vertex weights — is solved via **VQE circuit
simulation** on Qiskit Aer's matrix-product-state (MPS) backend.

VQE produces high-quality columns that capture structural correlations
in the graph, leading to tighter LP relaxations and strong integer solutions.
An analytical QAOA based computation provides fast supplementary columns.

| Parameter | Value |
| :--- | :--- |
| Ansatz | EfficientSU2 |
| Circuit depth | 4 |
| Optimizer | SPSA |
| Backend | Qiskit Aer MPS |

### Separation (Quantum Simulation)

Violated clique inequalities are identified by solving MWIS on the
complement graph using the same quantum simulation pipeline.

### Primal Heuristics

Integer solutions are obtained via genetic algorithm, greedy augmentation,
and local search, seeded with columns found by VQE during
column generation.

## Computational Setup

### Hardware

| Component | Specification |
| :--- | :--- |
| CPU | Intel Core i7-13850HX (20 cores / 28 threads) |
| RAM | 32 GB DDR5 |
| OS | Ubuntu 24.04 LTS (WSL2) |

### Software

| Package | Version |
| :--- | :--- |
| Python | 3.11 |
| Qiskit | 1.4.2 |
| qiskit-aer | 0.17.0 |
| CPLEX | 22.1 (full edition) |
| NetworkX | 3.4.2 |
| NumPy | 1.26.4 |
| SciPy | 1.14.1 |
| Numba | 0.60.0 |

### Runtime

Wall-clock time including all computation. No queuing or compilation
time excluded. We do not record the time to reach the best feasible solution, which is relatively shorter compared to the full time required for all the CG iterations to complete (which we report below). The algorithm is tuned for solution quality rather than speed.

## Results on QOBLIB Instances

| Instance | n | IS Size | Best Known | Runtime (s) |
| :--- | ---: | ---: | ---: | ---: |
| brock400-1 | 400 | **27** | 27 | 3,484 |
| brock800-1 | 800 | **23** | 23 | 6,430 |
| C4000-5 | 4,000 | 17 | 18 | 10,677 |
| frb45-21-3 | 945 | 44 | 45 | 1,581 |
| frb50-23-3 | 1,150 | 49 | 50 | 2,167 |
| frb53-24-1 | 1,272 | 52 | 53 | 1,969 |
| frb59-26-2 | 1,534 | 57 | 58 | 2,161 |
| frb100-40 | 4,000 | 95 | 96 | 6,733 |
| keller6 | 3,361 | **59** | 59 | 6,875 |
| socfb-trinity100 | 2,613 | **499** | 499 | 9,044 |
| sorrell4 | 2,048 | **24** | 24 | 5,496 |
| sorrell7 | 2,048 | **198** | 198 | 6,824 |

**Bold** = matches best-known value.

## Paradigm

Quantum Simulator — all quantum circuits simulated classically via
Qiskit Aer (MPS backend).

## Citation

Paper in preparation.
