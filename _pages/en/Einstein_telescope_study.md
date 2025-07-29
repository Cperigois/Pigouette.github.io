---
permalink: /gravitational-waves/Einstein_telescope_study
title: "Science with Einstein Telescope, on the behalf of the Einstein Telescope collaboration"
layout: projet
classes: wide
author_profile: true
tags: 
  - projets
method: 
  - Statistics
  - Regression
  - Simulation
data: 
  - Frequency-Spectrum
  - Time-Series
skills: 
  - Python
  - Modeling
initiative: 
  - Post-doc
description: "Impact study of the desingn for future Einstein telescope on future gravitational wave observations. A Princess study."
---


On this page I present my contribution to the design impact study of Einstein telescope and the predictions of scientific observations.

Einstein Telescope ids the european collaboration planning a built of a trird generation detector in Europe. At the timse this article is written
three sites are in competition to host the detector (or detectors), Sardegna in Italy, Limburg in Netherlands and Saxony in Germany.
There are a lot of design under study, I'll concentrate here on the one les plus envisagés et raisonnables doable (from the financial and technological aspect).

Le design historique et aujourd'hui le plus envisagé par la communauté est la forme triangulaire. Il s'agit de 6 interferometres de 10 ou 15 km de longueur de bras, placé en triangle.
Chacun des sommet est l'intersection de 2 interferometre différent, l'un sensible aux haute frés=quence l'autre aux basses fréquences.
Tous le triangle sera place sous terre à 250-300 metres de profondeur. 

Le second design propose de construire deux interferometre en "L" comme les détecteurs LIGO, mais souterrain et avec une longueur de bras de 15 ou 20 km.

Ces deux designs on un impact important sur la science que l'on pourra faire avec le détecteurmais aussi leurs defi technologique propre.

Les estimations réalisées pour les deux études que j'ai réalisé ici sont réalisés avec mon outils Princess.

 ### Cost Benefit Analysis (CoBA)

Dès 2021, je m'engage dans la communauté d'étude des design pour évaluer l'impact des deux formes et des différentes longueur de bras sur l'observation de fond gravitationnel astrophysique.
Ces resultats sont publiés dans un article à longue liste d'auteur. https://ui.adsabs.harvard.edu/abs/2023JCAP...07..068B/abstract

Je réalise a partir de modeles state of the art du moment les prediction du fond gravitationnel pour trois type de systeme binaires : Les binaires de trous noirs (BBH),
Les binaires d'étoile a neutron (BNS), et les binaire mixte etoile a neutron -trou noir (BHNS).
La contribution de ces trois categorie au fond gravitationel sont representé sur la figure suivente. Les courves parabolique mettent en évidence les sensibilité des détecteurs selon leurs design, c'est a dire leur capacité a 
observer le signal.

<p align="center"><img src="../assets/images/Coba_bkg.png" alt="Coba_bkg.png" width="600"/></p>
*Gravitational wave backgrounds for biniary black holes (BBH), binary neutrons star (BNS) and binary blackhole neutron star (BHNS). The parabolic curves represent
the detectors sentitivities, Triangle version of Einstein Telescope, with 10 or 15 km arms, and two 20km arm length "L" aligned (0°) or misaligned (45°).*

Je met en évidence 3 challenge majeurs liés au fond gravitationnel astrophysique pour Einstein telescope : 
- L'observation des premier trous noirs de l'univers provenant  de la population III (Pop III)
- La discrimination des canaux de formation des trous noirs (BBH channel + SFH)
- L'observation des fusion d'étoile a neutrons (BNS mergers residual)

Sur l'image suivante je récapitile ces challenge dans leur espace de parametre fréquence/amplitude, pour mettre en avant les capacités des différents design.
<p align="center"><img src="../assets/images/Coba_goal.png" alt="Coba_goal.png" width="600"/></p>
*Representation of the interferometer, and the critical lengths.*

Cette etude pemet de conclure que dans le cadre de l'observation du fond gravitationel astrophysique, il est préférable d'avoir 2Ls aves des bras paralleles (0°),
car la sensibilité de la combinaison de ces deux detecteurs est meilleure dans l'intervalle dna slequel on s'attend a observer la population III.
En revanche pour les challenge à plus haute fréquences, discrimination des caaux de formation et Star formation history et les fusions d'étoile a neutron, il est préférable d'avoir une configuration triangle
qui présente une meilleur sensibilité au dela de 120 Hz.

### Science with Einstein Telescope

TBW