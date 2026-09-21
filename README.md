# Financial Market Analysis: Predicting Post-Announcement Drift in US Tech Stocks Using Python & Machine Learning

**LSE Data Analytics Career Accelerator — Employer Project |November 2025**

> *Do stock price movements before earnings announcements provide predictive insights into price movements after earnings for Google, Apple and NVIDIA? (2015–2025)*

---

## Executive Summary

This project investigates whether pre-earnings price behaviour and earnings surprise metrics can reliably predict post-announcement stock returns and whether those patterns can be systematically exploited through a rules-based trading strategy.

Analysing **129 quarterly earnings events** across Apple (AAPL), Google (GOOGL), and NVIDIA (NVDA) over a **10-year period (2015–2025)**, the analysis identifies clear, repeatable patterns linking EPS surprise magnitude and pre-event momentum to post-earnings Cumulative Abnormal Returns (CAR). The project culminates in an **evidence-based trading framework** delivered to the client, VP Analytics, a predictive analytics firm serving hedge fund clients.

**Key result:** Large EPS beats (>5% surprise) combined with positive pre-earnings momentum consistently drive material post-announcement returns, most powerfully for NVDA, where 30-day CAR exceeds **11%** on large beats.

---

## Business Problem

The analytics company delivers predictive analytics to hedge fund clients, with a focus on modelling market reactions to corporate earnings announcements. The core business question was:

> **Can pre-earnings price behaviour and earnings surprise data provide actionable signals for post-announcement position sizing?**

Specifically, the project aimed to:
- Determine whether EPS surprise magnitude predicts post-earnings drift
- Assess whether pre-earnings momentum amplifies or moderates those signals
- Evaluate whether company lifecycle stage influences drift magnitude and persistence
- Deliver a practical, rules-based framework the client could act on

---

## Data Sources

| Source | Data Collected | Method |
|--------|---------------|--------|
| **Alpha Vantage API** | Quarterly EPS actuals, estimates, revenue, net income, announcement dates | Python API via `requests` |
| **Yahoo Finance (yfinance)** | Daily stock prices and volume (AAPL, GOOGL, NVDA, S&P 500) | `yf.download()` |
| **FRED (Federal Reserve)** | Macroeconomic indicators — rate decisions, recession periods, inflation | API integration |

The combined dataset covers **129 earnings events** across three companies, with daily pricing data from January 2015 to October 2025. Companies were selected deliberately to represent three distinct lifecycle stages: NVDA (explosive growth), GOOGL (balanced growth), AAPL (mature/stable).

---

## Tools & Skills Used

| Category | Tools / Libraries |
|----------|------------------|
| **Language** | Python 3 |
| **Data Processing** | pandas, NumPy |
| **Visualisation** | matplotlib, seaborn |
| **Machine Learning** | scikit-learn (Random Forest Regressor) |
| **API Integration** | Alpha Vantage API, yfinance, FRED API |
| **NLP** | NLTK VADER sentiment analysis |
| **Environment** | Jupyter Notebooks (modular 4-notebook structure) |
| **Version Control** | GitHub |

**Skills demonstrated:** End-to-end Python pipeline development · Multi-source API data integration · Feature engineering · Event study methodology · Machine learning (Random Forest) · NLP sentiment analysis · Data quality management · Stakeholder presentation

---

## Analytical Approach

The project followed a structured four-stage pipeline:

### 1. Data Collection & Cleaning
- Retrieved earnings data via Alpha Vantage API with defensive error handling and rate-limit management (12-second delays between requests)
- Forward-filled missing stock prices to account for market closures
- Removed duplicate earnings entries for pre-market reporters
- Aligned timezones and standardised "day 0" to the first trading day after announcement
- Handled the S&P 500 benchmark for abnormal return calculation

### 2. Feature Engineering
Each earnings event was aligned to its first post-announcement trading day. Engineered features included:

| Feature | Description |
|---------|-------------|
| **EPS Surprise (%)** | Percentage deviation between actual and consensus EPS |
| **EPS Beat/Miss Indicator** | Binary positive/negative classification |
| **20-Day Pre-Event Momentum** | % return over the 20 trading days prior to announcement |
| **Volatility** | 20-day rolling standard deviation of daily returns |
| **CAR (5/10/20/30-day)** | Cumulative Abnormal Return relative to S&P 500 across four windows |
| **Volume Ratio** | Trading volume relative to 20-day average |

### 3. Exploratory & Statistical Analysis
- Scatter plots, correlation matrices, and CAR event charts across beat/miss categories
- Large vs small beat threshold identified at **5% EPS surprise** — a structural breakpoint where post-earnings behaviour diverges significantly
- Pre-earnings momentum quartile analysis across all three tickers
- Stock lifecycle analysis comparing NVDA's post-peak trajectory against AAPL and GOOGL historical patterns
- NLP sentiment analysis (NLTK VADER) applied to company earnings calls — finding weak correlation (0.05–0.15) with 30-day returns, confirming EPS surprise as the dominant signal

### 4. Predictive Modelling
- **Model:** Random Forest Regressor (scikit-learn)
- **Target:** 30-day CAR post-earnings
- **Split:** 70/30 time-based train/test split (to prevent information leakage)
- **Hyperparameters:** 100 estimators, max depth 5, random state 42

| Ticker | R² Score | Interpretation |
|--------|----------|----------------|
| NVDA | 0.21 | Weak positive — directional tendency captured |
| AAPL | -1.08 | Model underperforms baseline — high noise |
| GOOGL | -1.27 | Model underperforms baseline — high noise |

Feature importance analysis confirmed **EPS surprise** and **pre-event CAR** as dominant predictors. The mixed R² results reflect the inherent noise in short-term financial data — the exploratory analysis and rules-based framework proved more actionable than the regression model alone.

---

## Key Findings & Business Recommendations

### Finding 1: EPS Surprise Magnitude is the Primary Driver
Large beats (>5% EPS surprise) consistently produce positive post-earnings CAR across all three companies. Small beats are inconsistent. Misses reliably produce negative or flat returns.

| CAR Window | AAPL Large Beat | GOOGL Large Beat | NVDA Large Beat |
|------------|----------------|-----------------|----------------|
| 0–5 days | +2.6% | +1.3% | +5.0% |
| 0–10 days | +2.3% | +1.3% | +5.7% |
| 0–20 days | +1.9% | +2.3% | +6.7% |
| 0–30 days | +1.5% | +2.8% | **+11.0%** |

### Finding 2: Pre-Earnings Momentum Amplifies Returns
High pre-earnings momentum (Q4) produces average 30-day CAR of **+5.51%** vs **-1.06%** in the lowest momentum group. Momentum is a material amplifier of the EPS signal, particularly for NVDA.

| Momentum Group | Avg 30-Day CAR |
|---------------|---------------|
| Q1 (Low) | -1.06% |
| Q2 | +4.02% |
| Q3 | +4.58% |
| Q4 (High) | +5.51% |

### Finding 3: Company Lifecycle Stage Determines Optimal Hold Period
- **Apple:** React quickly — 5-day window captures most gain; fade thereafter
- **Google:** Patient strategy — drift builds steadily to 20–30 days
- **NVIDIA:** Both — immediate reaction and persistent long drift on large beats

### Actionable Trading Framework

| Scenario | Action | Hold Period | Expected ROI |
|----------|--------|-------------|-------------|
| Large Beat (>5% EPS) | **BUY** | AAPL: ~5 days / GOOGL & NVDA: 20–30 days | AAPL ~3% / GOOGL ~3% / NVDA ~11% |
| Small Beat (0–5% EPS) + momentum > 0 | **BUY** | AAPL: ~10 days / GOOGL & NVDA: ~5 days | ~2–3% |
| Small Beat + negative momentum | **AVOID** | — | Inconsistent |
| Miss (negative EPS surprise) | **DO NOT BUY** | — | Negative/flat across all windows |

---

## Limitations

- **Sample size:** Only three companies — findings may not generalise beyond large-cap US tech
- **10-year window:** Includes unusual macro regimes (COVID, AI boom, rate hike cycle) which may not repeat
- **Transaction costs:** Analysis does not account for trading fees, slippage, or bid-ask spread — real-world returns would be lower
- **Model performance:** Random Forest R² scores are weak or negative for AAPL and GOOGL, suggesting the model captures directional tendency rather than precise magnitude
- **Data quality:** yfinance provides unverified data — results could not be cross-checked against premium sources
- **Pre-market reporters:** Duplicate earnings entries required manual resolution; edge cases may remain
- **NLP scope:** Sentiment analysis applied to AAPL and NVDA only — GOOGL earnings call data was not included

---

## Future Steps

- **Expand the stock universe** to 10–15 tech names (AMD, TSLA, META, MSFT) to test generalisability of the framework
- **Extend time windows** to 60- and 90-day CAR to assess longer drift persistence
- **Macro-regime analysis** - segment results by interest rate environment, VIX levels, and sector rotation cycles to understand when the strategy performs best and worst
- **Back testing framework** - build a formal back test with slippage, transaction costs, and drawdown controls to evaluate real-world strategy performance
- **Improve predictive modelling** - explore LSTM/time-series models and additional features (options implied volatility, analyst revision momentum) to improve R² scores
- **Real-time monitoring dashboard** - develop a live Tableau or Power BI dashboard tracking EPS surprise and momentum signals ahead of upcoming earnings dates
- **Expand NLP scope** - apply sentiment analysis to all three companies and explore whether social media sentiment (Reddit, Twitter/X) adds predictive signal

---

## Notebooks

The project is structured across four Jupyter notebooks:

| Notebook | Purpose |
|----------|---------|
| `Gather_Earnings_Data.ipynb` | Alpha Vantage API data collection — outputs CSV files |
| `Single_Ticker_AAPL.ipynb` | Single-ticker EDA template — methodology validation |
| `Multi_Ticker_EDA.ipynb` | Full analysis and modelling across all three tickers |
| `Final_Analysis.ipynb` | Visualisations and analysis used in client presentation |

---

## About

This project was delivered as part of the **LSE Data Analytics Career Accelerator (2025, Distinction)** in collaboration with an Analytics company as the employer client. The analysis was presented directly to the analytics team across two presentations.

**Andrew Willacy**
[LinkedIn](https://www.linkedin.com/in/andrew-willacy-572682347/) | [GitHub Portfolio](https://github.com/AndrewWillacy) | andrew.willacy.data@gmail.com

