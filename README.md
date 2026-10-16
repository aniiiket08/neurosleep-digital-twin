<div align="center">

# NeuroSleep Digital Twin Platform

**A multi-scale model of simulated sleep deprivation, from cell metabolism to brain-state classification.**

![Python](https://img.shields.io/badge/python-scikit--learn-3b4252?style=flat-square&logo=python&logoColor=white)
![React](https://img.shields.io/badge/react-18-3b4252?style=flat-square&logo=react&logoColor=white)
![VCell](https://img.shields.io/badge/vcell-cellular%20modeling-3b4252?style=flat-square)
![Cytoscape.js](https://img.shields.io/badge/cytoscape.js-connectome-3b4252?style=flat-square)
![Status](https://img.shields.io/badge/status-prototype-3b4252?style=flat-square)

[Overview](#-overview) · [Connectivity](#-connectivity) · [How it works](#-how-it-works) · [Results](#-results) · [Scope](#-scope) · [Structure](#-project-structure)

</div>

<br>

## 🧠 Overview

Sleep deprivation affects cellular energy metabolism, oxidative stress, neuronal activity, and communication between brain regions. Studying that chain experimentally across scales is slow and resource-intensive.

NeuroSleep models the chain computationally. VCell simulates cellular metabolic stress, a Hodgkin-Huxley / FitzHugh-Nagumo-inspired model represents neuronal instability, Cytoscape maps the resulting connectivity, and five ML classifiers predict overall brain state. An interactive dashboard ties it together and shows the progression from healthy, to severely deprived, to recovery.

|  |  |
|---|---|
| **Cellular** | VCell: ATP, ROS, SleepFactor |
| **Neuronal** | Hodgkin-Huxley / FitzHugh-Nagumo-inspired spiking model |
| **Network** | Cytoscape: 5 brain regions |
| **Classification** | Logistic Regression, Decision Tree, Random Forest, SVM, KNN |
| **Dashboard** | React, Tailwind, Cytoscape.js |
| **States** | Healthy, Mild, Recovery, Severe |

<p align="center">
  <img src="assets/pipeline_diagram.png" alt="Pipeline: sleep deprivation input through VCell metabolic simulation, dataset generation, ML classification and neural spiking model, Cytoscape connectivity, to the digital twin dashboard" width="900">
</p>

<br>

## 🕸️ Connectivity

Cytoscape connectome at the two ends of the progression. Network density drops sharply from Healthy to Severe, consistent with the saved Cytoscape session.

<p align="center">
  <img src="assets/Healthy_Network.png" alt="Healthy network" width="340">
  <img src="assets/Severe_Network.png" alt="Severe network" width="340">
</p>

<p align="center"><sub>Healthy · Severe</sub></p>

<br>

## 🔬 How it works

| Layer | Description |
|---|---|
| **1. Cellular** | Reaction-based VCell model of ATP, ROS, and SleepFactor across four conditions, solved as a time-dependent stiff system (CVODE/IDA). |
| **2. Neuronal** | Simplified spiking model showing how metabolic stress destabilizes membrane potential and ionic conductances as severity increases. |
| **3. Network** | Hippocampus, Prefrontal Cortex, Amygdala, Thalamus, and Brainstem as a connectivity graph that weakens under deprivation. |
| **4. AI** | Five classifiers predict brain state from ATP, ROS, SleepFactor, and a derived SeverityScore. |
| **5. Dashboard** | One Digital Twin view, with a scripted presentation mode: Healthy, 24h, 48h, 72h, Recovery. |

<br>

## 📊 Results

All five classifiers scored 100% accuracy, precision, recall, and F1 on the held-out test split of the simulation-derived dataset (1,189 samples, 4 balanced classes).

| Model | Accuracy |
|---|:---:|
| Logistic Regression | 1.00 |
| Decision Tree | 1.00 |
| Random Forest | 1.00 |
| SVM | 1.00 |
| KNN | 1.00 |

> [!NOTE]
> These scores are not clinical accuracy. The data is simulation-derived, and `SeverityScore` is computed from the same variables the models classify on, so the classes are highly separable. Real clinical data would need far more rigorous validation.

<br>

## ⚠️ Scope

Some parts of the pipeline are easy to overstate, so to be explicit:

- **No real NEURON simulation.** The spiking model is a JavaScript implementation inspired by Hodgkin-Huxley / FitzHugh-Nagumo dynamics, not output from the NEURON package.
- **Simulated EEG.** The Beta / Alpha / Theta / Delta visualization is procedurally generated, not recorded from a subject.
- **Not clinically validated.** This is an academic modeling and visualization prototype, not a diagnostic tool.
- **Two connectome models.** The saved Cytoscape session uses a 5-region network. The live dashboard uses a separate, more detailed 13-region procedural model.

<br>

## 🎯 Applications

- Prototyping for sleep-deprivation and computational neuroscience research
- Demonstrating multi-scale simulation and Digital Twin methodology
- Interactive visualization for teaching and demos

<br>

## 🧰 Tech stack

VCell, Cytoscape, Python, scikit-learn, React 18, Tailwind CSS, Cytoscape.js, Three.js, Web Audio API

<br>

## 📁 Project structure

```
.
├── vcell/
│   └── Sleep_Deprivation_Model.vcml           ATP / ROS / SleepFactor reaction model
├── cytoscape/
│   └── Sleep_Deprivation_Brain_Network.cys
├── ml/
│   ├── acm.csv                                simulation-derived dataset
│   └── acm_ml.ipynb                           classifier training and prediction
├── dashboard/
│   ├── index.html
│   ├── simulation.js                          cellular and neural simulation engine
│   ├── network.js                             13-node connectome engine
│   ├── sound.js
│   └── dashboard.js
├── assets/
│   ├── pipeline_diagram.png
│   ├── ATP_Healthy.png
│   ├── ATP_Mild.png
│   ├── ATP_Recovery.png
│   ├── ATP_Severe.png
│   ├── ROS_Healthy.png
│   ├── ROS_Mild.png
│   ├── ROS_Recovery.png
│   ├── ROS_Severe.png
│   ├── Healthy_Network.png
│   └── Severe_Network.png
└── README.md
```
