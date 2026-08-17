# Jour 6 — ControlNet Canny : Croquis vers rendu architectural réaliste

## Objectif

Transformer un croquis ou un wireframe architectural en rendu photoréaliste tout en respectant la géométrie, la perspective, les fenêtres et la toiture du bâtiment.

Le workflow utilise **SDXL + ControlNet Canny**.  
Canny extrait les contours du dessin et ControlNet les utilise comme contrainte pendant la génération.

---

## Résultat attendu

À partir d’un croquis architectural, générer une villa réaliste avec :

- La même structure générale
- La même perspective
- Les mêmes grandes ouvertures et lignes de toiture
- Des matériaux réalistes : béton, bois, verre
- Une lumière naturelle et un rendu architectural professionnel

---

## Modèles utilisés

| Élément | Fichier utilisé | Rôle |
|---|---|---|
| Checkpoint SDXL | `Juggernaut-XL_v9_RunDiffusionPhoto_v2.safetensors` | Génère le rendu réaliste |
| ControlNet Canny SDXL | `controlnet_canny_sdxl_xinsir.safetensors` | Force le respect des contours |
| Image source | `Wireframe 3d.jpg` ou votre croquis | Sert de guide structurel |

> Important : le checkpoint et le ControlNet doivent être de la même famille.  
> Ici, le checkpoint est **SDXL**, donc le ControlNet doit aussi être **SDXL**.

---

## Extensions nécessaires

Le workflow utilise les extensions suivantes :

| Extension | Utilisation |
|---|---|
| `ComfyUI-KJNodes` | Nœuds `SetNode`, `GetNode`, variables nommées et workflow propre |
| `rgthree-comfy` | `Any Switch` et `Fast Groups Bypasser` |
| `comfyui_controlnet_aux` ou équivalent | `CannyEdgePreprocessor` |
| Pack avec `ImageCompare` | Comparaison entre croquis et rendu final |

### Installation

1. Ouvrir **ComfyUI Manager**
2. Aller dans **Custom Nodes**
3. Rechercher et installer les extensions manquantes
4. Redémarrer ComfyUI
5. Recharger le workflow

Si un nœud apparaît en rouge, il manque généralement une extension ou une dépendance.

---

## Structure du workflow

```text
Image source
    ↓
Canny Edge Preprocessor
    ↓
ControlNet Canny
    ↓
ControlNet Apply
    ↓
KSampler
    ↓
VAE Decode
    ↓
Preview / Save Image
```

### Logique

```text
Croquis → extraction des contours → contrôle de la structure
        → génération SDXL → rendu architectural réaliste
```

---

## Rôle des nœuds principaux

| Nœud | Rôle |
|---|---|
| `Load Image` | Charge le croquis ou wireframe architectural |
| `CheckpointLoaderSimple` | Charge le modèle Juggernaut XL |
| `CannyEdgePreprocessor` | Extrait les lignes et contours de l’image |
| `ControlNetLoader` | Charge le modèle Canny SDXL |
| `ControlNetApply` | Applique la contrainte Canny au prompt positif |
| `CLIPTextEncode` | Transforme les prompts positif et négatif en instructions pour le modèle |
| `VAEEncode` | Convertit l’image source en espace latent pour la branche img2img |
| `KSampler` | Réalise la génération à partir du modèle, prompts et ControlNet |
| `VAEDecode` | Transforme le latent généré en image visible |
| `Any Switch` | Permet de choisir une branche ControlNet |
| `ImageCompare` | Compare l’image source et le rendu final |
| `SetNode / GetNode` | Évite les longs câbles en transmettant des variables nommées |

---

## Paramètres du workflow

### Canny Edge Preprocessor

| Paramètre | Valeur initiale | Rôle |
|---|---:|---|
| Low threshold | `100` | Détecte les contours moins forts |
| High threshold | `200` | Conserve les contours les plus marqués |
| Resolution | `1024` | Résolution de la carte de contours |

### ControlNet

Le workflow contient deux branches :

| Branche | ControlNet Strength | Résultat attendu |
|---|---:|---|
| Branche A | `0.80` | Bon équilibre entre fidélité et réalisme |
| Branche B | `0.89` | Respect plus strict des lignes du croquis |

### KSampler principal

| Paramètre | Valeur |
|---|---:|
| Steps | `22` |
| CFG | `6.5` |
| Sampler | `dpmpp_sde` |
| Scheduler | `karras` |
| Denoise | `1.0` |
| Batch size | `1` |

> `Denoise = 1.0` signifie que le rendu part du bruit.  
> La structure du bâtiment est conservée grâce au **ControlNet Canny**, pas grâce à l’img2img.

---

## Prompt positif

```text
photorealistic architectural visualization of the same contemporary two-story villa, preserve the exact original building geometry and camera perspective, flat roof with wide overhang, large floor-to-ceiling glass walls, white concrete volumes, warm vertical wood cladding, realistic interior visible through the windows, refined landscape, natural daylight, physically accurate materials, realistic glass reflections, soft shadows, professional architectural photography, highly detailed, ultra realistic
```

## Prompt négatif

```text
sketch, pencil drawing, line art, cartoon, illustration, blueprint, wireframe, distorted architecture, warped walls, crooked windows, extra floors, duplicate windows, broken perspective, asymmetrical building, deformed roof, floating objects, blurry, low resolution, noisy, oversaturated, text, watermark, logo
```

---

## Étapes d’utilisation

### 1. Charger le workflow

- Ouvrir ComfyUI
- Importer `Workflow-jour6.json`
- Installer les éventuels nœuds manquants
- Redémarrer ComfyUI si nécessaire

### 2. Charger l’image de référence

Dans le nœud `Load Image` :

- Remplacer `Wireframe 3d.jpg`
- Charger le croquis architectural à transformer

### 3. Vérifier le résultat Canny

Regarder le `PreviewImage` connecté au `CannyEdgePreprocessor`.

La carte doit afficher :

- Les lignes de toiture
- Les contours du bâtiment
- Les ouvertures
- Les baies vitrées
- Les lignes principales de perspective

### 4. Choisir une force ControlNet

Dans `Any Switch`, tester :

```text
Branche A : strength = 0.80
Branche B : strength = 0.89
```

### 5. Générer

- Garder le même prompt
- Garder le même seed pour comparer
- Cliquer sur **Queue Prompt**

### 6. Comparer

Utiliser `ImageCompare` pour observer :

- La fidélité de la toiture
- La position des fenêtres
- La perspective
- La stabilité des volumes
- La qualité des matériaux
- Le réalisme du verre, du béton et du bois

---

## Tests recommandés

| Test | Strength | Ce qu’il faut observer |
|---|---:|---|
| Test A | `0.70` | Plus de liberté créative, risque de déformation |
| Test B | `0.80` | Bon compromis réalisme / fidélité |
| Test C | `0.89` | Structure plus fidèle au croquis |
| Test D | `0.95` | Respect maximal, rendu parfois plus rigide |

Pour faire un comparatif propre :

- Garder le même croquis
- Garder le même prompt
- Garder le même checkpoint
- Garder le même seed
- Modifier uniquement `ControlNet strength`

---

## Dépannage

### Le rendu ne respecte pas le croquis

Essayer :

```text
ControlNet strength : 0.90 à 0.95
```

Vérifier aussi que la carte Canny contient clairement les lignes du bâtiment.

### Le rendu ressemble encore à un croquis

Vérifier que le prompt positif contient :

```text
photorealistic
architectural visualization
physically accurate materials
professional architectural photography
```

Vérifier que le prompt négatif contient :

```text
sketch
line art
pencil drawing
wireframe
blueprint
```

### Trop de lignes ou de bruit dans Canny

Augmenter progressivement :

```text
low threshold : 120 à 150
high threshold : 220 à 250
```

### Pas assez de lignes visibles dans Canny

Diminuer progressivement :

```text
low threshold : 50 à 80
high threshold : 120 à 180
```

### Erreur CUDA / manque de VRAM

Pour une RTX 3060 Laptop avec 6 Go VRAM :

- Utiliser un batch de `1`
- Commencer à `512 × 768`
- Éviter plusieurs ControlNet actifs en même temps
- Ne pas utiliser le Refiner SDXL
- Fermer les autres applications utilisant le GPU
- Réduire la résolution si nécessaire

---

## Livrable du Jour 6

Créer une planche comparative contenant :

1. Le croquis d’origine
2. La carte Canny
3. Un rendu avec `strength = 0.80`
4. Un rendu avec `strength = 0.89`
5. Un rendu avec `strength = 0.95`
6. Une conclusion courte sur le rapport entre force ControlNet et fidélité structurelle

### Exemple de conclusion

> Avec une force de 0.80, le modèle garde la structure générale tout en ajoutant plus de liberté dans les matières et l’éclairage. À 0.89, les lignes architecturales sont plus fidèles au croquis. À 0.95, la géométrie est très respectée, mais le rendu peut devenir plus rigide.