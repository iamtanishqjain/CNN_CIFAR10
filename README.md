<div align="center">

# 🖼️ CIFAR-10 Image Classification

### A Convolutional Neural Network built from scratch in TensorFlow / Keras

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/iamtanishqjain/CNN_CIFAR10/blob/main/CIFAR_10_CNN_Model.ipynb)
![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)
![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-FF6F00?logo=tensorflow&logoColor=white)
![Keras](https://img.shields.io/badge/Keras-D00000?logo=keras&logoColor=white)
![Accuracy](https://img.shields.io/badge/Test_Accuracy-77.82%25-success)

**No pretrained weights. No transfer learning. Just a CNN, built layer by layer.**

</div>

---

## 🎯 The Task

Classify 32×32 colour images into **10 categories**:

<div align="center">

✈️ airplane • 🚗 automobile • 🐦 bird • 🐱 cat • 🦌 deer
🐶 dog • 🐸 frog • 🐴 horse • 🚢 ship • 🚚 truck

</div>

**60,000 images** — 50,000 for training, 10,000 for testing.

---

## 📊 Results

<div align="center">

| Metric | Value |
|:---|:---|
| 🎯 **Test Accuracy** | **77.82%** |
| 📉 Test Loss | 0.6575 |
| 🧮 Parameters | 1,250,858 *(4.77 MB)* |
| ⏱️ Training Time | ~6s / epoch *(Colab GPU)* |
| 🔁 Epochs | 10 |

</div>

**Validation accuracy per epoch**

| Epoch | 1 | 3 | 5 | 7 | 10 |
|:---|:---:|:---:|:---:|:---:|:---:|
| Accuracy | 64.70% | 72.22% | 74.73% | 76.41% | **77.82%** |

> 💡 **The interesting part:** training accuracy finished at 78.64% against 77.82% validation — a gap of less than one point. The model isn't overfitting, it's **undertrained**. Accuracy was still climbing when training stopped.

---

## 🧠 Architecture

A VGG-style stack — paired convolutions at increasing depth, each block followed by pooling and dropout.

```
                    Input  (32 × 32 × 3)
                           │
        ┌──────────────────▼──────────────────┐
        │  Conv2D 32 · 3×3 · ReLU  (padding)  │
        │  Conv2D 32 · 3×3 · ReLU             │   Block 1
        │  MaxPool 2×2  →  Dropout 0.25       │
        └──────────────────┬──────────────────┘
        ┌──────────────────▼──────────────────┐
        │  Conv2D 64 · 3×3 · ReLU  (padding)  │
        │  Conv2D 64 · 3×3 · ReLU             │   Block 2
        │  MaxPool 2×2  →  Dropout 0.25       │
        └──────────────────┬──────────────────┘
                           │  Flatten
        ┌──────────────────▼──────────────────┐
        │  Dense 512 · ReLU  →  Dropout 0.5   │   Classifier
        │  Dense 10  · Softmax                │
        └─────────────────────────────────────┘
```

**Why it's shaped this way**

| Choice | Reasoning |
|:---|:---|
| 🔲 **Stacked 3×3 convs** | Two small kernels widen the receptive field with fewer parameters than one large kernel |
| 📈 **Filters 32 → 64** | Doubling depth as resolution halves keeps compute per block roughly constant |
| 💧 **Dropout 0.25 → 0.5** | The dense layer holds most of the parameters, so it gets the strongest regularization |

---

## ⚙️ Training Setup

| | |
|:---|:---|
| **Optimizer** | Adam *(default learning rate)* |
| **Loss** | Categorical cross-entropy |
| **Batch size** | 64 |
| **Preprocessing** | Pixels scaled to `[0, 1]`, labels one-hot encoded |
| **Validation** | Test set used directly as validation data |

---

## 🚀 Run It

**Fastest way** — click the Colab badge. Zero setup, CIFAR-10 downloads itself through Keras.

**Locally:**

```bash
pip install tensorflow numpy matplotlib
jupyter notebook CIFAR_10_CNN_Model.ipynb
```

Run all cells. About a minute on GPU, a few minutes on CPU. The last cell picks a random test image and shows the prediction against the true label.

---

## 📁 Structure

```
CNN_CIFAR10/
├── 📓 CIFAR_10_CNN_Model.ipynb   →  data · model · training · evaluation
└── 📄 README.md
```

Everything sits in one notebook, in build order: load → normalize → define → compile → train → evaluate.

---

## 🛣️ Roadmap

Ordered by expected payoff:

- [ ] **Train longer** — accuracy hadn't plateaued at epoch 10; the cheapest gain available
- [ ] **Data augmentation** — random flips, shifts, crops; usually worth several points on CIFAR-10
- [ ] **Batch normalization** after each conv, for stabler training at a higher learning rate
- [ ] **Learning-rate scheduling** — decay on plateau instead of a fixed Adam default
- [ ] **Transfer learning** with ResNet / MobileNetV2 for a realistic accuracy ceiling
- [ ] **Export the model** + a small inference script, so predictions don't need the notebook

---

## 💭 Takeaways

Building the network by hand instead of importing one made the trade-offs concrete — why convolutions stack before pooling, why filter counts double as resolution drops, why the dense layer needs the heaviest dropout.

The more useful skill turned out to be **reading the curves**: recognising that a *small* train/validation gap means undertrained, not finished.

<div align="center">

---

⭐ *Built while learning deep learning fundamentals*

</div>
