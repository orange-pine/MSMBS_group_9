# Assignment: Network biology (Week 3) 

**Course**: KEN3170 — Multi-scale modeling of biological systems
**Group number**: 9
---
 ### How to run the code


---
The aim of this assignment is to study how cancer-causing mutations change the behaviour of a Boolean model of a cell regulatory network. The normal network was compared with four mutated versions of the network.

For each network, the three scenarios from the practical were simulated and an attractor analysis was performed. The attractor analysis checks all 256 possible initial states and determines which stable states they reach. A state was considered cancer-like when Growth = ON and Death = OFF.

The four mutations tested were:

Mutation A: p53 Knockout (Loss of tumor suppressor):   network.add_rule('p53', lambda s: False, "p53 = BROKEN (always OFF)")

Mutation B: MYC Amplification (Oncogene overexpression):   network.add_rule('MYC', lambda s: True, "MYC = AMPLIFIED (always ON)")

Mutation C: MDM2 Overexpression (p53 pathway disruption):   network.add_rule('MDM2', lambda s: True, "MDM2 = OVEREXPRESSED (always ON)")

Mutation D: Design of our choice: p21 OFF + MYC ON — p21 is always OFF and MYC is always ON.

Before each mutation, the network is reset to the original rules so that the mutations are tested independently.

---
### Questions

##### Question 1: Which mutation is most dangerous and why? Provide quantitative evidence.
Answer:  
Following our analysis we did not identify only one mutation as the most dangerous. For all four mutations, 256 out of 256 possible initial states (so a 100%) reached a cancer-like attractor. Each mutation had two attractors with basin sizes of 128 states (50%) each. Therefore, according to this model and this measure, the four mutations were equally dangerous.
##### Question 2: Explain the role of feedback loops (e.g., MYC → MDM2 → p53)
Answer:

//Gonna expand on this later

Inside a healthy cell, p53 as well as the subsequent p21, are responsible for stunting its growth and causing eventual death. However, in case of either sustained overexpression of MDM2/MYC or underexpression of p53, a negative feedback loop gets triggered. MYC leads to expression of MDM2 which inhibits the expression 53, consequently increasing the expression of MYC again. This feedback loop is particularly dangerous, as it prevents apoptosis and stimulates growth regardless of the damage to the DNA, leading to the cell potentially becoming cancerous.

##### Question 3: What are the limitations of this Boolean network model? Discuss 3 specific limitations.

Answer: 
1) Genes are represented as either on or off, so we lose information about potential in-between situations (where states are in a state of balance between 0 and 1).
2) The time it takes to go from one state to the other is also disregarded, only the discrete states are preserved.
3) In this specific Boolean model, we need to specify a number of steps we want to simulate and hope that the steady state lies below that number. There is no function to automatically find the number of steps it takes to reach a steady state or to identify whether the state is unstable and how long is the potential cycle.