# CIFAR-10 Image Classification using TensorFlow & Keras
Custom CNN • Data Augmentation • 77% Accuracy

# 📌 Project Overview

This project implements a custom Convolutional Neural Network (CNN) trained on the CIFAR-10 dataset, achieving strong performance on 10 object categories. The model is built from scratch using TensorFlow/Keras and includes data augmentation, batch normalization, dropout, and learning-rate scheduling for performance stability.

# 🚀 Key Features

Custom CNN architecture built in Keras

Data preprocessing & augmentation pipeline

Train/validation split with reproducible experiments

Regularization: dropout & batch normalization

Optimizer and learning-rate experimentation

Achieved 77% test accuracy

Saved model (.h5) + inference script

Clean file structure and modular code

# 🧠 What I Learned

Designing and tuning CNN architectures

Best practices for structuring DL projects

Using TensorBoard for real-time visualization

Improving robustness with augmentations

Preparing inference-ready models

Evaluating model performance with metrics & plots

📁 cifar10-cnn/
│── 📄 train.py
│── 📄 model.py
│── 📄 inference.py
│── 📄 utils.py
│── 📄 requirements.txt
│── 📄 README.md
│── 📁 saved_model/
│     └── cifar10_cnn.h5
│── 📁 logs/   (TensorBoard)
│── 📁 data/   (auto-downloaded by Keras)

| Metric            | Score                   |
| ----------------- | ----------------------- |
| **Test Accuracy** | **77%**                 |
| Loss Curve        | Included in TensorBoard |
| Accuracy Plot     | Included in repo        |


# 🌟 Future Improvements

Transfer learning with MobileNetV2 / ResNet

Grad-CAM visualizations

Hyperparameter tuning

FastAPI inference endpoint

Dockerized deployment
