<div align="center">

# 🖼️ CIFAR-10 Image Classification

A CNN built from scratch in TensorFlow / Keras — no pretrained weights.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/iamtanishqjain/CNN_CIFAR10/blob/main/CIFAR_10_CNN_Model.ipynb)
![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-FF6F00?logo=tensorflow&logoColor=white)
![Accuracy](https://img.shields.io/badge/Test_Accuracy-77.82%25-success)

</div>

---

## 📊 Result

| Metric | Value |
|:---|:---|
| 🎯 Test accuracy | **77.82%** |
| 📉 Test loss | 0.6575 |
| 🧮 Parameters | 1,250,858 |
| 🔁 Epochs | 10 |

> 💡 Training ended at 78.64% vs 77.82% validation — a sub-1% gap. Not overfitting, just **undertrained**. Accuracy was still climbing.

## 🧠 Architecture

```
Input (32×32×3)
  ├─ Conv 32 ×2  →  MaxPool  →  Dropout 0.25
  ├─ Conv 64 ×2  →  MaxPool  →  Dropout 0.25
  └─ Flatten  →  Dense 512  →  Dropout 0.5  →  Dense 10 (softmax)
```

Stacked 3×3 convolutions widen the receptive field cheaply; filters double as resolution halves; dropout rises with depth because the dense layer holds most of the parameters.

**Training:** Adam (default LR) · categorical cross-entropy · batch 64 · pixels scaled to `[0,1]`.

## 🚀 Run It

Click the Colab badge, or locally:

```bash
pip install tensorflow numpy matplotlib
jupyter notebook CIFAR_10_CNN_Model.ipynb
```

## 🛣️ Next

- [ ] Train longer — hadn't plateaued at epoch 10
- [ ] Data augmentation (flips, shifts, crops)
- [ ] Batch normalization
- [ ] Transfer learning with ResNet / MobileNetV2
