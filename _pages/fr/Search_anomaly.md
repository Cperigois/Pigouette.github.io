---
permalink: /IAlab/
title : "Recherche d'anomalie dans une série temporelle"
layout: single
---
|                                                         | 
|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:| 
| Cette étude s'incrit dans une curiosité de ma part sur l'implémentation d'auteencodeur pour detecteur des anomalies electoniques en analyse du signal. Ce type d'étue est déja largement documenté sur paper with codes en voici quelques bonnes références :   . Pour moi c'est l'occasion de me formé a un nouveau domaine du deep learning les autoencodeurs
appliqués aux séries temporelles que je connais déja bien.|

L'objectif de cette étude est de tater les limite de l'utilisation d'autoencodeurs dans la recherche d'anomalies dans les signaux temporels.
La question etant très vaste je me suis limité ici aux signaux sinusoidaux affecté par du bruit gaussien et un bruit de dérive typique de l'usure de composants electroniques.
Pour ce billet je vous propose une petit résumé du fonctionnement des autoencodeurs, accompagné des parametres que j'ai choisit pour cette étude.
Dans unes second partie de detaillerai la stratégie d'entrainement "étape par étape" qui permet de palier au probleme de périodicité de la fonction sinus.
Enfin dans la derniere partie le model sera mis à l'épreuve avec une évaluation et des tests sur des données bruitées par un ratio de dérive.

## Model IA

### La base des autoencodeurs

"Ecrit une introduction basique aux autoencodeurs"

### Mon model

"Voila mon model : class CNNEncoder(nn.Module):
    def __init__(self, encoded_size):
        super(CNNEncoder, self).__init__()
        self.encoder = nn.Sequential(
            nn.Conv1d(1, 16, kernel_size=5, stride=2, padding=2),  # [B, 16, 250]
            nn.ReLU(),
            nn.Conv1d(16, 32, kernel_size=5, stride=2, padding=2),  # [B, 32, 125]
            nn.ReLU(),
            nn.Conv1d(32, 64, kernel_size=5, stride=2, padding=2),  # [B, 64, ~63]
            nn.ReLU(),
            nn.Flatten(),
            nn.Linear(64 * 63, encoded_size)
        )

    def forward(self, x):
        return self.encoder(x)


class CNNDecoder(nn.Module):
    def __init__(self, encoded_size):
        super(CNNDecoder, self).__init__()
        self.decoder_input = nn.Linear(encoded_size, 64 * 63)
        self.decoder = nn.Sequential(
            nn.Unflatten(1, (64, 63)),
            nn.ConvTranspose1d(64, 32, kernel_size=5, stride=2, padding=2, output_padding=1),  # [B, 32, ~125]
            nn.ReLU(),
            nn.ConvTranspose1d(32, 16, kernel_size=5, stride=2, padding=2, output_padding=1),  # [B, 16, ~250]
            nn.ReLU(),
            nn.ConvTranspose1d(16, 1, kernel_size=5, stride=2, padding=2, output_padding=1),   # [B, 1, ~500]
        )

    def forward(self, x):
        x = self.decoder_input(x)
        x = self.decoder(x)
        return x


class CNNAutoencoder(nn.Module):
    def __init__(self, encoded_size):
        super(CNNAutoencoder, self).__init__()
        self.encoder = CNNEncoder(encoded_size)
        self.decoder = CNNDecoder(encoded_size)
        self.encoded_size = encoded_size

    def forward(self, x):
        latent = self.encoder(x)
        reconstructed = self.decoder(latent)
        return reconstructed ecrit un court paragraphe pour le décrire
"

## Entrainement du model


### Les données d'entrainement

L'entrainement est réalisé sur des sets de 12000 signaux de 500s, envoyés par patch de 32 au GPU.

### La stratégie étape par étape

"L'entrainement du modele é été réalisé étape par étape, voici les différentes étapes et leurs caractéristique, il faudrait les repertorier dans un tableau.
Etape 1 : La sunisoïde pure, sur 10 epochs, amplitude 4, phase 0, fréquence 0.05Hz, bruit Aucun
Etape 2 : Légere variation de l'amplitude, sur 30 epochs, amplitude Uniform(3.5,4.5), phase 0, fréquence 0.05Hz, bruit Aucun
Etape 3 : Et légère variation de la phase, sur 30 epochs, amplitude Uniform(3.5,4.5), phase Uniform(0,0.5), fréquence 0.05Hz, bruit Aucun
Etape 4 : Et légère variation de la fréquence, sur 100 epochs, amplitude Uniform(3.5,4.5), phase Uniform(0,0.5), fréquence Uniform(0.025,0.075)Hz, bruit Aucun
Etape 5 : Et ajout d'un faible bruit gaussien, sur 100 epochs, amplitude Uniform(3.5,4.5), phase Uniform(0,0.5), fréquence Uniform(0.025,0.075)Hz, bruit Loi normale(mu = 0, std = 0.05)
Etape 6 : Augmantation de la variation des parametre de la sinusoïde, sur 250 epochs, amplitude Uniform(2,6), phase Uniform(0,2pi), fréquence Uniform(0.025,0.225)Hz, bruit Loi normale(mu = 0, std = 0.05)
Etape 7 : Augmantation du bruit gaussien, sur 400 epochs, amplitude Uniform(2,6), phase Uniform(0,2pi), fréquence Uniform(0.025,0.225)Hz, bruit Loi normale(mu = 0, std = 0.25)
Chaque étape de l'entrainement démarre avec un learning rate assez élevé de 1.e-4. Avec un scheduler, ReduceLROnPlateau, qui a chaque epoche verifie que l'entrainement d'ai pas atteint un seuil qui l'empeche de progresser, et si c'est le cas diminue le mearning rate.
"

### Le bruit résiduel

LEs signaux contenant du bruit gaussien il est impossible pour le modele de prédire parfaitement le signal d'entrée. Il y a donc dans cette étude un bruit résiduel.
Si l'on reprend la formule de notre fonction de perte cf eqation précédente, on peut y decomposer le signal target comme la somme d'un signal informatif
et d'un bruit aléatoire. En supposant que notre modele est parfait on a alors output = signal informatif. Il reste dons une erreur résiduelle : 
mse = mean((output - (informativ_signal + random_noise)) ** 2)
residual error = mean(random_noise ** 2)
Dans le cas de notre étude le bruit résiduel sur la fonction de perte est de 0.0025 pour le bruit gaussien des étapes 4-6, puis de 0.0625 pour l'étape finale d'entrainement.

Ecrit une place pour que j'insert ici un graphique "sinusoïd_recognition.png" réprésentant l'évolution de la loss de validation pendant l'entrainement avec une légende que je pourrait modifier.


## Evaluation et test sur des données bruitées

### Evaluation du model

Le model est évalué sur 10000 nouveaux signaux générés comme a l'étape 7 de l'entrainement.
On retrouve une valeur médiane de la perte de 0.078+0.053-0.012 ce qui est suffisament proche de l'erreur résiduelle pour notre cas d'études. 

### Les données bruitées
On mat notre model d'autoencodeur a l'épreuve d'un bruit de dérive "petite definition d'un bruit de dérive et formula mathématique ici"

Voici quelques exemple de signaux obtenus apres ajout du bruit de dérive pour quatra valeur différente "Ici j'insere une image avec 4 graphe représentant le signal avec sa composante dérive et sa reconstitution par le model"

### Performance de détection d'anomalie

Comme on l'attendait, le model n'arrive pas a reproduire le signal d'entrée si il y a un bruit supplémentaire auquel il n'est pas entrainé. Sur le graphique suivant j'ai quantifié le score de perte de 1000 signaux pour 8 valeurs différentes de ratios de dérive.
J'ai choisit de placer trois zones de confiance, lors du calcul de la perte. 
OK : lorsque la perte est inférieur au 95 eme percentille de notre sample d'évaluation, le signal est avec confiance considéré comme normal.
WARNING : lorsque la perte est comprise entre le 95eme percentille de du sample d'évaluation mais inférieure a la valaur maximale obtenue dans l'évaluation, le signal est considéré comme a risque, et la tendance doit etre surveillée.
ANOMALY : au dela de la valeur maximale de perte obtenue dans le sample d'évaluation le signal est considéré comme anormale et des investigations plus poussées sont necessaires

Pour avoir une valeur comparable des résultats avec d'autres problemes j'ai évalué le Noise ratio defini comme
Noise ratio = total_drift/residual error
avec total_drift la dérive totale du signal sur sa durée.
On constate que notre model detecte des anomalies de derive a partir de 1.6*bruit résiduel, ce qui est très performant. Ces performance pourraient etre encore augmenté en prenant des signaux plus long pour accentuer la dérive totale.

## Conclusion

Les autoencodeurs sont efficace pour detecter des anomalies dans les signaux sinusoidaux bruités avec un ratio de dérive léger. Lorque ce ratio se rapprohe du bruit gaussien avec un Noise ratio < 1.6, le bruit devient trop faible pour etre discerné du bruit blanc sur 
la durée évaluée ici 500 secondes.

Ouverture : faire des test sur des signaux plus long permet de mettre en évidente des ratio de dérive plus faible. 
J'ai affirmé dans mon étude que le probleme rencontré a l'entrainement direct avec les données complexee provenait e la caractéristique sinusoidale du signale, qui automatiquement
donne un signal moyen null. Cette affiramtion mérite une investigation plus large (peut etre une future étude),il sera possible d'ajouter un offser, de travailler avec des fonctions de heavyside, ou encore des triangle.
Ce cas reste un cas d'école, prototypé avec des données construitent, il faudrait mettre cette strategie a l'épreuve sur des données réelles.

