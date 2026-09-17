# Epidemiological Model Assignment — Parameter Exploration

**Course**: KEN3170 — Multi-scale modeling of biological systems
**Group number**: 9

---

## 1. Repository overview
- `analysis.ipynb` — main notebook containing all required sections
- `requirements.txt` — Python dependencies 
- `e_coli_core-1.json` — E. coli model import for calculations
- `KEN3170_Assignment_2026_e_coli_core_expression.csv` — maximal reaction activity flux values approximated from enzyme gene expression
- `README.md` — this file

**How to run**: [e.g. `pip install -r requirements.txt` then open and run `analysis.ipynb` top to bottom]

- Run "pip install -r requirements.txt" in the terminal
- Open analysis.ipynb and setup jupyter server
- When running code fields, run top-to-bottom to avoid missing dependencies and definitions

---

## 2. Part 1 — ESCHER map tool analysis
a) Inside a steady-state model simulation flux values remain equal in closed linear pathways due to mass balance constraints. However, the maximal reaction activity values are approximations stemming from enzyme gene expressions, thus they do not need to correspond to an existing steady-state. For example in glycolysis, PGI has a maximal activity of 11.1, while TPI has 70 mmol/gDW/hour. When the pathway is actually used in the model, the enzyme with lowest maximal activity can limit how much flux can pass through the whole pipeline, creating a bottleneck.

b) The tool uses grey arrows to display reactions in one of two states.
1. With 0 flux signifying the reaction is within the expression dataset, however no enzyme expression was detected, so no flux can be carried.
2. No data, meaning no meaningful constraints stemming from enzyme expression could be derived. Bounds assume an arbitrary, large default values of 1000.

---

## 3. Part 2 — Constraint specification

The goal of this task was to change the lower and upper bounds of reactions in the E. coli model to their observed maximal activities (as described in the instructions document on canvas). 
To do this, for each reaction in the model, we checked whether their id was in the "KEN3170_Assignment_2026_e_coli_core_expression.csv" document (in other words whether we had any data about their max activity), and if it was the case we: 
1) changed the lower and upper bound of the reaction if it was reversible.
2) changed only the upper bound of the reaction if it was irreversible.

As we had checked in advance that EX_glc__D_e was not part of the csv file, the implemented function did not affect it.


---

## 4. Part 3 — FBA optimization
FBA was performed under the specified reaction constraints and under different glucose uptake conditions.

---

## 5. Part 4 — Further analysis
The goal of this task was to investigate how the maximal biomass production rate changes when the glucose exchange reaction flux bound is varied.

For this analysis, the enzyme activity-constrained E. coli model from Task 2 was used. The glucose uptake bound for EX_glc__D_e was varied from 1 to 15 mmol/gDW/h in steps of 0.1, and FBA was performed for each value.

In addition to biomass production, the acetate exchange flux (EX_ac_e) was monitored to investigate how acetate secretion changes with increasing glucose availability. 

---

## 6. Conclusions

Maximal reaction activity data approximated based on enzyme gene, gives useful insights about inner workings of reaction within a cell. Although the data is not absolute, tighter constraints narrow down areas worth analyzing and speed up calculations and searches.
The data proved additionally useful in analysis of relationship between glucose intake and biomass growth rate in the model. It was found that the relationship is mostly linear with separation into 3 specific segments. In particular the second segment, a linear transition state between steady increase and a bottle-necked plateau, was explained by the activation an acetate exchange reaction. 

