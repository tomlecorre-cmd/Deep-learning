# 🔬 PathMNIST : Classification de Tissus Histologiques par Deep Learning

Ce projet explore différentes architectures de Deep Learning (de la création de modèles *from scratch* au Transfer Learning) pour classifier des images microscopiques de tissus du côlon en 9 maladies distinctes. 

**🚀 Résultat clé :** Notre architecture CNN sur-mesure (391k paramètres) a surpassé les performances d'un modèle ResNet-18 pré-entraîné (11M paramètres) avec une précision de **88.30%**.

## 📊 Le Dataset (PathMNIST)
Le dataset utilisé est **PathMNIST** (issu de la collection MedMNIST).
* **Données :** 100 000 images médicales (coupes histologiques du côlon).
* **Coloration :** Hématoxyline et Éosine (H&E).
* **Format :** Images RGB de résolution native 28x28 pixels.
* **Classes :** 9 catégories (tissus sains, stroma, épithélium tumoral, etc.).

## 🧠 Approche et Modèles Testés

Le projet est structuré en plusieurs expériences progressives (Notebooks) :

* **Expérience 1 : Baseline MLP (Réseau Dense)**
  * Modèle de référence pour valider le pipeline.
  * Précision : ~62%.
  * *Conclusion :* La couche Flatten détruit la topologie 2D, prouvant la nécessité des convolutions.

* **Expérience 2 : Custom CNN "From Scratch" (Le Gagnant 🏆)**
  * Architecture sur-mesure optimisée pour les images 28x28.
  * Stratégie de régularisation agressive : BatchNorm2d, Max Pooling, et Dropout spatial progressif (0.2 -> 0.3 -> 0.4 -> 0.5).
  * Précision : **88.30%** (avec augmentation de données géométriques).

* **Expérience 3 : Transfer Learning (ResNet-18)**
  * Comparaison entre l'extraction de caractéristiques (Frozen) et le Full Fine-Tuning (Unfrozen).
  * Nécessite un upscaling des images à 224x224.
  * Précision max : 86.02%.
  * *Conclusion :* Le modèle est trop lourd pour le dataset et l'upscaling crée de la distorsion. Le petit CNN est plus adapté.

* **Expérience 4 : Vision Transformer (ViT)**
  * Exploration des architectures basées sur l'Attention.
  * Découpage en patchs de 7x7.

* **Expérience 5 : Explicabilité avec Grad-CAM**
  * Ouverture de la "boîte noire" du meilleur CNN.
  * Génération de Heatmaps (cartes de chaleur) pour s'assurer que le modèle se concentre sur les amas cellulaires et non sur le bruit de fond, validant ainsi la pertinence médicale des prédictions.

## 🛠️ Stack Technique
* **Langage :** Python
* **Framework :** PyTorch, Torchvision
* **Outils :** Scikit-learn, Matplotlib, NumPy, MedMNIST API

## 📈 Récapitulatif des Performances

| Modèle | Paramètres | Upscaling | Accuracy (Test) |
|--------|------------|-----------|-----------------|
| MLP (Baseline) | ~1.3M | Non | ~62.00% |
| ResNet-18 (Frozen) | 11M | Oui (224x224) | ~85.00% |
| ResNet-18 (Unfrozen)| 11M | Oui (224x224) | 86.02% |
| **Custom CNN (Augmented)** | **391k** | **Non (28x28)** | **88.30%** |

## 💡 Conclusion
Ce projet démontre qu'en imagerie médicale, l'utilisation systématique de modèles massifs pré-entraînés sur ImageNet n'est pas toujours la solution optimale. Une architecture convolutive sur-mesure, dont la capacité (nombre de paramètres) est alignée sur la complexité de la donnée et qui respecte la résolution native des images, offre une meilleure efficacité paramétrique et une capacité de généralisation supérieure.
