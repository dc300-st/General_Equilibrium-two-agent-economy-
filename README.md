# Computational General Equilibrium: Models & Simulations

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Languages](https://img.shields.io/badge/Languages-Python%20%7C%20MATLAB-orange)]()
[![Status: Research](https://img.shields.io/badge/Status-Academic%20Research-success)]()

## Overview

This repository serves as a compendium of **Computational General Equilibrium (CGE)** models, designed to bridge the gap between neoclassical economic theory and complex system dynamics.

The projects herein explore the mechanics of the **Walrasian Auctioneer**, price discovery processes, and market clearing conditions. Whilst some modules demonstrate the efficacy of the **First Fundamental Theorem of Welfare Economics** under idealised conditions, others serve as "stress tests", introducing structural frictions to simulate endogenous market failures and monopolisation.

## 📂 Repository Contents

### 1. Endogenous Monopoly Simulation (Python)
* **Objective:** A computational "stress test" of the First Welfare Theorem.
* **Methodology:** This module relaxes standard assumptions regarding convex production sets. By introducing **increasing returns to scale** (non-convexities) and **network externalities** into a Walrasian framework, the model demonstrates how a perfectly competitive market can evolve into a "Winner-Takes-All" monopoly purely through endogenous dynamics.
* **Key Features:**
    * Dynamic integration parameter ($L$) simulating market liberalisation.
    * Algorithmic representation of financial frictions and capital accumulation.
    * Visualisation of the **Herfindahl-Hirschman Index (HHI)** phase transition.

### 2. Systematised General Equilibrium Solver (MATLAB)
* **Objective:** An algorithmic approach to solving complex, multi-market equilibrium problems derived from advanced macroeconomic theory.
* **Methodology:** This module systematises the solution of a rigorous General Equilibrium problem (originally posed as a theoretical examination query). It employs MATLAB’s matrix operations to solve for vector prices and allocations that satisfy simultaneous market clearing conditions across interconnected sectors.
* **Key Features:**
    * Exact numerical solutions for utility maximisation under budget constraints.
    * Modelling of interdependence between factor markets and goods markets.
    * Robust handling of system equations for determining equilibrium price vectors.

## Theoretical Framework

The codebase is grounded in rigorous microeconomic theory, specifically focussing on:

1.  **Walrasian Price Process:** The iterative process by which excess demand ($Z(p)$) drives price adjustments:
    $$p_{t+1} = p_t + \kappa \cdot Z(p_t)$$
    
2.  **Pareto Efficiency vs. Market Failure:** Analysing the conditions under which the allocation of resources deviates from the Pareto optimal frontier due to externalities or imperfect competition.

## Usage & Installation

### Prerequisites
* **Python 3.8+** (Required libraries: `numpy`, `matplotlib`)
* **MATLAB R2020a+** (for the `.m` solver scripts)
