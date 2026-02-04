# Coral Reef Deterioration: Agent-Based Modeling

> Simulating the impact of water temperature on coral reef ecosystems using agent-based modeling in NetLogo

## 📋 Overview

**Course:** [Computational/Environmental Modeling Course]  
**Institution:** Tilburg University  
**Date:** 2022  
**Status:** ✅ Completed  

This project develops an agent-based model (ABM) to simulate how water temperature affects coral reef deterioration. By expanding on Dennis Hubbard's "The Non-linearity of Environmental Change" model, we investigate the relationship between rising ocean temperatures, water acidity (pH), algae proliferation, and coral death—a critical environmental issue facing marine ecosystems worldwide.

---

## 🎯 Research Question

**"How does water temperature affect the deterioration of coral reefs?"**

### Motivation

**Critical Environmental Context:**
- Great Barrier Reef lost **50.07% coral cover** (1985-2012)
- Atmospheric CO₂ projected to exceed 500 ppm by 2050-2100
- Global temperatures expected to rise by at least 2°C
- Ocean acidification threatens marine ecosystems evolved over 420,000 years

**Project Goals:**
1. Model coral reef deterioration under different temperature scenarios
2. Visualize the relationship between temperature, pH, algae, and coral survival
3. Create awareness about this ecological crisis through simulation
4. Provide a foundation for more complex environmental models

---

## 🔧 Technologies Used

**Modeling Platform:**
- **NetLogo** - Agent-based modeling environment

**Analysis Tools:**
- **R** - Statistical analysis and visualization
- **R packages:** Base R for t-tests and data analysis

**Key Concepts:**
- Agent-Based Modeling (ABM)
- Complex systems simulation
- Ecological modeling
- Statistical hypothesis testing

---

## 🏗️ Model Description

### Base Model

**Original:** "The Non-linearity of Environmental Change: A coral reef model" (Dennis Hubbard & Oberlin College)

**Our Modifications:**
- Removed urchin agents (non-critical for research question)
- Implemented temperature-pH relationship
- Added human impact factors (global warming effects)
- Focused on coral-algae-fish interaction dynamics

### Agent Types

#### 1. Fish Agents

**Properties:**
- `fishenergy` - Controls population size and individual energy levels
- `fishstomach` - Tracks fullness (capacity: 2 algae)
- `tickcounter` - Manages feeding intervals

**Behaviors:**
- **Movement:** Random swimming (costs 5 energy per tick)
- **Feeding:** Eat algae when in range to regain energy
- **Reproduction:** Occurs when total fish < 4 × total algae

**Logic:**
```
If fishstomach = 2:
    tickcounter += 1
    If tickcounter = 2:
        fishstomach = 0
        tickcounter = 0
```

---

#### 2. Coral Agents

**Properties:**
- `corallife` - Lifespan set to 125 ticks

**Behaviors:**
- **Death Conditions:**
  - Algae within 3.5 space radius (suffocation + nutrient competition)
  - Life counter reaches 0
  
- **Reproduction:**
  - Creates 1 new coral when fewer than 3 exist
  - Total extinction if no corals remain

**Ecological Role:**
- Create habitat for marine life
- Indicate ecosystem health
- Sensitive to environmental changes

---

#### 3. Algae Agents

**Properties:**
- Controlled by `AlgaeReproduction` slider (user-defined)

**Behaviors:**
- **Spawning:** Based on user settings and temperature
- **Negative Effects:**
  - Suffocate nearby corals (< 3.5 distance)
  - Compete for nutrients
  - Inhibit coral growth

**Temperature Sensitivity:**
- Proliferate rapidly in warmer, more acidic waters
- Limited growth in cooler temperatures

---

### Environmental Parameters

#### Temperature (`Temp`)

**Range:** 10°C, 20°C, 30°C
- **10°C:** Below natural ocean temperature
- **20°C:** Natural ocean temperature (baseline)
- **30°C:** Elevated due to climate change

**Effects:**
- Directly affects algae reproduction rate
- Indirectly impacts coral survival through algae

#### pH Relationship

**Formula:**
```
ΔpH = -0.2 per 10°C increase from baseline (20°C)
```

**Natural pH:** 8.1 (at 20°C)

**Temperature-pH Relationship:**
- **10°C:** pH = 8.3 (more basic)
- **20°C:** pH = 8.1 (natural)
- **30°C:** pH = 7.9 (more acidic)

**Note:** While pH was calculated, implementation focused on temperature due to complexity. Temperature served as a proxy for combined temperature-pH effects.

---

## 🧪 Experimental Design

### Simulation Parameters

**Run Configuration:**
- **Datasets:** 3 (one per temperature condition)
- **Runs per dataset:** 25
- **Ticks per run:** 30,000
- **Total data points per dataset:** 750,000

**Initial Conditions:**
- Corals: 48 agents
- Algae: 45 agents
- Fish: [Population controlled by model dynamics]

**Independent Variable:** Temperature (10°C, 20°C, 30°C)

**Dependent Variables:**
- Coral count over time
- Algae count over time
- Fish population dynamics

---

## 📈 Results

### Statistical Analysis

#### Coral Population Means by Temperature

| Temperature | Mean Coral Count | Interpretation |
|-------------|------------------|----------------|
| **10°C** | 80.62 | Highest survival (below natural temp) |
| **20°C** | 63.64 | Natural condition baseline |
| **30°C** | 15.19 | **Severe decline** (elevated temp) |

#### Algae Population Means by Temperature

| Temperature | Mean Algae Count | Interpretation |
|-------------|------------------|----------------|
| **10°C** | 4.005 | Minimal algae (coral-favorable) |
| **20°C** | 26.58 | Natural balance |
| **30°C** | 122.9 | **Algae bloom** (coral-threatening) |

### Welch Two-Sample T-Tests

**All comparisons showed statistically significant differences:**

| Comparison | p-value | t-statistic | Interpretation |
|-----------|---------|-------------|----------------|
| 10°C vs 20°C | 2.2e-16 | 461 | Highly significant |
| 10°C vs 30°C | 2.2e-16 | **3434** | Extremely significant |
| 20°C vs 30°C | 2.2e-16 | 1352 | Highly significant |

**Key Finding:** Higher t-values with increasing temperature differences confirm strong temperature effect on coral populations.

---

### Temporal Dynamics

#### Coral Population Trends

**At 10°C (Fig 1.1):**
- Sharp increase to ~80 corals at 100,000 ticks
- Slight decrease then stabilization
- Upward trend near end of simulation
- **Pattern:** Healthy coral growth

**At 20°C (Fig 1.2):**
- Relatively linear increase from 48 to 80 corals
- Reaches 80 around 700,000 ticks
- **Pattern:** Slow, steady growth

**At 30°C (Fig 1.3):**
- **Sharp decrease to 14 corals at 100,000 ticks**
- Fluctuates between 14-16 for remainder
- **Pattern:** Population collapse

#### Algae Population Trends

**At 10°C (Fig 1.4):**
- Decrease to ~4 algae at 100,000 ticks
- Fluctuation at low levels
- **Pattern:** Limited algae growth

**At 20°C (Fig 1.5):**
- Slow, linear decrease to ~10 entities at 700,000 ticks
- **Pattern:** Balanced ecosystem

**At 30°C (Fig 1.6):**
- **Sharp increase to 122 algae at 100,000 ticks**
- Stabilizes with minimal deviation
- **Pattern:** Algae bloom (coral-threatening)

---

## 💡 Discussion

### Key Findings

**1. Temperature-Coral Relationship:**
- **Inverse correlation:** Higher temperature → Fewer corals
- Temperature effect mediated through algae proliferation
- Tipping point observed between 20°C and 30°C

**2. Algae-Coral Interaction:**
- **Competitive exclusion:** Algae suffocate corals at high temperatures
- Algae blooms occur when temperature exceeds natural levels
- Spatial proximity (< 3.5 distance) causes coral death

**3. Ecosystem Balance:**
- **10°C:** Coral-dominated (80 corals, 4 algae)
- **20°C:** Balanced (64 corals, 27 algae)
- **30°C:** Algae-dominated (15 corals, 123 algae)

**4. pH-Temperature Linkage:**
- 10°C increase reduces pH by 0.2
- Acidification creates favorable conditions for algae
- Corals cannot adapt quickly to rapid pH changes

### Real-World Implications

**Model Predictions:**
- Current trajectory toward 2°C warming will devastate coral reefs
- Tipping point may be closer than expected (20-30°C range)
- Algae blooms will accelerate coral decline
- Ecosystem shifts from coral-dominated to algae-dominated

**Conservation Insights:**
- Urgent need for temperature stabilization
- Coral restoration efforts must account for temperature sensitivity
- Protection of remaining coral habitats critical
- Climate action necessary for reef survival

---

## 📖 References

1. Bergstrom, C., McKeel, C., & Patel, S. (2007). Effects of pH on algal abundance: A model of Bay Harbor, Michigan. Deep Blue, University of Michigan.

2. De'ath, G., Fabricius, K. E., Sweatman, H., & Puotinen, M. (2012). The 27–year decline of coral cover on the Great Barrier Reef and its causes. *Proceedings of the National Academy of Sciences*, 109(44), 17995–17999.

3. Gillespie, C. (2019). The effects of temperature on the pH of water. Sciencing.

4. Hoegh-Guldberg, O., et al. (2007). Coral reefs under rapid climate change and ocean acidification. *Science*, 318(5857), 1737–1742.

5. Hubbard, D. (n.d.). The Non-linearity of Environmental Change: A coral reef model. Oberlin College NetLogo Models Library.

6. NOAA Florida Keys National Marine Sanctuary. (2011). How does pollution impact corals?

7. UCAR Center for Science Education. Corals and Climate.


---

## 📄 License

This project is for educational purposes as part of the coursework at Tilburg University.

---

## 📧 Contact

**Beliz Pekkan**  
Email: belizpekkan@gmail.com  
Website: [belizpekkan.com](https://belizpekkan.com)  
LinkedIn: [linkedin.com/in/beliz-pekkan](https://www.linkedin.com/in/beliz-pekkan)

---

## 🌍 Impact Statement

This project demonstrates the application of computational modeling to pressing environmental issues. By simulating coral reef deterioration under different temperature scenarios, we provide a tangible visualization of climate change impacts on marine ecosystems. The model serves as both an educational tool and a foundation for more sophisticated environmental simulations, contributing to scientific communication and conservation awareness.

**Key Message:** Our simulations show that even moderate temperature increases (10°C above natural levels) cause catastrophic coral decline. **Urgent climate action is necessary to preserve coral reef ecosystems for future generations.**

---

**Note:** This agent-based model combines ecological theory, computational simulation, and statistical analysis to address a critical environmental challenge. The project showcases interdisciplinary skills spanning ecology, computer science, and data analysis while addressing real-world conservation concerns.
