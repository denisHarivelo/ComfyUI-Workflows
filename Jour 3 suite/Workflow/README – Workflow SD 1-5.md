# README – Workflow SD 1.5 - Comparaison de Samplers

## 📖 Présentation

Ce workflow ComfyUI a été conçu pour générer simultanément plusieurs images à partir des **mêmes paramètres** afin de comparer les performances de différents **Samplers**.

Toutes les générations utilisent :

- Le même modèle (Checkpoint)
- Le même Prompt
- Le même Prompt Négatif
- La même résolution
- Le même Seed
- Le même Latent

Seul le **Sampler** est différent, ce qui permet d'observer rapidement les différences de rendu.

---

# ✨ Fonctionnalités

- ✅ Interface simple grâce aux paramètres centralisés.
- ✅ Un seul Prompt pour toutes les générations.
- ✅ Un seul Prompt Négatif.
- ✅ Un seul Seed partagé.
- ✅ Un seul Latent partagé.
- ✅ Comparaison instantanée de plusieurs Samplers.
- ✅ Plusieurs fenêtres Preview pour comparer les résultats.

---

# 📦 Prérequis

## Version

- ComfyUI récent

## Modèle utilisé

Le workflow est configuré avec :

```
realisticStockPhoto_v30SD15.safetensors
https://civitai.com/models/139565/realistic-stock-photo?modelVersionId=524032
```

À placer dans :

```
ComfyUI/models/checkpoints/
```

---

# 🔧 Custom Nodes

Aucun.

Ce workflow utilise uniquement les **Nodes natifs de ComfyUI**.

---

# 🗂 Structure du workflow

## 1. PARAMETRE IMAGE

Cette section regroupe tous les paramètres que l'utilisateur peut modifier.

Vous pouvez changer uniquement :

- Prompt
- Prompt négatif
- Largeur (Width)
- Hauteur (Height)
- Batch Size
- Seed

Toutes les autres parties du workflow seront automatiquement mises à jour.

---

## 2. Checkpoint Loader

Charge le modèle Stable Diffusion utilisé par toutes les générations.

---

## 3. CLIP Encode

Deux encodeurs sont présents :

- Prompt Positif
- Prompt Négatif

Ils convertissent le texte en conditionnement utilisable par le modèle.

---

## 4. Empty Latent

Création du bruit initial (Latent).

Toutes les générations utilisent exactement le même Latent afin d'obtenir une comparaison fiable.

---

## 5. KSampler

Chaque branche possède un KSampler différent.

Exemple :

- DPM++ 2M SDE
- Euler

Chaque Sampler produit sa propre image.

---

## 6. VAE Decode

Transformation du Latent en image RGB.

---

## 7. Preview

Chaque résultat est affiché dans une fenêtre Preview indépendante.

---

# 🚀 Utilisation

## Étape 1

Choisir le modèle (Checkpoint)."realisticStockPhoto_v30SD15.safetensors"

---

## Étape 2

Modifier le Prompt.

Exemple :

```
high-end contemporary architectural interior, large living room with polished oak wood panels, floor-to-ceiling glass walls, complex daylight reflections, textured stone floor, soft shadows, visible wood grain, realistic glass reflections, precise perspective, ultra detailed, photorealistic architectural visualization
```

---

## Étape 3

Modifier le Prompt Négatif.

Exemple :

```
low quality, blurry, noisy image, distorted geometry, warped lines, duplicate objects, extra furniture, bad reflections, plastic materials, flat lighting, text, watermark, overexposed, underexposed
```

---

## Étape 4

Choisir la résolution.

Exemple :

```
1024 x 1024
```

ou

```
768 x 1024
```

---

## Étape 5

Définir le Seed.

Pour reproduire exactement une image :

```
Randomize OFF
```

Pour générer une nouvelle image à chaque exécution :

```
Randomize ON
```

---

## Étape 6

Cliquer sur :

```
Queue Prompt
```

Le workflow génère automatiquement une image pour chaque Sampler.

---

# 💡 Conseils

Pour une comparaison pertinente :

Toujours conserver :

- Le même Prompt
- Le même Seed
- La même résolution
- Le même modèle

Modifier uniquement le Sampler.

---

# 📊 Résultats

Le workflow permet de comparer :

- La netteté
- Les détails
- Les textures
- Le respect du Prompt
- La qualité de l'éclairage
- Les performances des différents Samplers

---


