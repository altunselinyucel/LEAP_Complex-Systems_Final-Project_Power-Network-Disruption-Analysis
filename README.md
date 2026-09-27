# Power-Network Disruption: Predicting Substation Interruptions

**Author:** Altun Selin Yücel
**Context:** Complex Systems Final Project – LEAP Program

## 1. Project Overview & Objective
This project investigates whether incorporating a network's topological information improves the prediction of subsequent power substation interruptions. The central objective is to evaluate if local connectivity features provide a measurable predictive advantage over standard baseline metrics when using a classical machine learning model.

## 2. Dataset & Features
* **Topology:** A synthetic grid-like network consisting of 144 substations (nodes) and 344 undirected physical links (edges).
* **Baseline Features:** `load_ratio` (pre-disruption load relative to the nominal limit) and `equipment_age_years`.
* **Target Variable:** `interrupted_later` (a binary outcome indicating a simulated interruption over the following 24 hours).

## 3. Methodology & Workflow
* **Network Construction:** An undirected graph was built using `NetworkX` representing the physical power grid connections.
* **Feature Engineering:** Two structural network features were calculated to capture local topology:
  * **Degree:** The number of direct physical links connected to a substation.
  * **Local Clustering Coefficient:** The proportion of possible connections among a substation's direct neighbors.
* **Modeling Pipeline:** A Logistic Regression classifier was utilized. Both the baseline model and the network-aware model were trained and evaluated using the exact same observations and a 30% stratified train/test split. Feature scaling was applied strictly within the training pipeline using `StandardScaler`.

## 4. Key Results & Evaluation
Incorporating network features yielded strong performance improvements across all primary classification metrics.

| Metric | Baseline Features | Baseline + Network Features |
| :--- | :--- | :--- |
| **Accuracy** | 0.767 | 0.860 |
| **F1 Score** | 0.444 | 0.750 |
| **ROC AUC** | 0.828 | 0.949 |

The comparative confusion matrices demonstrate that the network-aware model successfully altered the balance of false positives (false alarms) and false negatives (missed interruptions), providing a vastly more reliable predictive boundary for heavily imbalanced target classes.

## 5. Ethical Considerations & Critical Reflection
It is important to distinguish that *degree* only counts direct physical links, whereas *local clustering* measures how interconnected a substation's immediate neighbors are. From an ethical and operational standpoint, using these predictions to prioritize grid maintenance requires strict human oversight. A false alarm (false positive) could waste limited repair resources, while a missed interruption (false negative) might leave vulnerable communities without power for extended periods. Therefore, domain experts must review these automated flags before dispatching repair crews.
