# MSMBS_group_9
Assignments for group 9 of the Multi-scale Modeling of Bioogical Systems course at Maastricht University


## Task 2:
The goal of this task was to change the lower and upper bounds of reactions in the E. coli model to their observed maximal activities (as described in the instructions document on canvas). 
To do this, for each reaction in the model, we checked whether their id was in the "KEN3170_Assignment_2026_e_coli_core_expression.csv" document (in other words whether we had any data about their max activity), and if it was the case we: 
1) changed the lower and upper bound of the reaction if it was reversible.
2) changed only the upper bound of the reaction if it was irreversible.

As we had checked in advance that EX_glc__D_e was not part of the csv file, the implemented function did not affect it.


