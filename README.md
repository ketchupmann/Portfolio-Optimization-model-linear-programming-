# Constrained Portfolio Optimization via Linear Programming

**Author:** Thibaut Chen
**Date:** December 2025
**Tech Stack:** Python, CVXPY, NumPy, Pandas, Matplotlib

## 1. Executive Summary
This project implements a quantitative portfolio optimization engine that moves beyond standard Mean-Variance theory by incorporating real-world investment mandates. Using **Linear Programming (LP)**, the model constructs an optimal equity portfolio that maximizes expected daily returns while strictly adhering to "hard" risk constraints, such as sector exposure caps and position limits.

Key feature: The model performs **Sensitivity Analysis (Duality)** to calculate the "Shadow Price" of each constraint, quantifying exactly how much specific risk rules cost the portfolio in terms of potential return.

## 2. Mathematical Formulation
Unlike Quadratic Programming (Markowitz), this model utilizes a Linear Programming framework to handle complex constraint sets efficiently.

**Objective Function:**
$$\text{Maximize } Z = \mu^T w$$

**Subject to Constraints:**
1.  **Budget Identity:** $\sum w_i = 1$ (Fully Invested)
2.  **Long-Only Mandate:** $w_i \ge 0$ (No Short Selling)
3.  **Concentration Limit:** $w_i \le 0.25$ (Max 25% in any single asset)
4.  **Sector Risk Cap:** $\sum_{j \in \text{Tech}} w_j \le 0.35$ (Max 35% Tech exposure)

## 3. Methodology
* **Data Pipeline:**
    * Ingests historical adjusted close prices via `yfinance`.
    * Implements a robust fallback mechanism that generates synthetic market data (Geometric Brownian Motion) using `numpy` if the live API is rate-limited.
* **Optimization Engine:**
    * Utilizes **CVXPY** with the **OSQP** solver for efficient convex optimization.
    * Calculates the dual variables (shadow prices) for every constraint.
* **Visualization:**
    * Reconstructs historical price paths to verify data integrity.
    * Plots the expected return vector ($\mu$) and final optimal weights.

## 4. Key Results (Example)
* **Unconstrained:** The solver allocates 100% of capital to the single asset with the highest historical drift.
* **Constrained:** The solver creates a diversified portfolio (e.g., 25% JPM, 25% AAPL, 15% MSFT...) that lies on the boundary of the feasible region defined by the risk limits.
* **Shadow Price Insight:** The Tech Sector cap often yields a positive shadow price (e.g., +0.05%), indicating that the risk constraint is "binding" and actively reducing theoretical returns in exchange for safety.

## 5. How to Run
1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/](https://github.com/)[ketchupmann]/Portfolio-Optimization-model-linear-programming-.git
    ```
2.  **Create the environment:**
    ```bash
    conda create -n quant_club python=3.9 pandas numpy matplotlib cvxpy yfinance
    conda activate quant_club
    ```
3.  **Run the analysis:**
    Open `portfolio_optimization.ipynb` in VS Code or Jupyter Lab and run all cells.