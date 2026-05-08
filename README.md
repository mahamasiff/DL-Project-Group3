# Decoding Cerebellar Purkinje Cell Dynamics with Deep Learning

## Overview

This project explores deep learning approaches for decoding cerebellar Purkinje cell activity into meaningful representations of motor behavior.

Purkinje cells are the sole output neurons of the cerebellar cortex, making them essential for understanding motor control, coordination, and motor learning. This work bridges the gap between neural spike activity and fine-grained behavioral decoding using modern deep learning methods.

The repository includes:

- A State-of-the-Art (SOA) literature survey
- A data preprocessing and alignment pipeline
- Exploratory Data Analysis (EDA)
- Deep learning experiments for behavior decoding
- Feature engineering for motor control signals such as movement deceleration

---

# Repository Structure

```bash
Code/
├── EDA/
│   ├── 1D_1_licks.csv
│   ├── aligned_spikes_1D2.csv
│   └── PC_CELL_EDA.ipynb
│
├── Models/
│   ├── 1D_1.mat
│   ├── 1D_2.mat
│   ├── Pipeline.ipynb
│   └── poyo-weights-pukinje-cells.ipynb
│
├── Literature/
│
├── README.md
└── .gitattributes
```

---

# Research Motivation

Recent neuroscience studies show that Purkinje cells encode:

- Movement timing
- Motor deceleration
- Behavioral coordination
- Synchronous population dynamics

Most existing machine learning approaches remain limited to binary classification tasks such as lick vs no-lick and often rely on handcrafted statistical features.

This project investigates whether deep spatio-temporal neural architectures can better model behaviorally relevant motor dynamics from Purkinje cell activity, including movement-related behavioral states associated with tongue movement tasks.

The work is inspired by prior research including:

- Markanday et al. (2020) on automated complex spike detection
- Sedaghat-Nejad et al. (2022) on synchronous Purkinje population activity
- Hage et al. (2025) on Purkinje control of movement deceleration
- Zhang et al. (2025) on attention-based neural decoding
- Zeeshan et al. (2026) on deep spatio-temporal Purkinje decoding architectures

---

# Problem Statement

The goal of this project is to decode high-dimensional Purkinje cell spike activity into meaningful motor behavior representations such as movement deceleration and behavioral intent.

## Core Challenges

### Data Scarcity

High-quality electrophysiology recordings are difficult and expensive to obtain.

### Signal Sparsity

Complex spikes (CS) are rare events embedded within noisy recordings.

### High Dimensionality

Neural recordings contain multiple electrodes, temporal dependencies, cross-neuron correlations, and session variability.

### Spatio-Temporal Complexity

Models must learn:

- Where spikes originate
- When spikes occur
- How neuron populations synchronize over time

---

# Dataset

The dataset consists of cerebellar Purkinje cell recordings collected during licking and tongue movement tasks.

## Neural Signals

- `SS_time` → Simple spikes
- `CS_time` → Complex spikes

These labels are provided directly in the dataset and are not inferred.

## Behavioral Signals

Behavioral measurements include:

- Lick onset timing
- Tongue movement trajectories
- Velocity profiles
- Deceleration-related motor features

---

# Project Pipeline

Primary implementation notebook:

```bash
Code/Models/Pipeline.ipynb
```

## 1. Data Ingestion

The pipeline loads raw MATLAB electrophysiology recordings:

```python
.mat → h5py/scipy → Python structures
```

Processing includes:

- MATLAB cell reference resolution
- Neural signal extraction
- Behavioral signal extraction
- Metadata parsing

## 2. Data Transformation

Nested MATLAB structures are flattened into structured representations.

Operations include:

- Spike time extraction
- Timestamp normalization
- Missing value handling
- Session consistency checks

## 3. Data Alignment

Neural activity is aligned to behavioral events.

### Alignment Strategy

- Align spikes to lick onset
- Extract neural activity within a 100 ms pre-movement window
- Normalize timestamps relative to movement onset

This produces temporally aligned spike-event representations suitable for sequential modeling.

## 4. Feature Engineering

### Deceleration Strength

Motor deceleration is approximated using:

\[
\text{Deceleration Strength} =
\frac{v_{max}}{t_{vmin} - t_{vmax}}
\]

Where:

- \(v_{max}\) = maximum velocity
- \(t_{vmax}\) = time of peak velocity
- \(t_{vmin}\) = time of minimum velocity

This feature acts as a proxy for:

- Motor braking signals
- Cerebellar stopping control
- Precision movement regulation

---

# Output Dataset Format

Final processed datasets are exported as CSV files.

| Column | Description |
|---|---|
| `lick_id` | Unique behavioral event ID |
| `label` | Behavior class |
| `electrode` | Recording channel |
| `spike_time` | Spike time relative to event |
| `spike_type` | SS or CS |
| `velocity_max` | Peak movement velocity |
| `deceleration_min` | Minimum velocity |
| `decel_strength` | Engineered motor feature |

---

# Exploratory Data Analysis (EDA)

EDA notebooks include:

```bash
Code/EDA/PC_CELL_EDA.ipynb
```

The analysis includes:

- Spike raster visualizations
- SS vs CS distribution analysis
- Session-wise activity inspection
- Behavioral alignment validation
- Spike timing histograms
- Neural activity trend analysis

---

# Modeling Approaches

This project explores several neural decoding approaches.

## Classical Baselines

- XGBoost
- Support Vector Machines (SVM)

## Deep Learning Models

- CNN-based spike decoders
- LSTM temporal models
- Attention-based architectures
- Spatio-temporal neural networks

## Long-Term Research Direction

The long-term goal is to investigate attention-based spatio-temporal architectures capable of learning coordinated Purkinje cell population dynamics for decoding continuous motor behavior.

---

# Data Integrity Checks

The preprocessing pipeline includes:

- Duplicate spike removal
- Timestamp consistency validation
- Session integrity verification
- Time glitch detection
- Missing data handling

---

# Key Literature

The repository is motivated by findings from the following works:

- Markanday et al. (2020)  
  Using deep neural networks to detect complex spikes of cerebellar Purkinje cells.

- Sedaghat-Nejad et al. (2022)  
  Synchronous spiking of cerebellar Purkinje cells during control of movements.

- Hage et al. (2025)  
  Purkinje cells of the cerebellum control deceleration of tongue movements.

- Zhang et al. (2025)  
  Decoding decision-making behavior from sparse neural spiking activity.

- Zeeshan et al. (2026)  
  Deep spatio-temporal models for decoding Purkinje cell activity.

---

# Future Work

Potential extensions include:

- Cross-session generalization
- Multi-animal training
- Transformer-based neural decoders
- Continuous movement reconstruction
- Behavioral intent classification
- Interpretable attention mechanisms
- Real-time neural decoding

---

# Authors

- Rahym Faisal Khan
- Maham Asif

---

# License

This repository is intended for academic and research purposes only.
