# Job-Shop Scheduling Problem (JSSP) Solvers Benchmark

This repository contains the complete experimental evaluation, algorithmic implementations, and comparative benchmarks for the **Job-Shop Scheduling Problem (JSSP)**. We evaluate Uninformed Search (**DFS with Upper Bound Pruning**), Informed Search (**A* / Weighted A* with admissible heuristics), and Stochastic Local Search (**Multi-Start Hill-Climbing with VND**) under a strict **60.0-second execution time budget** per instance.

## 👥 Authors & Academic Context

* **Course**: TC6001.570 - Foundations of Artificial Intelligence

* **Institution**: Tecnológico de Monterrey

* **Team Members**:

  * A01986448

  * A01633395

  * A01985570

  * A01174432

* **Date**: October 30, 2026

## 📌 Problem Overview

The Job-Shop Scheduling Problem (JSSP) is an NP-hard combinatorial optimization problem where a set of $n$ jobs must be processed across $m$ dedicated machines. Each job requires a fixed sequence of operations with specified processing durations. A feasible schedule must strictly satisfy:

1. **Precedence Constraints**: Operations within the same job must execute in sequence.

2. **Machine Capacity Constraints**: Each machine can process at most one operation at a time (no overlap).

3. **Non-preemption Constraints**: Operations cannot be interrupted once started.

**Objective**: Minimize the global **makespan** ($C_{max}$), defined as the total completion time of all jobs.

## 📁 Repository Structure

```
├── README.md                           # Main documentation & reproduction guide
├── 1. JSSP_Report.pdf                  # Official academic research paper
├── data/
│   ├── README.md                       # Data formats, parser specs & generator details
│   ├── benchmarks/                     # Standard OR-Library instances (e.g., ft06, ft10, la01, la36)
│   └── own_instances/                  # 20 custom seeded instances (10 Easy, 8 Medium, 2 Hard)
├── notebooks/
│   ├── JSSP.ipynb                      # Notebook 1: Base 3x3 Toy Model & Gantt visualization
│   └── JSSP_Instances_and_Benchmark.ipynb # Notebook 2: Full Benchmark Pipeline (20+ instances)
└── results/
    ├── benchmarks/                     # Output logs for OR-Library benchmark runs
    ├── own_instances/                  # Output logs for custom instance runs
    ├── Best solution_JSSP_Gantt_Chart.png # Best generated Gantt chart output
    ├── JSSP_Gantt_Chart.png            # Initial schedule Gantt chart
    ├── results_DFS.csv                 # Detailed execution metrics for DFS
    ├── results_AStar.csv               # Detailed execution metrics for A* / Weighted A*
    └── results_MSHC.csv                # Detailed execution metrics for Multi-Start Hill Climbing

```

## 🛠️ Implemented Algorithms

### 1. Uninformed Search Baseline: Depth-First Search (DFS) with Pruning

* **Strategy**: Explores paths in LIFO order to quickly reach leaf nodes and establish an initial **Upper Bound (UB)**.

* **Pruning Rule**: Any candidate branch with projected completion time $f(s) \ge UB$ is immediately discarded, reducing node expansions while maintaining extreme memory efficiency.

### 2. Informed Search: $A^*$ and Weighted $A^*$ Search

* **Evaluation Function**: $f(n) = g(n) + w \cdot h(n)$ where $g(n)$ is the accumulated partial makespan.

* **Ablation Heuristics**:

  * $H_1$ **(Cheap - Remaining Job Time)**: Calculates the longest remaining operation chain for any job ($O(n)$ complexity).

  * $H_2$ **(Bottleneck - Machine Workload)**: Computes the maximum pending processing workload across all machines and returns $\max(H_1, \text{Workload})$. Admissible and significantly tighter lower bound.

* **Weight Variants**: Tested $w = 1.0$ (standard $A^*$), $w = 1.5$, and $w = 2.0$ (Weighted $A^*$).

### 3. Stochastic Local Search: Multi-Start Hill-Climbing (MSHC) with VND

* **Representation**: Operation-based list representation mapped via a deterministic decoder.

* **Variable Neighborhood Descent (VND)**:

  * **Neighborhood 1 (**$N_1$ **- Adjacent Swap)**: Swaps adjacent operations belonging to different jobs for fast local descent.

  * **Neighborhood 2 (**$N_2$ **- Global Swap)**: Swaps any two non-adjacent operations across the sequence to escape shallow local optima.

* **Multi-Start**: Performs random sequence restarts when local optima are reached to continuously sample the global search space within the 60-second budget.

## 🚀 How to Reproduce Results

### Option 1: Google Colab (Recommended - One Click)

1. Click the **`Colab` badges** at the top of this README to launch either notebook directly in Google Colab.

2. In Colab, execute all cells sequentially via **Runtime > Run all** (`Ctrl + F9`).

3. The pipeline will automatically load datasets from `data/`, execute the 60-second evaluation protocol, and export summary tables and Gantt plots directly to `results/`.

### Option 2: Local Execution

1. Clone the repository:

   ```
   git clone https://github.com/ambaralmaraz73/Solving-Job-Shop-Scheduling-via-Search-Heuristics-and-Local-Search-.git
   cd Solving-Job-Shop-Scheduling-via-Search-Heuristics-and-Local-Search-
   
   ```

2. Install dependencies:

   ```
   pip install pandas matplotlib
   
   ```

3. Launch Jupyter Notebooks:

   ```
   jupyter notebook notebooks/JSSP.ipynb
   # OR
   jupyter notebook notebooks/JSSP_Instances_and_Benchmark.ipynb
   
   ```

## 📊 Summary Results (3x3 Base Model Verification)

All algorithms successfully converged to the global optimal makespan of **12 time units** on the canonical $3\times3$ instance under the 60.0s time budget:

| Algorithm | Heuristic / Mode | Best Makespan | Execution Time (s) | Time to Best (s) | Success Rate (%) | Expanded Nodes (Iterations) | Max Frontier / Memory | 
| ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | 
| **DFS** | Upper Bound Pruning | **12** | 0.0085 | 0.0018 | 100% | 1,263 | **10** (Stack) | 
| **A* Search*\* | $H_1$ (Max Job Time) | **12** | 0.0057 | 0.0057 | 100% | 272 | 448 (Heap) | 
| **A* Search*\* | $H_2$ (Machine Workload) | **12** | **0.0025** | 0.0025 | 100% | **147** | 250 (Heap) | 
| **MSHC** | VND ($N_1 + N_2$) | **12** | 60.0002 | **0.0006** | 100% | 6,192,874 | 140,189 (Restarts) | 

### Winner Schedule Output

$A^*$ Search with $H_2$ certified global optimality with an **85.0% reduction in node expansions** compared to $H_1$, delivering the most mathematically robust schedule:

## 📑 Experimental Protocol & Rigor

* **Execution Limit**: Hard threshold of **60.0 seconds** per instance per solver.

* **Random Seed**: Fixed seed (`random.seed(42)`) for 100% deterministic instance generation and local search reproducibility.

* **Logged Metrics**: Final Makespan, Total Time (s), Time to Best (s), Success Rate (%), Node Expansions / Iterations, Max Frontier / Restarts, Timed Out flag.
