# Metaheuristic Optimization: Simulated Annealing, Genetic Algorithms and PSO

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?logo=numpy&logoColor=white)
![Topic](https://img.shields.io/badge/Topic-Combinatorial%20Optimization-blueviolet)
![License](https://img.shields.io/badge/License-MIT-green)

Formulation, implementation and empirical comparison of metaheuristics on classic NP-hard and benchmark problems. The 25-page report ([`docs/metaheuristics_report.pdf`](docs/metaheuristics_report.pdf)) covers two problems; the notebooks here contain the code for the Bin Packing study and for Particle Swarm Optimization.

> Team project (report) with **Antoni Lopacz**.

## Problems

### 1. One-dimensional Bin Packing (code in `notebooks/01_...`)

Pack $n$ items of sizes $s_i$ into the minimum number of bins of capacity $C$:

$$
\min \sum_{j} y_j \quad\text{s.t.}\quad \sum_{j} x_{ij}=1\;\;\forall i,\qquad \sum_{i} s_i x_{ij}\le C\,y_j\;\;\forall j,\qquad x_{ij},y_j\in\{0,1\}.
$$

A simple lower bound is $\left\lceil \sum_i s_i / C \right\rceil$.

* **Simulated Annealing (SA):** solution = list of bins; initial solution from a randomised First-Fit; moves relocate items while preserving feasibility; acceptance probability $\exp(-\Delta/T)$ for worse solutions with geometric cooling (T₀ = 1000, α = 0.95).
* **Genetic Algorithm (GA):** population of 100 feasible packings built with randomised First-Fit; tournament selection (size 3), crossover (p = 0.9), mutation (p = 0.2) and elitism.
* **Experiments:** synthetic instances (item sizes 1-10, capacity 15), convergence curves, and a scaling study of bins used versus number of items.

**Findings**

* On the base instance both methods reach the same number of bins (40).
* On a harder, more constrained instance the **GA stayed flat at 40 bins while SA escaped the local optimum and found 39** (around iteration 55), consistent with SA's ability to accept uphill moves.
* Bins used grow approximately linearly with the number of items: ≈ 0.36·n + 3.2 on the base instance and ≈ 0.56·n + 5.9 on the harder one (SA fit).

### 2. Particle Swarm Optimization (code in `notebooks/02_...`)

Each particle $i$ updates its velocity and position as

$$
v_i \leftarrow w\,v_i + c_1 r_1 (p_i - x_i) + c_2 r_2 (g - x_i),\qquad x_i \leftarrow x_i + v_i,
$$

with inertia $w$, cognitive and social coefficients $c_1,c_2$, personal best $p_i$ and global best $g$. The algorithm is tested on two multimodal benchmark functions:

* **Rastrigin**: $f(x)=10n+\sum_i\left[x_i^2-10\cos(2\pi x_i)\right]$, many regularly spaced local minima.
* **Ackley**: smoother landscape with a narrow global basin.

A one-factor-at-a-time study of swarm size, $w$, $c_1$, $c_2$, number of iterations and maximum velocity yields the tuned configuration used for the final comparison: **36 particles, 100 iterations, w = 0.7, c₁ = c₂ = 1.5, v_max = 0.5**.

### 3. Fire Station Location (report only)

Modelled as a **Minimum Dominating Set** on a graph: choose the fewest vertices such that every vertex is selected or adjacent to a selected one,

$$
\min \sum_i x_i \quad\text{s.t.}\quad x_i+\sum_{j:\{v_j,v_i\}\in E}x_j\ge 1\;\;\forall i .
$$

SA (greedy vs random initialisation, penalised cost $|D|+\lambda\cdot\#\text{uncovered}$) and a GA (tournament selection, uniform crossover, bit-flip mutation, elitism, optional hill-climbing) were compared. In the SA parameter sweep, greedy initialisation reached cost 18 in all tested settings, whereas random initialisation gave 23-67. *The source code for this part is not included in this repository.*

## Repository structure

```
metaheuristics-optimization/
├── docs/metaheuristics_report.pdf
├── notebooks/
│   ├── 01_bin_packing_sa_vs_ga.ipynb
│   └── 02_particle_swarm_optimization.ipynb
├── data/README.md
├── requirements.txt
└── LICENSE
```

## Quick start

```bash
git clone https://github.com/alejandrofer24/metaheuristics-optimization.git
cd metaheuristics-optimization
python -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook notebooks/
```

## Author

**Alejandro Fernández Trillo** · Mathematics graduate · MSc student in Artificial Intelligence
[LinkedIn](https://www.linkedin.com/in/alejandro-fernandez-trillo)
