# AMPL - Production / Inventory example

Problem
-------
A company must meet the following on-time demands:
- Quarter 1: 30 units
- Quarter 2: 20 units
- Quarter 3: 40 units
- Quarter 4: 70 units

There are three worker types. Each quarter:
- Type 1: up to 15 units, cost $40 / unit
- Type 2: up to 20 units, cost $60 / unit
- Type 3: up to 25 units, cost $80 / unit

Production defects and spoilage:
- 20% of produced units are unsuitable and cannot be used to meet demand.
- At the end of each quarter, 10% of units on hand spoil and cannot be used for future demands.

Inventory and costs:
- Holding cost: $15 per unit on hand at the end of each quarter.
- Initial usable inventory: 20 units at the beginning of quarter 1.

Objective: Minimize total cost of production and inventory holding while meeting all demands on time.

How to run (AMPL)
-----------------
1. Start AMPL and select a solver (e.g., Gurobi(which is what I used)):
   ampl: option solver gurobi;
2. Load the model and the data with .txt files:
   ampl: model github1a.txt;
   ampl: data github1adata.txt;
3. Solve and display the variables:
   ampl: solve;
   ampl: display x, I;

Expected solver output
-----------------------
```text
ampl: option solver gurobi;
ampl: model github1a.txt
ampl: data github1adata.txt
ampl: solve;
Gurobi 12.0.3: optimal solution; objective 11436.70782
4 simplex iterations
ampl: display x,I;
x :=
1 1 15
1 2 11.7661
1 3 0
2 1 15
2 2 20
2 3 0
3 1 15
3 2 20
3 3 25
4 1 15
4 2 20
4 3 25
;
I [*] :=
0 20
1 10.2716
2 16.4444
3 22
4 0
;
ampl:
```

Interpretation
--------------
- The solver chooses production close to capacity for many quarters (see `x`).
- End-of-quarter inventories (`I`) show the carryover that helps meet future demand after accounting for spoilage and unsuitable units.
- Objective value is the sum of production costs (c[j] * x[i,j]) plus inventory holding costs.
- The overall optimal production strategy has a cost of 11,436.70782$
