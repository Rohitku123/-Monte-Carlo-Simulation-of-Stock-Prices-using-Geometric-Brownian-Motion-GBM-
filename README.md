# -Monte-Carlo-Simulation-of-Stock-Prices-using-Geometric-Brownian-Motion-GBM-
Monte Carlo Simulation of Microsoft Stock using GBM SP and Python 
# Monte Carlo Simulation of Stock Prices using Geometric Brownian Motion (GBM)

## Project Overview

This project implements a **Monte Carlo simulation** of stock price evolution using the **Geometric Brownian Motion (GBM)** model. The simulation is designed to explore possible future outcomes for stock prices, assess risk, and visualize the probability distribution of terminal prices.

The model can fetch historical stock data (default: MSFT) to estimate drift and volatility parameters or use fallback constants. It generates thousands of potential future price paths, animates their evolution, and provides detailed statistical summaries.

---

## Features

- Fetch historical stock prices using Yahoo Finance (`yfinance`).
- Estimate **annualized drift (μ)** and **volatility (σ)** from historical data.
- Simulate stock price evolution using GBM with:
  - Drift component
  - Random component (Brownian motion)
  - Vectorized Monte Carlo simulations
- Generate multiple simulated paths (configurable, default 10,000 paths)
- Animate the evolution of stock price paths over time
- Plot **terminal price distribution** and **per-step return distribution**
- Compute summary statistics:
  - Mean, median, standard deviation
  - 25th, 50th, 75th percentiles
  - Probability of being above or below initial price
  - Daily return statistics including skewness, kurtosis, and probability of negative returns
- Save animation as HTML and optionally MP4

---

## Installation

1. Clone the repository:

```bash
git clone https://github.com/yourusername/monte-carlo-gbm.git
cd monte-carlo-gbm
Create a Python virtual environment (optional but recommended):

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
(Optional) To save MP4 animations, install ffmpeg.

Usage
Open the Jupyter Notebook or Python script.

Configure parameters at the top of the script:

TICKER: Stock symbol (default "MSFT")

LOOKBACK_YEARS: Number of years to fetch historical data

TRADING_DAYS: Trading days per year

N_PATHS: Number of Monte Carlo simulations

N_STEPS: Number of steps in simulation horizon

SEED: Random seed for reproducibility

Run the script to:

Fetch stock data

Simulate GBM paths

Generate animation

Display statistical summaries and histograms

The output includes:

HTML animation of stock price paths

Terminal price distribution plot

Per-step log return distribution plot

Console summary of statistics

**Technical Details**

## Mathematical Model and Formulas

### 1. Stock Price Evolution (Geometric Brownian Motion)

The stock price \( S_t \) is modeled as a **stochastic process**:

\[
dS_t = \mu S_t \, dt + \sigma S_t \, dW_t
\]

Where:

- \( S_t \) = stock price at time \( t \)  
- \( \mu \) = drift (expected annualized return)  
- \( \sigma \) = volatility (annualized standard deviation of returns)  
- \( dW_t \) = Brownian motion increment (\( \sim \mathcal{N}(0, \sqrt{dt}) \))  

---

### 2. Logarithmic Returns (Daily Returns)

\[
R_t = \ln \frac{S_t}{S_{t-1}}
\]

- Captures the percentage change in stock price in log scale.

---

### 3. Drift Component

\[
\text{drift} = \mu - \frac{1}{2}\sigma^2
\]

- Accounts for expected growth, adjusted for variance.

---

### 4. Random Component (Shock / Diffusion)

\[
\text{shock} = \sigma \cdot Z
\]

- \( Z \sim \mathcal{N}(0,1) \) is a standard normal random variable.
- Introduces randomness to the price evolution.

---

### 5. Discretized GBM (Monte Carlo Step)

For small time increments \( \Delta t \):

\[
S_{t+\Delta t} = S_t \cdot 
\exp \Big( (\mu - \frac{1}{2}\sigma^2)\Delta t + \sigma \sqrt{\Delta t} Z \Big)
\]

- Used iteratively to simulate stock paths over multiple steps.

---

### 6. Iterative Simulation

\[
S_{t+1} = S_t \cdot e^{\text{drift} \cdot \Delta t + \sigma \sqrt{\Delta t} Z_{t+1}}, \quad t = 0,1,2,\dots,n-1
\]

- Repeat for \( n \) steps and \( N \) paths to generate Monte Carlo outcomes.

---

### 7. Statistical Metrics from Simulation

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

- **Skewness and Kurtosis** (for return distribution):

\[
\text{Skew}(X) = 
\frac{\mathbb{E}[(X-\bar{X})^3]}
{\big(\mathbb{E}[(X-\bar{X})^2]\big)^{3/2}}, \quad
\text{Kurt}(X) = 
\frac{\mathbb{E}[(X-\bar{X})^4]}
{\big(\mathbb{E}[(X-\bar{X})^2]\big)^2} - 3
\]


Geometric Brownian Motion (GBM)

Drift term: captures expected growth

Diffusion term: introduces randomness

Random variable Z: independent standard normal draws

Monte Carlo Simulation
Generates multiple paths by applying the GBM step iteratively

Collects statistics across all paths

Visualizes both the typical and extreme outcomes

Statistical Analysis
The simulation provides:

Terminal price distribution:

Mean, median, standard deviation

25th, 50th, 75th percentiles

Minimum and maximum

Probability above/below initial price

Per-step log return distribution:

Mean and standard deviation

Skewness and kurtosis

Probability of negative returns

Results Interpretation
The mean terminal price indicates the expected value across all scenarios.

The median gives the central tendency, less affected by extreme outcomes.

The interquartile range (25th–75th percentile) shows the range of typical outcomes.

Probability metrics help assess the likelihood of gains vs. losses.

Day-to-day returns show short-term volatility even if the long-term trend is positive.

Project Structure
graphql
Copy code
monte-carlo-gbm/
│
├── gbm_simulation.py        # Main Python script with Monte Carlo GBM
├── README.md                # Project overview and instructions
├── requirements.txt         # Python dependencies
├── gbm_MSFT_simulation.html # Sample HTML animation output
├── data/                    # (Optional) Historical CSV data
└── notebooks/               # Optional Jupyter notebooks
Example Output
Terminal price histogram with KDE curve

Per-step log return histogram

Animation of multiple simulated stock paths

Console summary:

bash
Copy code
Ticker: MSFT
S0 = $506.69, μ = 17.0%, σ = 26.0%
Horizon: 0.5 years, Paths: 10,000
Terminal mean=$550.75, std=$104.00
25/50/75th = $478.00/$540.91/$613.00
Paths > S0: 6,403, Paths ≤ S0: 3,597
Per-step log return mean=0.062%, std=1.47%, skew=0.01, kurt=-0.04, P(negative)=49.0%
License
This project is open-source and available under the MIT License.

References
Hull, John C. Options, Futures, and Other Derivatives, 10th Edition

Glasserman, P. Monte Carlo Methods in Financial Engineering

Yahoo Finance API (yfinance)

Wikipedia: Geometric Brownian Motion

markdown
Copy code

---

This README includes:

1. **Overview & motivation**  
2. **Features list**  
3. **Installation instructions**  
4. **Usage instructions**  
5. **Mathematical & technical explanation**  
6. **Statistical output interpretation**  
7. **Folder structure example**  
8. **Sample console output**  
9. **License & references**

---

If you want, I can also **create a ready-to-upload GitHub repository structure** with `requirements.txt`, sample data, and a notebook linked to this README, so you can literally push it directly.  

Do you want me to prepare that full repo structure next?
