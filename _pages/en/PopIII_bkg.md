---
permalink: /gravitational-waves/PopIII_BH_study
title: "Future observations of black holes from Population III stars"
layout: projet
classes: wide
author_profile: true
tags: 
  - projets
method: 
  - Population Synthesis
  - Simulation
  - Gravitational-waves Detection
  - Stochastic Spectrum
  - Bayesian analysis
  - Subtraction methods
skills: 
  - Python
  - Monte Carlo Methods
  - Statistics
  - Modeling
  - Project Management
  - Scientific Communication
initiative: 
  - PhD
description: "What can we learn about the first stars of the Universe from the gravitational-wave background? This project explores the imprint of Population III remnants on the stochastic gravitational-wave signal and discusses their detectability with third-generation observatories."
papers:
  - title: "StarTrack predictions of the stochastic gravitational-wave background from compact binary mergers"
    url: "https://arxiv.org/abs/2008.04890"
  - title: "Footprints of Population III Stars in the Gravitational-wave Background"
    url: "https://ui.adsabs.harvard.edu/abs/2022ApJ...940...29M/abstract"
  - title: "The Science of the Einstein Telescope"
    url: "https://ui.adsabs.harvard.edu/abs/2025arXiv250312263A/abstract"
---

# Scientific Context: Population III Stars and Black Holes

The very first generation of stars, known as **Population III (Pop III)**, formed out of pristine, metal-free gas at high redshift (*z* ≳ 10–20). Their properties — massive, short-lived, and prone to collapse directly into black holes — differ strongly from later stellar populations (Pop I/II). The black hole binaries that they created could merge and leave a distinct imprint in the **stochastic gravitational-wave background (SGWB)**.

Studying this imprint is essential to probe:
- The mass spectrum and formation history of the first stars.
- The early enrichment of the Universe.
- The transition between Pop III and later stellar populations.

---

## Stochastic Gravitational-Wave Background from Pop I/II and Pop III

### Population I/II  
Mergers from Pop I/II dominate the **total** SGWB in the frequency band of current detectors. Their combined signal follows approximately a power law:
\[
\Omega_{\text{GW}}(f) \propto f^{2/3}
\]
characteristic of the inspiral phase of compact binaries. At 25 Hz, the expected amplitude is of order  
\[
\Omega_{\text{GW}} \sim 10^{-9},
\]  
within reach of advanced LIGO/Virgo after several years of observation:contentReference[oaicite:0]{index=0}.

### Population III  
The Pop III contribution is typically an **order of magnitude weaker** in the total background. However, because of their high masses and redshifted signals, Pop III binaries **dominate the residual background** after subtracting individually resolved sources with 3G detectors (ET, CE). This modifies the spectral shape:  
- Deviations from the usual *f*^2/3 power law appear below ~40 Hz.  
- Bumps in the spectrum are associated with specific mass ranges of Pop III remnants (~70–90 M☉ BHs):contentReference[oaicite:1]{index=1}.

---

## Detectability and Source Subtraction

### Residual Backgrounds
As detector sensitivity improves, individually detectable events will be removed from the data stream.  
- **Second-generation (2G)** networks (HLV, HLVIK): residual background remains dominated by unresolved Pop I/II sources. Pop III is subdominant.  
- **Third-generation (3G)** detectors (Einstein Telescope, Cosmic Explorer): subtraction removes most Pop I/II signals, leaving a residual where **Pop III dominates below 40 Hz**:contentReference[oaicite:2]{index=2}.

This residual carries the cleanest imprint of Pop III stars.

### Signal-to-Noise Ratio (SNR) Predictions
- With 2G detectors: SNR ≲ 1 for the Pop III background (undetectable).  
- With 3G detectors: SNR ~ 30–250 for Pop III, depending on configuration (ET, ET+2CE).  
- With LISA: SNR > 1000, but dominated by overlapping galactic binaries, making subtraction more complex:contentReference[oaicite:3]{index=3}.

---

## Future Observations: Towards the First Stars

The detection of a **non-power-law SGWB** would be a smoking gun for Pop III. Several observational strategies are envisaged:

- **Optimal filtering with 3G detectors**: requires accurate theoretical modeling of Pop III populations, since a simple *f*^2/3 template does not apply.  
- **Source subtraction pipelines**: removing Pop I/II signals could expose the Pop III residual.  
- **Joint space-ground efforts**: combining LISA (low frequencies) with ET/CE (tens of Hz) may help track long-lived binaries across bands, improving subtraction.  

Even without a direct detection, upper limits on the Pop III background will **constrain the formation rate of the first stars**, providing astrophysical insights into cosmic dawn.

---

## Summary

- **Pop I/II binaries** dominate the total SGWB detectable with current detectors.  
- **Pop III binaries** leave a unique imprint at low frequencies, emerging in the residual background after subtraction with 3G detectors.  
- Their detection would open a direct window into the **first generation of stars** and their black hole remnants.  
- Next-generation observatories (ET, CE, LISA) will be crucial to probe this signal, either through detection or strong upper limits.  
