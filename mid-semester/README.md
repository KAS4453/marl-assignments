
---

# ECS 427/647 – Multi-Agent Reinforcement Learning

## Mid-Semester Exam Submission

**Name:** Kunwar Arpit Singh
**Course Instructor:** P.B. Sujit
**Date:** 1 March 2026

This repository contains the implementation and results for the Mid-Sem examination problems as described in the exam document .

The assignment consists of three reinforcement learning problems:

1. Zermelo’s Navigation Problem
2. Travelling Salesman Problem (20 cities)
3. Fuel-Constrained Travelling Salesman Problem

All implementations are provided as Jupyter notebooks along with generated result visualizations.

---

# 📁 Repository Structure

## Notebooks

* `22185_ms_q1.ipynb` → Zermelo’s Navigation Problem
* `22185_ms_q2.ipynb` → TSP using Policy Gradient (REINFORCE)
* `22185_ms_q3.ipynb` → Fuel-Constrained TSP

---

## Question 1 – Zermelo’s Navigation Problem
Implementation: `22185_ms_q1.ipynb` 

## RL Formulation

* **State:** Discretized grid position (x, y)
* **Action:** 8 discrete heading directions
* **Reward:**

  * −1 per step
  * +100 on reaching goal
  * −50 on timeout
* **Objective:** Minimize time to reach destination

## Method Used

### Q-Learning (Tabular, Model-Free)

* Off-policy TD control
* Q-table of size `(grid_size × grid_size × actions)`
* Epsilon-greedy exploration
* Exponential epsilon decay
* Discount factor γ = 0.99

### Why Q-Learning?

* Environment is fully observable after discretization
* State space is small enough for tabular learning
* Off-policy learning enables stable convergence
* Suitable for deterministic dynamics with time-penalty reward structure

The final greedy policy produces the optimal navigation trajectory under current flow.

### Output Visualizations

* `q1_path.png` → Optimal navigation trajectory
* `q1_training_1000.png`
* `q1_training_2000.png`
* `q1_training_3000.png`
* `q1_training_5000.png`
* `q1_training_10000.png`
* `q1_training_50000.png`
* `q1_training_100000.png`
* `q1_training_1000000.png`

These images show learning progression and convergence behavior over increasing training steps.

### Summary

* Problem formulated as an MDP with discretized state space.
* Solved using Q-Learning.
* Objective: Minimize time to reach destination (maximize negative time reward).
* Environment accounts for current velocity equal to half boat speed.

---

## Question 2 – Travelling Salesman Problem (20 Cities)

Implementation: `22185_ms_q2.ipynb` 

## RL Formulation

* **State:**

  * Current city (one-hot, 20)
  * Visited mask (20)
    → 40-dimensional input

* **Action:** Next unvisited city (masked softmax)

* **Reward:**
  Negative total Euclidean tour length
  (computed at episode end)

* **Episode ends:** After visiting all cities and returning to start

## Method Used

### REINFORCE (Monte Carlo Policy Gradient)

* Two-layer MLP (implemented from scratch using NumPy)
* ReLU hidden layer
* Masked softmax for valid actions
* Episodic return used for gradient update
* Moving-average baseline to reduce variance
* Gradient clipping for stability

### Why Policy Gradient?

* TSP is combinatorial and high-dimensional
* Action space depends on visited cities
* Q-table is infeasible due to factorial state space
* Policy directly learns permutation distribution
* Masking integrates naturally into softmax policy

The best sampled tour significantly improves over random permutations.

### Output Visualizations

* `q2_tour.png` → Final learned tour
* `q2_training.png` → Training performance curve

### Summary

* State representation:

  * Current city (one-hot encoding)
  * Visited mask
* Action space: Unvisited cities (masked softmax)
* Method used: REINFORCE Policy Gradient
* Reward: Negative total Euclidean tour length

The agent learns a permutation policy to generate near-optimal tours.

---

## Question 3 – Fuel-Constrained TSP

Implementation: `22185_ms_q3.ipynb` 

## RL Formulation

* **State:**

  * Current city (20)
  * Visited mask (20)
  * Normalized remaining fuel (1)
    → 41-dimensional input

* **Action:** Next reachable, unvisited city
  (masked by visited and fuel constraint)

* **Reward:** +1 per city visited

* **Episode ends when:**

  * No reachable cities remain
  * All cities visited

* **Objective:** Maximize number of cities visited

## Method Used

### REINFORCE with Baseline

* Two-layer MLP (NumPy implementation)
* Fuel included in state representation
* Masking enforces fuel feasibility
* Moving-average baseline
* Gradient clipping

### Why Policy Gradient?

* State is continuous due to fuel variable
* Action feasibility changes dynamically
* Objective shifts from minimizing length to maximizing coverage
* Tabular Q-learning is not scalable
* Direct policy optimization handles constraints naturally

### Output Visualizations

* `q3_tour.png` → Best tour under fuel constraint
* `q3_training.png` → Training performance
* `q3_fuel_variation.png` → Effect of varying fuel capacity

### Summary

* Modified TSP with fuel budget constraint.
* Episode terminates when fuel is exhausted.
* Objective: Maximize number of cities visited (or total reward collected).
* Policy Gradient–based solution used.
* Additional experiment: performance vs fuel capacity.

---

# 🛠️ Dependencies

The code requires the following Python packages:

```
numpy
matplotlib
jupyter
```

If using pip:

```bash
pip install numpy matplotlib jupyter
```

Python version recommended: **Python 3.9+**

---

# ▶️ How to Run

1. Install dependencies.
2. Open terminal in project directory.
3. Launch Jupyter Notebook:

```bash
jupyter notebook
```

4. Open:

   * `22185_ms_q1.ipynb`
   * `22185_ms_q2.ipynb`
   * `22185_ms_q3.ipynb`

5. Run all cells sequentially.

Each notebook:

* Defines the environment
* Implements the RL algorithm
* Trains the agent
* Generates and saves plots

---

# 📊 Results Overview

* Q1 converges to time-optimal navigation path.
* Q2 learns structured tours for randomly generated 20-city TSP.
* Q3 demonstrates trade-off between fuel capacity and cities visited.

Training curves and final visualizations are included as PNG files.

---

# 📌 Notes

* All results are reproducible by running the notebooks.
* Random seeds are set where applicable for consistency.
* Code is modular and clearly commented for evaluation.

---