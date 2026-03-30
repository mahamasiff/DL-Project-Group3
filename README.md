# Decoding Cerebellar Purkinje Cell Dynamics with Deep Learning

## Overview

This project explores **deep learning approaches for decoding cerebellar Purkinje cell activity** into meaningful representations of motor behavior.

Purkinje cells are the **sole output of the cerebellum**, making them critical for understanding motor control and learning. This work focuses on bridging the gap between **neural spike data** and **fine-grained behavioral decoding**.

This repository includes:

* A **State-of-the-Art (SOA) survey**
* A **data processing + alignment pipeline**
* Feature engineering for **motor control signals (e.g., deceleration)**

📄 Full survey: 

---

## Problem Statement

We aim to:

> Decode high-dimensional neural spike data into meaningful motor behavior representations such as **movement deceleration and behavioral intent**. 

### Key Challenges

* **Data scarcity** → limited neural recordings
* **Signal sparsity** → rare spike events
* **High dimensionality & noise**
* **Spatio-temporal complexity** → need to model *when* and *where* spikes occur 

---

## Project Pipeline

📄 Implementation notebook: 

### 1. Data Ingestion

* Load `.mat` files using `h5py` / `scipy`
* Resolve MATLAB cell references
* Extract neural and behavioral signals

### 2. Data Transformation

* Flatten nested structures
* Convert spike timings into structured arrays
* Handle missing values and inconsistencies

### 3. Data Conversion

* Convert processed data → **pandas DataFrame**
* Export to `.csv` for downstream tasks

```
OG/1D_1.mat → cleaned data → DataFrame → CSV
```

---

## Data Processing Details

### Neural Data

* `SS_time` → Simple spikes
* `CS_time` → Complex spikes

These labels are **provided in the dataset (not inferred)** 

---

### Alignment Strategy

* Align neural spikes to behavioral events (lick onset)
* Extract spikes within a **100ms pre-movement window**
* Normalize spike times relative to event

---

### Feature Engineering

#### Deceleration Strength

We approximate motor deceleration using:

$$
\text{Deceleration Strength} = \frac{v_{max}}{t_{vmin} - t_{vmax}}
$$

This captures:

* How quickly movement slows down
* A proxy for cerebellar motor control signals 

---

## Output Format

Final dataset structure:

| Column           | Description         |
| ---------------- | ------------------- |
| lick_id          | Unique event ID     |
| label            | Behavior class      |
| electrode        | Neural channel      |
| spike_time       | Relative spike time |
| spike_type       | SS or CS            |
| velocity_max     | Peak velocity       |
| deceleration_min | Minimum velocity    |
| decel_strength   | Engineered feature  |

---

## Data Integrity Checks

* Duplicate spike removal
* Timestamp consistency validation
* Detection of time glitches

---

## Visualizations

* Spike raster plots (SS vs CS)
* Session-wise neural activity inspection

---

## Research Motivation

Existing models:

* Work well for **binary tasks** (e.g., lick vs no-lick)
* Fail to capture **fine motor dynamics**

### Gap

* Lack of models for **continuous motor variables**
* Limited interpretability of neural coordination


---

## Authors

* Rahym Faisal Khan
* Maham Asif

---

## License

This project is for academic/research purposes.
