# Fnancial-Asset-Portfolio-Optimization-and-Forecast

> A weekly, factor-driven stock-picking workflow that forecasts returns, then builds a 25-stock portfolio via linear optimization to maximize excess return while controlling risk.

---

## 🥅 Goal (TL;DR)

Build a weekly investment strategy that **beats the market on a risk-adjusted basis**.  
We forecast each stock’s expected return using the **Fama–French 3-Factor model (FF3)**, compute **alpha** (excess return vs. risk exposure), then solve a **linear optimization** to pick **exactly 25 stocks** each week under practical constraints (sector caps, risk limits, minimum weights).

---

## 📊 Data

- **Universe:** S&P 500 constituents (prices **2021–2023**), plus market series.  
- **Weekly features:** returns, volatility, **5% VaR**, downside deviation, and idiosyncratic variance derived from FF3 exposure.  
- **Training window:** **2017–2019** for factor betas (to avoid COVID distortion); **testing:** **2021–2023**.

---

## 🔮 Prediction Model (FF3)

For each stock, we estimate factor **betas** to **Market**, **SMB (size)**, and **HML (value)** on 2017–2019 data, then roll them forward to predict **weekly expected returns** in 2021–2023.  
**Alpha = Realized return – Expected return** (signal to maximize). In-sample adjusted R² averages ~30% (typical for equities); out-of-sample performance remains meaningful across names.

---

## 🧮 Optimization Model

**Objective:** maximize the **sum of (weight × alpha)** for that week.

**Key constraints**
- Exactly **25 stocks**; each weight **≥ 1%**.  
- **Sector weight ≤ 20%** (diversification).  
- Portfolio **VaR ≥ mean(VaR)**, **Downside Deviation ≤ mean(DD)**, and expected **CAPM volatility ≤ mean(vol)** (tunable risk).

Risk is approximated by **CAPM exposure + idiosyncratic variance** to avoid full covariance matrices.

---

## ✅ Results (2021–2023 backtest)

- **Total return:** **19.83%** vs. market **14.75%**.  
- **2022 resilience:** portfolio **−2.95%** vs. market **−20.36%**.  
- **Volatility:** ~**18.42%**; **Sharpe ~0.34**, exceeding market on return per unit risk.  
- Sector and CAPM-volatility constraints are typically binding; relaxing them increases return *and* risk (as expected).

**Limitations:** ignores trading frictions/fees, assumes fractional shares & free market exposure, and uses **FF3 only** (no momentum/liquidity/news features).

---

## 🗂️ Repository Structure

├─ Data Forecast/  
│ └─ Weekly416DataTest (2).csv → Cleaned weekly features (universe + risk metrics)  
├─ Jupyter Notebook (code) Forecast/  
│ └─ Weekly-Linear-MGSC416-Group11-FinalCode (1).ipynb  
├─ Deliverable Forecast/  
│ ├─ Final Report Forecast.pdf → Full write-up (methods, results)  
│ └─ Presentation Deck Forecast.pdf → Slides (overview & visuals)  
├─ LICENSE  
└─ README.md  
- **Slides** summarize the story & outcomes at a glance.  
- **Final report** documents data engineering, modeling, constraints, and detailed findings.

---

## 🚀 Quickstart (Run the Notebook)

You can use **local Jupyter**, **VS Code**, or **Google Colab**.

1. **Clone or download** this repo.  
2. Open **`Jupyter Notebook (code) Forecast/Weekly-Linear-...ipynb`**.  
3. Ensure the CSV from **`Data Forecast/`** is available at the path expected by the notebook.  
4. Install dependencies (example):
   ```bash
   pip install pandas numpy scipy matplotlib pulp ortools
   ```
5. Run all cells to:

- Fit **FF3 betas** (loads prebuilt values if included, or recompute).  
- Build weekly features (**returns, VaR, DD, volatility**).  
- Solve the weekly LP to get **25-stock portfolios** over 2021–2023.  
- Plot **cumulative returns vs. market** and inspect constraint slacks.  

💡 **Tip:** tighten or loosen the **CAPM-volatility** and **sector constraints** to explore the **risk/return trade-off**.  

---

## 🧩 How the Pieces Fit

- **Prediction:** FF3 betas (2017–2019) → weekly expected returns → **alpha per stock**.  
- **Construction:** weekly LP chooses 25 names maximizing alpha under **risk and diversification rules**.  
- **Evaluation:** out-of-sample (2021–2023) **returns, drawdowns, volatility, Sharpe ratio, and constraint slacks**; visualized in the deck.  

---

## 🔧 Reproduce or Extend

- Swap in **Fama–French 5-Factor** (profitability, investment) or add **momentum features**.  
- Add **transaction costs** and **turnover penalties** to the LP.  
- Replace CAPM-based risk with a **full covariance matrix** (factor or shrinkage estimator).  
- Parameterize constraints for **investor profiles** (e.g., sector bans, ESG tilts).  

---

## 📜 License

**MIT** — see `LICENSE` for details.

---

   


