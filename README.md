

## 🌟 Project at a Glance

| 📌 Component                  |               🔢 Result |
| ----------------------------- | ----------------------: |
| 🏥 Clinical encounters        |             **101,766** |
| 👤 Unique patients            |              **71,518** |
| 📋 Original variables         |                  **50** |
| 🧬 Candidate patient features |                  **38** |
| ⚙️ Transformed features       |                  **57** |
| 🔗 Training network nodes     |              **50,062** |
| 🔗 Training network edges     |             **434,595** |
| 🕸️ Primary network           | **Mutual k-NN, k = 10** |
| 📐 Mean network degree        |               **17.36** |
| 🔗 Mean edge similarity       |               **0.918** |
| 👥 Louvain communities        |                 **105** |
| 📊 Network modularity         |              **0.8652** |
| 🤖 Final baseline model       |       **Random Forest** |
| 📈 Test AUROC                 |              **0.8709** |
| 📈 Test AUPRC                 |              **0.5883** |
| 🎯 Test F1-score              |              **0.5720** |
| 🔬 Intervention budget        |                 **10%** |
| 💉 Intervention policies      |                   **4** |

---

<div align="center">

##  The Core Idea

### Can the structure of a clinical patient similarity network be combined with Agent-Based Modelling to understand readmission risk and evaluate targeted intervention strategies?

</div>

---

##  Why This Project?

Hospital readmission is influenced by multiple interacting clinical and utilization characteristics.

Traditional predictive modelling answers:

> **"Which patients are at higher risk?"**

This project asks an additional question:

> **"How does the position of a patient within a clinical similarity network change how we understand and simulate intervention strategies?"**

The framework therefore combines:

```text
Clinical Data
     +
Patient Similarity
     +
Network Structure
     +
Machine Learning
     +
Agent-Based Modelling
     +
Intervention Experiments
     =
Evidence-Based Simulation Framework
```

---

##  Problem Statement

Hospital readmission represents an important healthcare analytics problem because patients can differ substantially in their clinical characteristics, healthcare utilization patterns, and similarity to other patients.

A conventional predictive model treats patients largely as independent observations.

However, patients may form **groups of clinically similar individuals**.

This project constructs a **patient similarity network**, where:

* **Node** = patient
*  **Edge** = clinical similarity between two patients
*  **Edge weight** = strength of similarity
*  **Community** = group of structurally similar patients

The resulting network is then combined with a readmission model and an Agent-Based Model to simulate intervention strategies under a fixed resource budget.

> **Important:** This is a **clinical patient similarity network**, not an observed social network. The dataset does not contain real-world social relationships between patients.

---

##  Research Question

> **How can patient similarity network structure be integrated with Agent-Based Modelling to identify high-priority patient groups and evaluate targeted intervention strategies for hospital readmission?**

### Supporting Questions

1. Can patients be represented consistently at the patient level?
2. Can a leakage-safe clinical similarity network be constructed?
3. What structural patterns and communities emerge?
4. How accurately can readmission risk be predicted?
5. Can an ABM reproduce observed validation-level readmission behaviour?
6. How do different intervention policies behave under the same intervention budget?
7. How sensitive are the results to network construction, intervention effectiveness, and stochasticity?

---

<div align="center">

## 🗂️ Dataset

### Diabetes 130-US Hospitals for Years 1999–2008

</div>

The project uses the **Diabetes 130-US Hospitals for Years 1999–2008** dataset.

### Dataset characteristics

```text
101,766 encounters
        │
        ▼
71,518 unique patients
        │
        ▼
50 original variables
        │
        ▼
Patient-level analytical representation
```

### Original Readmission Distribution

| Category | Meaning                    | Encounters |
| -------- | -------------------------- | ---------: |
| `NO`     | No observed readmission    | **54,864** |
| `>30`    | Readmission after 30 days  | **35,545** |
| `<30`    | Readmission within 30 days | **11,357** |

### Primary Patient-Level Target

The primary target used in the project is:

```text
any_observed_readmission_under_30d
```

A patient receives:

```text
1 → if at least one observed encounter is labelled <30

0 → otherwise
```

---

##  Why Patient-Level Aggregation?

The original dataset is **encounter-level**.

Therefore:

```text
One patient
     │
     ├── Encounter 1
     ├── Encounter 2
     ├── Encounter 3
     └── ...
```

would otherwise appear as multiple observations.

The project therefore aggregates encounters by:

```text
patient_nbr
```

to produce:

```text
One patient → One analytical representation
```

This makes the patient the fundamental unit for:

* similarity
* network construction
* SNA
* prediction
* ABM
* intervention simulation

---

##  Leakage-Control Strategy

A major design principle of the project is:

> **Information used to define the target must not be allowed to define patient similarity or influence model fitting improperly.**

The pipeline therefore separates:

```text
                    ┌──────────────────────┐
                    │   Patient Dataset    │
                    └──────────┬───────────┘
                               │
                 ┌─────────────┴─────────────┐
                 ▼                           ▼
        Clinical Features             Readmission Target
                 │                           │
                 ▼                           │
       Similarity / Network                 │
                 │                           │
                 ▼                           │
              SNA                           │
                 │                           │
                 └──────────────┬────────────┘
                                ▼
                         ABM / Evaluation
```

### Leakage safeguards

* Patient-level split
* No patient overlap between train/validation/test
* Readmission target excluded from similarity features
* Patient identifier excluded from similarity features
* Feature preprocessing fitted on training data
* Test data kept untouched until final evaluation
* Intervention experiments performed on validation population
* Observed outcomes not used to select intervention targets
* Equal intervention budget across targeted policies

### Split

| Partition     |   Patients |
| ------------- | ---------: |
| 🟦 Training   | **50,062** |
| 🟨 Validation | **10,728** |
| 🟥 Test       | **10,728** |
| **Total**     | **71,518** |

> The dataset does not contain a reliable encounter timestamp suitable for a strict temporal split. Therefore, the project uses a reproducible patient-level random split and explicitly documents this limitation.

---

## ⚙️ Complete 01 → 12 Methodology

<div align="center">

```text
01
📋 Dataset Audit
      ↓
02
🧹 Cleaning & Variable Dictionary
      ↓
03
👤 Patient Representation
      ↓
04
🔒 Target Definition & Leakage-Safe Split
      ↓
05
⚙️ Feature Engineering & Similarity Representation
      ↓
06
🔗 Patient Similarity Network
      ↓
07
🕸️ SNA & Community Detection
      ↓
08
🤖 Baseline Readmission Model
      ↓
09
🧠 ABM Design & Calibration
      ↓
10
🔬 ABM Validation
      ↓
11
💉 Intervention Experiments
      ↓
12
🧪 Robustness, Ablation & Final Evaluation
```

</div>

---

## 📓 Notebook Structure

| Notebook                              | Purpose                                           |
| ------------------------------------- | ------------------------------------------------- |
| `01_dataset_audit.ipynb`              | Dataset integrity and reproducibility audit       |
| `02_cleaning.ipynb`                   | Missing values, cleaning and variable selection   |
| `03_patient_representation.ipynb`     | Encounter → patient representation                |
| `04_split_leakage_check.ipynb`        | Target definition and patient-level split         |
| `05_features_similarity.ipynb`        | Feature engineering and similarity representation |
| `06_similarity_network.ipynb`         | Patient similarity network construction           |
| `07_sna_communities.ipynb`            | Centrality and community analysis                 |
| `08_baseline_readmission_model.ipynb` | Readmission prediction                            |
| `09_abm_design_calibration.ipynb`     | ABM design and calibration                        |
| `10_abm_validation.ipynb`             | Validation against observed outcomes              |
| `11_intervention_experiments.ipynb`   | Intervention policy simulation                    |
| `12_robustness_final.ipynb`           | Robustness, ablation and final evidence           |

---

##  Patient Representation

The patient representation combines multiple dimensions of healthcare utilization and clinical information.

### Representation dimensions

```text
🏥 Healthcare Utilization
       │
       ├── Encounter counts
       ├── Inpatient utilization
       ├── Emergency utilization
       └── Outpatient utilization

💊 Clinical Complexity
       │
       ├── Medication burden
       ├── Diagnosis burden
       └── Procedure burden

👤 Demographic / Contextual Variables
       │
       ├── Age
       ├── Gender
       └── Other available context
```

The resulting representation contains:

```text
38 candidate features
        ↓
57 transformed features
```

---

##  Similarity Methodology

The primary similarity metric is:

### Cosine Similarity

For two patient vectors:

```text
A = patient A feature vector
B = patient B feature vector

cosine(A,B)
=
(A · B) / (||A|| ||B||)
```

Higher similarity indicates that the two patients have more similar analytical representations.

The project also evaluates similarity stability against alternative distance-based representations.

---

##  Patient Similarity Network

The primary network uses:

```text
Similarity Metric : Cosine
Network Rule      : Mutual k-NN
k                 : 10
Population        : Training patients only
```

### Network Statistics

| Metric               |               Value |
| -------------------- | ------------------: |
| Nodes                |          **50,062** |
| Edges                |         **434,595** |
| Density              |       **0.0003468** |
| Mean degree          |           **17.36** |
| Median degree        |              **18** |
| Mean edge weight     |          **0.9184** |
| Median edge weight   |          **0.9260** |
| Connected components |              **82** |
| Largest component    | **98.33%** of nodes |
| Isolated nodes       |              **77** |

### Interpretation

The network is sparse relative to a fully connected graph, while the largest connected component contains the overwhelming majority of training patients.

This allows the project to study both:

* individual patient structural position
* group/community structure

without constructing an infeasible dense all-pairs similarity matrix.

---

##  Network Analysis

The project extracts structural measures including:

* Degree
* Weighted degree / strength
* Betweenness centrality
* Closeness approximation
* Eigenvector centrality
* Connected components
* Community membership

### Community Detection

Primary method:

**Weighted Louvain**

Results:

```text
105 communities

Largest community:
6,767 patients

Weighted modularity:
0.8652
```

A label-propagation sensitivity analysis was also performed to examine structural dependence on the community detection method.

---

##  Baseline Readmission Model

Two baseline models were evaluated:

```text
                    Readmission Prediction
                             │
              ┌──────────────┴──────────────┐
              ▼                             ▼
      Logistic Regression             Random Forest
              │                             │
              └──────────────┬──────────────┘
                             ▼
                    Validation Comparison
                             │
                             ▼
                      Model Selection
```

### Validation Results

| Model               |      AUROC |      AUPRC |  Brier |
| ------------------- | ---------: | ---------: | -----: |
| Logistic Regression |     0.8708 |     0.5770 | 0.0751 |
| Random Forest       | **0.8770** | **0.5933** | 0.1203 |

The Random Forest was selected using **validation AUPRC**.

The classification threshold was selected using the validation set and then frozen before evaluating the test set.

---

##  Final Test Performance

The frozen Random Forest model achieved:

| Metric          | Test Result |
| --------------- | ----------: |
| **AUROC**       |  **0.8709** |
| **AUPRC**       |  **0.5883** |
| **Brier Score** |  **0.1224** |
| Accuracy        |  **0.8702** |
| Precision       |  **0.4826** |
| Recall          |  **0.7019** |
| F1-score        |  **0.5720** |

### Confusion Matrix

```text
                    Predicted
                  0          1
              ┌────────┬────────┐
Actual   0    │  8,406 │    997 │
              ├────────┼────────┤
         1    │    395 │    930 │
              └────────┴────────┘

TN = 8,406
FP =   997
FN =   395
TP =   930
```

---

##  Agent-Based Model

The ABM treats each patient as an individual agent.

### Agent State

```text
┌─────────────────────────────────┐
│          Patient Agent          │
├─────────────────────────────────┤
│ Patient ID                      │
│ Baseline readmission risk       │
│ Utilization / clinical state    │
│ Network position                │
│ Community membership            │
│ Intervention state              │
│ Outcome state                   │
└─────────────────────────────────┘
```

### Transition Mechanism

The basic transition probability is represented as:

```text
P(readmission)
        │
        ▼
Baseline Patient Risk
        │
        ▼
Calibration
        │
        ▼
Intervention Effect
        │
        ▼
Final Transition Probability
        │
        ▼
Random Draw
        │
        ├──── probability crossed ────► 🔴 Readmitted
        │
        └──── probability not crossed ► 🟢 Stable
```

The ABM is designed as a **probabilistic simulation**, rather than a deterministic rule.

---

## 🔬 ABM Validation

Notebook 10 evaluates whether the ABM produces simulated behaviour that is consistent with the observed validation population.

The validation process compares:

```text
Observed patient outcome
          VS
Simulated patient outcome
```

while preserving the same validation population.

The ABM validation stage is kept separate from intervention experiments so that intervention results are not confused with baseline model validation.

---

## 💉 Intervention Experiments

Notebook 11 evaluates four policies:

```text
1️⃣ No Intervention
2️⃣ Random
3️⃣ Risk-Based
4️⃣ Network-Informed
```

### Equal Resource Constraint

All targeted policies receive the same intervention budget:

```text
Validation population
       │
       ▼
10,728 patients
       │
       ▼
10% intervention budget
       │
       ▼
1,073 intervention targets
```

This ensures that policy comparisons are based on different **targeting strategies**, rather than different numbers of interventions.

---

## 🧠 Network-Informed Targeting

The network-informed policy incorporates information from:

```text
Patient Risk
      +
Network Structure
      +
Community Context
      ↓
Network-Informed Target Score
```

This allows the experiment to investigate whether network structure provides information beyond individual risk alone.

---

## 🔬 Robustness Analysis

Notebook 12 examines whether conclusions are sensitive to modelling choices.

### Robustness dimensions

```text
             Robustness
                 │
     ┌───────────┼───────────┐
     ▼           ▼           ▼
Network-k    Intervention   Effect
Sensitivity    Budget       Size
     │           │           │
     └───────────┼───────────┘
                 ▼
          Stochasticity
                 │
                 ▼
             Ablation
```

### Network Sensitivity

The project evaluates alternative network `k` values rather than assuming that `k=10` is universally optimal.

The purpose is to determine whether network-based findings remain structurally consistent under reasonable network construction changes.

---

## 🧪 Ablation Analysis

Ablation experiments remove selected components from the modelling framework to examine their contribution.

Conceptually:

```text
Full Framework
      │
      ├── Remove Network Information
      │
      ├── Remove Community Information
      │
      ├── Alter Risk Component
      │
      └── Compare Result
```

This helps distinguish:

> **What the complete framework does**

from:

> **What individual components contribute.**

---

## 🎲 Stochasticity & Uncertainty

Because the ABM contains probabilistic transitions, different random seeds can produce different individual-level outcomes.

Therefore the project evaluates:

* repeated simulations
* seed sensitivity
* mean outcomes
* uncertainty intervals
* intervention-effect variation

The goal is to avoid interpreting a single simulation run as definitive evidence.

---

## 📊 Evidence Chain

The project follows this evidence hierarchy:

```text
DATA QUALITY
     ↓
PATIENT REPRESENTATION
     ↓
LEAKAGE CONTROL
     ↓
SIMILARITY NETWORK
     ↓
NETWORK STRUCTURE
     ↓
READMISSION PREDICTION
     ↓
ABM CALIBRATION
     ↓
ABM VALIDATION
     ↓
INTERVENTION EXPERIMENTS
     ↓
ROBUSTNESS / ABLATION
     ↓
FINAL EVIDENCE
```

Each stage depends on the validity of the previous stage.

---

## 🔬 Methodological Contribution

The project integrates several analytical perspectives into one reproducible framework:

| Layer                  | Method                                 |
| ---------------------- | -------------------------------------- |
| 🏥 Clinical analytics  | Patient-level representation           |
| 🧬 Similarity learning | Cosine similarity                      |
| 🔗 Network science     | Mutual k-NN graph                      |
| 🕸️ SNA                | Centrality + community detection       |
| 🤖 Machine learning    | Readmission prediction                 |
| 👥 ABM                 | Patient-level stochastic simulation    |
| 💉 Policy simulation   | Intervention targeting                 |
| 🧪 Robustness          | Sensitivity + ablation + stochasticity |

The key methodological contribution is the integration of:

```text
Patient Similarity
        +
Network Structure
        +
Readmission Risk
        +
Agent-Based Simulation
        +
Resource-Constrained Intervention
```

into a single experimental pipeline.

---

## ⚠️ What This Project Can and Cannot Conclude

### ✅ The project can investigate

* Whether patients can be represented consistently at patient level.
* The structural properties of a clinical similarity network.
* The existence of clinically similar patient communities.
* Predictive performance for observed readmission.
* How simulated intervention policies behave under controlled assumptions.
* Sensitivity of simulation results to modelling choices.

### ❌ The project cannot establish

* That network proximity represents actual social relationships.
* That an intervention would causally reduce readmission in a real hospital.
* That simulated intervention effects automatically translate to clinical effectiveness.
* That the model is temporally validated when no reliable encounter timestamp is available.
* That network structure itself causes readmission.

The ABM should therefore be interpreted as a **simulation and policy-exploration framework**, not as a clinical deployment system.

---

##  Final Research Takeaway

<div align="center">

###  Clinical Data

⬇️

###  Patient Representation

⬇️

###  Similarity

⬇️

###  Network Structure

⬇️

###  Readmission Prediction

⬇️

###  Agent-Based Simulation

⬇️

###  Targeted Intervention

⬇️

###  Robustness Evaluation

⬇️

##  Evidence-Based Simulation Framework

</div>

The project demonstrates a complete analytical workflow connecting **clinical data, patient similarity, network science, machine learning, and agent-based intervention simulation**.

The final interpretation should focus on the evidence produced by the pipeline and the assumptions required to generate the simulated outcomes.

---

## 📁 Repository Structure

```text
ABM-medical_readmission/
│
├── 🏥 diabetes+130-us+hospitals+for+years+1999-2008/
│   ├── diabetic_data.csv
│   └── IDS_mapping.csv
│
├── 📓 notebooks/
│   ├── 01_dataset_audit.ipynb
│   ├── 02_cleaning.ipynb
│   ├── 03_patient_representation.ipynb
│   ├── 04_split_leakage_check.ipynb
│   ├── 05_features_similarity.ipynb
│   ├── 06_similarity_network.ipynb
│   ├── 07_sna_communities.ipynb
│   ├── 08_baseline_readmission_model.ipynb
│   ├── 09_abm_design_calibration.ipynb
│   ├── 10_abm_validation.ipynb
│   ├── 11_intervention_experiments.ipynb
│   ├── 12_robustness_final.ipynb
│   └── ABM_SNA_Final_Presentation.ipynb
│
├── 📊 results/
│   ├── Dataset audit outputs
│   ├── Patient representations
│   ├── Feature matrices
│   ├── Network outputs
│   ├── SNA outputs
│   ├── Prediction outputs
│   ├── ABM outputs
│   ├── Intervention results
│   └── Robustness results
│
├── 🖼️ figures/
│   ├── Network figures
│   ├── Model evaluation figures
│   ├── Intervention figures
│   └── Robustness figures
│
├── 📄 ABM_SNA_Proposal.docx
│
└── 📖 README.md
```

---

## 📊 Key Output Files

The `results/` directory contains machine-readable evidence generated throughout the pipeline.

### Network

```text
06_patient_similarity_edges_k10.csv
06_patient_similarity_nodes.csv
06_network_summary.json
06_network_config.json
```

### SNA

```text
07_centrality.csv
07_centrality_summary.csv
07_community_assignments.csv
07_community_summary.csv
07_component_summary.csv
07_sna_summary.json
```

### Prediction

```text
08_test_predictions.csv
08_model_comparison.csv
08_test_metrics.json
```

### ABM

```text
09_abm_agent_state.csv
09_abm_calibration.csv
09_abm_config.json
09_abm_train_no_intervention.csv
09_abm_validation_no_intervention.csv
09_abm_validation_risk_bins.csv
```

### Intervention

```text
11_intervention_replication_results.csv
11_intervention_policy_summary.csv
11_paired_policy_comparisons.csv
11_target_selection_audit.csv
11_intervention_config.json
```

### Robustness

```text
12_network_sensitivity.csv
12_robustness_summary.csv
12_effect_sizes.csv
12_seed_robustness.csv
12_seed_summary.csv
12_ablation_results.csv
12_ablation_summary.csv
12_final_evidence_summary.csv
12_network_target_stability.csv
```

---



## 🛠️ Technology Stack

<div align="center">

<img src="https://img.shields.io/badge/Python-3.11-blue?style=flat-square&logo=python">
<img src="https://img.shields.io/badge/Pandas-Data%20Processing-150458?style=flat-square&logo=pandas">
<img src="https://img.shields.io/badge/NumPy-Numerical%20Computing-013243?style=flat-square&logo=numpy">
<img src="https://img.shields.io/badge/Scikit--learn-Machine%20Learning-F7931E?style=flat-square&logo=scikit-learn">
<img src="https://img.shields.io/badge/NetworkX-Network%20Analysis-green?style=flat-square">
<img src="https://img.shields.io/badge/Mesa-Agent%20Based%20Modelling-purple?style=flat-square">
<img src="https://img.shields.io/badge/Jupyter-Notebooks-orange?style=flat-square&logo=jupyter">

</div>

---

## 🔁 Reproducibility

The project uses fixed random seeds where stochastic procedures are involved.

The pipeline records:

* dataset audit information
* feature-selection decisions
* split configuration
* similarity configuration
* network configuration
* model configuration
* ABM calibration configuration
* intervention budget
* simulation seeds
* robustness settings

This allows the analysis to be reproduced and independently inspected.

---
