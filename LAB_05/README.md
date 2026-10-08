# LAB 05: HOG-Based Industrial Defect Detection and Classification



An automated visual inspection prototype for steel surfaces. It extracts Histogram of Oriented Gradients (HOG) features from images and uses machine-learning classifiers to decide whether a product is defective and should be rejected.

## Pipeline

```
Image -> Grayscale + Resize (128x128) -> HOG features -> Classifier (SVM / Random Forest) -> Accept / Reject
```

## Dataset

**NEU Surface Defect Database (NEU-DET)**, Kaggle train/validation-split version:
https://www.kaggle.com/datasets/sovitrath/neu-steel-surface-defect-detect-trainvalid-split

- 6 defect classes: crazing, inclusion, patches, pitted surface, rolled-in scale, scratches
- Grayscale steel-surface images (200 x 200 pixels in the original database)
- The dataset has **no normal (non-defective) class**, so the main experiment is 6-class defect classification, and every recognised defect leads to REJECT.
- To test true binary accept/reject, add images named `normal_1.jpg`, `normal_2.jpg`, and so on. The code then switches to binary mode automatically.

## Files

| File | Description |
|---|---|
| `Lab05_HOG_Defect_Detection.ipynb` | Complete Google Colab notebook (all lab tasks) |
| `README.md` | This file |
| `lab05_output/` | Created when you run the notebook (figures, tables, saved model) |

## How to run (Google Colab)

1. Download the dataset zip from the Kaggle link above (log in to Kaggle in your browser).
2. Open https://colab.research.google.com and choose **File -> Upload notebook**, then select `Lab05_HOG_Defect_Detection.ipynb`.
3. Run the cells from top to bottom.
4. Step 0 downloads the dataset directly; if that fails it asks you to upload the zip.
5. The last cell downloads `lab05_output.zip` with all results.

> If Colab restarts the runtime, run Steps 0, 1 and 2 again before the later cells, because variables are lost.

## What the notebook does

| Step | Lab tasks |
|---|---|
| 0 | Load the dataset |
| 1 | Define helper functions (load, preprocess, HOG, models, distortions, decision module) |
| 2 | Inspect data, resize, convert to grayscale, stratified 70/15/15 split (tasks 1-3) |
| 3 | Visualise HOG for sample images (task 5) |
| 4 | Train SVM and Random Forest, confusion matrix, classification report, accuracy / precision / recall / F1 (tasks 6-10) |
| 5 | HOG parameter study: cell sizes 4/8/16 x orientations 6/9/12 (task 11) |
| 6 | Robustness test: brightness, Gaussian noise, rotation, blur (task 12) |
| 7 | Final inspection decision module (task 13) |
| 8 | Zip and download all outputs |

## Methodology notes

- **Split:** one stratified 70/15/15 split with a fixed seed (42).
- **Classifiers:** RBF-SVM (C = 10, standardised features) and Random Forest (200 trees), both with balanced class weights.
- **HOG:** 2 x 2 block normalisation (L2-Hys). Baseline setting is 8 x 8 cells and 9 orientations.
- **Model selection:** the best HOG setting is chosen on the validation set, not the test set, to avoid data leakage.
- **Metrics:** accuracy and macro-averaged precision, recall and F1. Feature length and timing are also recorded.
- **Robustness:** the model is trained on clean images and tested on distorted test images (brightness +/-40, noise sigma 15, rotation 15 degrees, blur 5 x 5).

## Output example

```
PRODUCT INSPECTION RESULT
Prediction: DEFECTIVE (scratches)
Confidence: XX%
Action: REJECT PRODUCT
```

In binary mode, a non-defective prediction gives `Action: ACCEPT PRODUCT`.

## Output files (in `lab05_output/`)

- `class_distribution.csv`
- `hog_visualisation.png`
- `cm_*.png` (confusion matrices)
- `classifier_comparison.csv`
- `hog_parameter_study.csv`
- `robustness.csv` and `robustness.png`
- `final_model.joblib` (trained model and HOG settings)

## Limitations

- All results come from a single run on one split, so small differences are not reliable.
- NEU-DET has no normal class, so true binary accept/reject could not be tested without extra images.
- Images were captured under controlled conditions; real production lines may differ.
- Classifier hyperparameters were only lightly tuned.
- HOG is not rotation invariant.
- The webcam bonus is not included because Colab cannot access a local camera.

## Requirements

Python 3, `numpy`, `opencv-python`, `scikit-image`, `scikit-learn`, `pandas`, `matplotlib`, `joblib`, `kagglehub`, `tabulate` (all installed or available by default in Colab).
