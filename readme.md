# WADI Industrial Control Anomaly Detection

A deep learning-based anomaly detection system for the **WADI (Water Distribution) dataset**, designed to identify cyber-physical attacks in industrial control systems.

## 📌 Project Overview

This project implements a distributed MLP-based time-series prediction approach for detecting anomalies in the WADI water distribution system.

The pipeline focuses on attack-related sensors and actuators and uses historical sensor observations to predict future values. Prediction errors are then used to identify abnormal behavior associated with cyber attacks.

## 🔍 Methodology

The project follows these major stages:

1. **Dataset Loading**
   - WADI 14-day normal operation dataset is used for training.
   - WADI attack dataset is used for testing.

2. **Attack-Related Feature Selection**
   - Sensors and actuators associated with documented attack scenarios are selected.
   - The selected features include flow, pressure, level, pump, valve, controller, and quality-related measurements.

3. **Data Quality Analysis**
   - NaN and infinite values are checked.
   - Invalid values are cleaned when necessary.

4. **Min-Max Normalization**
   - Features are scaled to the range `[0, 1]`.

5. **Sequential Bilateral Filtering**
   - A sequential bilateral filter is applied to reduce noise while preserving important changes in the time-series signals.

6. **Downsampling**
   - The processed data is downsampled using every 5th sample to reduce computational requirements.

7. **Training and Validation Split**
   - The normal training data is divided into:
     - 80% Training
     - 20% Validation

8. **Distributed MLP Prediction Model**
   - Historical observations are used to predict future sensor values.
   - Historical window (`L1`) = 96 timesteps
   - Future prediction window (`L2`) = 12 timesteps
   - 3 MLP blocks are used.
   - Batch size = 512
   - Learning rate = 0.0005
   - MSE is used as the prediction loss.

9. **Anomaly Detection**
   - Prediction errors are calculated between predicted and actual future observations.
   - Feature-specific thresholds are generated.
   - Smoothed prediction errors are compared with the thresholds to identify anomalies.

10. **Ground Truth Evaluation**
    - Attack timestamps from the WADI attack information are mapped to the test data.
    - Predictions are compared against the ground-truth attack labels.

## 🧠 Model Architecture

The prediction model consists of:

- Input time-series windows
- Patch-based representation
- Multiple MLP blocks
- ReLU activation
- Dropout
- Layer Normalization
- Residual connections
- Output projection for future-value prediction

The model predicts the future behavior of the selected industrial sensors and uses prediction error as the basis for anomaly detection.

## 📊 Evaluation Metrics

The system evaluates anomaly detection performance using:

- Accuracy
- Precision
- Recall
- F1-Score
- Confusion Matrix
- ROC Curve
- Area Under the ROC Curve (AUC)

## 🛠️ Technologies Used

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- PyTorch
- CUDA / GPU acceleration

## 📂 Dataset

The project uses the **WADI (Water Distribution) dataset**, containing normal operating data and attack scenarios from an industrial water distribution system.

The notebook expects the dataset files in:

```text
wadi_dataset/
├── WADI_14days.csv
└── WADI_attackdata.csv
