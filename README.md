# 🚗 Electric Vehicle Population Analysis & Adoption Dynamics

An empirical exploratory data analysis (EDA) and visualization pipeline evaluating over 200,000 active electric vehicle (EV) registrations across Washington State. 

This repository contains the complete end-to-end data analytics lifecycle: from ingestion, schema sanitation, and feature engineering to static statistical distributions (Matplotlib/Seaborn), interactive multi-chart dashboards (Plotly), and policy-oriented recommendations.

---

## 📌 Project Overview & Objectives

Understanding electric vehicle adoption requires moving past macro sales figures to evaluate real-world fleet compositions on the road. By analyzing official Department of Licensing (DOL) title records, this project investigates:

1. **Market Concentration vs OEM Diversity:** Measuring whether EV adoption is distributed across competitive automakers or monopolized by early market leaders.
2. **Powertrain Utility:** Quantifying the operational differences and range trajectories between pure Battery Electric Vehicles (BEVs) and Plug-in Hybrid Electric Vehicles (PHEVs).
3. **Geographic Adoption Corridors:** Pinpointing county-level adoption clusters across urban hubs and underserved rural regions.
4. **Data Hygiene & Survivor Bias:** Methodologically isolating active registry survivorship dynamics and reporting artifacts (such as zero-coded ranges).

---

## 📊 Dataset Summary

* **Source:** Washington State Department of Licensing (DOL) via [Data.wa.gov](https://data.wa.gov)
* **Scale:** 200,000+ active titled vehicle registration rows
* **Features:** 17 raw columns standardized to 18 operational fields post-feature engineering
* **Key Attributes:**
  * `vin_1_10`: First 10 digits of the Vehicle Identification Number (unique record tracking)
  * `county`, `city`, `postal_code`: Geographic residency of the titled owner
  * `model_year`: Vehicle manufacturing model year
  * `make`, `model`: Automaker and specific vehicle nameplate
  * `electric_vehicle_type`: Powertrain classification (BEV vs. PHEV)
  * `electric_range`: Certified battery range in miles
  * `cafv_type`: Clean Alternative Fuel Vehicle eligibility status

---

## 🛠️ Tech Stack & Libraries

* **Core Runtime:** Python 3.10+ (Executed in Google Colaboratory)
* **Data Manipulation & Cleaning:** Pandas, NumPy
* **Static Statistical Visualizations:** Matplotlib (`pyplot`), Seaborn
* **Interactive Visualizations & Dashboard:** Plotly Express, Plotly Graph Objects (`make_subplots`)

---

## ⚙️ Data Preprocessing & Pipeline Architecture

To maintain reproducible execution and prevent downstream runtime or `KeyError` exceptions, preprocessing steps were structured sequentially:

1. **Schema Standardization:** Strip whitespace, remove hyphens, and convert mixed-case headers to lowercase `snake_case` (e.g., `model_year`, `electric_range`).
2. **Duplicate Verification:** Validated unique row-level persistence using `df.duplicated().sum()`.
3. **Missing Value Treatment:**
   * Dropped coordinate-deficient geographical records (`county`, `city`, `postal_code`) via `dropna()` to avoid spatial aggregation distortions.
   * Filled unrated numeric metrics (`electric_range`, `legislative_district`) with `0` using `fillna(0)` to prevent calculation failures.
4. **Data Type Casting:** Explicitly cast `model_year`, `postal_code`, and `electric_range` to `int64`.
5. **Anomaly Handling:** Filtered out corrupted registration years ($< 1990$ or $> 2026$) and negative range readings.
6. **Pre-EDA Feature Engineering:**
   * `vehicle_age`: Calculated as `2026 - model_year` to track active fleet lifecycle.
   * `ev_type_short`: Simplified lengthy category names into binary tags (`BEV` and `PHEV`) for legible legends and groupings.

---

## 📈 Visualizations & Analytical Highlights

The visual suite combines static statistical rigor with dynamic exploration:

### 1. Core Visualizations (Matplotlib & Seaborn)
* **Top 10 Manufacturers (Seaborn Bar Plot):** Tesla commands the market with over 122,000 registered vehicles, followed by a second tier consisting of Chevrolet, Ford, and Nissan.
* **Model Year Registrations (Seaborn Line Plot):** Documents surviving active registrations rather than chronological annual sales velocity, peaking sharply between 2020 and 2024.
* **Model Year Distribution (Matplotlib Histogram):** Confirms an overwhelmingly modern fleet profile with pre-2017 models representing a minimal share.
* **Electric Range Spread (Seaborn Box Plot):** Highlights that BEVs exhibit a wide spread reaching 330+ miles, whereas PHEVs cluster tightly between 20 and 40 miles.
* **Range vs. Model Year (Seaborn Scatter Plot):** Illustrates generational battery density improvements in pure electric vehicles while PHEVs remain flat.
* **Fleet Split (Matplotlib Pie Chart):** Establishes an 80.6% to 19.4% fleet proportion favoring pure BEVs over PHEVs.
* **Feature Correlation (Seaborn Heatmap):** Demonstrates a moderate negative correlation ($r = -0.55$) between `model_year` and `electric_range` driven by recent zero-coded regulatory records. Excluded `vehicle_age` to prevent deterministic collinearity ($r = -1.00$).

### 2. Interactive Analysis & Dashboard (Plotly)
* **Granular Deep Dives:** Interactive horizontal bar charts for Top 10 Models (anchored by Model Y and Model 3) and Top 10 Counties.
* **Powertrain Range Trajectory:** Multi-line trend demonstrating divergence in non-zero range developments over successive model years.
* **Executive 6-Panel Dashboard:** A consolidated Plotly subplot dashboard uniting OEM share, powertrain distribution, model rankings, geographic clusters, and range dynamics into a single responsive view.

---

## 💡 Key Findings (Step 5 Summary)

* **80.6% Pure BEV Preference:** Consumers show decisive commitment to zero-emission driving rather than transitional hybrid platforms.
* **Extreme Geographic Clustering:** Nearly half of all registered EVs are centralized within **King County** (Seattle metro), with Snohomish and Pierce counties forming the secondary commuter cluster.
* **Data Nuance Identification:** Apparent decreases in newer model ranges reflect administrative reporting changes and unrated exemption filings rather than physical battery degradation.

---

## 🚀 Strategic Recommendations

1. **Targeted Rural Subsidies:** Introduce state purchase incentives and public charging installation grants focused outside the Puget Sound corridor to bridge urban-rural adoption disparities.
2. **Interstate Fast-Charging Corridors:** Prioritize high-power DC fast-charging hubs along cross-state arteries to expand EV utility beyond local commuting.
3. **Direct EPA Data Integration:** Link licensing pipelines directly with certified EPA vehicle databases to eliminate unrated range artifacts in active registration registries.

---

## 📂 Repository Structure

```text
├── data/
│   └── Electric_Vehicle_Population_Data.csv   # Raw DOL registration dataset
├── notebooks/
│   └── EV_Population_Analysis.ipynb           # Executable Google Colab / Jupyter notebook
├── reports/
│   ├── EV_Population_Analysis_Full_Report.docx # Comprehensive 15-section project report
│   └── Presentation_Deck.pptx                 # 12-slide executive presentation deck
├── README.md                                  # Project overview and documentation
└── requirements.txt                           # Python dependencies
```

---

## ⚡ Quickstart

1. **Clone the repository:**
   ```bash
   git clone https://github.com/<your-username>/electric-vehicle-population-analysis.git
   cd electric-vehicle-population-analysis
   ```

2. **Install required packages:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Launch the analysis:**
   ```bash
   jupyter notebook notebooks/EV_Population_Analysis.ipynb
   ```

---

## 📜 References & Acknowledgments
* **Dataset:** Washington State Department of Licensing ([data.wa.gov](https://data.wa.gov))
* **Libraries:** Python Software Foundation, Pandas, NumPy, Matplotlib, Seaborn, and Plotly.
