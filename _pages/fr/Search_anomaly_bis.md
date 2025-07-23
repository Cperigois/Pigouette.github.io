---
permalink: /IAlab/
title: "Recherche d'anomalie dans une série temporelle"
layout: single
---

| |
|:--|
| Cette étude s’inscrit dans une curiosité personnelle concernant l’implémentation d’**autoencodeurs** pour la détection d’anomalies électroniques en analyse du signal. Ce type d’approche est déjà largement documenté sur *Papers With Code* ; voici quelques références pertinentes. Pour moi, c’est également l’occasion de me former à un nouveau domaine du deep learning : **les autoencodeurs appliqués aux séries temporelles**, un sujet que je maîtrise déjà bien du point de vue signal. |

L’objectif de cette étude est de tester les limites de l'utilisation d’autoencodeurs dans la recherche d’anomalies dans les signaux temporels. Le sujet étant très vaste, je me suis limité ici à l’étude de **signaux sinusoïdaux affectés par un bruit gaussien et un bruit de dérive**, ce dernier étant typique de l’usure de composants électroniques.

Ce billet se compose de trois parties :
1. Un résumé du fonctionnement des autoencodeurs, avec les paramètres choisis pour cette étude.
2. La stratégie d’entraînement « étape par étape » pour contourner les difficultés liées à la périodicité des signaux.
3. Une évaluation du modèle sur des données bruitées contenant un ratio de dérive contrôlé.

---

## Modèle IA

### La base des autoencodeurs

Un autoencodeur est un type de réseau de neurones dont l’objectif est de **reconstruire l’entrée** après l’avoir compressée dans un espace latent de plus faible dimension. Il se compose de deux parties :

- **L’encodeur** : réduit la dimensionnalité de l’entrée pour extraire ses caractéristiques essentielles.
- **Le décodeur** : reconstruit l’entrée à partir de la représentation compressée.

En apprentissage non supervisé, un autoencodeur peut ainsi apprendre les motifs dominants des données. Toute **erreur de reconstruction importante** indique alors une **anomalie** (c’est-à-dire une donnée ne correspondant pas au motif appris).

La fonction de perte classique est l'erreur quadratique moyenne :

$$
\text{MSE} = \frac{1}{n} \sum_{i=1}^n (x_i - \hat{x}_i)^2
$$

où \( x_i \) est la donnée originale et \( \hat{x}_i \) sa reconstruction.

---

Mon modèle

Le modèle utilisé dans cette étude est un autoencodeur convolutionnel 1D conçu pour traiter des signaux temporels de longueur fixe (500 échantillons). Il se compose de deux blocs principaux : un encodeur et un décodeur, construits à partir de couches convolutives, adaptées aux données structurées dans le temps.

#### Encodeur

L’encodeur transforme le signal d’entrée en une représentation latente de dimension réduite (embedding). Il est constitué de :

Trois couches de convolution 1D successives avec :

- Un nombre croissant de canaux (16, puis 32, puis 64),

- Un noyau de convolution de taille 5,

- Un stride de 2 pour réduire progressivement la résolution temporelle,

- Une activation ReLU après chaque convolution.

À la fin de ces couches, la sortie est applatie (flatten) pour former un vecteur linéaire, qui est ensuite projeté via une couche linéaire (fully connected) vers l’espace latent de taille définie (encoded_size).

#### Décodeur

Le décodeur reconstruit le signal original à partir de la représentation latente. Il réalise l'opération inverse de l'encodeur, avec :

Une première couche linéaire pour transformer le vecteur latent en un tenseur compatible avec la structure attendue par les couches de déconvolution.

Trois couches de convolution transposée (aussi appelées "déconvolutions") :

Elles augmentent progressivement la taille du signal dans le temps,

Réduisent le nombre de canaux de 64 à 32, puis 16, puis 1 (la forme du signal d’origine),

Chaque étape est suivie d’une activation ReLU, sauf la dernière.

Ce design permet au modèle d’apprendre à extraire les motifs caractéristiques du signal d’entrée, puis à les reconstruire aussi fidèlement que possible. La compression dans l’espace latent force l’autoencodeur à filtrer le bruit et à ne conserver que les informations essentielles.

Ce modèle encode des signaux d’entrée de taille 500 vers un espace latent de dimension fixée à 30% de la taille d'entrée dans notre cas, avant de les reconstituer via des couches convolutionnelles transposées. L’architecture est volontairement compacte pour faciliter l’apprentissage progressif de motifs simples.

---

## Entraînement du modèle

### Les données d’entraînement

L’entraînement est réalisé sur des lots de 12000 signaux de 500 secondes, envoyés par batchs de 32 au GPU. Chaque signal est une sinusoïde bruitée, générée aléatoirement selon les paramètres de l’étape d’apprentissage.

---

### Stratégie d’apprentissage par étapes

L’entraînement suit une progression par **complexification croissante des signaux**. Voici un résumé en tableau :

| Étape | Époques | Amplitude | Phase | Fréquence | Bruit |
|------:|--------:|:----------|:------|:----------|:------|
| 1     | 10      | 4              | 0             | 0.05 Hz                | Aucun |
| 2     | 30      | Uniform(3.5, 4.5) | 0         | 0.05 Hz                | Aucun |
| 3     | 30      | Uniform(3.5, 4.5) | Uniform(0, 0.5) | 0.05 Hz | Aucun |
| 4     | 100     | Uniform(3.5, 4.5) | Uniform(0, 0.5) | Uniform(0.025, 0.075) Hz | Aucun |
| 5     | 100     | Uniform(3.5, 4.5) | Uniform(0, 0.5) | Uniform(0.025, 0.075) Hz | Bruit gaussien \( \mathcal{N}(0, 0.05) \) |
| 6     | 250     | Uniform(2, 6) | Uniform(0, 2π) | Uniform(0.025, 0.225) Hz | Bruit gaussien \( \mathcal{N}(0, 0.05) \) |
| 7     | 400     | Uniform(2, 6) | Uniform(0, 2π) | Uniform(0.025, 0.225) Hz | Bruit gaussien \( \mathcal{N}(0, 0.25) \) |

Un scheduler de type `ReduceLROnPlateau` est utilisé pour ajuster dynamiquement le **learning rate** à partir d’une valeur initiale de \( 10^{-4} \), en cas de stagnation des performances.

---

### Le bruit résiduel

Le modèle ne peut reconstruire parfaitement le signal d’entrée, à cause de la composante aléatoire du bruit. La perte finale atteint donc une valeur **non nulle**, appelée **bruit résiduel**.

Considérons :

$$
\text{signal}_{\text{target}} = \text{signal}_{\text{informatif}} + \text{bruit}_{\text{aléatoire}}
$$

et supposons une reconstruction parfaite du signal informatif. Alors, l’erreur moyenne devient :

$$
\text{MSE} = \frac{1}{n} \sum_{i=1}^n [(\text{output} - \text{signal}_{\text{target}})^2] = \frac{1}{n} \sum_{i=1}^n [\text{bruit}_{\text{aléatoire}}^2]
$$

Dans notre étude :
- Étapes 4–6 : bruit résiduel ≈ **0.0025**
- Étape 7 : bruit résiduel ≈ **0.0625**

### ![assets/images/sinusoïd_recognition.png](sinusoïd_recognition.png)

*Évolution de la perte de validation durant l'entraînement (légende à adapter).*

---

## Évaluation et tests sur signaux bruités

### Évaluation du modèle

Le modèle est évalué sur 10 000 nouveaux signaux générés avec les paramètres de l'étape 7.  
La perte médiane observée est :

$$
\text{MSE}_{\text{test}} = 0.078^{+0.053}_{-0.012}
$$

Ce qui reste compatible avec le bruit résiduel, indiquant un bon **pouvoir de généralisation**.

---

### Les données bruitées

Un **bruit de dérive** a été ajouté aux signaux :

> Une dérive est une **modification lente et continue** du signal dans le temps. Elle peut être modélisée par une fonction affine :

$$
\text{drift}(t) = \alpha \cdot t
$$

où \( \alpha \) est le coefficient de dérive.

![assets/images/combined_drift_comparison.png](combined_drift_comparison.png)

*Exemples de signaux avec dérive pour quatre valeurs de \( \alpha \), comparés à leur reconstruction.*

---

### Détection d’anomalie

Le modèle échoue à reconstruire correctement les signaux contenant une dérive non apprise, comme attendu. Pour chaque ratio de dérive, la perte est comparée aux percentiles de l’évaluation :

- **OK** : MSE < 95ᵉ percentile
- **WARNING** : 95ᵉ percentile < MSE < max
- **ANOMALY** : MSE > max (évaluation)

Pour quantifier le seuil de détection, on définit le **noise ratio** :

$$
\text{Noise ratio} = \frac{\text{total\_drift}}{\text{erreur résiduelle}}
$$

Le modèle détecte des anomalies dès que le noise ratio atteint **1.6**, ce qui indique une **très bonne sensibilité**. Cette performance pourrait être améliorée en augmentant la durée des signaux.

![assets/images/drift_impact_analysis.png](drift_impact_analysis.png)

*Exemples de signaux avec dérive pour quatre valeurs de \( \alpha \), comparés à leur reconstruction.*


---

## Conclusion

Les autoencodeurs se montrent efficaces pour détecter des anomalies dans des signaux sinusoïdaux contaminés par du bruit gaussien et de la dérive. La méthode est particulièrement sensible aux **déviations progressives**, tant que leur amplitude dépasse le bruit résiduel.

**Perspectives :**
- Travailler sur des signaux plus longs pour détecter des dérives plus faibles.
- Explorer des alternatives aux sinusoïdes : signaux triangulaires, fonctions de Heaviside, etc.
- Vérifier l'hypothèse selon laquelle les difficultés d'entraînement proviennent de la **moyenne nulle** des sinusoïdes — en ajoutant par exemple un offset.

Ce travail constitue un prototype sur données simulées. Il serait pertinent de valider cette approche sur des **données réelles** pour en mesurer le potentiel industriel.

---
```