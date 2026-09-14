# MSMBS_group_9
Assignments for group 9 of the Multi-scale Modeling of Bioogical Systems course at Maastricht University


## Task 2:
The goal of this task was to change the lower and upper bounds of reactions in the E. coli model to their observed maximal activities (as described in the instructions document on canvas). 
To do this, for each reaction in the model, we checked whether their id was in the "KEN3170_Assignment_2026_e_coli_core_expression.csv" document (in other words whether we had any data about their max activity), and if it was the case we: 
1) changed the lower and upper bound of the reaction if it was reversible.
2) changed only the upper bound of the reaction if it was irreversible.

As we had checked in advance that EX_glc__D_e was not part of the csv file, the implemented function did not affect it.

## Task 4
The goal of this task was to investigate how the maximal biomass production rate changes when the glucose exchange reaction flux bound is varied.

For this analysis, the enzyme activity-constrained E. coli model from Task 2 was used. The glucose uptake bound for EX_glc__D_e was varied from 1 to 15 mmol/gDW/h in steps of 0.1, and FBA was performed for each value.
The growth rate increased at first, but then it levelled at about 0.87 $h^{-1}$. around 10.5–11 mmol/gDW/h, showing that glucose stopped being the limiting factor beyond this point.
