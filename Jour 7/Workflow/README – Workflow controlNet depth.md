# JOUR 7 — ControlNet Partie 2 : Volumes & Profondeur

## 🎯 Objectif de la journée

Maîtriser **ControlNet Depth** pour comprendre comment l'IA interprète la profondeur et les volumes d'une scène.

L'objectif est de partir d'une image existante, d'en extraire une **Depth Map**, puis d'utiliser cette information avec ControlNet pour générer une image réaliste tout en conservant :

- la perspective ;
- les volumes ;
- la position des surfaces ;
- la profondeur de la scène ;
- la structure générale de l'architecture.

Le workflow permet également de comparer **ControlNet Canny** et **ControlNet Depth**.

---

# 🧠 Principe du workflow

Le fonctionnement principal est :

```text
IMAGE DE DÉPART
      │
      ├──────────────────────────────┐
      │                              │
      ▼                              ▼
 CONTROLNET CANNY              DEPTH ESTIMATION
      │                              │
      │                         MiDaS Depth
      │                              │
      │                              ▼
      │                         DEPTH MAP
      │                              │
      ▼                              ▼
 CONTROLNET CANNY              CONTROLNET DEPTH
      │                              │
      └──────────────┬───────────────┘
                     │
                     ▼
              CONDITIONING
                     │
                     ▼
                  KSAMPLER
                     │
                     ▼
                 VAE DECODE
                     │
                     ▼
                IMAGE FINALE
```

---

# 🏗️ Structure du workflow

Le workflow est organisé en plusieurs groupes :

```text
PARAMETRE GLOBAL
        │
        ▼
PROCESSING
        │
        ├── CONTROLNET
        │
        ├── CONTROLNET + Canny Edge
        │
        └── CONTROLNET + Depth Midas
```

Ces groupes sont directement présents dans le workflow JSON. :contentReference[oaicite:2]{index=2}

---

# 📥 1. IMAGE DE DÉPART

Le nœud :

```text
LoadImage
```

charge l'image utilisée comme référence.

Dans le workflow actuel, l'image chargée est :

```text
Croquis Architectural (1).jpg
```

:contentReference[oaicite:3]{index=3}

### Pour l'exercice

Tu peux remplacer cette image par :

- une photo d'un salon vide ;
- un rendu 3D d'un intérieur ;
- un croquis architectural ;
- une photo d'une villa ;
- une scène d'intérieur.

Pour l'objectif du Jour 7, une **pièce vide avec une perspective claire** est particulièrement intéressante.

---

# 🧠 2. MODÈLE DE GÉNÉRATION

Le workflow utilise :

```text
Juggernaut-XL_v9_RunDiffusionPhoto_v2.safetensors
```

avec un :

```text
CheckpointLoaderSimple
```

Le Checkpoint fournit :

```text
MODEL
CLIP
VAE
```

:contentReference[oaicite:4]{index=4}

### Rôle

Le modèle est responsable de la génération de l'image réaliste.

Dans ce workflow :

```text
Checkpoint
      │
      ├── MODEL
      ├── CLIP
      └── VAE
```

---

# ✍️ 3. PROMPT POSITIF

Le workflow possède un nœud :

```text
Prompt
```

qui contient actuellement une description d'architecture moderne. :contentReference[oaicite:5]{index=5}

Pour ton exercice d'aujourd'hui, tu peux remplacer ce prompt par une description correspondant à ton intérieur.

### Exemple

```text
photorealistic modern luxury living room, contemporary interior design, elegant beige sectional sofa, modern coffee table, natural oak furniture, indoor plants, large windows, realistic glass, natural daylight, realistic materials, realistic proportions, realistic shadows, architectural photography, photorealistic, highly detailed
```

---

# 🚫 4. PROMPT NÉGATIF

Le workflow contient également un :

```text
Prompt negative
```

avec notamment :

```text
low quality
blurry
sketch
drawing
illustration
cartoon
anime
watercolor
painting
line art
blueprint
wireframe
distorted perspective
duplicated windows
bad architecture
low resolution
noisy
oversaturated
```

:contentReference[oaicite:6]{index=6}

Son rôle est de demander au modèle d'éviter ces caractéristiques.

---

# 🔗 5. SETNODE / GETNODE

Ton workflow utilise **KJNodes** pour centraliser les paramètres.

Les nœuds sont identifiés dans le JSON comme :

```text
aux_id: kijai/ComfyUI-KJNodes
```

:contentReference[oaicite:7]{index=7}

## SetNode

Le SetNode définit une valeur.

Exemple :

```text
Set_Model
Set_VAE
Set_seed
Set_Prompt+
Set_prompt-
Set_image-depart
Set_rendu-image
```

:contentReference[oaicite:8]{index=8}

## GetNode

Le GetNode récupère ensuite cette valeur où elle est nécessaire.

Exemple :

```text
Get_Model
Get_VAE
Get_seed
Get_image-depart
Get_positive-prompt
Get_negative-prompt
```

:contentReference[oaicite:9]{index=9} :contentReference[oaicite:10]{index=10}

### Pourquoi ?

Cela permet d'éviter de relier physiquement chaque paramètre à plusieurs endroits.

Le workflow devient donc plus facile à modifier et à maintenir.

---

# 🧠 6. CONTROLNET CANNY

Une première branche utilise :

```text
ControlNet Canny
```

avec :

```text
controlnet_canny_sdxl_xinsir.safetensors
```

:contentReference[oaicite:11]{index=11}

Le workflow utilise ensuite :

```text
CannyEdgePreprocessor
```

avec :

```text
Low Threshold  = 100
High Threshold = 200
Resolution     = 1024
```

:contentReference[oaicite:12]{index=12}

### Rôle de Canny

Canny cherche principalement les **contours** :

```text
mur
fenêtre
porte
bord du bâtiment
ligne du toit
```

Il répond donc surtout à :

> "Où sont les contours de la scène ?"

---

# 🌐 7. CONTROLNET DEPTH

C'est la partie principale de l'exercice du Jour 7.

Le workflow utilise :

```text
controlnet_depth_sdxl_xinsir.safetensors
```

:contentReference[oaicite:13]{index=13}

---

# 📐 8. MiDaS Depth Map

Avant ControlNet, l'image passe dans :

```text
MiDaS-DepthMapPreprocessor
```

:contentReference[oaicite:14]{index=14}

Ses paramètres actuels sont :

```text
a            = 0.5
bg_threshold = 0.1
resolution   = 512
```

### Son rôle

MiDaS analyse l'image et estime la profondeur des différents éléments.

Conceptuellement :

```text
CAMÉRA
   │
   │
   ▼
[objet proche]
      ↓
   [sol]
      ↓
   [mur]
      ↓
[arrière-plan]
```

La sortie est une **Depth Map**.

---

# 👁️ 9. VISUALISER LA DEPTH MAP

Le workflow contient :

```text
PreviewImage
```

connecté directement à la sortie de MiDaS.

Cela permet de voir la carte de profondeur générée avant de l'envoyer à ControlNet. :contentReference[oaicite:15]{index=15}

### Ce que tu dois observer

Une bonne Depth Map doit représenter clairement :

- premier plan ;
- milieu de la scène ;
- arrière-plan ;
- murs ;
- sol ;
- plafond ;
- objets ;
- volumes.

---

# 🎮 10. ControlNetApplyAdvanced

La Depth Map est ensuite envoyée dans :

```text
ControlNetApplyAdvanced
```

:contentReference[oaicite:16]{index=16}

Paramètres actuels :

```text
Strength     = 0.6
Start        = 0
End          = 0.7
```

### Signification

#### Strength

Contrôle la force de l'influence de ControlNet.

```text
0.3 → faible contrôle
0.5 → contrôle moyen
0.6 → contrôle actuel
0.7 → contrôle fort
1.0 → contrôle très fort
```

#### Start Percent

```text
0
```

ControlNet commence dès le début de la génération.

#### End Percent

```text
0.7
```

ControlNet agit jusqu'à environ 70 % du processus de génération.

---

# ⚙️ 11. KSampler

Le workflow utilise :

```text
KSampler
```

avec actuellement :

```text
Steps     = 22
CFG       = 6.5
Sampler   = Euler
Scheduler = Karras
Denoise   = 1
```

:contentReference[oaicite:17]{index=17}

### Rôle

Le KSampler réalise la génération proprement dite.

Il reçoit :

```text
MODEL
   +
POSITIVE
   +
NEGATIVE
   +
LATENT
   +
SEED
```

et produit :

```text
LATENT IMAGE
```

---

# 🔢 12. SEED

Le workflow possède un nœud :

```text
Seed
```

puis :

```text
Set_seed
Get_seed
```

:contentReference[oaicite:18]{index=18}

### Pour tes comparaisons

Il est important d'utiliser **le même seed**.

Exemple :

```text
Seed = 123456789
```

Puis comparer :

```text
ControlNet OFF
ControlNet Canny
ControlNet Depth 0.3
ControlNet Depth 0.5
ControlNet Depth 0.7
ControlNet Depth 1.0
```

Cela permet de comparer réellement l'influence du contrôle.

---

# 🖼️ 13. VAE ENCODE

Le workflow contient :

```text
VAEEncode
```

qui transforme l'image de départ en représentation latente. :contentReference[oaicite:19]{index=19}

Conceptuellement :

```text
IMAGE
  ↓
VAE ENCODE
  ↓
LATENT
```

Le latent est ensuite utilisé par le KSampler.

---

# 🖼️ 14. VAE DECODE

Après la génération :

```text
KSampler
     ↓
LATENT
     ↓
VAE Decode
     ↓
IMAGE
```

Le workflow utilise :

```text
VAEDecode
```

:contentReference[oaicite:20]{index=20}

---

# 🔍 15. COMPARAISON DES IMAGES

Le workflow contient :

```text
ImageCompare
```

Ce nœud reçoit deux images :

```text
IMAGE A
IMAGE B
```

:contentReference[oaicite:21]{index=21}

Il est particulièrement utile pour ton apprentissage.

Tu peux comparer :

```text
Image originale
        VS
Image générée
```

ou :

```text
Canny
   VS
Depth
```

---

# 🧪 EXERCICE 1 — Comprendre la Depth Map

Utilise ton image architecturale actuelle.

### Étape 1

Charge :

```text
Croquis Architectural (1).jpg
```

### Étape 2

Lance :

```text
MiDaS-DepthMapPreprocessor
```

### Étape 3

Observe la sortie dans :

```text
PreviewImage
```

### Question

Identifie :

- premier plan ;
- milieu ;
- arrière-plan ;
- bâtiment ;
- arbres ;
- piscine ;
- sol.

---

# 🧪 EXERCICE 2 — Tester ControlNet Depth

Commence avec :

```text
Strength = 0.3
```

Puis :

```text
0.5
0.6
0.7
1.0
```

Garde :

```text
Seed       = identique
Prompt     = identique
Negative   = identique
Steps      = 22
CFG        = 6.5
Sampler    = Euler
Scheduler  = Karras
```

---

# 📊 TABLEAU DE COMPARAISON

| Test | Depth Strength | Ce que je dois observer |
|---|---:|---|
| 01 | 0.3 | Faible influence de la profondeur |
| 02 | 0.5 | Contrôle spatial modéré |
| 03 | 0.6 | Réglage actuel du workflow |
| 04 | 0.7 | Conservation plus forte des volumes |
| 05 | 1.0 | Contrôle très fort, risque de rigidité |

---

# 🧪 EXERCICE 3 — Canny VS Depth

Le workflow contient volontairement deux méthodes.

## Canny

```text
IMAGE
 ↓
Canny Edge
 ↓
ControlNet Canny
 ↓
Génération
```

Il contrôle principalement :

**LES CONTOURS**

---

## Depth

```text
IMAGE
 ↓
MiDaS
 ↓
Depth Map
 ↓
ControlNet Depth
 ↓
Génération
```

Il contrôle principalement :

**LA PROFONDEUR ET LES VOLUMES**

---

# 🧠 À retenir

## Canny

```text
Canny = STRUCTURE DES CONTOURS
```

Très utile pour :

- architecture ;
- lignes ;
- formes ;
- contours ;
- croquis.

---

## Depth

```text
Depth = PROFONDEUR SPATIALE
```

Très utile pour :

- intérieur ;
- architecture ;
- mobilier ;
- perspective ;
- volumes ;
- placement spatial.

---

# 🏠 APPLICATION À TON OBJECTIF DU JOUR

Pour atteindre réellement l'objectif :

> "Générer du mobilier parfaitement positionné dans l'espace"

utilise une **photo ou un rendu 3D d'un salon vide**.

Par exemple :

```text
SALON VIDE
     ↓
MiDaS
     ↓
DEPTH MAP
     ↓
CONTROLNET DEPTH
     ↓
Prompt :
modern beige sofa,
coffee table,
floor lamp,
indoor plants
     ↓
KSampler
     ↓
SALON MEUBLÉ
```

L'objectif n'est pas seulement d'obtenir un beau salon.

Tu dois vérifier si :

- le canapé respecte la perspective ;
- les pieds reposent correctement sur le sol ;
- la table respecte la profondeur ;
- les meubles ne flottent pas ;
- les proportions restent cohérentes ;
- les murs et ouvertures restent correctement positionnés.

---

# 🎯 EXERCICE FINAL DU JOUR

Créer une scène d'intérieur avec :

```text
Image de départ :
Salon vide

Depth :
MiDaS

ControlNet :
Depth SDXL

Modèle :
Juggernaut XL

Prompt :
Mobilier moderne

Tests :
0.3
0.5
0.7
1.0
```

Créer une planche :

```text
┌────────────┬────────────┐
│ Original   │ Depth Map  │
├────────────┼────────────┤
│ Depth 0.3  │ Depth 0.5  │
├────────────┼────────────┤
│ Depth 0.7  │ Depth 1.0  │
└────────────┴────────────┘
```

---

# 📦 Livrables

À la fin du Jour 7, je dois avoir :

- [ ] Workflow ControlNet Depth fonctionnel
- [ ] Image de départ
- [ ] Depth Map générée avec MiDaS
- [ ] Résultat avec ControlNet Depth
- [ ] Comparaison Strength 0.3 / 0.5 / 0.7 / 1.0
- [ ] Comparaison Canny VS Depth
- [ ] Planche comparative
- [ ] Workflow JSON documenté

---

# 🧠 Questions de validation

Je dois être capable d'expliquer :

### 1. Qu'est-ce qu'une Depth Map ?

Une représentation de la profondeur estimée d'une scène.

### 2. Quel est le rôle de MiDaS ?

Estimer la profondeur de l'image pour produire une Depth Map.

### 3. Quel est le rôle de ControlNet Depth ?

Utiliser cette information de profondeur pour guider la génération.

### 4. Quelle différence entre Canny et Depth ?

```text
Canny → contours
Depth → profondeur / volumes
```

### 5. Que fait Strength ?

Il contrôle l'intensité de l'influence de ControlNet.

### 6. Pourquoi garder le même Seed ?

Pour rendre les comparaisons plus fiables.

---

# 🏆 Objectif de maîtrise

À la fin de cette journée, je dois comprendre cette chaîne :

```text
IMAGE
  ↓
DEPTH ESTIMATION
  ↓
DEPTH MAP
  ↓
CONTROLNET DEPTH
  ↓
CONDITIONING
  ↓
KSAMPLER
  ↓
VAE DECODE
  ↓
IMAGE RÉALISTE
```

Et surtout comprendre la différence fondamentale :

```text
CANNY
  ↓
"CONSERVE LES CONTOURS"

DEPTH
  ↓
"CONSERVE LA PROFONDEUR ET LES VOLUMES"
```

---

# 📌 Note sur le workflow fourni

Le workflow actuel est déjà configuré pour l'apprentissage de **ControlNet Depth** avec :

```text
Checkpoint :
Juggernaut-XL_v9_RunDiffusionPhoto_v2.safetensors

Depth ControlNet :
controlnet_depth_sdxl_xinsir.safetensors

Depth Preprocessor :
MiDaS

MiDaS Resolution :
512

Depth Strength :
0.6

Start :
0

End :
0.7

KSampler :
22 steps
CFG 6.5
Euler
Karras
Denoise 1.0
```
