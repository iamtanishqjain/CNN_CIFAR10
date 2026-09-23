# CIFAR-10 Image Classification with a CNN

A convolutional neural network built from scratch in TensorFlow/Keras, trained to classify the ten CIFAR-10 object categories. **77.82% test accuracy** after 10 epochs.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/iamtanishqjain/CNN_CIFAR10/blob/main/CIFAR_10_CNN_Model.ipynb)
![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![TensorFlow](https://img.shields.io/badge/TensorFlow-Keras-orange)

---

## Overview

CIFAR-10 is 60,000 32×32 colour images across ten classes — airplane, automobile, bird, cat, deer, dog, frog, horse, ship, truck — split into 50,000 training and 10,000 test images.

This project builds the classifier end to end in a single notebook: no pretrained weights, no transfer learning. The point was to understand how each architectural choice affects what the network learns, so the model is deliberately simple enough to reason about.

## Results

| Metric | Value |
| --- | --- |
| Test accuracy | **77.82%** |
| Test loss | 0.6575 |
| Training accuracy (final epoch) | 78.64% |
| Parameters | 1,250,858 (4.77 MB) |
| Epochs | 10 |
| Training time | ~6s/epoch on Colab GPU |

Accuracy climbed steadily across all ten epochs and had not plateaued:

| Epoch | 1 | 3 | 5 | 7 | 10 |
| --- | --- | --- | --- | --- | --- |
| Validation accuracy | 64.70% | 72.22% | 74.73% | 76.41% | **77.82%** |

Training and validation accuracy finished within one point of each other (78.64% vs 77.82%), so the dropout is doing its job — the model is not overfitting, it is simply undertrained. More epochs alone should push this higher.

## Architecture

A VGG-style stack: pairs of convolutions at increasing depth, each pair followed by pooling and dropout.

```
Input (32, 32, 3)

Conv2D(32, 3×3, same, ReLU)  →  Conv2D(32, 3×3, ReLU)
MaxPooling2D(2×2)  →  Dropout(0.25)

Conv2D(64, 3×3, same, ReLU)  →  Conv2D(64, 3×3, ReLU)
MaxPooling2D(2×2)  →  Dropout(0.25)

Flatten  →  Dense(512, ReLU)  →  Dropout(0.5)
Dense(10, softmax)
```

Design notes:

- **Stacked 3×3 convolutions** before each pooling layer widen the receptive field while keeping the parameter count lower than a single larger kernel would.
- **Doubling filters** from 32 to 64 as spatial resolution halves keeps the compute per block roughly constant.
- **Dropout increases with depth** (0.25 → 0.5). The dense layer holds most of the parameters, so it gets the heaviest regularization.

**Training setup:** Adam (default learning rate), categorical cross-entropy, batch size 64, pixels scaled to `[0, 1]`, labels one-hot encoded. The test set is used directly as validation data.

## Running it

The fastest route is the Colab badge above — it needs no setup and CIFAR-10 downloads automatically through Keras.

To run locally:

```bash
pip install tensorflow numpy matplotlib
jupyter notebook CIFAR_10_CNN_Model.ipynb
```

Then run all cells. Training takes about a minute on a GPU, a few minutes on CPU.

## Repository

```
CNN_CIFAR10/
├── CIFAR_10_CNN_Model.ipynb   # the complete project: data, model, training, evaluation
└── README.md
```

Everything lives in the one notebook, in the order you would build it: load the data, normalize, define the model, compile, train, then evaluate and plot. The final cell picks a random test image and shows the prediction against the true label.

## Where this goes next

The gap between training and validation accuracy is small, which points at capacity and training length rather than overfitting. In rough order of expected payoff:

- **Train longer.** Accuracy was still improving at epoch 10; 40–50 epochs is the cheapest available gain.
- **Data augmentation** — random flips, shifts and crops. The standard next step on CIFAR-10 and usually worth several points.
- **Batch normalization** after each convolution, to stabilise training and allow a higher learning rate.
- **Learning-rate scheduling**, decaying on plateau rather than holding Adam's default throughout.
- **Transfer learning** with ResNet or MobileNetV2 for a realistic accuracy ceiling.
- **Export the trained model** and add a small inference script, so predictions do not require rerunning the notebook.

## What I took from it

Building the network by hand rather than importing one made the trade-offs concrete: why convolutions are stacked before pooling, why filter counts double as resolution drops, and why the dense layer needs the most aggressive dropout. Reading the training curves — and recognising that a *small* train/validation gap means undertraining rather than success — turned out to be the more useful skill.
