# Fibonacci Retracement Trading Strategy

## Overview

Welcome to the Fibonacci Retracement Trading Strategy! This repository introduces a powerful trading strategy based on Fibonacci retracement levels. The strategy involves identifying potential price reversal points by using key Fibonacci ratios, allowing traders to make informed decisions on entry and exit points.

### Disclaimer

**Important:** The information shared in this repository is for educational and research purposes only. Trading in financial markets involves significant risk, and past performance is not indicative of future results. Contributors to this repository are not financial advisors, and the strategies provided here should not be considered as financial advice. Users are encouraged to thoroughly understand and assess the risks before implementing any strategy.

## Strategy Highlights

### Key Fibonacci Ratios

- **38.2% Retracement:** Potential reversal level after a significant price movement.
- **50% Retracement:** Often considered a strong reversal level.
- **61.8% Retracement:** A key level indicating a potential continuation of the previous trend.

### Entry Criteria

- **Long Position:**
  - Price retraces to a Fibonacci support level.
  - Additional confirmation from other technical indicators.

- **Short Position:**
  - Price retraces to a Fibonacci resistance level.
  - Additional confirmation from other technical indicators.

### Exit Criteria

- Exit long position when the price reaches a Fibonacci resistance level.
- Exit short position when the price reaches a Fibonacci support level.
- Implement risk management strategies for stop-loss and take-profit levels.

## Strategy Implementation

The strategy can be implemented using your preferred trading platform or programming language (e.g., Python, R). Below is a simplified example using hypothetical price data:

```python
import yfinance as yf
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

### Load historical price data
```python
ticker_symbol = 'ETH-USD'  # You can change this to any valid stock symbol
start_date = '2023-12-01'
end_date = '2024-01-01'

stock_data = get_stock_data(ticker_symbol, start=start_date, end=end_date)
```

### Calculate Fibonacci retracement levels
```python
high_price = stock_data['Close'].max()
low_price = stock_data['Close'].min()

# Fibonacci retracement levels (common levels: 38.2%, 50%, 61.8%)
fibonacci_levels = [0.382, 0.5, 0.618]

# Calculate Fibonacci levels based on the high and low prices
fibonacci_values = [low_price + level * (high_price - low_price) for level in fibonacci_levels]
```
### Implement the trading strategy
```python
# Generate Fibonacci retracement-based trading signals
stock_data['Long_Signal'] = np.where(stock_data['Close'] < fibonacci_values[0], 1, 0)  # Buy signal below 38.2% level
stock_data['Short_Signal'] = np.where(stock_data['Close'] > fibonacci_values[2], 1, 0)  # Sell signal above 61.8% level
```
### Exit signals
- Exit trade when an opposite signal is generated.
- Exit trade when the set Stop Loss or Take Profit has been achieved.

## Strategy Performance
Explore the visual representation of the strategy's performance below:
```python
plt.figure(figsize=(10, 6))
plt.plot(stock_data['Close'], label='Closing Prices', color='blue')
plt.axhline(y=fibonacci_values[0], color='red', linestyle='--', label='38.2% Fibonacci Level')
plt.axhline(y=fibonacci_values[1], color='orange', linestyle='--', label='50% Fibonacci Level')
plt.axhline(y=fibonacci_values[2], color='green', linestyle='--', label='61.8% Fibonacci Level')
plt.title('Fibonacci Retracement Levels')
plt.xlabel('Date')
plt.ylabel('Price')
plt.legend()
plt.grid(True)
plt.show()
```
![image](https://github.com/Arin2k24/Algo_Strategies/assets/157686042/a6363e17-2232-40a2-bc5a-56da4db968f4)

The graph illustrates the Fibonacci Retracement Levels. By following the generated trade signals in conjunction with adjusted stop losses, this strategy demonstrates its effectiveness in various market conditions.
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
![image](https://github.com/Arin2k24/Algo_Strategies/assets/157686042/f802f6cc-b907-4dbc-af17-b3f52a202668)

## Further Improvements
- Advanced Techniques: Explore advanced Fibonacci tools and techniques.
- Pattern Recognition: Combine Fibonacci retracement with other technical patterns for enhanced accuracy.
- Risk Management: Implement effective risk management strategies for a disciplined approach.
## Implementation Guide
For a comprehensive implementation guide and further insights, visit the Strategy Implementation Guide.
