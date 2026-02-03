# 👁️ 6D Segmentation-Free Iris Recognition

This repository implements a **segmentation-free iris recognition system** based on **direct geometric regression** and a **Daugman-style recognition pipeline**.  
The system replaces pixel-level iris segmentation with a **6D regression of pupil and iris geometry**, while preserving classical normalization, encoding, and matching.

The implementation is fully notebook-based and designed for **reproducibility and clarity**, following the experiments described in the accompanying report.

---
## 📥 Dataset & Ground Truth

### 🔹 CASIA-IrisV3-Interval (Images)

The iris images are taken from the **CASIA-IrisV3-Interval** dataset.  
For convenience, the dataset can be downloaded from the Kaggle mirror:

👉 https://www.kaggle.com/datasets/swoyam2609/casia-iris-interval

After downloading, extract the dataset and place it in your preferred directory.  
Make sure the path is consistent with the `BASE_PATH` specified in the first cell of the training notebook.

---

### 🔹 Ground Truth Annotations

The official ground-truth annotations for CASIA-IrisV3-Interval are available at:

👉 https://github.com/HalmstadUniversityBiometrics/Iris-Segmentation-Groundtruth-Database/blob/main/CASIA-IrisV3-Interval_groundtruth.zip

However, **inconsistencies were found** between:
- image filenames,
- directory structure,
- and available annotation entries.

Because of these mismatches with the images, the official ground truth could not be used directly.

### 🔹 Custom Annotations

To ensure full consistency with the dataset used in this project, **custom annotations were generated** and used for training and evaluation.

- The annotations contain the pupil and iris circle parameters required by the 6D regression setup.
- The annotation file is **included in this repository** and should be used instead of the original ground truth.

No additional preprocessing is required: the notebooks automatically load the provided annotation file.

---


## 📂 Repository Content

- `6D_Models_Training.ipynb`  
  Training of the **Iris Localization Network (ILN)** and **Pupil Refinement Network (PRN)**.

- `6D_Verification_Detection.ipynb`  
  Full biometric evaluation:
  - Verification (1:1)
  - Closed-set identification (1:N)
  - Open-set identification

---

## 📓 1. Model Training

Open and run:

```
6D_Models_Training.ipynb
```

### 🔹 Dataset Loading (IMPORTANT)

The **first cell of the notebook** mounts Google Drive and loads the dataset from a predefined path.

Example (from the notebook):

```python
from google.colab import drive
drive.mount('/content/drive')

BASE_PATH = "/content/drive/MyDrive/iris_project/"
DATASET_PATH = BASE_PATH + "CASIA_IrisV3_Interval/"
```

👉 **You must replace this path** with your own preferred location.

For example:

**Google Drive**
```python
BASE_PATH = "/content/drive/MyDrive/my_dataset_folder/"
```

**Local machine**
```python
BASE_PATH = "/home/user/datasets/"
```

No other cells require path modification.

---

### 🔹 What the training notebook does

- Loads CASIA-IrisV3-Interval (NIR images)
- Trains:
  - **ILN**: global regression of pupil and iris circles (6D)
  - **PRN**: local refinement of pupil parameters
- Uses:
  - validation-based checkpointing
  - learning rate scheduling
  - geometric jitter for robustness

At the end, trained weights are saved and reused automatically during evaluation.

---

## 📓 2. Verification & Identification

Open and run:

```
6D_Verification_Detection.ipynb
```

This notebook:
- Builds iris templates
- Applies geometric validation and quality gating
- Extracts IrisCodes
- Performs:
  - verification (masked Hamming distance)
  - closed-set identification (rank-based)
  - open-set identification (DIR / FPIR)

No dataset paths need to be modified if the training notebook was run correctly.

---

## 📊 Output

The evaluation notebook produces:
- ROC and DET curves
- CMC curves
- Score distributions
- Tables with verification and identification metrics

All results match those reported in the final project report.

---

## ⚙️ Requirements

- Python ≥ 3.9
- PyTorch
- NumPy
- OpenCV
- scikit-learn
- matplotlib

GPU is **not required** (experiments run on CPU).

---

## 📌 Notes

- The system is **segmentation-free in the image domain**: no pixel-level semantic segmentation is used.
- Occlusions are handled through **statistical masking and angular gating** in the normalized domain.
- A reduced **6D geometry representation** is intentionally adopted to study the minimal configuration required for a stable Daugman-style pipeline.

---

## 📚 References

- J. Daugman, *How Iris Recognition Works*, IEEE TCSVT  
- T. Toizumi et al., *Segmentation-Free Direct Iris Localization Networks*


---

