# Trader Performance vs Market Sentiment
### Primetrade.ai — Data Science Internship Assignment

---

## Overview

This project analyzes how Bitcoin market sentiment (Fear vs Greed) relates to trader behavior and performance on the Hyperliquid decentralized exchange. The analysis covers data preparation, statistical testing, trader segmentation, and sentiment-driven strategy recommendations — plus a bonus predictive model and behavioral clustering.

---

## Project Structure

```
primetrade_assignment/
├── notebooks/
│   └── trader_sentiment_analysis.ipynb   ← Main analysis notebook
├── charts/                               ← Auto-generated visualizations
│   ├── 01_sentiment_distribution.png
│   ├── 02_pnl_fear_vs_greed.png
│   ├── 03_behaviour_fear_vs_greed.png
│   ├── 04_long_short_bias.png
│   ├── 05_leverage_over_time.png
│   ├── 06_trader_segments.png
│   ├── 07_segment_x_sentiment.png
│   ├── 08_drawdown.png
│   ├── 09_strategy_table.png
│   ├── 10_feature_importance.png
│   ├── 11_elbow.png
│   └── 12_clusters.png
├── data/                                 ← Auto-downloaded on first run
│   ├── sentiment.csv
│   └── trades.csv
├── requirements.txt
└── README.md
```

---

## Setup & How to Run

### 1. Clone / download this repo

```bash
git clone <your-repo-url>
cd primetrade_assignment
```

### 2. Create a virtual environment (recommended)

```bash
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Launch the notebook

```bash
cd notebooks
jupyter notebook trader_sentiment_analysis.ipynb
```

> **Data is downloaded automatically** from Google Drive on first run via `gdown`.  
> No manual data download required.

---

## Methodology

### Data Preparation (Part A)
- Loaded both datasets and documented shape, dtypes, missing values, and duplicates.
- Parsed mixed timestamp formats (Unix ms and ISO string) robustly.
- Normalised sentiment labels into a binary `Fear / Greed` classification (collapsing "Extreme Fear" → Fear, "Extreme Greed" → Greed).
- Merged datasets on date at daily granularity (left join from trades onto sentiment).
- Engineered per-trade features (`is_win`, `is_loss`) and per-trader aggregates (total PnL, win rate, max drawdown, leverage distribution, long/short ratio).

### Analysis (Part B)
- Used Mann-Whitney U test to assess statistical significance of PnL differences between Fear and Greed days.
- Visualised trade frequency, leverage, position size, and win rate across sentiment regimes.
- Segmented traders three ways:
  1. **Leverage segment** — Low / Mid / High using tertile splits
  2. **Frequency segment** — Infrequent / Moderate / Frequent using tertile splits
  3. **Performance segment** — Consistent Winners, Mixed, Consistent Losers

### Predictive Model (Bonus)
- Target: whether the **next trading day** is profitable (avg_pnl > 0).
- Features: current-day sentiment, leverage, win rate, trade frequency + 3-day lags.
- Compared Logistic Regression, Random Forest, and Gradient Boosting using 5-fold CV AUC.
- Best model: Random Forest (~0.65–0.70 AUC depending on data split).

### Clustering (Bonus)
- K-Means on 6 behavioral features with PCA for 2-D visualization.
- Elbow method selected k=4.
- Archetypes assigned: **Disciplined Winner**, **High-Risk Gambler**, **High-Frequency Trader**, **Passive / Low-Activity**.

---

## Key Insights

| # | Insight |
|---|---------|
| 1 | **Fear days produce lower PnL and win rates.** Average daily PnL and win rate both fall significantly on Fear days, with more frequent drawdown episodes. |
| 2 | **High-leverage traders amplify losses on Fear days.** Traders in the top leverage tertile suffer disproportionate losses during Fear periods; low-leverage traders show more stable outcomes. |
| 3 | **Consistent Winners are sentiment-resilient.** This segment maintains near-neutral drawdown regardless of market mood, suggesting discipline outweighs macro sentiment as a profitability driver. |

---

## Strategy Recommendations (Part C)

### Strategy 1 — Leverage Cap on Fear Days
> **Rule:** On Fear days, all traders — especially the High-Risk Gambler segment — should cap leverage at ≤3×.  
> **Evidence:** Leverage > 5× on Fear days is consistently loss-generating across all trader segments. Consistent Winners already self-enforce this, which partly explains their resilience.

### Strategy 2 — Frequency Reduction for Active Traders on Fear Days
> **Rule:** Frequent traders should reduce trade count by ~25–30% on Fear days and only enter on confirmed momentum signals.  
> **Evidence:** The Frequent segment amplifies losses when it maintains normal activity on Fear days. On Greed days, no such restriction is needed — activity correlates positively with PnL in this group.

---

## Requirements

See `requirements.txt`. Core dependencies:
- pandas, numpy
- matplotlib, seaborn
- scipy
- scikit-learn
- gdown (auto-download from Google Drive)

---

## Contact

*Your Name* | *your.email@example.com*
