---
permalink: /gravitational-waves/PRINCESS
title: "PRINCESS: Predicting Gravitational-Wave Observations"
layout: projet
classes: wide
author_profile: true
tags: 
  - projects
method: 
  - Population Synthesis
  - Monte Carlo Methods
  - Team Lead
  - Stochastic Spectrum
  - Frequency-Spectrum
data: 
  - Mass-Distribution
  - Redshift-Evolution
  - Stochastic Spectrum
skills: 
  - Python
  - Gravitational-Wave Modeling
  - Gravitational-Wave Detection
  - Detector Simulation
  - Statistical Astrophysics
  - Git-version-control
  - Documentation
  - Software Architecture
  - Project Management
  - Regression
  - Classification
initiative: 
  - Post-doc
description: "PRINCESS is an open-source tool designed to predict gravitational-wave observations, estimating both individual events and the associated stochastic background based on astrophysical models and detector configurations."
papers:
  - title: "PRINCESS: Prediction of compact binaries observations with gravitational waves"
    url: "https://arxiv.org/abs/2501.16127"
---

## Overview of the PRINCESS Code

**PRINCESS** (Prediction of Compact Binaries Observations with Gravitational Waves) is an **open-source Python tool** that forecasts both individual gravitational-wave events and the stochastic background from unresolved sources.

- Developed for use with **second- and third-generation detectors** (LIGO, Virgo, KAGRA, Einstein Telescope, Cosmic Explorer)
- Easy to install and use: includes **ready-to-run templates** and example configurations
- Designed to be **user-friendly and modular**, suitable for researchers and students
- Fully **documented using Sphinx**, with tutorials and examples available on [GitHub](https://github.com/Cperigois/Princess)

---

## Gravitational Wave Detection Theory

Gravitational-wave detections rely on computing the **signal-to-noise ratio (SNR)** of each event:

$$
\rho^2 = 4 \int_{f_{\min}}^{f_{\max}} \frac{|\tilde{h}(f)|^2}{S_n(f)} df
$$

Here, $$ \tilde{h}(f) $$ is the Fourier-domain waveform, and $$ S_n(f) $$ is the detector noise power spectral density. The SNR depends strongly on:

- the binary masses
- distance/redshift
- source orientation
- detector sensitivity

PRINCESS computes SNRs for each source in a catalog, based on waveform modeling, detector geometry, and sky-averaged antenna patterns.

---

## Detection Biases and Event Predictions

Detection requires a threshold (e.g., $$ \rho > 8 $$). PRINCESS takes into account the **selection bias** introduced by this threshold and forecasts the number of resolvable events per detector configuration.  
See **figure** below:

<p align="center">
  <img src="../assets/images/Detections_log.png" alt="Figure 1 - PRINCESS Detection Predictions" width="700"/>
</p>

The tool reproduces trends observed in real data. Overestimates are often due to optimistic merger rates or mass distributions in the input population models.

---

## Merger Rates and Observational Comparisons

The tested models span a wide range of local merger rates $$ R_0 $$ from 18 to 100 Gpc$$^{-3}$$yr$$^{-1}$$, compared to the observed LVK value of $$ R_0 = 23.9^{+14.9}_{-8.6} $$.

Key findings:

- Models with lower metallicity spread (σZ = 0.2) align better with current observations.
- Mass distributions with an excess of high-mass black holes inflate SNR values and detection counts.

---

## Stochastic Gravitational-Wave Background

In addition to individual detections, unresolved sources generate a **stochastic background**. Its amplitude is quantified through the energy density spectrum:

$$
\Omega_{\mathrm{GW}}(f) = \frac{f}{\rho_c c} F(f)
$$

with $$ F(f) $$ the total GW flux and $$ \rho_c $$ the Universe’s critical energy density. In PRINCESS, the spectrum is computed as:

$$
\Omega_{\mathrm{GW}}(f) \propto f^2 \langle |\tilde{h}(f)|^2 \rangle
$$

Both **total** and **residual** (after subtracting detected sources) spectra are calculated.

---

## Spectral Shape and Detectability

<p align="center">
  <img src="../assets/images/Figure_1.png" alt="Figure 3 - Stochastic Background Spectrum" width="700"/>
</p>

The typical $$ f^{2/3} $$ slope below ~200 Hz arises from the inspiral phase. High-frequency cutoffs reflect the end of the merger process. Spectral variations stem from different redshift and mass distributions.

PRINCESS also computes the background's detectability (SNR), including cross-correlation across detectors.

| Network     | SNR (total) | SNR (residual) |
|-------------|-------------|----------------|
| ET          | 1700        | 59             |
| 2×CE        | 3601        | 8              |
| ET + 2×CE   | 4332        | <1             |

This shows that the **residual background becomes undetectable** when using a full 3G detector network, after subtracting individually resolved sources.

---

## Conclusion

**PRINCESS bridges theoretical population models and observational forecasts.** It enables:

- Predicting the number and properties of detectable gravitational-wave events
- Modeling and assessing the astrophysical stochastic background
- Accounting for observational biases (mass, redshift, orientation)
- Testing detector configurations and network sensitivity

> PRINCESS is a key tool for preparing the next era of gravitational-wave astronomy and for constraining the astrophysical origin of compact binaries.
---