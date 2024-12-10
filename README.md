# README for Monte Carlo Portfolio Simulation Using Real-Time Data
This Python script implements a Monte Carlo simulation for portfolio performance analysis. It uses real-time financial data sourced from Yahoo Finance through the yfinance library to generate insights into portfolio returns and risk. The script simulates the potential future value of a portfolio using historical stock price data, statistical techniques, and random sampling.

## Features
#### Real-Time Data Retrieval
The script fetches historical stock price data for a specified period using Yahoo Finance's API. This data is used to compute:

#### Mean returns: 
The average daily return for each stock.
#### Covariance matrix: 
A matrix showing how stocks' returns change together, used to model portfolio risk.
#### Portfolio Weighting
Randomized portfolio weights are assigned to stocks to diversify the portfolio.

#### Monte Carlo Simulation
A Monte Carlo simulation is run to model the future portfolio value over a specified time horizon. The simulation accounts for randomness and correlations between assets.

#### Visualization
The script generates a plot showing the projected portfolio value over the simulation period.

# Prerequisites
Python 3.6 or higher
#### Libraries: 
pandas, numpy, matplotlib, datetime, pandas_datareader, yfinance
Install the required libraries using:
pip install pandas numpy matplotlib pandas-datareader yfinance
# How It Works
#### 1. Fetching Real-Time Data
The new_data() function downloads real-time historical stock data for the specified tickers (stocks) and date range (start, end). The closing prices are processed to calculate daily percentage changes, mean returns, and the covariance matrix.
### Example:
mean_of_return, cov_matrix = new_data(stocks, startDate, endDate)
#### 2. Portfolio Weights
Random weights are generated for the portfolio, ensuring they sum to 1.

### Example:

weight_port = np.random.random(len(mean_of_return))
weight_port /= np.sum(weight_port)
#### 3. Monte Carlo Simulation
A Cholesky decomposition is applied to the covariance matrix to incorporate correlations between stocks.
Simulations are run for no_of_simulations iterations, projecting the portfolio's performance over tf_days days.
A cumulative product of simulated returns is calculated to model the portfolio's value over time.
#### Key Code Snippet:
for i in range(0, no_of_simulations):
    x = np.random.normal(size=(tf_days, len(weight_port)))
    low_tri = np.linalg.cholesky(cov_matrix)
    daily_ret = mean_matrix + np.inner(low_tri, x)
    port_sims[:, i] = np.cumprod(np.inner(weight_port, daily_ret.T) + 1) * initial
#### 4. Visualization
A line plot displays the simulated portfolio values over time, illustrating potential future scenarios.

### Example:
plt.plot(port_sims)
plt.ylabel('Value of Portfolio')
plt.xlabel('No of Days')
plt.title('Monte Carlo Simulation of our Portfolio')
## Usage
#### Set Tickers
Update the stk_list variable with the desired stock tickers.

#### Specify Date Range
The simulation uses data from 300 days before the current date to today. Adjust startDate and endDate as needed.

#### Run the Script
Execute the script to view the mean returns, covariance matrix, and the Monte Carlo simulation results.

### Example Output
#### Mean Returns: 
Displays the average daily returns for each stock.
#### Covariance Matrix:
Shows the relationship between stock returns.
#### Portfolio Simulation Plot: 
Visualizes projected portfolio value over the simulation period.
# Notes
#### Real-Time Data: 
The script fetches the most up-to-date data from Yahoo Finance, ensuring that the analysis reflects current market conditions.
#### Monte Carlo Limitations: 
Simulations are probabilistic and assume normal distribution of returns, which might not fully capture market behavior.
#### Error Handling: 
The try-except block ensures the script continues running even if data retrieval fails for specific tickers.
# Potential Enhancements
Include user input for stock tickers and time periods.
Allow customization of simulation parameters (e.g., number of simulations, time frame).
Save simulation results and plots to files for later analysis.
