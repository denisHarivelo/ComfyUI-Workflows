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





## Auteur

Denis Harivelo