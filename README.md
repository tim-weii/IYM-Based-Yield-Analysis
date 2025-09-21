#  Intelligent Yield Management (IYM) — Key-Variable Search for Yield Loss Analysis  

## Background  

In advanced manufacturing, **yield is a critical factor that directly impacts both cost and competitiveness**.  
One key challenge in yield analysis is **detecting anomalies in manufacturing data**, which can generally be divided into two categories:  

---

### 1. Point-wise Anomaly  
- A single data point that significantly deviates from its neighbors.  
- Isolated and discontinuous, not following the normal pattern of the dataset.  

<p align="center">
  <img width="500" alt="point_anomaly_example" src="https://github.com/user-attachments/assets/fed0e3a8-69d8-44fa-b006-50f27e2489b7" />
</p>  

**Figure 1.** Case study example of a **point-wise anomaly**.  

---

### 2. Pattern-wise Anomaly  
- A sequence of data points within a time interval that deviates from the expected overall trend.  
- Unlike point anomalies, pattern anomalies reflect abnormal **relationships** or **continuous variations** across multiple data points.  

<p align="center">
  <img width="500" alt="pattern_anomaly_example" src="https://github.com/user-attachments/assets/c32fd472-482d-444a-ad6d-c550e7e0d658" />
</p>  

**Figure 2.** Case study example of a **pattern-wise anomaly**. 

### When Both Occur  
When **point-wise and pattern-wise anomalies occur simultaneously**, and the **data quality is poor**, the situation becomes a true disaster.  
- Spurious noise makes it harder to distinguish real anomalies from false alarms.  
- Overlapping effects of both anomaly types lead to confusing and inconsistent signals.  
- Traditional methods based on simple thresholding or historical pattern matching often fail completely.  

In this case study, however, the focus is primarily on Pattern-wise Anomalies,
as they are more strongly correlated with process variations and yield loss in the papermaking system.

---

**Traditional** yield analysis mainly relies on **historical pattern matching**.  
However, under **high-dimensional conditions (p ≫ n)** — where process parameters far exceed available samples — such methods often fail to identify the **true root causes**.  

To address this, I introduce **Intelligent Yield Management (IYM)** with the **Key-Variable Search Algorithm (KSA)**.  

---

## Case Study  

## Case Study  

In real manufacturing cases, **it is often difficult to visually distinguish process differences**.  


### Initial Attempt — Clustering Methods  
After **preprocessing**, I first attempted **unsupervised clustering methods**, including **K-means** and **DBSCAN**, to automatically separate process variations.  

<p align="center">
<img width="800" height="500" alt="clustering_result" src="https://github.com/user-attachments/assets/23a958b1-4029-4de0-9261-268d34ee0580" />
</p>  

**Figure 3 & 4.** Results of K-means and DBSCAN clustering.  

**Limitations:**  
- **K-means**: Assumes spherical clusters, sensitive to initialization, requires pre-defined k.  
- **DBSCAN**: Sensitive to ε and minPts, struggles with varying density, suffers from the curse of dimensionality.  

As a result, clustering alone was not sufficient to distinguish subtle anomalies.  

---

### Further Analysis — Fourier Transform (FT)  

After exhausting various approaches without satisfactory results,  
I decided to shift the analysis toward the **frequency domain**, starting with the **Fourier Transform (FT)**.  
FT decomposes a signal into different frequency components, allowing us to analyze the **energy distribution across frequencies** rather than only the time-domain waveform.  

<p align="center">
<img width="800" alt="螢幕擷取畫面 2025-09-21 235127" src="https://github.com/user-attachments/assets/31c3ea3d-e665-4cc1-af77-310270b0431a" />
</p>  

**Figure 5.** Fourier Transform result — frequency components of the process data.  

**Limitation:** In this figure, the blue line represents the original signal, while the red line is reconstructed from a selected frequency band after Fourier Transform. Although this highlights the low-frequency trend, in our case FT only provides a global frequency distribution and cannot indicate when anomalies occur. As a result, Fourier Transform alone does not offer meaningful progress for this study.  

---

### Next Step — Short-Time Fourier Transform (STFT)  

To address the limitations of conventional Fourier Transform, I adopted the **Short-Time Fourier Transform (STFT)**.  
STFT applies FT to short, overlapping segments of the signal, producing a **time–frequency representation**.  
This allows us to not only identify which frequency components are present, but also observe how they evolve over time.  

<p align="center">
  <img width="800" alt="stft_example" src="https://github.com/user-attachments/assets/5eda7461-6e4f-4c4d-abbe-df1586aaf1fe" />
</p>  

**Figure 6.** STFT result — time–frequency representation revealing subtle variations.  

---

After testing multiple approaches, I ultimately chose **STFT** because it preserves both **time** and **frequency** information,  
making it possible to track dynamic changes that other methods could not capture.  

**Key advantages of STFT in this case study:**  
- Conventional FT shows only the **global frequency distribution**, without indicating *when* anomalies occur.  
- STFT clearly reveals the **time intervals** where anomalies emerge.  
- This enables precise localization of problem points and supports the detection of subtle **pattern-wise anomalies**.  

---

The following two figures illustrate real cases in the papermaking process where STFT successfully detected actual problem points.  

<p align="center">
  <img width="850" alt="stft_case1" src="https://github.com/user-attachments/assets/36551946-3765-4d79-ae63-a9b3d074f3bd" />
</p>  

**Figure 7.** STFT-based detection of anomaly occurrence in time–frequency domain (Case 1).  

<p align="center">
  <img width="850" alt="stft_case2" src="https://github.com/user-attachments/assets/105fccca-2ec0-474a-a88d-afe058879fbd" />
</p>  

**Figure 8.** STFT-based detection of anomaly occurrence in time–frequency domain (Case 2).  


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
