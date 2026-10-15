<h1 align="center">🚦 Spatiotemporal Traffic Forecasting & Optimal Path Navigation</h1>

<p align="center">
  Forecasting road-network speeds with <b>graph neural networks</b> and using the predictions to find the fastest route.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white" />
  <img src="https://img.shields.io/badge/PyG_Temporal-3C2179?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white" />
</p>

---

## 📌 Overview

Traffic on a road network is both **spatial** (congestion spreads to neighbouring roads) and **temporal** (it follows daily patterns). This project models the road network as a graph — sensors/junctions are nodes, roads are edges — and trains spatio-temporal GNNs to predict speeds **15, 30 and 45 minutes ahead**. The forecasts feed a routing service ([TrafficServer](https://github.com/akshita251/TrafficServer)) that computes the optimal path with Dijkstra's algorithm.

## 🧠 Models compared

| Model | Idea |
|---|---|
| **A3T-GCN** | Attention Temporal Graph Convolutional Network — GCN + GRU with attention over time steps |
| **DCRNN** | Diffusion Convolutional Recurrent Neural Network — models traffic as a diffusion process on a directed graph |
| **ASTGCN** | Attention-based Spatial-Temporal GCN — separate spatial & temporal attention blocks |

Trained models for each horizon are saved in `a3tgcn{15,30,45}mins/` and `dcrnn{15,30,45}mins/`.

## 🗂️ Datasets

| Dataset | Description |
|---|---|
| `DIVE/` | 20-node road network in Mumbai — speeds, travel times, node coordinates, adjacency |
| `PeMSD7/` | Caltrans PeMS District 7 (Los Angeles) speeds + adjacency |
| `PeMSD8/` | Caltrans PeMS District 8 (San Bernardino) speeds + adjacency |

The adjacency matrix for DIVE is built from node lat/long with the haversine distance — see `Getting the ADJ.ipynb`.

## 🔬 Pipeline

```mermaid
flowchart LR
    A[Sensor speeds<br/>+ node coords] --> B[Adjacency matrix<br/>haversine distance]
    B --> C[StaticGraphTemporalSignal<br/>history = 4 steps]
    C --> D{GNN}
    D --> D1[A3T-GCN]
    D --> D2[DCRNN]
    D --> D3[ASTGCN]
    D1 & D2 & D3 --> E[Speed forecast<br/>15 / 30 / 45 min]
    E --> F[TrafficServer<br/>Dijkstra optimal path]
```

## 🚀 Run it

```bash
pip install torch torch-geometric torch-geometric-temporal pandas numpy scikit-learn seaborn matplotlib
jupyter notebook GNNs.ipynb
```

`DIVE.pdf` contains the project write-up.

## 👥 Team

Built as a team project with [@Vishesh-Mittal](https://github.com/Vishesh-Mittal). Licensed under MIT.
