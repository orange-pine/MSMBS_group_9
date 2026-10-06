# KEN3170 – Plant Tissue Simulations

## Assignment 5

### 1. Open pathogen_infection model and run for a duration of 2h. Screenshot initial and every 30 min. Describe how the infected region spreads and how the tissue deforms.

In "Screenshots(task1)" folder you can find the five screenshots. What we noticed is that in the first minutes the cells divided and they lost their rectangular shape to take on a more irregular one (sharp angles, etc..). Then the infection started spreading, first infecting the two layers (by layers we mean the columns of cells) next to the pathogen (on the very left) then gradual infecting the neighbouring layers to its right. We noticed that generally the central cell was infected first in each layer and then it spread to the other cells of that layer (so the cells above and underneath that central cell). This probably comes from the fact that the patogen made contact with the tissue around the center of its left side. 

### 2. In the model files (Github repo – Models – Infection – infection.cpp9: Read CellHouseKeeping. In your own words: how is a cell's wall stiffness reduced as a function of its chemical level? What does the pathogen do differently?

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

### 3. In the model files (Github repo – Models – Infection – infection.cpp9: Read CelltoCellTransport. How is the diffusion coefficient defined? Explain the feedback loop this creates and sketch it: chemical lowers stiffness, lower stiffness raises diffusion, faster diffusion spreads the chemical. Is this positive or negative feedback?

The diffusion coefficient is defined in function of the stiffness of the wall. If the stiffness is above a certain treshhold: a lower stiffness leads to a higher diffusion rate. So they are inversily proportional to each other.

Once the initial wall stiffness is defined, the diffusion coefficient is calculated based on it => Even if this diffusion is very low in the beginning, chemicals are going to be flowing from one cell to the next, which is going to reduce the wall stiffness => This in turn is going to increase the diffusion coefficient => Then the chemical flow will be even higher, which will again lower the wall stiffness. So in conclusion this is a positive feedback because the initial behaviour (diffusion) is amplified not reduced.
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

### 4. Raise and lower rel_cell_div_threshold. How does it change how fast the pathogen population expands? Document two runs.

### 5. What is a fundamental difference regarding cell neighbours in this model compared to all other models that you have worked with so far?

### 6. The plant evolves a defense: cells above a chemical threshold stiffen their walls. Describe in pseudocode where in CellHouseKeeping this would go and what sign of feedback it adds. Do not implement it. 

    //cell wall weakening happens here
    double patho_chem_level = c->Chemical(0) / (0.5);
    if (patho_chem_level > 1.2) {
        patho_chem_level = 1.2;
    }
    double stiffness_inf = 3;
    if(patho_chem_level>0.1 && c->CellType()!=2){
        c->SetCellVeto(false);
    
    //Defense would go here, after the pathogen-induced weakening
    has been calculated, but naturally before stiffness_inf is applied
    to the wall elements.
    
    //If pathogen chemical becomes higher than a defense threshold:
    if patho_chem_level > DEFENSE_THRESHOLD:
        //then;
        increase stiffnes_inf
    
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

This defense adds negative feedback. Since, as previous exercises showed higher wall stiffness reduces the diffusion coefficient of the pathogen chemical. 
Therefore, when cells above the chemical threshold stiffen their walls, further chemical spread is slowed. This defense counteracts the original positive 
feedback in where higher chemical concentrations caused wall weakening, which increased diffusion and caused further spread of the pathogen.