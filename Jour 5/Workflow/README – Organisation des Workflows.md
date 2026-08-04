# JOUR 5 - Organisation des Workflows & ComfyUI Manager

## 🎯 Objectif de la journée

Cette journée est consacrée à l'organisation des workflows ComfyUI afin de les rendre plus lisibles, réutilisables et faciles à maintenir.

L'objectif est également de découvrir le fonctionnement du **ComfyUI Manager**, d'installer des **Custom Nodes** et de comprendre comment résoudre les erreurs de **Red Nodes** (nœuds manquants).

---

# 📚 Compétences acquises

À la fin de cette journée, je suis capable de :

- Organiser un workflow complexe avec des groupes.
- Utiliser des couleurs pour identifier les différentes parties d'un workflow.
- Ajouter des notes afin de documenter un workflow.
- Comprendre le rôle des principaux nœuds.
- Utiliser les nœuds **SetNode** et **GetNode** pour centraliser les paramètres.
- Installer et gérer des Custom Nodes avec ComfyUI Manager.
- Identifier et résoudre les erreurs de nœuds manquants.

---

# 🖼️ Description du workflow

Le workflow est un **Image-to-Image** utilisant **ControlNet** afin de guider la génération tout en conservant la structure de l'image d'origine.

Il comprend :

- Chargement d'une image de référence.
- Chargement du modèle IA.
- Encodage des prompts.
- Application de ControlNet.
- Génération de l'image.
- Décodage du latent.
- Prévisualisation du résultat.

---

# 🧩 Structure du workflow


INPUT
│
├── Positive Prompt
└── Negative Prompt
├── Load Image
├── Largeur
└── Hauteur

MODEL
│
├── Checkpoint Loader
├── CLIP
└── VAE


CONTROL
│
├── ControlNet Loader
└── ControlNet Apply

PROCESSING
│
├── VAE Encode
├── KSampler
└── VAE Decode

OUTPUT
│
└── Preview Image
```

---

# 📖 Rôle des principaux nœuds

## Load Image

Charge l'image utilisée comme point de départ pour le workflow Image-to-Image.

---

## Checkpoint Loader

Charge le modèle de génération d'image.

Il fournit :

- le modèle IA
- le CLIP
- le VAE

---

## CLIP Text Encode

Transforme le prompt texte en informations compréhensibles par le modèle.

Deux nœuds sont utilisés :

- Prompt positif
- Prompt négatif

---

## VAE Encode

Convertit l'image en espace latent afin qu'elle puisse être traitée par le modèle.

---

## ControlNet Loader

Charge le modèle ControlNet utilisé pour conserver la structure de l'image.

---

## ControlNet Apply

Combine :

- l'image
- le ControlNet
- le prompt

afin de guider précisément la génération.

---

## KSampler

Le cœur du workflow.

Il génère la nouvelle image à partir :

- du modèle
- du latent
- du prompt
- du ControlNet

---

## VAE Decode

Transforme le latent généré en image visible.

---

## Preview Image

Affiche le résultat directement dans ComfyUI.

---

## SetNode

Permet de centraliser une valeur afin qu'elle puisse être réutilisée dans plusieurs endroits du workflow.

Exemple :

- largeur
- hauteur
- seed
- paramètres

---

## GetNode

Récupère une valeur définie par un SetNode.

Cette méthode évite de modifier plusieurs nœuds lorsqu'un paramètre change.

---

# 🎨 Organisation du workflow

Le workflow a été structuré en plusieurs groupes :

## 🟦 INPUT

Contient les images et paramètres d'entrée.


## 🟩 PROCESSING

Contient les prompts positif et négatif.


## 🟨 OUTPUT

Contient les nœuds d'affichage ou de sauvegarde.

---



# 🧠 Utilisation des SetNode / GetNode

L'utilisation des SetNode et GetNode améliore la lisibilité du workflow.

Au lieu de modifier plusieurs nœuds, une seule valeur est définie dans un SetNode puis récupérée partout avec un GetNode.

Avantages :

- maintenance plus simple
- workflow plus propre
- réduction des erreurs
- meilleure réutilisabilité

---

# 🔌 Custom Nodes étudiés

Au cours de cette journée, découverte de :

- ComfyUI Manager
- SetNode / GetNode
- ControlNet
- Custom Nodes

Objectif :

- installer de nouveaux plugins
- comprendre les dépendances d'un workflow
- résoudre les Red Nodes

---


# 📌 Bonnes pratiques retenues

- Toujours regrouper les nœuds par fonction.
- Utiliser des couleurs différentes pour chaque groupe.
- Donner un nom explicite aux groupes.
- Centraliser les paramètres avec SetNode/GetNode.
- Ajouter des notes pour documenter les parties importantes.
- Installer les Custom Nodes uniquement depuis des sources fiables.

