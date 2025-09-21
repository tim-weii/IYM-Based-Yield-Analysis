
#  Intelligent Yield Management (IYM) — Key-Variable Search for Yield Loss Analysis  
## Background  

In advanced manufacturing, **yield directly affects cost and competitiveness**.  


In manufacturing data, anomalies can generally be divided into two categories:  

1. **Point-wise Anomaly**  
   - Refers to a single data point that significantly deviates from its neighbors.  
   - Such anomalies are isolated and discontinuous, and do not follow the normal trend of the dataset.  
   - Example:  
   <p align="center">
<img width="500" alt="螢幕擷取畫面 2025-09-21 225915" src="https://github.com/user-attachments/assets/fed0e3a8-69d8-44fa-b006-50f27e2489b7" />
   </p>  

2. **Pattern-wise Anomaly**  
   - Refers to a sequence of data points over a certain time interval that deviates from the expected overall pattern.  
   - Unlike point anomalies, pattern anomalies reflect abnormal relationships or variations across multiple data points.  
   - Typically continuous and span across a time window.  
   - Example:  
   <p align="center">
<img width="500" alt="image" src="https://github.com/user-attachments/assets/c32fd472-482d-444a-ad6d-c550e7e0d658" />
   </p>  

---

Traditional yield analysis mainly relies on **historical pattern matching**.  
However, under **high-dimensional conditions (p ≫ n)** — where process parameters far exceed available samples — such methods often fail to identify the **true root causes**.  

To address this, I introduce **Intelligent Yield Management (IYM)** with the **Key-Variable Search Algorithm (KSA)**.  

---

## Case Study 

In real manufacturing cases, **it is often difficult to visually distinguish process differences**.  
The figure below shows one of the **most obvious cases**, yet even here the variations are subtle and cannot be reliably judged by the naked eye.  

---

<p align="center">
  <img width="300" height="150" alt="image" src="https://github.com/user-attachments/assets/838018e0-8d1d-45ab-9970-fab51cb944b1" />

</p>

**Figure 2.** Raw process data — this example was deliberately selected as one of the clearest cases, with problematic regions highlighted in red. Even so, proper classification still requires algorithmic analysis rather than visual inspection alone.

---

To address this challenge, I applied **unsupervised clustering methods** to reveal hidden patterns in the data:  

- **K-means Clustering & DBSCAN Clustering**  

<p align="center">
<img width="800" height="500" alt="螢幕擷取畫面 2025-09-20 221608" src="https://github.com/user-attachments/assets/23a958b1-4029-4de0-9261-268d34ee0580" />
</p>  

**Figure 3 & 4.** Results of K-means and DBSCAN clustering.  

### Limitations of K-means:
Assumes spherical clusters and performs poorly on non-linear boundaries.
Highly sensitive to initial centroid selection, often leading to local optima.
Requires pre-defined number of clusters (k), which is unknown in high-dimensional process data.

### Limitations of DBSCAN:
Extremely sensitive to parameter settings (ε neighborhood size, minPts).
Struggles with datasets of varying density, often misclassifying normal points as noise.
In high-dimensional data, distance metrics become less meaningful (“curse of dimensionality”), leading to distorted clustering results.


### IYM in the iFA System  

<p align="center" style="background-color:white; display:inline-block; padding:10px;">
  <img width="800" alt="iym_framework" src="https://github.com/user-attachments/assets/5eeda933-af05-4bbe-8982-5973fcff7816" />
</p>

**Figure 2.** The position of **Intelligent Yield Management (IYM)** within the **iFA system**.  
IYM serves as a data-driven module that integrates with production information flows to focus on yield-related analysis.  

---

### What IYM Does  

Traditional yield analysis mainly relies on **historical pattern matching**.  
However, under **high-dimensional conditions (p ≫ n)** — where process parameters far exceed available samples — such methods often fail to identify the **true root causes**.  

To address this, I introduce **Intelligent Yield Management (IYM)** with the **Key-Variable Search Algorithm (KSA)**.  

<p align="center" style="background-color:white; display:inline-block; padding:10px;">
<img width="1633" height="703" alt="螢幕擷取畫面 2025-09-19 144409" src="https://github.com/user-attachments/assets/3bb9d97e-9c58-4a52-bd06-f73afb41bbb8" />
</p>


**Figure 3.** Workflow of IYM — applying the Key-Variable Search Algorithm (KSA) to identify critical parameters for yield loss analysis.  


---

<img width="1548" height="1008" alt="image" src="https://github.com/user-attachments/assets/b95c51b0-858e-48df-91e1-c004f650b032" />

##  Methodology (KSA Framework)  
The **KSA scheme** combines greedy search and sparse regression to identify critical process variables:  

1. **Data Preprocessing**  
   - Align production routes (XR), process parameters (XP), inline inspection data (y), and final yield (Y).  
   - Normalize, clean, and organize batch/tool-level information.  

2. **Key-Variable Search (KSA Module)**  
   - In our approach, **Short-Time Fourier Transform (STFT)** is first applied to transform time-series signals into the time–frequency domain, allowing us to capture dynamic process patterns.  
   - Instead of directly relying on traditional variable selection algorithms (e.g., TPOGA, ALASSO), STFT-based features are extracted to highlight critical variations in process behavior that may contribute to yield loss.  

3. **Reliance Index (RIK)**  
   - Measures the confidence of identified parameters/features.  
   - RIK > 0.7 = high confidence in the extracted factors.  

---

##  Two-Phase Search Process  

1. **Phase I — Suspicious Stage/Tool Identification**  
   - Input: (Y, y, XR)  
   - Output: Top-N suspicious tools/stages with RIK confidence scores.  

2. **Phase II — Parameter Drill-Down**  
   - Input: (Y, XP) for the selected stage.  
   - Output: Key process parameters (e.g., coating speed, viscosity) with coefficient values & RIK scores.  

---

##  Why ALASSO?  

- **Handles p ≫ n:** Works in high-dimensional cases with limited defect samples.  
- **Sparsity assumption:** Selects only the true root-cause subset.  
- **Interpretability:** Keeps variable identity (vs PCA/PLS latent features).  
- **Improved stability:** Reweighted coefficients → less bias & better handling of correlated variables.  

 Compared to black-box ML models, **ALASSO provides interpretable engineering insights**, essential for process improvement.  

---

### Case Study: Paper Coating Defects (2023)

After a series of data preprocessing, statistical testing (e.g., t-test), and applying the IYM framework with the **Key-Variable Search Algorithm (KSA)**, we were able to pinpoint suspicious variables that differentiate **Normal** from **Abnormal** coating results.

The figure below illustrates one sample case.  
- **Blue curve (Normal data)** shows the expected process trend over time.  
- **Red curve (Abnormal data)** highlights the detected anomaly.  
- The abnormal segment deviates significantly from the normal trend, confirming the **root-cause signal** that contributes to coating defects.  

<p align="center">
<img width="720" alt="case_abnormal_detection" src="https://github.com/user-attachments/assets/your_image_id_here" />
</p>
<p align="center">Fig. X. Example of Normal vs. Abnormal data after IYM + KSA analysis. The abnormal segment (red) is clearly separated and identified as problematic.</p>

This demonstrates that the IYM + KSA approach not only narrows down the **critical sensors/variables** but also provides **visual evidence** of where abnormalities occur, enabling engineers to take corrective actions in production.


##  Contributions  

- Built an **IYM-based yield analysis pipeline**.  
- Developed a **two-phase KSA workflow** (stage → parameter).  
- Introduced **RIK index** for reliability assessment.  
- Successfully applied to **TFT-LCD coating defect analysis**, revealing true process root causes.  

---

##  Before vs After IYM  

| Aspect            | Before IYM                  | After IYM (KSA Applied)        |
|-------------------|-----------------------------|--------------------------------|
| Defect complaints | 5 cases in 2023             | Reduced significantly          |
| Root cause search | Manual, heuristic-based     | Automated via KSA (TPOGA + ALASSO) |
| Interpretability  | Low (black-box / PCA-based) | High (variable-level coefficients) |
| Reliability       | Not quantified              | RIK > 0.7 confidence scoring   |

---

## 📊 Case Study: Coating Defects in Paper Manufacturing (2023)

In this case study, we analyze **coating defects** in the paper manufacturing process.  
Customer complaints showed a surge in coating-related issues in **2023 (5 cases)** compared to only **1 case in 2022**.  
To investigate, we applied **Intelligent Yield Management (IYM)** with both **exploratory data analysis** and **statistical testing**.

---

### 1) Exploratory Data Analysis (EDA)
Production data was divided into **Normal (good)** and **Abnormal (defective)** groups.  
Scatter plots, histograms with KDE, and boxplots were used to visualize differences.

📷 **[Fig.1] Scatter / Histogram / Boxplot of Normal vs Abnormal Data**

- Scatter plot: Values overlap heavily but some deviations are visible.  
- Histogram: Abnormal data shows tighter clustering / shifts in distribution.  
- Boxplot: Abnormal group median differs noticeably from Normal.  

---

### 2) Statistical Testing (t-test)
To validate observed differences, we applied **Student’s t-test** to each parameter.

📋 **[Table 1] t-test results (parameters anonymized as XXX, YYY)**

| col_name | statistic | p_value   | Significant (p<0.05) |
|----------|-----------|-----------|-----------------------|
| **XXX**  | 348.20    | 1.28E-77  | ✅ Yes |
| **YYY**  | 2.02      | 0.154     | ❌ No  |

**Interpretation**:  
- **XXX** shows a highly significant difference (p ≈ 1.28E-77), strongly linked to coating defects.  
- **YYY** shows no significant difference and can be excluded from further investigation.  

---

### 3) Findings
- Pure visual inspection can be misleading — statistical validation ensures more reliable results.  
- **XXX** is a strong candidate parameter for further engineering validation (e.g., coating pressure, viscosity, bake curve).  
- **YYY** can be deprioritized, saving time and analysis resources.  

---
