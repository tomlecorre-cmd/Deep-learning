<p align="center">
  <img src="https://img.shields.io/badge/PyTorch-%23EE4C2C.svg?style=for-the-badge&logo=PyTorch&logoColor=white" alt="PyTorch">
  <img src="https://img.shields.io/badge/Deep%20Learning-000000?style=for-the-badge" alt="Deep Learning">
  <img src="https://img.shields.io/badge/Computer%20Vision-0078D4?style=for-the-badge" alt="Computer Vision">
  <img src="https://img.shields.io/badge/CNN-FF9900?style=for-the-badge" alt="CNN">
  <img src="https://img.shields.io/badge/ResNet_18-4B32C3?style=for-the-badge" alt="ResNet-18">
</p>

# 🔬 PathMNIST : Classification de Tissus Histologiques par Deep Learning

Ce projet explore différentes architectures de Deep Learning (de la création de modèles *from scratch* au Transfer Learning) pour classifier des images microscopiques de tissus du côlon en 9 maladies distinctes. 

**🚀 Résultat clé :** Notre architecture CNN sur-mesure et légère (391k paramètres) a surpassé les performances d'un modèle ResNet-18 pré-entraîné massif (11M paramètres) avec une précision de **88.30%**.

## 📊 Le Dataset (PathMNIST)
Le dataset utilisé est **PathMNIST** (issu de la collection MedMNIST).
* **Données :** 100 000 images médicales (coupes histologiques du côlon).
* **Coloration :** Hématoxyline et Éosine (H&E).
* **Format :** Images RGB de résolution native 28x28 pixels.
* **Classes :** 9 catégories (tissus sains, stroma, épithélium tumoral, etc.).

## 🧠 Approche et Modèles Testés

Le projet est structuré en plusieurs expériences progressives :

* **Expérience 1 : Baseline MLP (Réseau Dense)**
  * Modèle de référence pour valider le pipeline.
  * Précision : ~62%.
  * *Conclusion :* La couche `Flatten` détruit la topologie 2D de l'image, prouvant la nécessité absolue des convolutions pour comprendre les textures cellulaires.

* **Expérience 2 : Custom CNN "From Scratch" (Le Gagnant 🏆)**
  * Architecture sur-mesure optimisée pour lire les images dans leur résolution native (28x28).
  * Stratégie de régularisation agressive pour éviter le surapprentissage : `BatchNorm2d`, `MaxPool2d`, et un `Dropout` spatial progressif (0.2 -> 0.3 -> 0.4 -> 0.5).
  * Précision : **88.30%** (avec augmentation de données géométriques : rotations et symétries).

* **Expérience 3 : Transfer Learning (ResNet-18)**
  * Comparaison entre l'extraction de caractéristiques (réseau gelé) et le *Full Fine-Tuning* (réseau totalement dégelé avec un faible taux d'apprentissage).
  * Nécessite un *upscaling* mathématique des images de 28x28 vers 224x224.
  * Précision max : **86.02%**.
  * *Conclusion :* Le modèle est trop lourd pour la complexité de ce dataset et l'upscaling crée un flou d'interpolation nocif. Le petit CNN est bien plus adapté.

* **Expérience 4 : Vision Transformer (ViT)**
  * Exploration des architectures basées sur l'Attention.
  * Découpage des images microscopiques en patchs de 7x7.

* **Expérience 5 : Explicabilité avec Grad-CAM**
  * Ouverture de la "boîte noire" du meilleur CNN.
  * Génération de *Heatmaps* (cartes de chaleur) confirmant que le modèle prend ses décisions en regardant au bon endroit (les amas cellulaires denses) et non en exploitant des biais de fond.

## 🛠️ Stack Technique
* **Langage :** Python 3
* **Framework :** PyTorch, Torchvision
* **Outils & Libs :** Scikit-learn, Matplotlib, NumPy, MedMNIST API

## 📈 Récapitulatif des Performances

| Modèle | Paramètres | Upscaling | Accuracy (Test) |
|--------|------------|-----------|-----------------|
| MLP (Baseline) | ~1.3M | Non | ~62.00% |
| ResNet-18 (Frozen) | 11M | Oui (224x224) | ~85.00% |
| ResNet-18 (Unfrozen)| 11M | Oui (224x224) | 86.02% |
| **Custom CNN (Augmented)** | **391k** | **Non (28x28)** | **88.30%** |

## 💡 Conclusion
Ce projet démontre qu'en imagerie médicale, l'utilisation systématique de modèles massifs pré-entraînés sur ImageNet n'est pas toujours la solution optimale. Une architecture convolutive sur-mesure, dont la capacité (nombre de paramètres) est parfaitement alignée sur la complexité de la donnée et qui respecte la résolution native des images, offre une bien meilleure efficacité paramétrique et une capacité de généralisation supérieure.

---
**Mots-clés / Keywords :** *Deep Learning, Convolutional Neural Networks (CNN), ResNet-18, Vision Transformer (ViT), PyTorch, Classification d'images médicales, PathMNIST, MedMNIST, Histologie, Transfer Learning, Intelligence Artificielle en Santé, Computer Vision.*
