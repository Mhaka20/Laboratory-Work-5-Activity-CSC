# Laboratory-Work-5-Activity-CSC

# COLAB Link(lw5): https://colab.research.google.com/drive/1Ldt10AXs3mO8MGTnXtFAHhWOlyyoghIB
# Colab link(good model): https://colab.research.google.com/drive/1Zo-ZgVzDDGsaa2en8FQlwX7NuzdG79Af#scrollTo=yEFO1MgNqNN7


## 📊 Model Comparison Results

| Model | Train Accuracy | Train Loss | Test Accuracy | Test Loss | Precision | Recall | F1-Score | ROC | AUC |
|-------|---------------|------------|---------------|-----------|-----------|--------|----------|-----|-----|
| Pre-trained ResNet50 | 9.77% | 2.9514 | 11.2% | 2.9278 | 0.0811 | 0.1190 | 0.0571 | 0.6669 | 0.6669 |
| Pre-trained MobileNetV2 | 60.92% | 1.3152 | 63.8% | 1.3071 | 0.6403 | 0.6412 | 0.6369 | 0.9241 | 0.9241 |
| Pre-trained EfficientNetB0 | 5.22% | 3.0247 | 5.3% | 3.0154 | 0.0027 | 0.0500 | 0.0050 | 0.5411 | 0.5411 |
| Teachable Machine | 99.68% | 0.0220 | 91.79% | 1.665 | 0.9230 | 0.9180 | 0.9166 | 0.9952 | 0.9952 |
| CSC_130_Image_Classifier_LW3 | 48.23% | 1.6908 | 44.10% | 1.8605 | 0.4400 | 0.4300 | 0.4400 | 0.7500 | 0.7500 |
| CSC_130_Image_Classifier_Improved | 98.65% | 0.0549 | 79.60% | 0.8903 | 0.7900 | 0.7800 | 0.7800 | 0.9600 | 0.9600 |
| **best_model_enb0_fixed ** | **91.60%** | **0.6050** | **90.20%** | **0.6522** | **0.9397** | **0.9020** | **0.9189** | **0.9859** | **0.9859** |

### 🏆 Best Model: `best_model_enb0_fixed.keras`

**Architecture:** EfficientNetB0 with custom regularized head  
**Key Features:**
- Strong data augmentation (flip, rotation, zoom, translation, brightness, contrast)
- L2 regularization (0.001) + BatchNormalization + Dropout (0.5/0.3)
- Two-phase training: frozen base (15 epochs) → fine-tune top 20 layers (early stopped)
- BatchNorm layers kept frozen during fine-tuning for stability
- Class weights for balanced learning across 20 aquatic plant species

**Performance Highlights:**
- ✅ **90.2% Test Accuracy** — highest among all custom-trained models
- ✅ **91.6% Train Accuracy** with only **1.4% gap** — excellent generalization
- ✅ **0.9397 Precision** — very few false positives
- ✅ **0.9859 AUC** — near-perfect class separation
- ✅ No overfitting (unlike Improved model: 98.7% train vs 79.6% test)

### 📈 Training Strategy

| Phase | Layers | Epochs | Learning Rate | Result |
|-------|--------|--------|---------------|--------|
| Phase 1 (Frozen) | Base frozen, head trained | 15 | 1e-3 (Adam default) | 78.6% val acc |
| Phase 2 (Fine-tune) | Top 20 layers unfrozen | ~12-15 | 1e-4 → 5e-5 (ReduceLROnPlateau) | **90.2% val acc** |

### 🌿 Dataset
- **20 classes** of aquatic plants (Amazon_sword, Anubias, Arrowhead_plant, etc.)
- **~250 images per class** (5,001 total, 4,001 train / 1,000 validation)
- Images resized to **224×224** for EfficientNetB0 compatibility
