# Portfolio Optimization and Risk Analysis

This project applies mean-variance portfolio optimization to sector ETFs and gold. It compares classical allocation strategies, studies the effect of a 30% maximum asset weight, and evaluates every strategy on out-of-sample data.

The complete analysis is implemented in `Main.ipynb`.

## Project Objective

The central question is:

> Can mean-variance optimization produce portfolios with better out-of-sample risk-adjusted performance than a simple equal-weight allocation, and how do diversification constraints affect the results?

## Assets

The portfolio contains 10 sector ETFs and one gold ETF:

| Ticker | Asset |
|---|---|
| IYW | Technology |
| MOO | Agriculture |
| XLP | Consumer Staples |
| XLV | Healthcare |
| XLE | Energy |
| XLF | Financials |
| XLI | Industrials |
| XLY | Consumer Discretionary |
| XLU | Utilities |
| XLB | Materials |
| GLD | Gold |

## Data

- **Data source:** Yahoo Finance through `yfinance`
- **Price field:** Adjusted closing prices
- **Frequency:** Daily
- **Full period:** January 1, 2016 through December 31, 2025
- **Training period:** 2016-2021
- **Testing period:** 2022-2025
- **Risk-free-rate source:** 3-month U.S. Treasury yield from FRED

Adjusted closing prices account for distributions and other corporate actions. Simple daily returns are calculated from the prices with `pct_change`.

## Methodology

1. Download and inspect the adjusted closing prices.
2. Calculate daily returns, annualized returns, annualized volatility, and correlations.
3. Divide the observations into a training period and a testing period.
4. Generate and visualize 10,000 random portfolios.
5. Calculate the efficient frontier with constrained quadratic optimization.
6. Optimize five portfolio strategies using SLSQP:
   - Equal Weight (1/N)
   - Global Minimum Variance
   - Global Minimum Variance with a 30% weight cap
   - Maximum Sharpe Ratio
   - Maximum Sharpe Ratio with a 30% weight cap
7. Rebalance the test portfolios monthly with an initial capital of $10,000.
8. Compare cumulative wealth, annualized return, volatility, Sharpe ratio, maximum drawdown, and CVaR.

All optimized weights are estimated exclusively from the 2016-2021 training data. The 2022-2025 testing period is used only for out-of-sample evaluation.

## Portfolio Constraints

All strategies enforce:

- Weights between 0% and 100%
- Fully invested portfolios whose weights sum to 100%

The capped strategies additionally limit every asset to a maximum weight of 30%.

## Evaluation Metrics

The notebook evaluates portfolio performance using:

- Cumulative wealth
- Annualized return
- Annualized volatility
- Sharpe ratio
- Maximum drawdown
- Conditional Value at Risk (CVaR) at 95%

The Sharpe ratio uses the 3-month U.S. Treasury yield as the risk-free-rate proxy.

## Notebook Structure

The notebook contains:

- Data collection and asset selection
- Exploratory analysis of prices and returns
- Correlation and volatility analysis
- Efficient-frontier construction
- GMV and maximum-Sharpe optimization
- Capped-portfolio optimization
- Monthly rebalancing
- Out-of-sample performance and drawdown analysis

## Requirements

The main Python dependencies are:

- pandas
- NumPy
- Matplotlib
- Seaborn
- SciPy
- yfinance

The project includes a local virtual environment in `.venv`, but that folder should normally be excluded from GitHub rather than committed.

## Setup

From the project directory:

```bash
python -m venv .venv
```

Activate the environment:

```bash
# Windows PowerShell
.venv\Scripts\Activate.ps1

# Windows Command Prompt
.venv\Scripts\activate.bat
```

Install dependencies:

```bash
python -m pip install pandas numpy matplotlib seaborn scipy yfinance
```

Start Jupyter:

```bash
jupyter notebook
```

Then open `Main.ipynb` and run the cells from top to bottom.

## Reproducibility

Monte Carlo portfolio generation uses a fixed random seed. Historical internet data can change or become unavailable, and the testing-period risk-free-rate download requires internet access.

## Disclaimer

This project is for educational and research purposes only. It does not constitute investment advice, and historical performance does not guarantee future results.
