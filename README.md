# 👁️ 6D Segmentation-Free Iris Recognition

This repository implements a **segmentation-free iris recognition system** based on **direct geometric regression** and a **Daugman-style recognition pipeline**.  
The system replaces pixel-level iris segmentation with a **6D regression of pupil and iris geometry**, while preserving classical normalization, encoding, and matching.

The implementation is fully notebook-based and designed for **reproducibility and clarity**, following the experiments described in the accompanying report.

---

## 📂 Repository Content

- `6D_Models_Training.ipynb`  
  Training of the **Iris Localization Network (ILN)** and **Pupil Refinement Network (PRN)**.

- `6D_Verification_Detection.ipynb` 
  Daugman Style Pipeline
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

GPU is **not required** (experiments run on CPU/hosted free GPU).

---

## 📌 Notes

- The system is **segmentation-free in the image domain**: no pixel-level semantic segmentation is used.
- Occlusions are handled through **statistical masking and angular gating** in the normalized domain.
- A reduced **6D geometry representation** is intentionally adopted to study the minimal configuration required for a stable Daugman-style pipeline.

---

## 📚 References

- J. Daugman, *How Iris Recognition Works*, IEEE TCSVT  
- T. Toizumi et al., *Segmentation-Free Direct Iris Localization Networks*
