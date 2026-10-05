# KEN3170 – Plant Tissue Simulations

## Assignment 5

1. Open pathogen_infection model and run for a duration of 2h. Screenshot
initial and every 30 min. Describe how the infected region spreads and
how the tissue deforms.

### 2. In the model files (Github repo – Models – Infection – infection.cpp9:
Read CellHouseKeeping. In your own words: how is a cell's wall stiffness
reduced as a function of its chemical level? What does the pathogen do
differently?

``
void Infection::CellHouseKeeping(CellBase *c) {
    // add cell behavioral rules here
    if(c->CellType()==2){
        c->EnlargeTargetArea(2);
        if (c->Area() > par->rel_cell_div_threshold * c->BaseArea() ){
            c->Divide();
        }
    }

    // initial cell length setup
    double base_element_length = 25;
    c->LoopWallElements([base_element_length](auto wallElementInfo){
        if(std::isnan(wallElementInfo->getWallElement()->getBaseLength())){
        wallElementInfo->getWallElement()->setBaseLength(base_element_length);
        }
    });

    //cell wall weakening happens here
    double patho_chem_level = c->Chemical(0) / (0.5);
    if (patho_chem_level > 1.2) {
        patho_chem_level = 1.2;
    }
    double stiffness_inf = 3;
    if(patho_chem_level>0.1 && c->CellType()!=2){
        c->SetCellVeto(false);
        stiffness_inf = 3 - (patho_chem_level);
    c->LoopWallElements([stiffness_inf](auto wallElementInfo){
        wallElementInfo->getWallElement()->setStiffness(stiffness_inf);
    });
    }
    else{
        c->LoopWallElements([stiffness_inf](auto wallElementInfo){
        wallElementInfo->getWallElement()->setStiffness(stiffness_inf);
        });
        c->SetCellVeto(true);
    }
}
``

The model treats the pathogen as a chemical signal that over time weakens nearby non-pathogen cells. Under normal conditions cells have their default wall stiffness which is rigid and mechanically resistant. Once the pathogen chemical rises above a small activation threshold, the stiffness of non-pathogen cells decrease linearly with exposure. It's walls become softer and easier to deform as more pathogen chemical a cell senses. At maximum exposure, the wall retains only about 60% of its original stiffness so the cell is weaker but mechanically still stable.

Pathogen cells however are excluded from this weakening response and keep their regular wall stiffness. Instead they increase their preffered size and divide once they grow sufficiently, allowing the pathogen population to expand while the surrounding host cells become more vulnarable.

### 3. In the model files (Github repo – Models – Infection – infection.cpp9:
Read CelltoCellTransport. How is the diffusion coefficient defined?
Explain the feedback loop this creates and sketch it: chemical lowers
stiffness, lower stiffness raises diffusion, faster diffusion spreads the
chemical. Is this positive or negative feedback?

```
void Infection::CelltoCellTransport(Wall *w, double *dchem_c1, double *dchem_c2) {
	// add biochemical transport rules here
    double sum =  w->C1()->Area() + w->C2()->Area();
    double corr1 = w->C2()->Area() / sum;
    double corr2 = w->C1()->Area() / sum;

    double length = 1.0;
    double stiffness = 1.0;
    getLengthAndStiffness(w,&length,&stiffness);
    double diffusionCoef;
    if(stiffness>0.001){
        diffusionCoef = 0.00001/stiffness;
    }
    else{diffusionCoef=0.00001;}

    double phi = length * diffusionCoef * ( w->C2()->Chemical(0) - w->C1()->Chemical(0) );

    dchem_c1[0] += corr1 * phi;
    dchem_c2[0] -= corr2 * phi;

}
```

### 4. Raise and lower rel_cell_div_threshold.
How does it change how fast the pathogen population expands? Document two runs.

### 5. What is a fundamental difference regarding cell neighbours in this model
compared to all other models that you have worked with so far?

### 6. The plant evolves a defense: cells above a chemical threshold stiffen
their walls. Describe in pseudocode where in CellHouseKeeping this
would go and what sign of feedback it adds. Do not implement it.
Pseudocode for the different sections is enough!
Since you are not programming in this assignment, you will document the
simulations and observations in the readme file.
