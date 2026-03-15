# Computer Vision & Digit Recognition

This system uses computer vision techniques to extract the Sudoku grid from a photograph and classify its digits using a custom-trained Convolutional Neural Network (CNN). Below is an overview of the full process:

## Input Image → Grid Segmentation

Each input image is processed to detect the outer contour of the Sudoku grid, which is then warped into a perfect square. This grid is divided into 81 individual square cells.

<div align="center">
  <img src="readme_images/sudoku.jpg" alt="Sudoku Input Example" width="350"/>
</div>

## Cell Extraction and Labeling

The extracted cells are saved as individual grayscale images (50x50 pixels), ready to be labeled manually into categories: digits 1–9 or empty. These labeled images are later organized into folders for training.

<div align="center">
  <img src="readme_images/5_cell.png" alt="Digit 5 Example" width="125"/>
  <img src="readme_images/empty_cell.png" alt="Empty Cell Example" width="125"/>
</div>

## Dataset Structure

The labeled dataset is organized into three partitions for training, validation, and testing. Each partition contains one folder per digit class (0–9), where `0` represents an empty cell.

| Partition  | Path                  | Digits per Class | Total Images |
|------------|------------------------|------------------|--------------|
| Train      | `datasets/train/`      | ~450             | 4,064        |
| Validation | `datasets/val/`        | ~100             | 912          |
| Test       | `datasets/test/`       | ~100             | 900          |

> Dataset was created by manually labeling extracted cells using `extrac_cells.py` and human review.

## CNN Architecture

The CNN used to classify the digits is defined with the following architecture:

```bash
Input: (50, 50, 1) grayscale image
│
├── Conv2D(32, 3x3, relu)
├── MaxPooling2D(2x2)
├── Dropout(0.25)
│
├── Conv2D(64, 3x3, relu)
├── MaxPooling2D(2x2)
├── Dropout(0.25)
│
├── Flatten()
├── Dense(128, relu)
├── Dropout(0.5)
└── Dense(10, softmax)
```

- **Loss Function:** Categorical Crossentropy  
- **Optimizer:** Adam  
- **Epochs:** 100 (early stopping triggered at epoch 56)  
- **Callbacks:** EarlyStopping and ModelCheckpoint

The trained model is saved in:
```bash
cnn_classifier/model/digit_model.keras
```

## Evaluation Metrics

The final model achieved the following performance:

| Metric                      | Value   |
|-----------------------------|---------|
| Train Accuracy (final)      | 0.9967  |
| Validation Accuracy (final) | 1.0000  |
| Train Loss (final)          | 0.0089  |
| Validation Loss (final)     | 0.0000  |
| Test Accuracy               | 1.0000  |
| Test Loss                   | 0.0000  |

The classifier demonstrates near-perfect performance across all partitions, making it a reliable digit recognizer in the Sudoku solving pipeline.

## Training Visualizations

<div align="center">
  <img src="readme_images/training_plot.png" alt="Training Accuracy Curve" width="480"/>
</div>

## Confusion Matrix

<div align="center">
  <img src="readme_images/confusion_matrix.png" alt="Confusion Matrix" width="480"/>
</div>

The confusion matrix confirms that the classifier correctly predicted every single instance in the test set without error.

The classifier demonstrates excellent generalization across cleanly segmented Sudoku cells and is robust for use in the automated solving pipeline.

The model is stored in `cnn_classifier/model/digit_model.keras` and used at runtime to classify new cell images.
