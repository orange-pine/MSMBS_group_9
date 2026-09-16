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

PFK | lower bound:  0.0 AND upper bound:  13.1
PFL | lower bound:  0.0 AND upper bound:  0.0
PGI | lower bound:  -11.1 AND upper bound:  11.1
PGK | lower bound:  -24.0 AND upper bound:  24.0
PGL | lower bound:  0.0 AND upper bound:  7.3
ACALD | lower bound:  -0.0 AND upper bound:  0.0
AKGt2r | lower bound:  -0.0 AND upper bound:  0.0
PGM | lower bound:  -21.7 AND upper bound:  21.7
PIt2r | lower bound:  -5.2 AND upper bound:  5.2
ALCD2x | lower bound:  -0.0 AND upper bound:  0.0
ACALDt | lower bound:  -1000.0 AND upper bound:  1000.0
ACKr | lower bound:  -2.5 AND upper bound:  2.5
PPC | lower bound:  0.0 AND upper bound:  3.6
ACONTa | lower bound:  -21.4 AND upper bound:  21.4
ACONTb | lower bound:  -21.4 AND upper bound:  21.4
ATPM | lower bound:  8.39 AND upper bound:  1000.0
PPCK | lower bound:  0.0 AND upper bound:  13.3
ACt2r | lower bound:  -3.6 AND upper bound:  3.6
PPS | lower bound:  0.0 AND upper bound:  3.1
ADK1 | lower bound:  -27.4 AND upper bound:  27.4
AKGDH | lower bound:  0.0 AND upper bound:  26.7
ATPS4r | lower bound:  -80.1 AND upper bound:  80.1
PTAr | lower bound:  -4.47 AND upper bound:  4.47
PYK | lower bound:  0.0 AND upper bound:  28.2
BIOMASS_Ecoli_core_w_GAM | lower bound:  0.0 AND upper bound:  1000.0
PYRt2 | lower bound:  -0.0 AND upper bound:  0.0
CO2t | lower bound:  -1000.0 AND upper bound:  1000.0
RPE | lower bound:  -6.3 AND upper bound:  6.3
CS | lower bound:  0.0 AND upper bound:  21.4
RPI | lower bound:  -5.6 AND upper bound:  5.6
SUCCt2_2 | lower bound:  0.0 AND upper bound:  0.0
CYTBD | lower bound:  0.0 AND upper bound:  41.1
D_LACt2 | lower bound:  -0.0 AND upper bound:  0.0
ENO | lower bound:  -29.3 AND upper bound:  29.3
SUCCt3 | lower bound:  0.0 AND upper bound:  0.0
ETOHt2r | lower bound:  -1000.0 AND upper bound:  1000.0
SUCDi | lower bound:  0.0 AND upper bound:  27.3
SUCOAS | lower bound:  -19.4 AND upper bound:  19.4
TALA | lower bound:  -4.5 AND upper bound:  4.5
THD2 | lower bound:  0.0 AND upper bound:  6.5
TKT1 | lower bound:  -3.5 AND upper bound:  3.5
TKT2 | lower bound:  -3.5 AND upper bound:  3.5
TPI | lower bound:  -70.0 AND upper bound:  70.0
EX_ac_e | lower bound:  0.0 AND upper bound:  1000.0
EX_acald_e | lower bound:  0.0 AND upper bound:  1000.0
EX_akg_e | lower bound:  0.0 AND upper bound:  1000.0
EX_co2_e | lower bound:  -1000.0 AND upper bound:  1000.0
EX_etoh_e | lower bound:  0.0 AND upper bound:  1000.0
EX_for_e | lower bound:  0.0 AND upper bound:  1000.0
EX_fru_e | lower bound:  0.0 AND upper bound:  1000.0
EX_fum_e | lower bound:  0.0 AND upper bound:  1000.0
EX_glc__D_e | lower bound:  -1000 AND upper bound:  1000.0
EX_gln__L_e | lower bound:  0.0 AND upper bound:  1000.0
EX_glu__L_e | lower bound:  0.0 AND upper bound:  1000.0
EX_h_e | lower bound:  -1000.0 AND upper bound:  1000.0
EX_h2o_e | lower bound:  -1000.0 AND upper bound:  1000.0
EX_lac__D_e | lower bound:  0.0 AND upper bound:  1000.0
EX_mal__L_e | lower bound:  0.0 AND upper bound:  1000.0
EX_nh4_e | lower bound:  -1000.0 AND upper bound:  1000.0
EX_o2_e | lower bound:  -1000.0 AND upper bound:  1000.0
EX_pi_e | lower bound:  -1000.0 AND upper bound:  1000.0
EX_pyr_e | lower bound:  0.0 AND upper bound:  1000.0
EX_succ_e | lower bound:  0.0 AND upper bound:  1000.0
FBA | lower bound:  -30.4 AND upper bound:  30.4
FBP | lower bound:  0.0 AND upper bound:  1.2
FORt2 | lower bound:  0.0 AND upper bound:  1000.0
FORt | lower bound:  -1000.0 AND upper bound:  0.0
FRD7 | lower bound:  0.0 AND upper bound:  15.6
FRUpts2 | lower bound:  0.0 AND upper bound:  3.0
FUM | lower bound:  -24.4 AND upper bound:  24.4
FUMt2_2 | lower bound:  0.0 AND upper bound:  0.0
G6PDH2r | lower bound:  -6.5 AND upper bound:  6.5
GAPD | lower bound:  -24.5 AND upper bound:  24.5
GLCpts | lower bound:  0.0 AND upper bound:  21.1
GLNS | lower bound:  0.0 AND upper bound:  4.5
GLNabc | lower bound:  0.0 AND upper bound:  0.3
GLUDy | lower bound:  -7.6 AND upper bound:  7.6
GLUN | lower bound:  0.0 AND upper bound:  5.4
GLUSy | lower bound:  0.0 AND upper bound:  7.3
GLUt2r | lower bound:  -0.0 AND upper bound:  0.0
GND | lower bound:  0.0 AND upper bound:  5.6
H2Ot | lower bound:  -1000.0 AND upper bound:  1000.0
ICDHyr | lower bound:  -12.4 AND upper bound:  12.4
ICL | lower bound:  0.0 AND upper bound:  3.4
LDH_D | lower bound:  -0.0 AND upper bound:  0.0
MALS | lower bound:  0.0 AND upper bound:  4.5
MALt2_2 | lower bound:  0.0 AND upper bound:  0.0
MDH | lower bound:  -7.8 AND upper bound:  7.8
ME1 | lower bound:  0.0 AND upper bound:  3.3
ME2 | lower bound:  0.0 AND upper bound:  3.4
NADH16 | lower bound:  0.0 AND upper bound:  40.1
NADTRHD | lower bound:  0.0 AND upper bound:  1.3
NH4t | lower bound:  -1000.0 AND upper bound:  1000.0
O2t | lower bound:  -1000.0 AND upper bound:  1000.0
PDH | lower bound:  0.0 AND upper bound:  26.6

---

## 4. Part 3 — FBA optimization
For given constraints the maximal biomass production production rate is about 0.8732. Unconstrained glucose uptake ends up at about 10.62 mmol/gDW/hour, suggesting the presence of other limiting factors at play.

All bounds up to this point were either arbitrary or expression-based, meaning their values are tied to how much flux their corresponding enzymes can carry. This in turn depends on the amounts of each enzyme is expressed in the cell based on genetic material. These constraints are internal and stem directly from the systems structure. 
The new bound on glucose exchange represents an external constraint introduced by the environment. Its consequences can cascade through the entire system and affect the objective function or even prevent the system from working altogether. In this specific case it could represent a situation when the cell might have limited access to nutrition, stunting its ability to function and grow.

With the glucose bound of 5 mmol/gDW/hour the maximal biomass production rate drops from about 0.8732 to 0.4156. Glucose availability is now the bottleneck and the enzyme capacities are no longer reached, so less carbon and energy are available for biomass and growth rate goes down. The results suggest a strong, approximately linear relationship between glucose intake and biomass production at this scale.

| Condition           | Glucose uptake bound | Biomass growth rate |
|---------------------|----------------------|---------------------|
| High glucose uptake | 10.62 mmol/gDW/h     | 0.8732 h⁻¹          |
| Low glucose uptake  | 5 mmol/gDW/h         | 0.4156 h⁻¹          |


---

## 5. Part 4 — Further analysis
The glucose exchange reaction bound (EX_glc__D_e) was varied from 1 to 15 mmol/gDW/h in increments of 0.1. For each value, an FBA optimization was performed and the maximal biomass production rate was plotted against the glucose uptake bound.

Results show the growth rate does not increase indefinitely with increasing glucose exchange flux bound. The maximal biomass production rate first increases almost linearly as the glucose uptake bound increases, meaning that the glucose availability is limiting growth in this range. However, at about 10.62 mmol/gDW/h, the curve reaches a plateau of 0.87 $h^{-1}$ approx. Beyond this point, allowing more glucose uptake does not increase the predicted growth rate, indicating that glucose is no longer the limiting factor and that another constraint in the enzyme activity-constrained metabolic model becomes limiting.

In the second part acetate exchange (EX_ac_e) reaction becomes active. At a bound of 5 no acetate is secreted while at a bound of 10 about 1.25 mmol/gDW/hour is secreted. At the plateau it stays at 2.5. Since oxygen update is limited, the extra glucose cannot be fully utilized and part of the carbon is then secreted as acetate.

---


## 6. Conclusions

Maximal reaction activity data approximated based on enzyme gene, gives useful insights about inner workings of reaction within a cell. Although the data is not absolute, tighter constraints narrow down areas worth analyzing and speed up calculations and searches.
The data proved additionally useful in analysis of relationship between glucose intake and biomass growth rate in the model. It was found that the relationship is mostly linear with separation into 3 specific segments. In particular the second segment, a linear transition state between steady increase and a bottle-necked plateau, was explained by the activation an acetate exchange reaction. 

