# 🚦 Urban traffic analysis: Stochastic Simulation Engine

**Production-grade discrete-event simulation assessing multimodal urban traffic capacity and kinematic feasibility.**

<div align="center">

[![AnyLogic](https://img.shields.io/badge/AnyLogic-8.x-red.svg)](https://www.anylogic.com/)
[![Java](https://img.shields.io/badge/Java-11%2B-ED8B00.svg)](https://www.java.com/)
[![Simulation](https://img.shields.io/badge/Simulation-Discrete__Event-005CED.svg)]()
[![Stochastic](https://img.shields.io/badge/Modeling-Stochastic-5C4EE5.svg)]()
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

</div>

## 📌 Overview

The Urban Traffic analysis is a high-fidelity discrete-event and continuous spatial simulation designed to evaluate the physical and kinematic feasibility of road-space reallocation on the Jakobstraße / Gustav-Adolf-Straße arterial corridor in Magdeburg. 

It models complex, multimodal traffic flows to quantify the systemic impact of reducing motorized lanes to accommodate dedicated bicycle infrastructure. 

**Key highlights:**
- **Rigorous Stochastic Modeling:** Field data mathematically fitted to non-normal distributions  to accurately represent real-world inter-arrival times.
- **Architectural Petri Nets:** Complex corridor routing abstracted into Stochastic Petri Nets, isolating conflict-control logic and probabilistic state transitions prior to physical kinematic execution.
- **Microscopic Kinematics:** Implementation of Intelligent Driver Models (IDM) and MOBIL algorithms to capture realistic acceleration, braking, and lateral friction dynamics.
- **Monte Carlo Validation:** System resilience proven through 100-iteration paired batches, utilizing Welch's t-tests and Correlated Sampling to guarantee statistical significance.

---




https://github.com/user-attachments/assets/4c1f3d9f-28de-4ea3-bac9-2f477a1c9b6a







## ⚠️ Data Availability & NDA Notice

**Please note:** The core empirical data utilized to parameterize this simulation—including high-resolution traffic volume counts, precise municipal signal timings, and proprietary intersection geometries—was provided by the **Stadtplanungsamt Magdeburg** (City Planning Office of Magdeburg) under a strict Non-Disclosure Agreement (NDA). 

Consequently, the raw datasets cannot be published in this open-source repository. To ensure the model remains transparent and verifiable, the repository includes the abstracted mathematical distribution parameters (e.g., $\lambda$, $\alpha$, $\beta$) and the stochastic routing probability matrices derived from the raw data.

---


### Simulation Logic Flow

```text
┌─────────────────┐     ┌─────────────────────┐     ┌─────────────────────┐
│ Stochastic Data │───► │ Concept Abstraction │───► │ Kinematic Execution │
│    Modeling     │     │    (Petri Nets)     │     │     (AnyLogic)      │
└─────────────────┘     └─────────────────────┘     └─────────────────────┘
         │                         │                           │
         ▼                         ▼                           ▼
  Weibull / Exp.            Forward Chain (N-S)          IDM Car Following
  Platoon Effects           Reverse Chain (S-N)          MOBIL Lane Changing
  Peak-Hour Scaling         Side-Entry Integration       Signal S3 Bottlenecks

```

---

## 📊 Core Competency

### Stochastic Modeling & Analytics

| Technique | Application |
| --- | --- |
| **Distribution Fitting** | Modeling S-N traffic via Weibull ($\beta=0.849$, $\alpha=15.53$) and N-S traffic via Exponential ($\lambda=0.0388$) distributions. |
| **Monte Carlo Simulation** | Executing 100 1-hour iterations per scenario to generate confidence intervals for maximum/average queue lengths. |
| **Correlated Sampling** | Reducing variance between baseline and experimental runs via paired t-tests using identical random seed streams. |
| **Platoon Effect Handling** | Adjusting inter-arrival logic to account for upstream traffic light clustering, bypassing naive independence assumptions. |

### Simulation Architecture

| Component | Implementation |
| --- | --- |
| **Continuous Spatial Markup** | Exact physical geometries mapped from Jakobstraße / Gustav-Adolf-Straße topologies. |
| **Discrete-Event Blocks** | Probabilistic `SelectOutput` networks routing agents through `ES` (exit) and `EE` (entry) decision trees. |
| **State Machine Control** | Managing complex right-of-way priorities at unsignalized secondary intersections. |
| **JVM Memory Optimization** | Configured heap allocation to handle the intensive CPU/RAM overhead of tracking thousands of simultaneous agent kinematics. |

---

## ⚙️ Execution & Setup

### Prerequisites

* [AnyLogic 8.x](https://www.anylogic.com/) (Personal Learning or Professional Edition)
* Java SE 11 or higher
* Minimum 8GB RAM (16GB+ recommended for Monte Carlo batch execution)

### Running the Model

1. **Load the Project:**
Open AnyLogic and navigate to `File > Open`. Select `model/Jakobstrasse_Model.alp`.
2. **JVM Configuration:**
Due to the computational intensity of tracking simultaneous agent kinematics and lateral friction matrices, ensure your AnyLogic run configuration is allocated sufficient memory. Under the "Advanced" Java properties of the Run Configuration, set maximum available memory to `8192 MB` minimum.
3. **Running Experiments:**
Expand the `Jakobstrasse_Model` tree in the workspace. Right-click the desired experiment (e.g., `02_shared_corridor`) and select `Run`.


