# Epidemiological Model Assignment — Parameter Exploration

**Course**: KEN3170 — Multi-scale modeling of biological systems
**Group number**: 9

---

## 1. Repository overview
- `analysis.ipynb` — main notebook containing all required sections (Setup, Part 1–3, Conclusions)
- `requirements.txt` — Python dependencies (numpy, matplotlib, pandas, scipy, seaborn)
- `README.md` — this file

**How to run**: [e.g. `pip install -r requirements.txt` then open and run `analysis.ipynb` top to bottom]

- Run "pip install -r requirements.txt" in the terminal
- Open analysis.ipynb and setup jupyter server
- When running code fields, run top-to-bottom to avoid missing dependencies and definitions

---

## 2. Part 1 — Parameter analysis function
**Function**: `analyze_recovery_rates(beta, mu, N, I0, simulation_days)`
- The function defines a SIRD model and simulates its progress over [simulation_days]
steps using scipy.integrate.odeint. The results are tracked and stored inside a dataframe
according to specification. Following that a nested graphic function is defined and
subsequently ran.
- Example output dataframe: 
  gamma   R0  peak_infected  peak_day  total_deaths
0   0.05  6.0            158        29           533
1   0.10  3.0             69        33           296
2   0.15  2.0             23        32           139
3   0.20  1.5             10         0            45
4   0.25  1.2             10         0            16

---

## 3. Part 2 — Scenario comparison
- Result tables for Scenario A (High Transmission) and Scenario B (Low Transmission):

    gamma        R0  peak_infected  peak_day  total_deaths
0   0.05  8.000000            520        21           284
1   0.10  4.000000            340        22           159
2   0.15  2.666667            213        24           102
3   0.20  2.000000            123        27            67
4   0.25  1.600000             63        30            42

    gamma        R0  peak_infected  peak_day  total_deaths
0   0.05  4.000000            284        46           265
1   0.10  2.000000             96        56           113
2   0.15  1.333333             16        70            35
3   0.20  1.000000              5         0             3
4   0.25  0.800000              5         0             1
#### Analysis

- The first codition is worse for public heatlh than the second one. Two main reasons for this:
- The first is that the peak of infected people is systematically higher (regardless of the gamma being modeled),
which means that hospitals will have to take care of more people at once, this may lead to overwhelmed hospitals.
This in turn will probably affect the quality of the care being given in those hospitals, and so result in a less
efficient recovery for sick people.
- Another reason why the first is worse is that the total number of dead people is also systematically higher.
This can effect the moral (mental state) of the medical staff.
- As a conclusion the first scenario where transmission is high, is likelier to wear out people working in public health,
both phyiscally and psychologically, which is bound to have a negative effect on their work.

---

## 4. Part 3 — Policy recommendations
- 4.1 Parameter impact analysis:

Increasing the recovery rate (gamma has an strong negative relationship with both peak infections and total deaths.
In the first set of results, increasing gamma from 0.05 to 0.25 reduces the peak number of infected people
from 158 to 10, a decrease of 148 people or approximately 94%. Total deaths decrease from 533 to 16,
which is a reduction of approximately 97%. Peak infections decrease consistently by around 56–67%
for every 0.05 increase until gamma=0.20, after which the peak remains stable at 10. Total deaths also decrease
strongly, by roughly 45–68% for each 0.05 increase in gamma.

The same relationship is visible in both transmission scenarios, with some slight variation. In the high transmission
scenario, peak infections decrease from 520 to 63 as gamma increases from 0.05 to 0.25, corresponding
to an approximately 88% reduction. Deaths decrease from 284 to 42, or approximately 85%. In the low transmission
scenario, the effect is even stronger, peak infections decrease from 371 to 5, approximately 99%, while deaths decrease
from 88 to 0. This suggests that increasing the recovery rate may be more effective at controlling the epidemic
when the transmission rate is already relatively low.

In the high transmission scenario, the peak becomes smaller but occurs later, moving from day 21 at gamma=0.05 to day 30
at gamma=0.25 . A similar pattern initially occurs in the low transmission scenario, where the peak moves from day 44 
to day 67 as gamma increases from 0.05 to 0.15. However, at gamma=0.20 and gamma=0.25, the recorded peak 
changes suddenly to day 0. This indicates that the initial number of infected people is already the maximum and that 
the infection does not grow into a larger epidemic. The same behaviour is seen in the first set of results, where the 
peak changes to day 0 at the two highest recovery rates.

In general, a higher recovery rate is expected to shorten the epidemic because infected individuals leave the infected 
compartment more quickly. The results appear to stabilize at the highest tested recovery rates, particularly between 
gamma=0.20 and gamma=0.25. However, more values above gamma=0.25 would be needed to determine whether this pattern
continues, especially given the consistent increase in peak day observed in the high transmission scenario.

In conclusion, increasing the recovery rate reduces peak infections and deaths. Its effect on the timing of the 
epidemic is not linear. At lower and intermediate recovery rates, the epidemic peak may occur later while becoming
smaller, while at sufficiently high recovery rates the outbreak may no longer grow beyond the initially infected 
population, which suggests that the epidemic may also become shorter.


- 4.2 Intervention analysis:
#%% md
#### 3.2

Using Scenario A as the baseline, if a recovery rate of gamma=0.10 results in 159 total deaths and an intervention 
increases the recovery rate by 50%, the new recovery rate would be:

0.10 * 1.5 = 0.15

At gamma=0.15, Scenario A results in 102 total deaths. Therefore, the intervention would reduce the expected number
of deaths by:

159 - 102 = 57

This results in a percentage reduction of:

(57/159) * 100 ~= 35.8%

Therefore, a 50% increase in recovery rate has approximately 57 fewer deaths, or a 36% reduction in total deaths,
in Scenario A. This suggests that interventions that allow infected individuals to recover more quickly  could have
a postivie effect on survival.

(used baseline recovery rate of 0.1 because it is the only tested value that has another tested value when increasing
the rate by 50%)




- 4.3 Real-world application:

#### 3.3
A real medical intervention that could increase the recovery rates is oseltamivir (Tamiflu) a medicine specified
in treating influenza. Oseltamivir is a neuraminidase inhibitor, meaning that it blocks a protein that influenza
viruses need to release newly formed virus particles from infected cells. By reducing further viral spread within
the body, the treatment can reduce the duration of influenza symptoms.

When started within 48 hours after symptoms appear oseltamivir can shorten influenza illness by approximately one day.
This estimate can be represented in the SIRD model as an increase in the recovery rate gamma. For illustration,
lets assume that an untreated influenza infection lasts approximately 6 days.

The corresponding untreated recovery rate would be:

gamma_untreated = 1 / 6 ~= 0.167

If oseltamivir reduces the illness duration by approximately one day, the duration would decrease from 6 to 5 days:

gamma_treated = 1/5 = 0.20

Hence, the relative increase in recovery rate would be:

((0.20 - 0.167) / 0.167)  * 100 ~= 20


Under this simplified assumption, shortening illness duration by one day could be represented as approximately a 20%
increase in the recovery rate gamma.

However, this is only an approximation. The effectiveness of oseltamivir depends strongly on how early treatment
is started and can vary between patients. In addition, symptom duration is not exactly the same as the infectious 
period represented by gamma in the SIRD model. This model assumes one constant recovery rate for the whole infected 
population, while real patients differ in age, disease severity, health conditions and response to treatment. 
Therefore, representing oseltamivir as a 20% increase in gamma is useful for imagining its possible effect 
in the model, but it should not be interpreted as an exact real-world treatment effect.

---

## 5. Conclusions
Within each of the scenarios we examined, increasing the recovery rate has consistently reduced both peak infections and total deaths. High transmission conditions produce higher peaks and more deaths, and accordingly, with the feedback we received during the tutorial, increase the strain on healthcare capacity and staff. Interventions such as Oseltamivir raise the recovery rate (and reduce the illness duration) and can lower mortality. Overall, the results highlight recovery rate improvements as an important complementary lever for epidemic control alongside measures of the transmission rates.