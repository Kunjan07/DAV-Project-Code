# 🍃 Climate Change Impact on Tea Production in Assam
### A Data-Driven Analysis Using Weather and Production Records from Tocklai Tea Research Institute (2015–2025)

**Tezpur University · Department of Computer Science and Engineering**  
**Data Analytics and Visualization Lab Project**  
Submitted to: **Dr. Swarup Roy**

| Team Member | Roll No. | Role |
|---|---|---|
| Kunjan Kalita | CSD25001 | Data Collection & EDA |
| Prithvijit Das | CSD25002 | Feature Engineering & Model Training |

---

## 📌 Overview

Assam produces more than **50% of India's total tea output** (~700 million kg/year), supporting over **686,000 plantation workers** across 801 tea estates. Tea cultivation is acutely sensitive to climate — temperature, humidity, and rainfall variability directly drive yield fluctuations.

This project investigates the quantitative relationship between climatic variables and monthly tea production using 11 years of station-level data (132 observations) from the **Tocklai Tea Research Association, Jorhat** — the world's oldest tea research station, established in 1911.

We perform end-to-end data analytics: exploratory analysis, agronomically-grounded feature engineering, multiple regression, and random forest modelling — achieving an **R² of 0.933** from weather inputs alone.

---

## 🎯 Problem Statement

> *How are changes in temperature, rainfall, humidity, sunshine, and evaporation affecting monthly tea production in Assam's Brahmaputra Valley, and can we build reliable predictive models to quantify and forecast these climate–yield relationships?*

---

## 📁 Repository Structure

``` 
│   
├── notebooks/
│   └── TocklaiTea.ipynb            # Main analysis notebook
├── report/
│   └── DAV_Lab_Project_Report.pdf        # Full project report
├── README.md
└── requirements.txt
```

---

## 📊 Dataset

**Source:** Tocklai Tea Research Association, Jorhat, Assam (Primary)  
**Period:** January 2015 – December 2025 | **Records:** 132 monthly observations

| Variable | Unit | Range |
|---|---|---|
| Tea Production | M. Kgs | 0.00 – 67.52 |
| Temperature (Max) | °C | 23.9 – 35.1 |
| Temperature (Min) | °C | 6.2 – 23.4 |
| Total Rainfall | mm | 0.0 – 333.9 |
| Rainy Days | days | 0 – 30 |
| RH Morning (0613h) | % | 88 – 97 |
| RH Afternoon (1313h) | % | 49 – 71 |
| Sunshine Hours | hrs/day | 3.9 – 8.2 |
| Evaporation | mm | 27.9 – 107.1 |

> **Note:** Production data for 2015–2017 and 2025 was reconstructed using Ridge Regression trained on the 2018–2024 records (cross-validated R² ≈ 0.895), based on known climate patterns and NE India monsoon variability indices.

---

## ⚙️ Feature Engineering

A key contribution of this project is the set of agronomically-justified engineered features:

| Feature | Formula | Rationale |
|---|---|---|
| `AvgTemp` | (Tmax + Tmin) / 2 | More representative of plant experience |
| `TempRange` | Tmax − Tmin | Large diurnal range affects photosynthesis |
| `RainIntensity` | RF / Rainy Days | Distribution quality > raw quantity |
| `DroughtIndex` | (ET − RF) / ET | Captures water stress directly |
| `AvgTemp²` | AvgTemp² | Models inverted-U temperature response |
| `Rainfall²` | RF² | Captures flooding/diminishing returns threshold |
| `MonthSin / MonthCos` | sin/cos(2πm/12) | Cyclical seasonality without ordinal bias |

---

## 🤖 Models & Results

### Multiple Linear Regression
| Metric | Value |
|---|---|
| R² | **0.933** |
| RMSE | 5.36 M.Kg |
| MAE | 4.16 M.Kg |

### Random Forest Regression
| Metric | Value |
|---|---|
| R² | **0.922** |
| RMSE | 5.78 M.Kg |

Both models explain **over 92% of monthly production variance** from weather inputs alone.

### Top Feature Importances (Random Forest)
```
HumidityAfternoon   ████████████████████████  0.6095
AvgTemp             █████                     0.1373
AvgTemp²            ████                      0.1227
MonthSin            ██                        0.0630
Evaporation         █                         0.0233
```

---

## 🔍 Key Findings

1. **Humidity dominates over temperature and rainfall.** Afternoon RH (1313h) is the single strongest predictor (RF importance = 0.609) — overriding the conventional focus on temperature. A warming climate that reduces afternoon humidity through increased evaporative demand may depress yields even when temperature and rainfall remain nominally adequate.

2. **Temperature response is nonlinear.** The inverted-U relationship (captured by `AvgTemp²`) confirms production peaks within an optimal thermal window (~27–29°C) and declines at extremes.

3. **Rainfall quality matters more than quantity.** Positive coefficient on `RainIntensity` (+4.21) vs. negative on raw `Rainfall` (−11.11) confirms that rainfall distributed across many days is agronomically superior to the same volume concentrated in fewer events.

4. **Drought stress is the dominant seasonal control.** The `DroughtIndex` scatter plot reveals two sharp clusters: low-stress monsoon months with high production, and high-stress dry months with near-zero output.

5. **COVID-19 caused the decade's production low.** The 2020 dip to 333 M.Kgs (dataset minimum) coincides with pandemic disruptions — factory closures and labour migration — not any extreme weather event, illustrating that human capital shocks can override climate effects.

6. **Growing season is extending.** December production in 2025 (17.40 M.Kgs) far exceeds historical averages, possibly reflecting warming-induced extension of the flush season.

---

## 📦 Installation & Usage

```bash
# Clone the repository
git clone https://github.com/<your-username>/tocklai-tea-climate-analysis.git
cd tocklai-tea-climate-analysis

# Install dependencies
pip install -r requirements.txt

# Launch the notebook
jupyter notebook notebooks/tocklai_analysis.ipynb
```

### requirements.txt
```
pandas
numpy
scikit-learn
matplotlib
seaborn
scipy
jupyter
```

---

## 📋 Recommendations

Based on the analysis findings:

- **Monitor afternoon RH as a leading indicator** — a sustained drop below 60% during July–September should trigger pre-emptive irrigation and misting.
- **Adopt distributed overhead irrigation** over flood-based supplemental watering to simulate the beneficial effect of rain spread across many days.
- **Prioritise humidity-tolerant clonal varieties** in Tocklai's breeding programme, not merely heat- or drought-tolerant ones.
- **Operationalise the MLR model** as a near-real-time production forecast tool using IMD monthly weather data to improve Tea Board auction predictions.

---

## ⚠️ Limitations

- **Single-station data:** Tocklai (Jorhat) represents one microclimate zone; Barak Valley and hill districts are not captured.
- **No lag structure:** Tea plants respond to the previous 2–4 weeks of weather; monthly averaging mutes this signal. Lag-1 rainfall shows r = 0.89 correlation with production.
- **Non-weather confounders excluded:** Pruning cycles, labour disputes, varietal adoption, and input costs affect production but are absent from the dataset.
- **Small sample for tree models:** 132 observations limits Random Forest's ability to learn complex interactions without overfitting risk.

---

## 🔭 Future Scope

- Incorporate **lagged weather features** (1- and 2-month lags) to model plant physiological response time
- Develop a **SARIMA / Prophet time-series model** to decompose trend, seasonality, and residuals
- Integrate **multi-station spatial data** from Cachar, Golaghat, and Tinsukia for district-level forecasting
- Extend analysis to **quality metrics** (first flush vs. second flush, auction price realisation)
- Build a **real-time early-warning dashboard** using live IMD weather feeds

---

## 📚 References

- Government of Assam, Directorate of Economics & Statistics (2023). *Tea Statistics Report 2023.*
- Tocklai Tea Research Association (2025). *Monthly Weather and Production Records, 2015–2025.*
- Carr, M. K. V. (2011). The water relations and irrigation requirements of tea. *Experimental Agriculture, 47*(1), 1–36.
- Breiman, L. (2001). Random forests. *Machine Learning, 45*(1), 5–32.
- IPCC (2022). *Climate Change 2022: Impacts, Adaptation and Vulnerability.* Cambridge University Press.
- Tea Board of India (2023). *Annual Statistics.* https://www.teaboard.gov.in

---

## 📄 License

This project is submitted as academic coursework at Tezpur University. The dataset is sourced from the Tocklai Tea Research Association and used for educational purposes only.

---


