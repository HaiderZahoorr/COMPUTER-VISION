# Computer Vision Lab Mid: HOG vs SIFT on Blood-Cell Images

**Name:** Haider
**Reg No:** FA23-BAI-015
**Course:** Computer Vision Lab, BS Artificial Intelligence, COMSATS University Islamabad, Wah Campus

## Aim
Build two classical blood-cell classifiers, **HOG + LinearSVC** and **SIFT + LinearSVC**, and find out which one is more reliable when test images are **rotated (20 degrees)** or **noisy (Gaussian, sigma = 0.05)**.

## Personal settings (from registration number FA23-BAI-015)

| Setting | Rule | My value |
|---|---|---|
| Random seed | last two digits | **15** |
| HOG cell size | last digit 5 is odd | **16 x 16** |
| SIFT grid size | last digit 5 is in 5-9 | **6 x 6 (36 keypoints)** |

## Fixed settings (same for everyone)

| Item | Value |
|---|---|
| Dataset | BloodMNIST, size 64 (Kaggle: `flyt007/bloodmnist`) |
| Classes | eosinophil (1), monocyte (5), neutrophil (6), platelet (7) |
| Images per class | 100 train, 30 validation, 30 test (picked with seed 15) |
| Image preparation | grayscale, resized to 128 x 128 |
| HOG | 9 orientations, 2 x 2 cells per block |
| Classifier | LinearSVC, C chosen from {0.1, 1, 10} on the validation set |
| Rotation test | 20 degrees |
| Noise test | Gaussian noise, sigma = 0.05 (image scaled to 0-1) |

## Files

| File | Description |
|---|---|
| `FA23-BAI-015_Haider_LabMid.ipynb` | Main notebook (code, text answers, outputs) |
| `FA23-BAI-015_Haider_LabMid.md` | Markdown copy of the executed notebook |
| `FA23-BAI-015_Haider_LabMid_files/` | Images (plots) used by the MD file |
| PDF copy | PDF version of the executed notebook |

## How to run
1. Open the notebook in **Google Colab** (or Kaggle).
2. Run all cells (Runtime -> Run all). The first cells install the packages and download the dataset with `kagglehub`:
   ```python
   import kagglehub
   path = kagglehub.dataset_download("flyt007/bloodmnist")
   ```
3. If the Kaggle copy is missing or not 64 x 64, the loader falls back to the `medmnist` package (`size=64`).
4. Packages used: `kagglehub`, `medmnist`, `opencv-python`, `scikit-image`, `scikit-learn`, `pandas`, `matplotlib`, `numpy`.

To create the MD file after running:
```
jupyter nbconvert --to markdown FA23-BAI-015_Haider_LabMid.ipynb
```

## What the code does

**Q1: Data setup**
- Loads BloodMNIST, keeps the four classes, picks 100/30/30 images per class using `RandomState(15)`.
- Converts images to grayscale 128 x 128.
- Prints shapes and class counts and shows a 4 x 4 grid of example images.

**Q2: HOG + SVM**
- `skimage.feature.hog` with 9 orientations, 16 x 16 cells, 2 x 2 cells per block.
- Feature length by hand: 128/16 = 8 cells per side, 8 - 2 + 1 = 7 blocks per side, 7 x 7 = 49 blocks, 49 x (2 x 2 x 9) = **1764**. The code checks this against the real output.
- Shows HOG pictures, trains LinearSVC, picks C on the validation set, and reports accuracy, macro-F1 and the confusion matrix.

**Q3: SIFT + SVM**
- Normal SIFT (`detectAndCompute`): prints average, minimum and maximum keypoints per image and the number of images with zero keypoints, and draws keypoints on one image.
- Problem: a different number of keypoints per image gives feature vectors of different sizes, which an SVM cannot use.
- Fix: a fixed 6 x 6 grid of keypoints (size 32), `sift.compute` at each grid point, and the descriptors joined into one vector. Feature length = 36 x 128 = **4608**.
- SIFT features are divided by 512 inside a pipeline because the raw values made LinearSVC converge too slowly. C is chosen on the validation set.

**Q4: Robustness**
- Prediction is written before the test.
- Both models are frozen and evaluated on the test set in three cases: clean, rotated by 20 degrees, noisy (sigma = 0.05). The change is applied to the images before feature extraction.
- The result table and the prediction check are produced by the code.

**Q5: Short answers**
- Block normalisation, why C must not be chosen on the test set, the most confused class pair (computed from the confusion matrices), and the AI log.

## Results (fill in from the executed notebook)

| Model | Feature length | Chosen C | Val accuracy | Val macro-F1 |
|---|---|---|---|---|
| HOG + SVM | 1764 | | | |
| SIFT (grid) + SVM | 4608 | | | |

| Model | Clean accuracy | Rotated accuracy | Noisy accuracy |
|---|---|---|---|
| HOG + SVM | | | |
| SIFT + SVM | | | |

Most confused classes: ______________________

## Rules followed
- C is chosen using the validation set only. The test set is used only in Q4.
- Every number in the answers comes from an output in the notebook.
- AI tools were used and every result was checked (printed shapes, feature lengths, images).
