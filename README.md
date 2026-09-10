# Skin Lesion Classification and Model Benchmarking

A computer vision project that evaluates several CNN architectures for multi-class skin-cancer image classification. The project also investigates whether deep CNN features can work effectively with conventional machine learning classifiers.

##  Results

### CNN Model Comparison

| Model | Accuracy (%) | Precision (%) | Recall (%) | F1-Score (%) | AUC (%) |
|---|---:|---:|---:|---:|---:|
| AlexNet | 48.305085 | 53.041045 | 48.305085 | 44.223493 | 87.141402 |
| VGG16 | 51.694915 | 63.776938 | 51.694915 | 47.366096 | 87.621411 |
| VGG19 | 50.000000 | 59.720837 | 50.000000 | 45.322933 | 83.401679 |
| ResNet18 | 55.932203 | 56.268707 | 55.932203 | 51.894714 | 87.969714 |
| ResNet50 | 55.084746 | 56.410360 | 55.084746 | 52.556535 | 87.085121 |
| ResNet101 | 56.779661 | 60.270689 | 56.779661 | 54.407321 | 87.103761 |
| DenseNet121 | 57.627119 | 62.280431 | 57.627119 | 55.557868 | 88.693196 |
| **EfficientNet-B0** | **60.169492** | **63.872117** | **60.169492** | **56.988500** | **90.424162** |

**Top CNN:** EfficientNet-B0, achieving 60.17% accuracy and 90.42% AUC.

### Deep Feature Classifier Results

| Classifier | Accuracy (%) | Precision (%) | Recall (%) | F1-Score (%) | AUC (%) |
|---|---:|---:|---:|---:|---:|
| Logistic Regression | 55.084746 | 63.066631 | 55.084746 | 53.031102 | 89.453891 |
| Decision Tree | 50.000000 | 53.534192 | 50.000000 | 48.822035 | 71.133845 |
| Random Forest | 56.779661 | 61.513453 | 56.779661 | 52.797898 | 79.759490 |
| KNN | 55.084746 | 53.332642 | 55.084746 | 52.325414 | 84.991944 |
| Linear SVM | 55.932203 | 66.720205 | 55.932203 | 53.601097 | **91.176471** |
| RBF-SVM | 59.322034 | 61.921800 | 59.322034 | 56.181498 | 90.706648 |
| XGBoost | 56.779661 | 61.951404 | 56.779661 | 53.010762 | 89.281359 |

**Highest classifier accuracy:** RBF-SVM — 59.32%  
**Highest classifier AUC:** Linear SVM — 91.18%

### Efficiency Benchmark

| Model | Parameters (M) | Size (MB) | FLOPs (G) | Inference (ms) | Accuracy (%) |
|---|---:|---:|---:|---:|---:|
| AlexNet | 57.040713 | 217.593052 | 0.731048 | 0.432223 | 48.305085 |
| VGG16 | 134.297417 | 512.303989 | 15.466255 | 5.746416 | 51.694915 |
| VGG19 | 139.607113 | 532.558872 | 19.628054 | 6.470977 | 50.000000 |
| ResNet18 | 11.181129 | 42.683936 | 1.823256 | 1.070550 | 55.932203 |
| ResNet50 | 23.526473 | 89.949413 | 4.131713 | 3.665665 | 55.084746 |
| DenseNet121 | 6.963081 | 26.882061 | 2.895992 | 3.951768 | 57.627119 |
| **EfficientNet-B0** | **4.019077** | **15.492214** | **0.413877** | **1.515961** | **60.169492** |

EfficientNet-B0 provides the strongest combination of classification performance and relatively low computational cost in this experiment.

##  Project Summary

The workflow consists of:

1. Automatic dataset retrieval through KaggleHub.
2. Image preprocessing and augmentation.
3. Fine-tuning of eight pretrained CNNs.
4. Performance evaluation using Accuracy, Precision, Recall, F1-Score and AUC.
5. Selection of the best CNN for feature extraction.
6. Classification of extracted features with seven ML algorithms.
7. CNN resource and inference benchmarking.
8. Export of the comparison results to Excel and CSV.

##  Main Tools

- Python
- PyTorch
- Torchvision
- Scikit-learn
- XGBoost
- KaggleHub
- THOP
- Pandas
- OpenPyXL

## Files

```text
skin-lesion-classification-and-model-benchmarking/
├── skin_cancer_model_benchmark.py
├── README.md
├── requirements.txt
└── results/
    ├── model_comparison_results.xlsx
    ├── classifier_comparison.csv
    └── weights/
```

The dataset is downloaded at runtime and is not stored in the GitHub repository.

##  Running the Project

Install the dependencies:

```bash
pip install -r requirements.txt
```

Run the program:

```bash
python skin_cancer_model_benchmark.py
```

The script automatically locates the dataset's `Train` and `Test` directories and creates the result files.

##  Purpose

This project was developed for an academic computer-vision study of transfer learning, deep feature extraction, classifier comparison, and CNN computational efficiency.

