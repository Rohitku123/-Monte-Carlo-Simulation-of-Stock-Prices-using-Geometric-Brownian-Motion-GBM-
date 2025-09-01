# Monte Carlo Simulation of Stock Prices using Geometric Brownian Motion (GBM)

## Project Overview

This project implements a **Monte Carlo simulation** to model stock price evolution using **Geometric Brownian Motion (GBM)**. The simulation generates thousands of potential future stock price paths and calculates statistical metrics to help with **risk assessment**, **decision-making**, and **investment planning**.  

The default stock in this project is **MSFT (Microsoft)**, but it can be easily configured to simulate any other stock.

---

## Features

- Fetch historical stock prices using Yahoo Finance (`yfinance`)  
- Estimate annualized **drift (μ)** and **volatility (σ)** from historical data  
- Monte Carlo simulation of stock prices using GBM  
- Configurable number of paths and steps  
- Animated visualization of multiple simulated paths  
- Histogram and KDE plots for terminal prices and per-step log returns  
- Statistical metrics including mean, median, standard deviation, percentiles, skewness, kurtosis, and probability metrics  
- Save animation as HTML and optionally MP4  

---

## Mathematical Model and Formulas

### 1. Geometric Brownian Motion (GBM)

The GBM stochastic differential equation models the stock price \(S_t\) as:

$$
dS_t = \mu S_t \, dt + \sigma S_t \, dW_t
$$

Where:

- \(S_t\) = stock price at time \(t\)  
- \(\mu\) = drift (expected annualized return)  
- \(\sigma\) = volatility (annualized standard deviation of returns)  
- \(dW_t\) = Brownian motion increment (\(\sim \mathcal{N}(0, \sqrt{dt})\))  

---

### 2. Logarithmic Returns

Daily log return is calculated as:

$$
R_t = \ln \frac{S_t}{S_{t-1}}
$$

---

### 3. Drift Component

The drift term represents expected growth, adjusted for variance:

$$
\text{drift} = \mu - \frac{1}{2}\sigma^2
$$

---

### 4. Random Component (Shock / Diffusion)

Randomness in price movement is introduced by:

$$
\text{shock} = \sigma \cdot Z, \quad Z \sim \mathcal{N}(0,1)
$$

---

### 5. Discretized GBM (Monte Carlo Step)

The discrete formula to simulate stock price at each step is:

$$
S_{t+\Delta t} = S_t \cdot 
\exp \Big( (\mu - \frac{1}{2}\sigma^2)\Delta t + \sigma \sqrt{\Delta t} Z \Big)
$$

- Applied iteratively over multiple steps to generate paths.

---

### 6. Iterative Simulation for Multiple Steps

$$
S_{t+1} = S_t \cdot \exp(\text{drift} \cdot \Delta t + \sigma \sqrt{\Delta t} Z_{t+1}), \quad t = 0,1,2,\dots,n-1
$$

---

### 7. Statistical Metrics from Simulation

- **Mean Terminal Price**:

$$
\bar{S_T} = \frac{1}{N} \sum_{j=1}^{N} S_T^{(j)}
$$

- **Standard Deviation**:

$$
\text{Std}(S_T) = \sqrt{\frac{1}{N-1} \sum_{j=1}^N 
\big(S_T^{(j)} - \bar{S_T}\big)^2}
$$

- **Percentiles (25th, 50th, 75th)**:

$$
Q_p = \text{quantile at } p\%
$$

- **Probability above initial price**:

$$
P(S_T > S_0) = \frac{ \ \{ S_T^{(j)} > S_0 \} }{N}
$$

- **Per-step log-return mean and standard deviation**:

$$
\mu_{\Delta t} = (\mu - \frac{1}{2}\sigma^2)\Delta t, \quad 
\sigma_{\Delta t} = \sigma \sqrt{\Delta t}
$$

- **Probability of negative return per step**:

$$
P(\Delta r < 0) = 
\frac{ \ \{ \Delta r_i < 0 \} }{N \cdot \text{steps}}
$$

- **Skewness and Kurtosis**:

$$
\text{Skew}(X) = 
\frac{\mathbb{E}[(X-\bar{X})^3]}
{\big(\mathbb{E}[(X-\bar{X})^2]\big)^{3/2}}, \quad
\text{Kurt}(X) = 
\frac{\mathbb{E}[(X-\bar{X})^4]}
{\big(\mathbb{E}[(X-\bar{X})^2]\big)^2} - 3
$$
<img width="1525" height="615" alt="MSFT_Simulation 1" src="https://github.com/user-attachments/assets/071a050d-faef-41b2-98fc-087f04634c40" />

---

## Installation

1. Clone the repository:

```bash
git clone https://github.com/yourusername/monte-carlo-gbm.git
cd monte-carlo-gbm
Create a Python virtual environment:

bash
Copy code
python -m venv venv
source venv/bin/activate  # Linux/Mac
venv\Scripts\activate     # Windows
Install dependencies:

bash
Copy code
pip install -r requirements.txt
Dependencies:

nginx
Copy code
numpy
pandas
matplotlib
scipy
yfinance
(Optional) Install ffmpeg to save MP4 animations.

Usage
Configure simulation parameters in the script or notebook:

TICKER: Stock symbol (default "MSFT")

LOOKBACK_YEARS: Number of years of historical data

N_PATHS: Number of Monte Carlo paths

N_STEPS: Number of simulation steps

SEED: Random seed

Run the script:

bash
Copy code
python gbm_simulation.py
Outputs:

HTML animation of simulated stock paths

Histogram of terminal prices

Histogram of per-step log returns

Console summary of statistics

Results Interpretation
Mean terminal price: expected outcome across all simulated paths

Median: the middle outcome, less affected by extreme paths

Interquartile range (25th–75th percentile): typical spread of outcomes

Probability above/below initial price: likelihood of gain or loss

Daily returns: short-term fluctuations even if the long-term trend is positive

These metrics help in risk assessment, investment planning, and derivative pricing.

Project Structure
bash
Copy code
monte-carlo-gbm/
│
├── gbm_simulation.py        # Main Python script
├── README.md                # Project documentation
├── requirements.txt         # Python dependencies
├── gbm_MSFT_simulation.html # Sample HTML animation
├── data/                    # Optional CSV data
└── notebooks/               # Optional Jupyter notebooks
License
MIT License

References
Hull, John C. Options, Futures, and Other Derivatives, 10th Edition

Glasserman, P. Monte Carlo Methods in Financial Engineering

Yahoo Finance API (yfinance)

Wikipedia: Geometric Brownian Motion

yaml
Copy code

---
