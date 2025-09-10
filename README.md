# Integer-Programming-Fantasy-NFL-Lineup-Optimization

## Project Overview
This project formulates **fantasy American football lineup selection** as a **0–1 Integer Linear Program**. Given FanDuel-like salaries, fantasy point projections, and positional eligibility for Week 12, the model selects an optimal roster under salary and position constraints to **maximize total projected fantasy points**. A follow up run computes the **second best** lineup by excluding the first solution set.

## Key Features
- Full end to end pipeline: data load → preprocessing → Pyomo model → CBC solve → roster extraction.
- Position constraints and a salary cap.
- Optional team exposure constraints such as at least one player from specific teams.
- Ability to compute the **next best lineup** by adding no-good cuts for the previously selected indices.

## Optimization Model
**Decision variables**
- x_i in {0, 1} for each player i. 1 means player i is selected in the roster.

**Objective**
- Maximize sum_i Points[i] * x_i

**Constraints**
- Roster construction by position:
  - 1 QB
  - 2 FB
  - 2 RB
  - 3 WR
  - 2 TE
- Salary cap:
  - sum_i Salary[i] * x_i ≤ 50,000
- Optional team exposure, example:
  - At least one player from KC, NE, DAL
- Second best lineup:
  - Add a constraint that excludes the exact set of indices selected in the first solve.

## Repository Structure
```
.
├─ M5_solution.ipynb                # Main notebook with the full solution
├─ README.md                        # You are here
├─ requirements.txt                 # Python dependencies
```

## Dataset
The notebook fetches a shared CSV from Google Drive and then filters Week 12. Columns used include Position, Salary, Points, and Team. You can replace the source URL with your own file if needed.

## How to Run
1. Create and activate a fresh environment.
2. Install Python deps.
3. Install a MIP solver, CBC recommended.

### Python dependencies
```
pip install -r requirements.txt
```

### CBC solver
CBC is required by Pyomo for this notebook. Options:
- macOS with Homebrew: `brew install cbc`
- Linux apt: `sudo apt-get update && sudo apt-get install -y coinor-cbc`
- conda forge: `conda install -c conda-forge coincbc`

Verify CBC is on your PATH:
```
cbc -stop
```

### Run the notebook
```
jupyter notebook M5_solution.ipynb
```

## Results
- The notebook prints the optimal **Fantasy Points** and shows the resulting **Roster** rows from the dataframe.
- The final cell demonstrates how to add constraints to retrieve the **second best** lineup.
