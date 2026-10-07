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



Control (rel_cel_div_threshold = 2) Screenshots -> ["Control 4","Control 8","Control 12]
| Time | Pathogen Area | Pathogen Cells | Weakened Cells | Procambium Cells | Xylem Cells   |
|------|---------------|----------------|----------------|------------------|---------------|
| 4h   | 3637          | 2              | 17             | 19               | 10            |
| 8h   | 14201         | 16             | 22             | 13               | 10            |
| 12h  | 41101         | 31             | 36             | 0                | 10            |
|------|---------------|----------------|----------------|------------------|---------------|

Higher (rel_cel_div_threshold = 4) Screenshots -> ["High 4","High 8","High 12]
| Time | Pathogen Area | Pathogen Cells | Weakened Cells | Procambium Cells | Xylem Cells   |
|------|---------------|----------------|----------------|------------------|---------------|
| 4h   | 3209          | 1              | 16             | 20               | 10            |
| 8h   | 7356          | 2              | 19             | 17               | 10            |
| 12h  | 14959         | 8              | 27             | 9                | 10            |
|------|---------------|----------------|----------------|------------------|---------------|

Lower (rel_cel_div_threshold = 1) Screenshots -> ["Low 4","Low 8","Low 12]
| Time | Pathogen Area | Pathogen Cells | Weakened Cells | Procambium Cells | Xylem Cells   |
|------|---------------|----------------|----------------|------------------|---------------|
| 4h   | 5828          | 7              | 17             | 19               | 10            |
| 8h   | ~71590        | 55             | 29             | 7                | 10            |
| 12h  | ~287120       | 109            | 43             | 0                | 3             |
|------|---------------|----------------|----------------|------------------|---------------|

Time - approximate total simulation time
Pathogen Area - sometimes approximated via average cell area from random sampling
Pathogen Cells - (red)
Weakened Cells - attacked procambium (violet) and xylem (light green)
Procambium Cells - healthy (cyan)
Xylem Cells - healthy (green)


Cell division threshold very significantly affects the rate at which the pathogen infection progresses. With only the fungi cells being able to divide,
the change becomes directly proportional to the rate at which the infected area spreads. Since each fungi cell is set to act as though its chemical
concentration is constant, more instances leads to greater exchange of chemicals between cells, conversely creating a positive feedback loop,
leading to quicker cell wall deterioration, faster diffusion of chemicals, and overall swifter infection spread.



### 5. What is a fundamental difference regarding cell neighbours in this model compared to all other models that you have worked with so far?

The main difference lies in neighbour mutability. In the previous models, a cell could feasibly only gain neighbours via division. In case of the pathogen infection model, once a cell gets infected,
they become subjected to movement, meaning a given cell's healthy neighbours can be pushed out and replaced by pathogenic cells. Paired with the fungal cells constant chemical concentration, as well as its
monopoly on expansion and division, such displacement allows the fungal infection to grow at an increasingly faster rate.


### 6. The plant evolves a defense: cells above a chemical threshold stiffen their walls. Describe in pseudocode where in CellHouseKeeping this would go and what sign of feedback it adds. Do not implement it. Pseudocode for the different sections is enough! Since you are not programming in this assignment, you will document the simulations and observations in the readme file.

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