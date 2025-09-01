# Monte Carlo Simulation of Stock Prices using Geometric Brownian Motion (GBM)

## Project Overview

This project implements a **Monte Carlo simulation** to model stock price evolution using the **Geometric Brownian Motion (GBM)** model. It simulates thousands of possible future paths of a stock (default: MSFT) over a given time horizon, calculates key statistical metrics, and visualizes results using animations and histograms.

**Purpose:**

- Assess the range of possible stock prices under uncertainty
- Quantify probabilities of gains and losses
- Visualize potential scenarios for better decision-making
- Provide statistical summaries for investors, analysts, and risk managers

---

## Features

- Fetch historical stock prices using Yahoo Finance (`yfinance`)
- Estimate **annualized drift (μ)** and **volatility (σ)** from historical data
- Monte Carlo simulation with configurable number of paths and steps
- Animated visualization of stock price paths
- Histogram and KDE plots for terminal prices and per-step log returns
- Compute statistical metrics:
  - Mean, median, standard deviation
  - Percentiles (25th, 50th, 75th)
  - Skewness, kurtosis, probability above/below initial price
- Save animation as HTML and optionally MP4

---

## Mathematical Model and Formulas

### 1. Geometric Brownian Motion (GBM)

\[
dS_t = \mu S_t \, dt + \sigma S_t \, dW_t
\]

Where:

- \( S_t \) = stock price at time \( t \)  
- \( \mu \) = drift (expected annualized return)  
- \( \sigma \) = volatility (annualized standard deviation of returns)  
- \( dW_t \) = Brownian motion increment (\( \sim \mathcal{N}(0, \sqrt{dt}) \))  

---

### 2. Logarithmic Returns

\[
R_t = \ln \frac{S_t}{S_{t-1}}
\]

- Measures relative price changes in log scale.

---

### 3. Drift Component

\[
\text{drift} = \mu - \frac{1}{2}\sigma^2
\]

- Captures expected growth adjusted for variance.

---

### 4. Random Component (Shock / Diffusion)

\[
\text{shock} = \sigma \cdot Z, \quad Z \sim \mathcal{N}(0,1)
\]

- Introduces randomness into stock price evolution.

---

### 5. Discretized GBM (Monte Carlo Step)

\[
S_{t+\Delta t} = S_t \cdot 
\exp \Big( (\mu - \frac{1}{2}\sigma^2)\Delta t + \sigma \sqrt{\Delta t} Z \Big)
\]

- Applied iteratively over multiple steps to simulate price paths.

---

### 6. Statistical Metrics

- **Mean Terminal Price**:

\[
\bar{S_T} = \frac{1}{N} \sum_{j=1}^{N} S_T^{(j)}
\]

- **Standard Deviation**:

\[
\text{Std}(S_T) = \sqrt{\frac{1}{N-1} \sum_{j=1}^N 
\big(S_T^{(j)} - \bar{S_T}\big)^2}
\]

- **Percentiles (25th, 50th, 75th)**:

\[
Q_p = \text{quantile at } p\%
\]

- **Probability above initial price**:

\[
P(S_T > S_0) = \frac{ \# \{ S_T^{(j)} > S_0 \} }{N}
\]

- **Per-step log-return mean and standard deviation**:

\[
\mu_{\Delta t} = (\mu - \frac{1}{2}\sigma^2)\Delta t, \quad 
\sigma_{\Delta t} = \sigma \sqrt{\Delta t}
\]

- **Probability of negative return per step**:

\[
P(\Delta r < 0) = 
\frac{ \# \{ \Delta r_i < 0 \} }{N \cdot \text{steps}}
\]

- **Skewness and Kurtosis**:

\[
\text{Skew}(X) = 
\frac{\mathbb{E}[(X-\bar{X})^3]}
{\big(\mathbb{E}[(X-\bar{X})^2]\big)^{3/2}}, \quad
\text{Kurt}(X) = 
\frac{\mathbb{E}[(X-\bar{X})^4]}
{\big(\mathbb{E}[(X-\bar{X})^2]\big)^2} - 3
\]

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
Install required packages:

bash
Copy code
pip install -r requirements.txt
requirements.txt should include:

nginx
Copy code
numpy
pandas
matplotlib
scipy
yfinance
(Optional) Install ffmpeg to save MP4 animations.

Usage
Configure simulation parameters in the Python script or notebook:

TICKER: Stock symbol (default "MSFT")

LOOKBACK_YEARS: Historical data years

TRADING_DAYS: Trading days per year (default 252)

N_PATHS: Number of Monte Carlo paths

N_STEPS: Number of steps in simulation horizon

SEED: Random seed for reproducibility

Run the script:

bash
Copy code
python gbm_simulation.py
Outputs:

HTML animation of simulated stock paths

Histograms of terminal prices and per-step returns

Console summary of statistical metrics

Interpretation of Results
Mean terminal price: expected outcome across all simulated paths

Median terminal price: central tendency, less influenced by extreme paths

Interquartile range (25th–75th percentile): typical spread of likely outcomes

Probability metrics: likelihood of ending above or below starting price

Per-step return metrics: daily volatility and risk of short-term losses

Implications for MSFT:

Positive drift indicates an expected upward trend over 6 months

Wide standard deviation shows significant uncertainty

Even with upward trend, about half of the daily steps may be negative

Skewed distribution: mean often higher than median due to extreme high outcomes

Project Structure
graphql
Copy code
monte-carlo-gbm/
│
├── gbm_simulation.py        # Main Python script with Monte Carlo GBM
├── README.md                # Project documentation
├── requirements.txt         # Python dependencies
├── gbm_MSFT_simulation.html # Sample HTML animation output
├── data/                    # (Optional) Historical CSV data
└── notebooks/               # Optional Jupyter notebooks
License
This project is licensed under the MIT License.

References
Hull, John C. Options, Futures, and Other Derivatives, 10th Edition

Glasserman, P. Monte Carlo Methods in Financial Engineering

Yahoo Finance API (yfinance)

Wikipedia: Geometric Brownian Motion

yaml
Copy code

---

This **README.md** includes:

- Project overview & motivation  
- Features  
- Mathematical model and formulas  
- Installation and usage instructions  
- Interpretation of results  
- Project structure  
- License and references  
