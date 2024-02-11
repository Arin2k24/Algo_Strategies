# RSI and SMA Crossover Trading Strategy

## Overview

Welcome to the RSI and SMA Crossover Trading Strategy! This repository presents a quantitative approach to trading decisions based on the Relative Strength Index (RSI) and Simple Moving Average (SMA) crossovers. The strategy is designed to capture potential trend reversals by considering short-term and long-term moving averages alongside RSI momentum.

### Disclaimer

**Important:** The information shared in this repository is for educational and research purposes only. Trading in financial markets involves significant risk, and past performance is not indicative of future results. Contributors to this repository are not financial advisors, and the strategies provided here should not be considered as financial advice. Users are encouraged to thoroughly understand and assess the risks before implementing any strategy.

## Strategy Highlights

### Entry Criteria

- **Long Position:**
  - SMA for the past week crosses over the SMA for the past month.
  - RSI is above a specified threshold (e.g., 60).

- **Short Position:**
  - SMA for the past week crosses under the SMA for the past month.
  - RSI is below a specified threshold (e.g., 40).

### Exit Criteria

- Exit trade when an opposite signal is generated.
- Exit trade when the set Stop Loss or Take Profit has been achieved.

## Strategy Implementation

The strategy can be implemented using your preferred programming language (e.g., Python, R). Below is a simplified Python example using pandas and numpy for calculation:

```python
import yfinance as yf
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```
### Load historical price data
```python
ticker_symbol = 'BTC-USD'  # You can change this to any valid stock symbol
start_date = '2011-01-01'
end_date = '2021-01-01'

stock_data = yf.download(ticker_symbol, start=start_date, end=end_date)
```
### Calculate SMA for the past week and past month
```python
stock_data['SMA_5'] = stock_data['Close'].rolling(window=5).mean()
stock_data['SMA_22'] = stock_data['Close'].rolling(window=22).mean()
```
### Calculate RSI
```python
def calculate_rsi(data, column='Close', period=14):
    delta = data[column].diff(1)
    gain = delta.where(delta > 0, 0)
    loss = -delta.where(delta < 0, 0)

    avg_gain = gain.rolling(window=period, min_periods=1).mean()
    avg_loss = loss.rolling(window=period, min_periods=1).mean()

    rs = avg_gain / avg_loss
    rsi = 100 - (100 / (1 + rs))
    
    return rsi

stock_data['RSI'] = calculate_rsi(stock_data)
```

### Implement the trading strategy
```python
# These signals provided are illustrative examples.
stock_data['Long_Signal'] = np.where((stock_data['SMA_5'].shift(1) < stock_data['SMA_22'].shift(1)) & (stock_data['SMA_5'] > stock_data['SMA_22']) & (stock_data['RSI'] > 65), 1, 0)
stock_data['Short_Signal'] = np.where((stock_data['SMA_5'].shift(1) > stock_data['SMA_22'].shift(1)) & (stock_data['SMA_5'] < stock_data['SMA_22']) & (stock_data['RSI'] < 35), 1, 0)
```

### Strategy Performance
Explore the visual representation of the strategy's performance below:
```python
fig, ax1 = plt.subplots(figsize=(24, 12))

# Plot Close Price and SMAs
ax1.plot(stock_data['Close'], label='Close Price', alpha=0.5)
ax1.plot(stock_data['SMA_5'], label='5-day SMA', linestyle='--')
ax1.plot(stock_data['SMA_22'], label='22-day SMA', linestyle='--')
ax1.scatter(stock_data.index[stock_data['Long_Signal'] == 1], stock_data['Close'][stock_data['Long_Signal'] == 1], marker='^', color='g', label='Long Signal')
ax1.scatter(stock_data.index[stock_data['Short_Signal'] == 1], stock_data['Close'][stock_data['Short_Signal'] == 1], marker='v', color='r', label='Short Signal')

# Create a secondary y-axis for RSI
ax2 = ax1.twinx()
ax2.plot(stock_data['RSI'], label='RSI', color='purple', alpha=0.5)

# Highlight overbought and oversold regions on RSI
ax2.fill_between(stock_data.index, y1=70, y2=100, color='red', alpha=0.1, label='Overbought')
ax2.fill_between(stock_data.index, y1=0, y2=30, color='green', alpha=0.1, label='Oversold')

ax1.legend(loc='upper left')
ax2.legend(loc='upper right')

ax1.set_xlabel('Date')
ax1.set_ylabel('Price')
ax2.set_ylabel('RSI')

plt.title(f'{ticker_symbol} - SMA and RSI Crossover Strategy')
plt.show()
```
![image](https://github.com/Arin2k24/Algo_Strategies/assets/157686042/ee686c0a-e093-407f-bf2d-e4b0e1c2ea94)

The chart illustrates the generated buy and sell signals, along with the SMAs, RSI, and daily Close price. By following these trade signals in conjunction with adjusted stop losses, this strategy demonstrates its effectiveness in various market conditions.
```python
# After generating a dataframe containing daily P&L 
plt.figure(figsize=(12, 6))
plt.plot(pnl.index, pnl['Net_PnL'].cumsum(), label='Net P&L', color='blue')
plt.title('Net P&L Over Time')
plt.xlabel('Date')
plt.ylabel('Net P&L Value')
plt.legend()
plt.show()
```
![image](https://github.com/Arin2k24/Algo_Strategies/assets/157686042/d3e098f7-9048-4e08-bd91-c087b1388b25)

### Further Improvements
- Enhanced Timing: Consider fine-tuning the strategy parameters for optimal timing.
- Backtesting: Backtest the strategy on historical data to assess its performance over different market conditions.
- Risk Management: Explore additional risk management techniques to further safeguard your trades.
### Implementation Guide
For a comprehensive implementation guide and further insights, visit the Strategy Implementation Guide.
