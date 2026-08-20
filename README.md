# ComfyUI Formation

Ce dépôt contient mes exercices et workflows réalisés pendant ma formation ComfyUI.

## Structure


### Jour 1
#### Objectif
```
Être capable d'installer son environnement de travail autonome, de comprendre le cycle de vie d'une image SD/Flux basique et d'initier un dépôt de sauvegarde.

```
#### Livrables 
```
- Installation de ComfyUI (via Express Install ou Stability Matrix pour plus de simplicité).
- Création d'un workflow minimaliste de découpe / traitement d'image (Crop/Pad).
- Initialisation d'un dépôt Git (ex. sur GitLab/GitHub) et premier git push du fichier de workflow JSON.


```



### Jour 2
#### Objectif
```
Maîtriser le concept de routage de données, comprendre la différence entre les types de données (Latent, Image, Model, Conditioning) et utiliser des nœuds primitifs pour paramétrer proprement son espace.

```
#### Livrables 
```
- Un workflow Text-to-Image fonctionnel utilisant des nœuds primitifs pour centraliser les contrôles (Seed, Dimensions, Prompts).
- Génération de 3 variantes d'une scène d'architecture extérieure à partir d'un prompt textuel pur.

```


### Jour 3
#### Objectif
```
Comprendre l'architecture d'un modèle d'IA générative. Savoir identifier et utiliser un Checkpoint (SDXL vs Flux vs SD 1.5), configurer le VAE approprié pour éviter les couleurs ternes et ajuster les Samplers/Schedulers (K-Sampler).

```
#### Livrables 
```
- Un workflow documenté comparant l'impact de 2 échantillonneurs différents (ex. Euler a vs DPM++ 2M SDE) sur le rendu des matières (bois, verre).

```



### Jour 4
#### Objectif
```
Utiliser une image existante (un rendu 3D basique ou un croquis de villa) comme point de départ pour générer un visuel réaliste en contrôlant finement le facteur de débruitage (Denoising Strength).

```
#### Livrables 
```
- Un workflow Img2Img fonctionnel.
- Une planche comparative montrant une progression de denoise de 0.1 à 0.9 pour comprendre le niveau de liberté laissé à l'IA sur un bâtiment d'architecture.

```

### Jour 5
#### Objectif
```
- Savoir organiser ses workflows complexes pour qu'ils restent lisibles (utilisation des groupes, couleurs, notes)
- Maîtriser le ComfyUI Manager pour installer des noeuds personnalisés (Custom Nodes) et résoudre les conflits de noeuds manquants (Red Nodes).

```
#### Livrables 
```
- Un workflow nettoyé, structuré par "Boîtes de Groupes" de couleurs différentes (Input, Processing, Output).
- Installation réussie de ses premiers Custom Nodes (ex. Pixaroma Nodes ou Impact Pack).

```

### Jour 6
#### Objectif
```
Forcer l'IA à respecter les contours d'un projet architectural ou d'un produit en utilisant ControlNet (modèle Canny ou Lineart).

```
#### Livrables 
```
- Un workflow ControlNet Canny.
- Transformation d'un simple dessin au trait d'un bâtiment en une photo de bâtiment ultra-réaliste respectant strictement les lignes d'origine.

```


### Jour 7
#### Objectif
```
Maîtriser la gestion de la profondeur spatiale pour intégrer des objets ou générer des scènes d'intérieur réalistes en utilisant ControlNet Depth.

```
#### Livrables 
```
- Un workflow convertissant une image de salon vide en carte de profondeur (Depth Map) pour y générer du mobilier parfaitement positionné dans l'espace.

```

## Auteur

Denis Harivelo