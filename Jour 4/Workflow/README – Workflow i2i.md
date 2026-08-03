# JOUR 4 - Image-to-Image (Img2Img) & Denoising Strength

## 🎯 Objectif de la journée

Apprendre à utiliser le mode **Image-to-Image (Img2Img)** dans ComfyUI afin de transformer une image existante (croquis, rendu 3D ou modèle blanc) en un rendu photoréaliste grâce au contrôle du paramètre **Denoising Strength**.

L'objectif est de comprendre comment le niveau de **Denoise** influence la fidélité de l'image originale et la liberté laissée au modèle d'IA.

---

# 📚 Modèle utilisé

**Juggernaut Ragnarök XL (.safetensors)**

- **Architecture :** SDXL
- **Type :** Modèle photoréaliste
- **Utilisation :** Architecture, design d'intérieur, immobilier, produits, portraits et paysages.
- **Points forts :**
  - Excellent rendu des matériaux (bois, béton, verre)
  - Très bon réalisme des textures
  - Gestion naturelle de la lumière
  - Idéal pour les visualisations architecturales

---

# 🎓 Compétences acquises

À la fin de cette journée, vous serez capable de :

- Comprendre le fonctionnement du workflow Image-to-Image.
- Encoder une image avec le VAE.
- Transformer un croquis ou un rendu 3D en image photoréaliste.
- Contrôler précisément le paramètre **Denoising Strength**.
- Comprendre la relation entre le Denoise et la créativité du modèle.
- Choisir la bonne valeur de Denoise selon le résultat recherché.

---

# 🔄 Workflow utilisé


Load Image
      │
      ▼
VAE Encode
      │
      ▼
Checkpoint Loader (Juggernaut Ragnarök XL)
      │
      ├────────► CLIP Text Encode (Positive)
      │
      ├────────► CLIP Text Encode (Negative)
      │
      ▼
KSampler
      │
      ▼
VAE Decode
      │
      ▼
Save Image


---

# ⚙️ Paramètres du KSampler

| Paramètre | Valeur |
|-----------|---------|
| Modèle | Juggernaut Ragnarök XL |
| Steps | 45 |
| CFG | 8 |
| Sampler | Euler |
| Scheduler | Karras |
| Seed | Fixe |
| Denoise | Variable (0.1 → 0.9) |

---

# 🧪 Expérience réalisée

Une seule variable est modifiée :

## Denoising Strength

| Test | Denoise |
|------|---------:|
| Test 1 | 0.1 |
| Test 2 | 0.2 |
| Test 3 | 0.3 |
| Test 4 | 0.4 |
| Test 5 | 0.5 |
| Test 6 | 0.6 |
| Test 7 | 0.7 |
| Test 8 | 0.8 |
| Test 9 | 0.9 |

Tous les autres paramètres restent identiques afin d'observer uniquement l'impact du **Denoising Strength**.

---

# 📖 Comprendre le Denoise

Le **Denoising Strength** détermine le niveau de liberté laissé au modèle pour modifier l'image de départ.

| Denoise | Résultat attendu |
|----------|------------------|
| 0.1 | L'image reste presque identique. |
| 0.2 | Très légère amélioration des détails. |
| 0.3 | Ajout discret de textures et de lumière. |
| 0.4 | Amélioration visible du réalisme. |
| 0.5 | Bon équilibre entre fidélité et créativité. |
| 0.6 | Le modèle commence à modifier davantage l'image. |
| 0.7 | Changements importants des matériaux et des détails. |
| 0.8 | Forte réinterprétation de l'image. |
| 0.9 | Nouvelle image inspirée de l'original. |

---

# 🏡 Prompt utilisé


contemporary two-story house, minimalist modern architecture, large floor-to-ceiling glass walls, clean white concrete volumes, flat roof with wide overhang, warm wood ceiling panels, elegant balcony on the upper floor, open living room interior visible through the glass, modern kitchen, interior plants, realistic daylight, soft shadows, architectural visualization, photorealistic, high detail, premium residential desig   

---

# 🚫 Negative Prompt

sketch, pencil drawing, line art, low quality, blurry, distorted geometry, crooked lines, extra floors, warped windows, broken perspective, duplicate furniture, messy interior, cartoon, unrealistic materials, oversatur ated, text, watermark              

---

# 📊 Observations

## Denoise faible (0.1 - 0.3)

- Respecte fortement la géométrie.
- Préserve les proportions.
- Ajoute uniquement des détails.
- Convient aux améliorations légères.

### Cas d'utilisation

- Amélioration d'un rendu existant.
- Nettoyage d'image.
- Upscaling.
- Architecture.

---

## Denoise moyen (0.4 - 0.6)

- Excellent compromis.
- Les matériaux deviennent plus réalistes.
- L'éclairage est amélioré.
- La structure du bâtiment reste fidèle.

### Cas d'utilisation

- Visualisation architecturale.
- Design d'intérieur.
- Présentation client.
- Variantes de matériaux.

---

## Denoise élevé (0.7 - 0.9)

- L'IA devient beaucoup plus créative.
- Les matériaux changent fortement.
- Certains éléments architecturaux peuvent être modifiés.
- Le rendu final peut être très différent de l'image originale.

### Cas d'utilisation

- Concept Art.
- Recherche de variantes.
- Exploration créative.
- Inspiration de design.

---

# 📌 Conclusion

Cette expérience montre que le **Denoising Strength** est l'un des paramètres les plus importants du workflow **Image-to-Image**.

- Une valeur faible conserve la structure originale.
- Une valeur moyenne offre le meilleur équilibre entre fidélité et réalisme.
- Une valeur élevée laisse davantage de liberté au modèle pour réinterpréter l'image.

Avec **Juggernaut Ragnarök XL**, les meilleurs résultats pour des projets d'architecture sont généralement obtenus avec un **Denoise compris entre 0.3 et 0.6**, permettant d'améliorer le réalisme tout en conservant la composition et les volumes du bâtiment.

---

# 📁 Structure du projet


Jour4-ImageToImage/
│
├── workflow/
│   └── Workflow i2i.json
│
├── input/
│   └── Croquis Architectural.png
│
├── output/
│   ├── denoise_01.png
│   ├── denoise_02.png
│   ├── denoise_03.png
│   ├── denoise_04.png
│   ├── denoise_05.png
│   ├── denoise_06.png
│   ├── denoise_07.png
│   ├── denoise_08.png
│   └── denoise_09.png
│
└── README.md


---

# ✅ Livrables

- ✔ Workflow **Image-to-Image** fonctionnel.
- ✔ Série de 9 images comparant un Denoise de **0.1 à 0.9**.
- ✔ Analyse de l'impact du Denoising Strength.
- ✔ Documentation complète de l'expérience dans ce **README**.

---

# 📚 Points clés à retenir

- Le **Denoise** contrôle le niveau de transformation de l'image d'origine.
- Plus le Denoise est faible, plus l'image reste fidèle au visuel initial.
- Plus le Denoise est élevé, plus l'IA est libre de créer une nouvelle interprétation.
- Le choix du Denoise dépend de l'objectif : amélioration, variation ou transformation complète.
- **Juggernaut Ragnarök XL** est particulièrement performant pour les projets d'architecture grâce à son excellent rendu des matériaux, de la lumière et des textures.

