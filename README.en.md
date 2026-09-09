# Voice-Controlled Wheelchair

[Português](README.md) | [English](README.en.md)

Machine-learning prototype that recognises spoken movement commands and uses them to control a wheelchair in an interactive simulation. The project covers the complete audio-classification pipeline: signal exploration, feature engineering, model selection, evaluation and a real-time microphone demo.

> This is an academic software simulation. It is not certified or suitable for controlling a real mobility device.

## Supported commands

| Audio class | Simulation action |
|---|---|
| `forward` | Move forward |
| `backward` | Move backward |
| `left` | Turn left |
| `right` | Turn right |
| `stop` | Stop movement |
| `_silence_` | Ignore silence/background |
| `_unknown_` | Reject unsupported words |

## Pipeline

```mermaid
flowchart LR
    A[WAV recordings] --> B[Cleaning and normalisation]
    B --> C[Audio feature extraction]
    C --> D[Feature selection]
    D --> E[KNN and MLP evaluation]
    E --> F[Serialised model]
    F --> G[Microphone inference]
    G --> H[Pygame simulation]
```

The notebook implements:

1. dataset inspection, class distribution and WAV normalisation;
2. duration/amplitude analysis and Z-score, IQR, K-Means and DBSCAN outlier detection;
3. time-domain, frequency-domain, STFT, MFCC and wavelet features;
4. statistical tests and selection with PCA, Fisher Score and ReliefF;
5. train/test, train/validation/test and stratified K-fold evaluation;
6. hyperparameter comparison for KNN and scikit-learn MLP;
7. a neural network and backpropagation implemented from scratch for learning purposes;
8. real-time microphone inference in a Pygame maze.

## Technology stack

- Python and Jupyter Notebook;
- NumPy, pandas and SciPy;
- librosa and PyWavelets for audio/signal processing;
- scikit-learn for preprocessing, feature selection and classification;
- Matplotlib for analysis and evaluation;
- sounddevice for microphone input;
- Pygame for the interactive simulation.

## Repository structure

```text
Voice_controlled_Wheelchair/
├── project.ipynb      # analysis, training, evaluation and simulation
├── requirements.txt   # Python dependencies
├── README.md
└── README.en.md
```

The audio dataset and generated model files are intentionally not included.

## Local setup

```bash
git clone https://github.com/josepedrocunhazzz/voice_controlled_wheelchair.git
cd voice_controlled_wheelchair
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

On Windows, activate with `.venv\Scripts\activate`. `sounddevice` may also require PortAudio from the operating system.

Place WAV files in one directory per class:

```text
dataset/
├── forward/
├── backward/
├── left/
├── right/
├── stop/
├── _silence_/
└── _unknown_/
```

The notebook retains the author's original absolute `dataset_path` in several cells. Replace each assignment with the path to the local dataset, then run:

```bash
jupyter lab project.ipynb
```

Training creates `mlp_combined_model.pkl` and `feature_statistics.pkl` for the real-time demo. The final simulation requires a graphical session, microphone permission and a working input device.

## Evaluation notes

The experiments compare weighted F1, accuracy, precision, recall and confusion matrices. KNN and scikit-learn MLP achieved moderate performance. A higher raw score from the network implemented from scratch was affected by imbalance and predictions concentrated in only a few classes, so it should not be interpreted as the best model.

For assistive control, aggregate accuracy can hide dangerous errors in less frequent commands. Per-class recall, confusion matrices, latency and robust rejection of silence/unknown speech are more relevant measures for future work.

## Academic context

Work developed in an academic context of digital signal processing and machine learning.
