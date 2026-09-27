# Energy-Efficient Implementation of SNNs using RRAM-based In-Memory Computing (IMC)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Framework: SNNTorch](https://img.shields.io/badge/Framework-SNNTorch-orange.svg)](https://snntorch.readthedocs.io/)
[![EDA Tool: Cadence Virtuoso](https://img.shields.io/badge/EDA-Cadence%20Virtuoso-red.svg)](https://www.cadence.com/)

This repository contains the software models, EDA hardware simulation schematics, and Verilog-A behavioral code for an energy-efficient **Spiking Neural Network (SNN)** implemented on a **Resistive Random Access Memory (RRAM)** based **In-Memory Computing (IMC)** architecture.

---

## 📌 Project Overview

Traditional implementations of Neural Networks on von Neumann architectures suffer from severe memory-wall bottlenecks due to continuous data movement between processing units and memory. This project addresses the issue by integrating computation directly into memory using **1T-1R RRAM crossbar arrays** and **Leaky Integrate-and-Fire (LIF) sensing neurons**.

Key highlights include:
1. **Algorithmic SNN Training:** A 3-layer MLP SNN trained on the MNIST dataset using `SNNTorch` with Backpropagation Through Time (BPTT), achieving **96.16% test accuracy**.
2. **RRAM Behavioral Modeling:** Verilog-A behavioral modeling of 1T-1R RRAM cells integrated with 90nm access transistors.
3. **Neuromorphic Sensing Circuits:** Custom-designed analog LIF neuron circuits using RC integration networks and comparator blocks in Cadence Virtuoso.
4. **Co-Validation:** Full hardware-software co-simulation comparing discrete Python update equations against Cadence analog transient responses for a $2 \times 2$ crossbar VMM configuration.

---

## 🏗— System Architecture

The overall hardware pipeline follows an end-to-end neuromorphic sensing and computation paradigm:

```
[ Spike Input / Rate Encoding ] 
              │
              ▼
    [ Input Voltage Encoders ]
              │
              ▼
┌─────────────────────────────────┐
│     RRAM Memristive Crossbar    │  ◄── Conductance Matrices (Weights mapped from SNNTorch)
│  (Vector-Matrix Multiplication) │
└─────────────────────────────────┘
              │
              ▼ [ Accumulated Bit-line Currents ]
┌─────────────────────────────────┐
│        LIF Sensing Neurons      │  ◄── RC Integration & Spike Thresholding
└─────────────────────────────────┘
              │
              ▼
       [ Spike Outputs ]

```

---

## ⚡ Hardware Parameters & Specifications

### 1. RRAM & Access Transistor
* **Cell Architecture:** 1T-1R (1 Transistor - 1 Resistor)
* **Access Transistor Node:** 90nm CMOS ($V_{dd} = 1.2\text{V}$, $V_{\text{gate}} = 1.8\text{V}$)
* **Switching States:** Non-volatile switching between High Resistance State (HRS) and Low Resistance State (LRS) via SET ($V_{\text{SET}}$) and RESET ($V_{\text{RESET}}$) potentials.

### 2. Hardware LIF Sensing Neuron
* **Membrane Resistance ($R_{mem}$):** $500\text{ k}\Omega$
* **Membrane Capacitance ($C_{mem}$):** $100\text{ pF}$
* **Resting Potential ($V_{rest}$):** $-0.7\text{ mV}$
* **Threshold Potential ($V_{th}$):** $-0.5\text{ mV}$
* **Spike Pulse Width:** $0.5\text{ ms}$

### 3. SNN Architecture & Training
* **Topology:** 196 (Input, $14\times14$ MNIST) $\rightarrow$ 400 (Hidden) $\rightarrow$ 10 (Output)
* **Decay Factor ($\beta$):** 0.95
* **Time Steps:** $200\text{ ms}$ spike train duration
* **Framework:** PyTorch & `SNNTorch`

---

## 📊 Results

* **SNN Test Accuracy:** **96.16%** on resized $14\times14$ MNIST test set after 5 epochs.
* **Co-Simulation Match:** Waveform analysis in Cadence Virtuoso confirms exact alignment of spike output times and membrane charge trajectories ($V_{charge}$) with Python discretized update equations under equivalent pulse trains.

---


## 🔮 Future Prospects
* Scaling to multi-layer, high-density crossbar arrays.
* Designing on-chip input/output pulse-encoding and interface circuits.
* Implementing on-site real-time learning using **Spike Timing Dependent Plasticity (STDP)**.
* Comprehensive energy-per-spike benchmarks versus standard digital CMOS platforms.

---

## 📜 Authors & Acknowledgments

* **Kartik Agrawal** (EE21BTECH11030)
* **Bipul Biswas** (EE24MTECH12003)
* **D Krupakar Reddy** (EE24RESCH11001)

*Department of Electrical Engineering, Indian Institute of Technology Hyderabad (IITH)*

---
## 📄 License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
