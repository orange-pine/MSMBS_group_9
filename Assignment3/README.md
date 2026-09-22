# Assignment: Network biology (Week 3) 

**Course**: KEN3170 — Multi-scale modeling of biological systems
**Group number**: 9

---

### Questions

##### Question 1: Which mutation is most dangerous and why? Provide quantitative evidence.

##### Question 2:

##### Question 3: What are the limitations of this Boolean network model? Discuss 3 specific limitations.

Answer: 
1) Genes are represented as either on or off, so we lose information about potential in-between situations (where states are in a state of balance between 0 and 1).
2) The time it takes to go from one state to the other is also disregarded, only the discrete states are preserved.
3) In this specific Boolean model, we need to specify a number of steps we want to simulate and hope that the steady state lies below that number. There is no function to automatically find the number of steps it takes to reach a steady state or to identify whether the state is unstable and how long is the potential cycle.