# Optimization Modeling Portfolio (Gurobi)
**Linear Programming · Mixed-Integer Programming · Constraint Logic · Linearization · Decision Analytics**

---

## Overview

This project is a **decision analytics and optimization modeling notebook** implemented in **Python + Gurobi**. It solves three applied optimization cases end-to-end (formulation → implementation → solution interpretation), demonstrating proficiency in:

- **LP / MILP formulation**
- **absolute-value linearization**
- **integer/continuous decision modeling**
- **logical constraints (OR, implication, cardinality)**
- **business-rule constrained design**
- **transportation planning with shortage modeling**
- validation checks to confirm feasibility and constraint satisfaction

---

## Case 1 — Apportionment Problem (Fair Allocation of Parliamentary Seats)

### Application
Governments must allocate a **fixed number of seats** across provinces proportional to population, but seats must be **integers**. Naive proportional quotas create fractional allocations, so a fair integer allocation must be computed.

### What is optimized
The notebook implements a **Min-Sum fairness model** (and associated interpretation), allocating integer seats while minimizing deviation from proportional quotas.

### Decision variables
- \( x_i \in \mathbb{Z}_{\ge 0} \): seats assigned to province \(i\)

### Objective (Min-Sum)
Minimize the total deviation from quotas (absolute gaps):
\[
\min \sum_i |x_i - q_i|
\]
The absolute value is handled using **linearization** (auxiliary variables / constraints).

### Constraints
- **Total seats fixed**: \( \sum_i x_i = S \)
- **Nonnegativity / integrality**
- Additional interpretive checks and constraint validation are included in the notebook outputs.

---

## Case 2 — Designing a Shopping Mall Floor Plan (LP → MILP + Business Logic)

### Application
You are designing the floor plan of a mall by choosing how many units of each store/facility to include (restaurants, clothing, tech, pharmacy, department store, toys, toilets, security). Each has:
- expected annual profit
- occupied area
- max number of units

### Core objective
Maximize expected annual profit:
\[
\max \sum_j (\text{profit}_j \cdot x_j)
\]

### Core constraints
- **Total area limit** (e.g., mall capacity)
- **Minimum facilities**: at least one toilet and at least one security division
- **Upper bounds**: max units per store type

### Two modeling regimes included
#### Q4 — Continuous relaxation (LP)
- \( x_j \ge 0 \) continuous  
Used to compute a theoretical upper bound and analyze marginal profitability under area constraints.

#### Q5 — Integer decision model (MILP)
- \( x_j \in \mathbb{Z}_{\ge 0} \)  
Enforces implementable floor plans.

### Advanced business constraints (Q6 / Q7)
The notebook adds realistic logical constraints, including:

- **Toilet-to-restaurant ratio** requirements  
- **At least one premium store** (Department OR Tech)
- **Clothing store area cap**
- **Conditional security requirement** (implication / if-then logic)

Additionally, a **diminishing-returns profit structure** is modeled using integer logic (piecewise / stepwise profit behavior), creating a more realistic profit function than purely linear scaling.

---

## Case 3 — BalancedMilk Transportation Planning (Shortage-Aware LP/MILP + Logical Constraints)

### Application
BalancedMilk must ship milk from multiple supply centers to multiple demand markets under capacity limits and business policies. The company is allowed to **undersupply** (with a penalty) but must **not oversupply** any market.

### Key modeling features
- **Transportation flows** from supply nodes \(S_i\) to demand nodes \(D_j\)
- **Supply capacity constraints**
- **Demand satisfaction with shortage**:
  - undersupply allowed via shortage variables
  - **no oversupply** enforced
- **Penalty cost** for unmet demand (given as **\$15/tonne** in the case)

### Business logic constraints (high signal)
The notebook encodes non-trivial policies as MILP logic:

- **Flexibility constraint**:
  - at least one of:
    - \(D_5\) receives ≥ 80% of its demand **OR**
    - \(D_6\) receives ≥ 60% of its demand  
  (modeled via binary variables + linear constraints)

- **Company policy / utilization rule**:
  - at least **two of** \(S_3, S_4, S_5\) must operate at **≥ 50% capacity**
  (cardinality constraint over binary indicators)

- **Anti-discrimination constraint**:
  - certain markets must receive **≥ 50%** of their demand (e.g., specified markets in the case)
  (lower-bound service-level constraints)

This case demonstrates **shortage modeling**, **service-level constraints**, and **binary-logic enforcement** commonly used in real supply-chain optimization.

---

## Tech Stack

### Languages & Tools
- **Python (Jupyter Notebook)**
- **Gurobi Optimizer (gurobipy)**

### Libraries
- `gurobipy`
- `pandas`, `numpy`

### Optimization Concepts Demonstrated
- Linear Programming (LP)
- Mixed-Integer Linear Programming (MILP)
- Absolute-value linearization
- Big-M style logical constraints (OR / implication)
- Cardinality constraints
- Shortage variables + penalty costs
- Piecewise / diminishing-returns modeling

---

## Repository Contents

```text
├── MGSC662_FV_HELIA_MAHMOODZADEH_261224416_Assignment_2.ipynb
└── README.md
