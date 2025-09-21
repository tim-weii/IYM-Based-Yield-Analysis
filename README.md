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

---

### From STFT to IYM  

While STFT alone already provides valuable insights,  
the ultimate goal of this work is to **integrate it into the Intelligent Yield Management (IYM) framework**.  

In the original IYM design, the **Key-Variable Search Algorithm (KSA)** serves as the core module for identifying root-cause variables.  
In my approach, this module is replaced with the **STFT-based feature extraction and anomaly detection mechanism**,  
allowing IYM to directly capture subtle, time–frequency anomalies and link them to yield loss.  

This integration ensures that the proposed method is not only a standalone analysis,  
but also a **functional component within the IYM framework**, enabling end-to-end yield management.

---

### What IYM Does  

Traditional yield analysis mainly relies on **historical pattern matching**.  
However, under **high-dimensional conditions (p ≫ n)** — where process parameters far exceed available samples — such methods often fail to identify the **true root causes**.  

To overcome this, I adopted **Intelligent Yield Management (IYM)**, originally built on the **Key-Variable Search Algorithm (KSA)**,  
but in this work, the key search module was **replaced with STFT-based features**, allowing the system to directly capture subtle, time–frequency anomalies in process data.  

<p align="center" style="background-color:white; display:inline-block; padding:10px;">
  <img width="1633" height="703" alt="iym_workflow" src="https://github.com/user-attachments/assets/3bb9d97e-9c58-4a52-bd06-f73afb41bbb8" />
</p>

**Figure 10.** Workflow of IYM — applying STFT-based features to identify critical parameters for yield loss analysis.  

---

### IYM in the iFA System  

<p align="center" style="background-color:white; display:inline-block; padding:10px;">
  <img width="800" alt="iym_framework" src="https://github.com/user-attachments/assets/5eeda933-af05-4bbe-8982-5973fcff7816" />
</p>

**Figure 9.** The position of **Intelligent Yield Management (IYM)** within the **iFA system**.  
IYM serves as a data-driven module that integrates with production information flows to focus on yield-related analysis.  

Within the iFA framework, all modules are **centrally managed and coordinated**.  
This means that once IYM identifies potential yield issues, the system can automatically trigger corresponding responses —  
such as alerting operators, adjusting process parameters, or initiating further root-cause analysis — ensuring that abnormalities are not only detected but also systematically addressed.

---

## Methodology (STFT-enhanced IYM Framework)  

1. **Data Preprocessing**  
2. **STFT-based Feature Extraction**  
3. **Reliance Index (RIK)**  

---

With STFT integrated into the IYM framework,  
we are able to **pinpoint the true root-cause variables** behind yield loss,  
providing interpretable insights for process engineers and enabling data-driven corrective actions.  
