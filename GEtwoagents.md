
# General Equilibrium Model (Symbolic Solver)
```
Repository: Economics-GE-Model
Description: Solves for competitive equilibrium in a production economy.
Language: MATLAB (Symbolic Math Toolbox)
```
```matlab
clear; clc;
```
# 1. Initialization and Assumptions

Define symbolic variables. IMPORTANT: We assume k > 0 to ensure real solutions for sqrt functions.

```matlab
syms px py pz k real positive
syms z_alpha z_beta real positive
syms IA IB real

% Numeraire Assumption: Price of input Z is normalized to 1.
pz = 1; 
```
# 2. Firm Optimization (Profit Maximization)
```matlab
% --- Firm Alpha (Production: x = 2z) ---
% Market: Perfect Competition, CRS.
% Equilibrium Condition: Price = Marginal Cost
eq_firm_alpha = 2*px - pz == 0; 

% --- Firm Beta (Production: y = sqrt(z)) ---
% Market: Perfect Competition, DRS.
% Optimization: Maximize Profit = py*sqrt(z) - pz*z
% FOC: d(Profit)/dz = 0
eq_firm_beta = 0.5 * py * z_beta^(-0.5) - pz == 0;

% Firm Beta's Profit Function (symbolic)
profit_beta_func = py * sqrt(z_beta) - pz * z_beta;
```
# 3. Consumer Optimization (Utility Maximization)
```matlab
% --- Incomes ---
% Consumer A: Owns initial endowment k of Z.
Income_A = k * pz; 

% Consumer B: Owns Firm Beta (receives profit).
Income_B = profit_beta_func;

% --- Demands (Cobb-Douglas Preferences U = xy) ---
% Consumers spend 50% of income on each good.
demand_xA = (0.5 * Income_A) / px;
demand_yA = (0.5 * Income_A) / py;

demand_xB = (0.5 * Income_B) / px;
demand_yB = (0.5 * Income_B) / py;
```
# 4. Market Clearing Conditions
```matlab
% Market for Input Z (Labor/Capital)
% Endowment = Firm Alpha Input + Firm Beta Input
eq_market_Z = k == z_alpha + z_beta;

% Market for Good Y
% Supply Y = Total Demand Y
supply_Y = sqrt(z_beta);
total_demand_Y = demand_yA + demand_yB;
eq_market_Y = supply_Y == total_demand_Y;
```
# 5. Solving the System

Solve for \[px, py, z\_alpha, z\_beta\]

```matlab
S = solve([eq_firm_alpha, eq_firm_beta, eq_market_Z, eq_market_Y], ...
          [px, py, z_alpha, z_beta], 'ReturnConditions', true);
```
# 6. Filtering Solutions

'solve' may return multiple roots (positive/negative). We must select the economically valid solution (Price > 0).

```matlab
% Check which solution index yields a positive Py
idx = find(isAlways(S.py > 0));

if isempty(idx)
    warning('No positive solution found. Checking constraints.');
    idx = 1; % Fallback
end

% Extract the valid symbolic expressions
px_sol = S.px(idx);
py_sol = S.py(idx);
za_sol = S.z_alpha(idx);
zb_sol = S.z_beta(idx);
```
# 7. Verification and Welfare Analysis
```matlab
% A. Verify Walras' Law (Market Clearing)
% Check if Supply equals Demand for Good Y at equilibrium prices.
% We substitute the found values back into the supply and demand equations.
chk_supply_Y = subs(supply_Y, z_beta, zb_sol);
chk_demand_Y = subs(total_demand_Y, [px, py, z_alpha, z_beta], [px_sol, py_sol, za_sol, zb_sol]);

excess_demand = simplify(chk_demand_Y - chk_supply_Y);

% B. Calculate Welfare (Indirect Utility)
% Substitute equilibrium values into Income and Utility functions
Inc_A_val = subs(Income_A, k, k); % Identity
Inc_B_val = subs(Income_B, [py, z_beta], [py_sol, zb_sol]);

% Allocations
xA_final = (0.5 * Inc_A_val) / px_sol;
yA_final = (0.5 * Inc_A_val) / py_sol;
UA = simplify(xA_final * yA_final);

xB_final = (0.5 * Inc_B_val) / px_sol;
yB_final = (0.5 * Inc_B_val) / py_sol;
UB = simplify(xB_final * yB_final);

Welfare_Ratio = simplify(UA / UB);
```
# 8. Display Results
```matlab
fprintf('==================================================\n');
fprintf('       GENERAL EQUILIBRIUM RESULTS (Symbolic)     \n');
fprintf('==================================================\n');
fprintf('1. Equilibrium Prices:\n');
fprintf('   Px (Price of X) : %s\n', px_sol);
fprintf('   Py (Price of Y) : %s\n', py_sol);
fprintf('\n');
fprintf('2. Resource Allocation (Input Z):\n');
fprintf('   Firm Alpha Use  : %s\n', za_sol);
fprintf('   Firm Beta Use   : %s\n', zb_sol);
fprintf('\n');
fprintf('3. Market Clearing Check (Walras Law):\n');
if excess_demand == 0
    fprintf('   [SUCCESS] Excess Demand is 0. Markets clear.\n');
else
    fprintf('   [WARNING] Excess Demand is not zero: %s\n', excess_demand);
end
fprintf('\n');
fprintf('4. Welfare Analysis:\n');
fprintf('   Utility A       : %s\n', UA);
fprintf('   Utility B       : %s\n', UB);
fprintf('   Ratio (UA/UB)   : %s\n', Welfare_Ratio);
fprintf('==================================================\n');
```
