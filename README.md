# A Reservoir Network Based Approach to Intrusion Detection Using Hybrid Transformer-ESN Architecture on CIC-IDS2017

A research implementation of a hybrid **Transformer–Echo State Network (ESN)** architecture for intrusion detection using the **CIC-IDS2017** dataset.

The proposed approach combines a fixed recurrent ESN reservoir with a Transformer encoder to learn nonlinear representations from network-flow features while keeping the recurrent reservoir weights untrained.

---

## 📌 Overview

Intrusion Detection Systems (IDS) play an important role in identifying malicious network activities and protecting computer networks from cyber threats.

Traditional machine-learning approaches can achieve high classification accuracy on network intrusion datasets, but highly imbalanced attack classes and the tabular nature of flow-based network data can make minority-class detection challenging.

This project investigates a hybrid architecture that combines:

* **Echo State Network (ESN)** for nonlinear reservoir-based feature transformation
* **Transformer Encoder** for attention-based representation learning
* **Multi-Head Self-Attention** for modeling relationships between encoded features
* **Trainable classification layers** for final intrusion detection

The experiments are conducted using the **CIC-IDS2017** benchmark dataset.

---

## 🎯 Research Objective

The main objective of this work is to investigate whether a hybrid **ESN + Transformer** architecture can provide an effective and computationally efficient approach for network intrusion detection.

The study focuses on:

1. Transforming network-flow features into nonlinear reservoir states.
2. Using a fixed ESN reservoir to reduce the number of trainable parameters.
3. Applying Transformer self-attention to the resulting representations.
4. Evaluating performance across individual intrusion classes.
5. Comparing the proposed architecture with conventional and published IDS approaches.
6. Examining the relationship between sequential learning architectures and flow-based tabular network features.

---

## 🧠 Proposed Architecture

The proposed **Transformer-ESN** architecture consists of two major learning components.

### 1. Echo State Network Reservoir

The ESN acts as a nonlinear feature transformation stage.

The preprocessed network-flow features are projected into a **256-dimensional reservoir state space**. The recurrent reservoir weights remain fixed during training, allowing the model to avoid training the recurrent connections.

### 2. Transformer Encoder

The generated reservoir representations are passed to a Transformer encoder.

The Transformer uses:

* Multi-head self-attention
* Positional information
* Feed-forward transformations
* Layer normalization
* Residual connections

The resulting representation is then passed to trainable projection and classification layers.

### Architecture Flow

```text
CIC-IDS2017 Network Flow Features
                │
                ▼
       Data Preprocessing
                │
                ▼
       Feature Standardization
                │
                ▼
       Fixed ESN Reservoir
                │
                ▼
      256-D Reservoir States
                │
                ▼
       Transformer Encoder
                │
        ┌───────┴───────┐
        │               │
        ▼               ▼
 Multi-Head          Feed-Forward
 Self-Attention       Network
        │               │
        └───────┬───────┘
                ▼
       Feature Representation
                │
                ▼
    Trainable Classification Head
                │
                ▼
       Intrusion Class Prediction
```

---

## 🔬 Dataset

This research uses the **CIC-IDS2017** intrusion detection dataset.

The dataset contains benign network traffic and multiple categories of malicious network activity generated in a realistic network environment.

The study considers the following classes:

| Category | Class                      |
| -------- | -------------------------- |
| 1        | BENIGN                     |
| 2        | DDoS                       |
| 3        | DoS Hulk                   |
| 4        | DoS GoldenEye              |
| 5        | DoS Slowloris              |
| 6        | DoS Slowhttptest           |
| 7        | Botnet ARES                |
| 8        | PortScan                   |
| 9        | FTP-Patator                |
| 10       | SSH-Patator                |
| 11       | Web Attack – Brute Force   |
| 12       | Web Attack – SQL Injection |
| 13       | Web Attack – XSS           |
| 14       | Infiltration               |

The dataset contains flow-level statistical features generated using **CICFlowMeter**.

---

## ⚙️ Data Preprocessing

The preprocessing pipeline includes:

* Removal of non-numeric and unsuitable features
* Handling of missing values
* Removal of constant features where applicable
* Feature standardization
* Label encoding
* Train/test data preparation
* Handling of class imbalance during model evaluation

The resulting network-flow representation is used as input to the ESN reservoir.

---

## 🏗️ Model Configuration

The main architectural configuration includes:

| Component            | Configuration                              |
| -------------------- | ------------------------------------------ |
| Input representation | Network-flow features                      |
| ESN reservoir size   | 256                                        |
| Reservoir type       | Fixed recurrent reservoir                  |
| Transformer          | Transformer Encoder                        |
| Attention mechanism  | Multi-Head Self-Attention                  |
| Classification       | Trainable projection/classification layers |
| Trainable parameters | Approximately 508K                         |

One of the central design choices is keeping the ESN recurrent weights fixed while training the downstream Transformer and classification components.

---

## 📊 Experimental Results

Under the experimental protocol used in this study, the proposed Transformer-ESN achieved:

| Metric          | Transformer-ESN |
| --------------- | --------------: |
| Accuracy        |      **99.38%** |
| Macro Precision |      **85.34%** |
| Macro Recall    |      **82.79%** |
| Macro F1-Score  |      **73.55%** |

The results demonstrate high overall accuracy while also highlighting the effect of severe class imbalance on macro-averaged performance.

In particular, the results show that overall accuracy alone does not fully represent performance across minority intrusion classes.

---

## 📈 Key Observations

The class-wise evaluation shows that:

* The majority of well-supported classes achieve strong F1-scores.
* Several classes achieve F1-scores above 0.94.
* Extremely rare classes remain substantially more difficult to classify.
* Overall accuracy is considerably higher than macro-averaged F1.
* This difference demonstrates the importance of reporting per-class and macro-level metrics for imbalanced intrusion-detection datasets.

The study therefore reports both aggregate metrics and detailed class-wise results rather than relying only on accuracy.

---

## 📁 Repository Structure

```text
transformer-esn-cic-ids2017/
│
├── notebooks/
│   └── Transformer-ESN-CIC-IDS2017.ipynb
│
├── Figure1_Accuracy_Ranking_Elsevier.png
├── Figure2_Architecture_Comparison.png
├── Figure3_Grouped_DotPlot.png
├── Figure4_TransformerESN_RadarChart.png
├── Figure5_Average_Architecture_Accuracy.png
├── Figure6_Evolution_IDS.png
├── Figure7_Transformer_ESN_Workflow.png
├── Figure8_Performance_Summary.png
│
├── main.pdf
├── main.tex
└── README.md
```

---

## 📓 Notebook

The complete experimental notebook is available in:

```text
notebooks/Transformer-ESN-CIC-IDS2017.ipynb
```

The notebook contains the implementation and experimental workflow associated with the research study.

---

# 🔍 Model Characteristics

The proposed architecture is designed around the separation of **nonlinear recurrent feature transformation** and **attention-based representation learning**.

The architecture therefore separates:

```text
Feature Encoding
      ↓
Fixed ESN Reservoir
      ↓
Reservoir States
      ↓
Transformer Encoder
      ↓
Trainable Projection / Classification
```

This design reduces the number of trainable recurrent parameters while allowing the Transformer component to learn task-specific representations.

---

## 🧩 Why Combine ESN and Transformer?

The two components provide complementary capabilities.

### Echo State Network

The ESN provides:

* Nonlinear state transformation
* Recurrent dynamics
* High-dimensional reservoir representations
* Fixed recurrent weights
* Reduced recurrent training requirements

### Transformer

The Transformer provides:

* Self-attention
* Multi-head feature interactions
* Flexible representation learning
* Trainable contextual transformations

Combining these components allows the model to use the ESN as a fixed nonlinear representation layer while using the Transformer to learn task-specific relationships in the resulting representation.

---

# 📊 Comparison With Other Approaches

The study considers both reproduced and previously published IDS approaches.

The reproduced Random Forest baseline achieved:

| Model           |   Accuracy |   Macro F1 |
| --------------- | ---------: | ---------: |
| Random Forest   | **99.80%** | **88.40%** |
| Transformer-ESN | **99.38%** | **73.55%** |

Previously reported accuracy values from related studies are also discussed in the research paper.

Because different studies may use different preprocessing pipelines, train/test partitions, sampling strategies, feature representations, and evaluation protocols, numerical results from separate publications should not be interpreted as a direct ranking of model performance.

---

# 🧪 Evaluation Metrics

The project reports multiple evaluation metrics to provide a more complete view of intrusion-detection performance.

### Accuracy

Measures the proportion of correctly classified samples across the complete test set.

### Precision

Measures the proportion of predicted samples belonging to a class that are actually members of that class.

### Recall

Measures the proportion of samples belonging to a class that are correctly identified.

### F1-Score

Provides a harmonic mean of precision and recall.

### Macro-Averaged Metrics

Macro averaging gives equal weight to each class and is particularly useful when evaluating datasets containing severe class imbalance.

---

# ⚠️ Class Imbalance

CIC-IDS2017 contains substantial differences in the number of samples available for different intrusion categories.

This creates an important distinction between:

* **Overall accuracy**
* **Class-wise performance**
* **Macro-averaged performance**

A model can achieve very high overall accuracy while still performing poorly on extremely rare attack categories.

For this reason, this project reports class-wise precision, recall, and F1-score in addition to overall accuracy.

---

# 📉 Minority-Class Performance

The evaluation highlights the difficulty of detecting extremely rare attack categories.

The reported test-set results include:

| Class            | Support | F1-Score |
| ---------------- | ------: | -------: |
| WA-Brute Force   |     301 |     0.33 |
| WA-XSS           |     130 |     0.06 |
| WA-SQL Injection |       4 |     0.13 |
| Infiltration     |       7 |     0.31 |

These results illustrate how very small class support can strongly affect minority-class detection performance and macro-averaged metrics.

---

# 📚 Research Contributions

The main contributions of this study are:

1. **Hybrid Transformer-ESN Architecture**

   A hybrid architecture combining a fixed ESN reservoir with a Transformer encoder for intrusion detection.

2. **Fixed Reservoir Design**

   The ESN recurrent weights are kept fixed, reducing the number of parameters that must be trained.

3. **Attention-Based Representation Learning**

   Transformer self-attention is applied to the reservoir representations to learn relationships between encoded network-flow features.

4. **Detailed Class-Wise Evaluation**

   The study evaluates individual intrusion categories instead of relying exclusively on overall accuracy.

5. **Reproduced Baseline**

   A Random Forest baseline is reproduced under the study's experimental protocol for comparison.

6. **Analysis of Sequential Inductive Bias**

   The work examines the relationship between sequential neural architectures and the flow-based, pre-aggregated nature of CIC-IDS2017 features.

---

# 📖 Research Paper

The complete research paper is available in this repository:

**[Research Paper – PDF](main.pdf)**

The LaTeX source is also provided:

**[LaTeX Source – main.tex](main.tex)**

---

# 🖼️ Figures

The repository contains the figures used for analysis and presentation of the research:

* `Figure1_Accuracy_Ranking_Elsevier.png`
* `Figure2_Architecture_Comparison.png`
* `Figure3_Grouped_DotPlot.png`
* `Figure4_TransformerESN_RadarChart.png`
* `Figure5_Average_Architecture_Accuracy.png`
* `Figure6_Evolution_IDS.png`
* `Figure7_Transformer_ESN_Workflow.png`
* `Figure8_Performance_Summary.png`

---

# 💻 Reproducibility

The implementation is provided through the Jupyter/Google Colab notebook:

```text
notebooks/Transformer-ESN-CIC-IDS2017.ipynb
```

The notebook is intended to provide the implementation and experimental workflow associated with the proposed model.

For reproducibility, users should ensure that the required dataset files are available and that the preprocessing steps are executed consistently with the experimental protocol described in the paper.

---

# 🛠️ Technologies Used

* Python
* Jupyter Notebook
* Google Colab
* NumPy
* Pandas
* Scikit-learn
* PyTorch
* Matplotlib
* Seaborn

---

# 📌 Important Note on Results

The reported results correspond to the experimental setup described in the research paper.

Performance on CIC-IDS2017 can vary depending on:

* Data preprocessing
* Feature selection
* Sampling strategy
* Class balancing
* Train/test partition
* Random seed
* Model configuration
* Evaluation protocol

Therefore, reported numerical results should always be interpreted together with the corresponding experimental methodology.

---

# 📜 Citation

If you use this work in academic research, please cite the associated paper.

```text
L. K. Suresh Kumar, Jay Shreeram Yeraballi,
Ravi Uyyala, Padmavathi Vurubindi, and Ashok Kumar Das.

"A Reservoir Network Based Approach to Intrusion Detection
Using Hybrid Transformer-ESN Architecture on CIC-IDS2017."
```

---

# 👥 Authors

### L. K. Suresh Kumar

Department of Computer Science and Engineering
University College of Engineering, Osmania University
Hyderabad, India

### Jay Shreeram Yeraballi

Department of Computer Science and Engineering
University College of Engineering, Osmania University
Hyderabad, India

### Ravi Uyyala

Department of Computer Science and Engineering
Chaitanya Bharathi Institute of Technology (CBIT)
Hyderabad, India

### Padmavathi Vurubindi

Department of Computer Science and Engineering
Chaitanya Bharathi Institute of Technology (CBIT)
Hyderabad, India

### Ashok Kumar Das

Center for Security, Theory and Algorithmic Research
International Institute of Information Technology
Hyderabad, India

---

# ⭐ Acknowledgement

This work uses the CIC-IDS2017 benchmark dataset for evaluating the proposed intrusion-detection architecture.

The research is intended to contribute to the study of hybrid reservoir-computing and attention-based approaches for cybersecurity and intrusion detection.
