#  Intelligent Yield Management (IYM) — Key-Variable Search for Yield Loss Analysis  

##  Background  
In advanced manufacturing, **yield directly impacts cost and competitiveness**.  
Customer complaint records showed a **surge of coating defects in 2023** (5 cases) compared to only 1 case in 2022.  

Traditional yield analysis relies on historical pattern matching, but under **high-dimensional conditions (p ≫ n)** — where process variables far outnumber available samples — these methods often fail to pinpoint the **true root causes**.  

To address this, we adopt **Intelligent Yield Management (IYM)** with the **Key-Variable Search Algorithm (KSA)**.  

---

##  Methodology (KSA Framework)  

The **KSA scheme** combines greedy search and sparse regression to identify critical process variables:  

1. **Data Preprocessing**  
   - Align production routes (XR), process parameters (XP), inline inspection data (y), and final yield (Y).  
   - Normalize, clean, and organize batch/tool-level information.  

2. **Key-Variable Search (KSA Module)**  
   - **TPOGA (Triple-Phase Orthogonal Greedy Algorithm):** Fast variable selection to find correlated factors.  
   - **ALASSO (Automated LASSO):** Adaptive sparse regression that selects root-cause parameters.  

3. **Reliance Index (RIK)**  
   - Measures overlap between TPOGA and ALASSO results.  
   - RIK > 0.7 = high confidence in identified parameters.  

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

##  Case Study: TFT-LCD Coating Defects (2023)  

- **Data size:** 789 process variables, 28 samples (typical p ≫ n problem).  
- **Phase I Result:** Identified a specific coating chamber as suspicious.  
- **Phase II Result:** Highlighted *Control Voltage* as a root-cause parameter.  
- **RIK = 0.932 (>0.7):** Strong reliability score.  

 **Outcome:** Provided actionable ranges (e.g., speed 1.15–1.25 m/s, viscosity 25–28 s), reducing coating defects.  

---

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

##  Repository Layout  

```text
.
├─ data/                  # Complaint records, inline inspection, process parameters
├─ src/
│  ├─ preprocess.py       # Data preprocessing (normalize, route alignment)
│  ├─ tpoga.py            # TPOGA search module
│  ├─ alasso.py           # Adaptive LASSO implementation
│  ├─ ksa.py              # Key-variable search (two-phase integration)
│  ├─ rik.py              # Reliance Index calculation
│  └─ analysis.ipynb      # Example notebook for coating defect analysis
└─ README.md
```
