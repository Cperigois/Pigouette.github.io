---
permalink: /gravitational-waves/PRINCESS
title: "PRINCESS : Python tool to predict and characterize gravitational wave observation from astrophysical models"
layout: projet
classes: wide
author_profile: true
tags: 
  - projets
method: 
  - Population synthesis
  - Stochastic modeling
  - Monte Carlo
  - Markov chains
data: 
  - Frequency spectrums
  - sky-location
skills: 
  - Python
  - Tool-devlopement
  - Git
  - Machine learning
  - Astrophysics
initiative: 
  - Post-doc
description: "Princess is a fast and flexible tool to simulate gravitational-wave backgrounds from compact binary populations. It supports realistic population models and is designed for scalability and ease of use."
papers:
  - title: "PRINCESS: Prediction of compact binaries observations with gravitational waves"
    url: "https://arxiv.org/abs/2501.16127"
---

## A Fast and Scalable Code to Explore the Gravitational-Wave Background

**Princess** is a Python-based code developed to model the gravitational-wave background produced by the population of compact binary mergers — including binary black holes (BBH), binary neutron stars (BNS), and mixed systems (BHNS).  

It was designed to answer a growing need in the gravitational-wave community: the ability to quickly generate synthetic background signals across a wide range of astrophysical scenarios, detector sensitivities, and subtraction strategies. It is open-source and documented to promote reproducibility and collaboration.

---

## Key Features and Capabilities

Princess was built around three main objectives: **speed**, **flexibility**, and **realism**.

- 🔁 **Fast generation of population catalogs** using Monte Carlo sampling of binary parameters.
- ⚙️ **Flexible modeling** of astrophysical assumptions: merger rates, mass/spin distributions, metallicity evolution, delay times, etc.
- 🎯 **Realistic observability filters** to separate resolved and unresolved sources, supporting signal subtraction studies.
- 🔉 **Spectral computation of gravitational-wave energy density** across frequency, including residual backgrounds.
- 🔬 **Support for detector network configurations** (e.g. ET, CE, LIGO, Virgo), using instrument sensitivity curves and overlap reduction functions.

<p align="center">
  <img src="../assets/images/princess_workflow.png" alt="Princess Workflow" width="600"/>
</p>

*Simplified workflow of the Princess code: from astrophysical assumptions to stochastic background prediction.*

---

## Applications and Scientific Goals

Princess was used in several published studies to assess the capabilities of current and future detectors to:

- Constrain **compact binary formation channels** via the spectral features of the gravitational-wave background;
- Detect or place limits on the background from **Population III remnants**;
- Evaluate the impact of **binary subtraction techniques** on the detectability of stochastic signals;
- Compare different **detector designs** (e.g. triangle vs. dual L-shapes in Einstein Telescope).

Its performance allows researchers to explore large parameter spaces efficiently, and to simulate observational scenarios under varying model uncertainties.

---

## Access and Documentation

Princess is open source and available on GitHub:  
🔗 [https://github.com/CPerigois/Princess](https://github.com/CPerigois/Princess)

The repository includes:

- 📘 A detailed user guide and API reference;
- 📊 Example notebooks to reproduce published figures;
- 🔧 Tutorials for customizing input models and adding new detectors.

---

## Associated Publication

This tool is described in the following article:

- **"Princess: A tool for fast and flexible modeling of gravitational wave backgrounds from compact binary coalescences"**  
  C. Perigois, et al. (2025)  
  [https://arxiv.org/abs/2501.16127](https://arxiv.org/abs/2501.16127)

Princess is actively maintained and used in current research projects involving the Einstein Telescope, LISA, and next-generation detector networks.

---