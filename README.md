# Transfer Learning for Smile Classification (CelebA)

This project explores **transfer learning for binary facial attribute classification**, focusing on detecting whether a person is smiling or not from an image.  
A pre-trained **VGG-16** model is adapted to the task using two training strategies and evaluated on a subset of the CelebA dataset.

The goal is to analyze how **feature freezing vs. fine-tuning** impacts performance and generalization.

---

## Overview
Training deep convolutional networks from scratch requires large datasets and significant compute.  
Instead, this project leverages **VGG-16 pre-trained on ImageNet** and adapts it to facial expression recognition using transfer learning.

Two strategies are compared:
- **Frozen Feature Extractor:** Only the classifier head is trained
- **Fine-Tuning:** The final convolutional block and classifier are trained jointly

---

## Dataset
- **Dataset:** CelebA (30,000-image subset)
- **Task:** Binary smile classification (smiling vs not smiling)
- **Split:**
  - Train: 80%
  - Validation: 10%
  - Test: 10%
- **Preprocessing:**
  - Resize to 224×224
  - Normalization with mean/std = [0.5, 0.5, 0.5]

### Data Augmentation (training only)
- Random horizontal flip
- Color jitter (brightness & contrast)

---

## Model Architecture
- **Backbone:** VGG-16 (ImageNet pre-trained)
- **Classifier Head:** Fully connected layer → single logit
- **Loss:** Binary Cross-Entropy with Logits
- **Optimizer:** Adam (lr = 1e-4)

---

## Training Strategies
### Strategy A — Frozen Features
- All convolutional layers frozen
- Only classifier trained
- Faster convergence but limited task specificity

### Strategy B — Fine-Tuning
- First four VGG blocks frozen
- Final convolutional block + classifier trained
- Allows adaptation to facial expression features

---

## Results
| Strategy | Best Val Accuracy | Best Val Loss | Test Accuracy |
|--------|------------------|---------------|---------------|
| Frozen | 86.23% | 32.60% | — |
| Fine-Tuned | **92.57%** | **18.77%** | **91.77%** |

Fine-tuning the final convolutional block significantly improved generalization and stability.

---

## Analysis & Insights
- Frozen models rely on generic ImageNet features, which are insufficient for subtle facial cues
- Fine-tuning enables adaptation to high-level features such as mouth curvature and cheek structure
- Data augmentation improved robustness to lighting and orientation variations
- Most errors occurred in ambiguous expressions, occluded faces, or low-quality images

---

## Key Takeaways
- Fine-tuning deeper layers yields substantial gains for domain-specific tasks
- Transfer learning reduces training cost while maintaining strong performance
- Careful augmentation and error analysis are essential for reliable facial attribute classification

---

## Possible Extensions
- Multi-attribute classification (e.g., smile + gender)
- Lightweight architectures for faster inference
- Class imbalance mitigation
- Visualization of learned feature maps
