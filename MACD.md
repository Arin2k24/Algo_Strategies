# MACD Trading Strategy

## Overview

Welcome to the MACD Trading Strategy! This repository introduces a powerful trading approach based on the Moving Average Convergence Divergence (MACD) indicator. The strategy is designed to identify potential trend changes and generate buy or sell signals by analyzing the convergence and divergence of two moving averages.

### Disclaimer

**Important:** The information shared in this repository is for educational and research purposes only. Trading in financial markets involves significant risk, and past performance is not indicative of future results. Contributors to this repository are not financial advisors, and the strategies provided here should not be considered as financial advice. Users are encouraged to thoroughly understand and assess the risks before implementing any strategy.

## Strategy Highlights

### Entry Criteria

- **Long Position:**
  - MACD line crosses above the Signal line.
  - MACD histogram is positive.

- **Short Position:**
  - MACD line crosses below the Signal line.
  - MACD histogram is negative.

### Exit Criteria

- Exit long position when the MACD line crosses below the Signal line.
- Exit short position when the MACD line crosses above the Signal line.
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
end_date = '2024-01-01'

stock_data = yf.download(ticker_symbol, start=start_date, end=end_date)
```

### Calculate MACD
```python
# Calculate EMAs
stock_data['EMA_12'] = stock_data['Close'].ewm(span=12, adjust=False).mean()
stock_data['EMA_26'] = stock_data['Close'].ewm(span=26, adjust=False).mean()

# Calculate MACD line
stock_data['MACD'] = stock_data['EMA_12'] - stock_data['EMA_26']

# Calculate the signal line (9-day EMA of the MACD)
stock_data['Signal_Line'] = stock_data['MACD'].ewm(span=9, adjust=False).mean()
```
### Implement the trading strategy
```python
# Generate trade signals
stock_data['Long_Signal'] = np.where((stock_data['MACD'] > stock_data['Signal_Line']), 1, 0)
stock_data['Short_Signal'] = np.where((stock_data['MACD'] < stock_data['Signal_Line']), 1, 0)
```
### Exit signals
- Exit trade when an opposite signal is generated.
- Exit trade when the set Stop Loss or Take Profit has been achieved.

## Strategy Performance
Explore the visual representation of the strategy's performance below:
```python
fig, (ax1, ax2) = plt.subplots(2, 1, figsize=(24, 18), gridspec_kw={'height_ratios': [3, 1]})

# Plot Close Price
ax1.plot(stock_data['Close'], label='Close Price', alpha=0.5)
ax1.scatter(stock_data.index[stock_data['Long_Signal'] == 1], stock_data['Close'][stock_data['Long_Signal'] == 1], marker='^', color='g', label='Long Signal')
ax1.scatter(stock_data.index[stock_data['Short_Signal'] == 1], stock_data['Close'][stock_data['Short_Signal'] == 1], marker='v', color='r', label='Short Signal')

ax1.legend(loc='upper left')
ax1.set_xlabel('Date')
ax1.set_ylabel('Price')

# Plot MACD and Signal Line
ax2.plot(stock_data['MACD'], label='MACD', color='blue')
ax2.plot(stock_data['Signal_Line'], label='Signal Line', color='orange')
ax2.bar(stock_data.index, stock_data['MACD'] - stock_data['Signal_Line'], color='gray', label='MACD-Histogram')

ax2.legend(loc='upper left')
ax2.set_xlabel('Date')
ax2.set_ylabel('MACD')

plt.suptitle(f'{ticker_symbol} - MACD Strategy')
plt.show()
```
![image](https://github.com/Arin2k24/Algo_Strategies/assets/157686042/486420bd-a4f1-4d68-8c5b-b5085ba18660)
![image](https://github.com/Arin2k24/Algo_Strategies/assets/157686042/c94b8d4d-23ac-4ba5-9943-526a0f49342a)

The chart illustrates the generated buy and sell signals, along with the MACD line, Signal line, and MACD histogram. By following these trade signals in conjunction with adjusted stop losses, this strategy demonstrates its effectiveness in various market conditions.
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
![image](https://github.com/Arin2k24/Algo_Strategies/assets/157686042/3ff27f9e-ea1e-486f-b9c7-e96b29a5c070)


## Further Improvements
- Optimization: Consider adjusting the MACD parameters for optimal performance.
- Backtesting: Backtest the strategy on historical data to assess its performance.
- Risk Management: Explore additional risk management techniques to enhance trade stability.
## Implementation Guide
For a comprehensive implementation guide and further insights, visit the Strategy Implementation Guide.

