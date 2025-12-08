---
permalink: /BeyondTheLab/AILab/hackathon_climat_olivier
title: ""
layout: projet
classes: wide
author_profile: true
tags: 
  - projets
method: 
  - Climate Indicators Computation
  - Python
  - Xarray
  - Data Visualization
data: 
  - Geographical
  - Climate
skills: 
  - Python
  - Git
  - Xarray
  - Geospatial Analysis
  - Visualization
initiative: 
  - Hackathon
description: "Scientific analysis of future biogeographical suitability for olive cultivation in Occitanie under TRACC climate scenarios, developed during the Climate Data Hackathon (bronze medal)."
---

# Biogeographical Compatibility of Olive Cultivation Under Future TRACC Climate Scenarios

This work was conducted during the **Hackathon Climat en Données**, where the project won a **bronze medal among 32 submissions**.  
It addresses **Challenge 2 and Challenge 8 – Agricultural Transition Issues**, focusing on identifying **regional biogeographical areas compatible with emerging crops**, under **future TRACC climate trajectories**.  
Our case study centers on the **olive tree** and its potential suitability in **Occitanie**, including the department of **Gers (32)**.

Participants:
- **Mathilde Hibon** – GIP LIA: agronomy engineering, indicators, data visualization  
- **Aurélien Mure** – CEREMA: data science  
- **Carole Perigois** – astrophysics & data  
- **Marion Houdayer** – programming & data handling  
- **Guillaume Taburet** – SOLAGRO: programming & advanced visualization  

🔗 **GitHub repository:** https://github.com/GuillaumeTaburet/hackathon-defi2-agricole  
🌐 **Deployed platform:** https://guillaumetaburet.github.io/hackathon-defi2-agricole/

---

## Context and Motivation

As climate change accelerates, territorial actors—public services, agricultural organizations, cooperatives, and local decision-makers—need **operational tools** to explore adaptation pathways for agriculture. While climate impact platforms (e.g., Climadiag) exist, **dynamic, map-based visualizations** at relevant territorial scales remain rare, particularly for evaluating the future suitability of emerging crops.

Our project tackles the question:

> **Are there biogeographical areas within Occitanie compatible with olive cultivation under the future TRACC climate scenarios (2°C, 2.7°C, 4°C)?**

We created a workflow capable of:
- Computing climate-based **abiotic stress indicators**,
- Mapping their evolution across **three TRACC horizons**,  
- Identifying **compatible or incompatible zones** for olive cultivation,  
- Highlighting the potential role of the **Gers**, where olive trees are traditionally absent due to past climatic constraints.

---

## Overview

The study aims to provide a **scientific, transparent, and reproducible approach** to evaluating crop suitability under climate change by using:

- **Two regional climate models (RCM)** at 8 km resolution,  
- Forced by **two different global circulation models (GCM)**,  
- Producing maps for each indicator and each scenario,  
- Over **20-year TRACC windows**:
  - 2°C
  - 2.7°C
  - 4°C warming

Indicators were selected from the literature on **olive phenology**, **agronomy**, and **climate constraints**, focusing on abiotic stress thresholds that strongly affect survival or productivity.

---

## Scientific Approach

### Selected Indicators

Based on scientific literature, four key phenological stress categories were retained:

1. **Minimum temperatures (winter frost risk)**
   - Occurrences of:
     - **Tmin < –12°C**, or
     - **≥ 5 occurrences of Tmin < –7°C** within one winter
   - Even a single event may irreversibly damage the olive tree.

2. **Maximum temperatures (extreme heat stress)**
   - Occurrences of:
     - **Tmax > 40°C**, or
     - **Tmax > 35°C for 3 consecutive days** between April and June
   - This threshold corresponds to physiological failure.

3. **Hydric deficit (JJA water stress)**
   - Computed via the **daily hydric deficit**, as used in climatology and agronomy.

4. **Chilling requirement**
   - Number of years in which:
     - The mean temperature (Tmin + Tmax)/2 ≤ 12°C  
     - For fewer than 70 days between November and February

Each indicator is mapped across all TRACC horizons to identify **temporary suitability windows** and **long-term compatible zones**.

![ ](assets/images/Projet_Hackaton_Aires_Biogeo_Olivier.pdf)


---

## Mathematical Definitions of Climate Indicators

### 🌧️ Daily and Seasonal Hydric Deficit

Daily hydric deficit is defined as:

\[
Déficit(t) = \max(0,\ ET_0(t) - P(t))
\]

where  
- \(ET_0\) = reference evapotranspiration,  
- \(P\) = precipitation.

The cumulative hydric deficit over a period (e.g., JJA) is:

\[
Déficit_{\text{cumulé}} = \sum_{t \in \text{période}} Déficit(t)
\]

This follows classical definitions used in FAO-56, hydrology, agronomy, and crop water balance models.

---

### 🌡️ 1. Saturation Vapor Pressure \(e_s(T)\)

\[
e_s(T) = 0.6108\, \exp\left(\frac{17.27\,T}{T + 237.3}\right)
\]

Daily mean saturation vapor pressure:

\[
e_s = \frac{e_s(T_{\min}) + e_s(T_{\max})}{2}
\]

Source: FAO-56, Allen et al. (1998).

---

### 💨 2. Actual Vapor Pressure \(e_a\)

Using specific humidity \(q = huss\):

\[
q = \frac{0.622\, e_a}{P - 0.378\, e_a}
\]

Isolating \(e_a\):

\[
e_a = \frac{q\, P}{0.622 + 0.378\, q}
\]

Widely used in NOAA, ECMWF reanalysis, and FAO formulations.

---

### 🔥 3. Psychrometric Constant \( \gamma \)

\[
\gamma = 0.000665\, P
\]

Pressure from altitude (\(z\)):

\[
P = 101.3\left(\frac{293 - 0.0065\, z}{293}\right)^{5.26}
\]

---

### 📈 4. Slope of Saturation Vapor Curve \( \Delta \)

\[
\Delta = \frac{4098\, e_s(T)}{(T + 237.3)^2}
\]

Critical for weighting energy vs. aerodynamic terms in Penman-Monteith.

---

### 🌍 5. Soil Heat Flux Approximation

At daily resolution:

\[
G \approx 0
\]

Following FAO-56 recommendations for Rn–G terms.

---

### 🌤️ Penman–Monteith Reference Evapotranspiration

\[
ET_0 = 
\frac{
0.408\, \Delta\, (R_n - G) + \gamma \frac{900}{T + 273} u_2 (e_s - e_a)
}{
\Delta + \gamma(1 + 0.34 u_2)
}
\]

Where all variables follow FAO-56 definitions.

---

### 🌱 Actual Crop Evapotranspiration

\[
ET_c = K_c\, ET_0
\]

---

## Key Findings

### **1. Intense Frost Risk (Tmin < –12°C)**

- Historically (1960–1979), the **Gers was exposed to 1–3 days below –12°C**, making olive cultivation highly unsuitable.
- Under TRACC **4°C horizon (2071–2090)**:
  - Nearly the entire Occitanie region (excluding mountains) sees **0–1 frost days**,  
  - Making the region **largely compatible** with respect to this indicator.

![Evolution of the frost indicator (number of days with Tmin < –12°C) in Occitanie](assets/images/indicateur_gel.png)


### **2. Extreme Heat Risk (Tmax > 40°C)**

- Historically absent in Occitanie.
- Under TRACC trajectories:
  - By **2032–2051 (+2°C)**: Eastern Gers already shows **1–2 days >40°C** per year.
  - By **2052–2071 (+2.7°C)**: Gers reaches **2–3 days >40°C**.
  - By **2071–2090 (+4°C)**:
    - Gers averages **5 days >40°C**,  
    - Mediterranean coast reaches **5–12 days** per year.
- This indicator makes the **Gers mostly incompatible**, despite its improved frost resilience.

This contrast illustrates how **single-indicator assessments can be misleading**, reinforcing the need for multi-criteria integration.


![Evolution of the extreme-heat indicator (number of days with Tmax > 40°C) in Occitanie](assets/images/indicateur_canicule.png)


---

## Limitations and Future Improvements

- Only two indicators (frost, heat) were fully processed.  
- Remaining indicators (hydric deficit, chilling requirement) must be computed for a complete assessment.  
- Phenological stage sensitivity (juvenile phase, flowering, fruit set) requires finer temporal analysis.  
- Indicator-specific colormaps should align with olive physiological thresholds.  
- A **multi-criteria composite suitability index** would allow a more holistic view.  
- Visualization enhancements include:
  - **Multi-map grids** per indicator and horizon,
  - **Scenario slider** for dynamic comparison.

The long-term objective is a **systemic tool** helping territorial actors evaluate the compatibility of emerging crops under future climates.

---

## References

- Orlandi, F., Garcia-Mozo, H., Dhiab, A.B. et al. *Climatic indices in the interpretation of the phenological phases of the olive …* Climatic Change 116, 263–284 (2013).  
- International Olive Council: *World Olive Catalogue – High Temperature Module*.  
- Allen et al. (FAO-56), *Irrigation and Drainage Paper 56*.




