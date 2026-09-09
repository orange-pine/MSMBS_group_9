# Epidemiological Model Assignment — Parameter Exploration

**Course**: KEN3170 — Multi-scale modeling of biological systems
**Group number**: 9

---

## 1. Repository overview
- `analysis.ipynb` — main notebook containing all required sections (Setup, Part 1–3, Conclusions)
- `requirements.txt` — Python dependencies (numpy, matplotlib, pandas, scipy, seaborn)
- `README.md` — this file

**How to run**: [e.g. `pip install -r requirements.txt` then open and run `analysis.ipynb` top to bottom]

---

## 2. Part 1 — Parameter analysis function
**Function**: `analyze_recovery_rates(beta, mu, N, I0, simulation_days)`
- Brief description of your approach
- Output DataFrame (γ = 0.05–0.25), matching your notebook exactly

---

## 3. Part 2 — Scenario comparison
- Result tables for Scenario A (High Transmission) and Scenario B (Low Transmission)
- Which scenario is worse for public health, and why

---

## 4. Part 3 — Policy recommendations
- 4.1 Parameter impact analysis
- 4.2 Intervention analysis
- 4.3 Real-world application

---

## 5. Conclusions
Within each of the scenarios we examined, increasing the recovery rate has consistently reduced both peak infections and total deaths. High transmission conditions produce higher peaks and more deaths, and accordingly, with the feedback we received during the tutorial, increase the strain on healthcare capacity and staff. Interventions such as Oseltamivir raise the recovery rate (and reduce the illness duration) and can lower mortality. Overall, the results highlight recovery rate improvements as an important complementary lever for epidemic control alongside measures of the transmission rates.