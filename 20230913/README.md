# What is S&P 500
Companies included in the S&P 500 index are selected by the index committee who asses the company's merit using the following criteria[^1]:
* Company should be from the U.S.
* Market Capitalization should exceed a certain value ($14.5 B as of July 2023) 
* Shares must be liquid (i.e. listed in a U.S. exchange such as NYSE, Nasdaq, etc.)
* At least 50% of outstanding shards musty be available for public trading
* Must report positive earnings int he most recent quarter
* Sum of its earnings in the previous four quarters must be positive

# S&P Quarterly Rebalance
On September 1, index provider S&P Dow Jones Indices (SPDJI) announced changes in their U.S. large-cap benchmark. Notable changes include the entrance of world's largest PE firm Blackstone ($BX) and Airbnb ($ABNB) and deletion of insurance company Lincoln National ($LNC) and consumer conglomerate Newell Brands ($NWL).

```
import yfinance as yf
import matplotlib.pyplot as plt
import matplotlib.dates as mdates
from datetime import datetime, timedelta
import numpy as np

# Define the ticker symbols
ticker1 = 'BX'
ticker2 = 'ABNB'

# Calculate the end date as the current date and time
end_date = datetime.now()
start_date = "2023-09-01"  # When SP500 rebalancing was announced

# Fetch data for Blackstone and Airbnb for the specified date range
data1 = yf.download(ticker1, start=start_date, end=end_date).reset_index()[['Date', 'Adj Close']]
data2 = yf.download(ticker2, start=start_date, end=end_date).reset_index()[['Date', 'Adj Close']]

# Create a figure and plot both datasets on the same y-axis
plt.figure(figsize=(12, 6))

plt.plot(data1['Date'], data1['Adj Close'], label='Blackstone Share Price', color='blue')
plt.plot(data2['Date'], data2['Adj Close'], label='Airbnb Share Price', color='red')

# Calculate percent change from start to end
percent_change1 = ((data1['Adj Close'].iloc[-1] - data1['Adj Close'].iloc[0]) / data1['Adj Close'].iloc[0]) * 100
percent_change2 = ((data2['Adj Close'].iloc[-1] - data2['Adj Close'].iloc[0]) / data2['Adj Close'].iloc[0]) * 100

# Annotate the percent change at the end of the chart
plt.annotate(f'Blackstone: {percent_change1:.2f}%', (mdates.date2num(data1['Date'].iloc[-1]), data1['Adj Close'].iloc[-1]), xytext=(-60, 20), textcoords='offset points',
             arrowprops=dict(arrowstyle='->', color='blue'))
plt.annotate(f'Airbnb: {percent_change2:.2f}%', (mdates.date2num(data2['Date'].iloc[-1]), data2['Adj Close'].iloc[-1]), xytext=(-60, -40), textcoords='offset points',
             arrowprops=dict(arrowstyle='->', color='red'))

# Set labels and legend
plt.xlabel('Date')
plt.ylabel('Share Price')
plt.title('Blackstone and Airbnb Share Price')
plt.legend()

# Show the plot
plt.grid()
plt.show()
```
<img src="https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20230913/BX%20ABNB%20Share%20Price.png" width="600" />

```
# Define the ticker symbols
ticker1 = 'LNC'
ticker2 = 'NWL'

# Calculate the end date as the current date and time
end_date = datetime.now()
start_date = "2023-09-01"  # When SP500 rebalancing was announced

# Fetch data for Blackstone and Airbnb for the specified date range
data1 = yf.download(ticker1, start=start_date, end=end_date).reset_index()[['Date', 'Adj Close']]
data2 = yf.download(ticker2, start=start_date, end=end_date).reset_index()[['Date', 'Adj Close']]

# Create a figure and plot both datasets on the same y-axis
plt.figure(figsize=(12, 6))

plt.plot(data1['Date'], data1['Adj Close'], label='Lincoln National Share Price', color='blue')
plt.plot(data2['Date'], data2['Adj Close'], label='Newell Brands Share Price', color='red')

# Calculate percent change from start to end
percent_change1 = ((data1['Adj Close'].iloc[-1] - data1['Adj Close'].iloc[0]) / data1['Adj Close'].iloc[0]) * 100
percent_change2 = ((data2['Adj Close'].iloc[-1] - data2['Adj Close'].iloc[0]) / data2['Adj Close'].iloc[0]) * 100

# Annotate the percent change at the end of the chart
plt.annotate(f'Lincoln: {percent_change1:.2f}%', (mdates.date2num(data1['Date'].iloc[-1]), data1['Adj Close'].iloc[-1]), xytext=(-60, -40), textcoords='offset points',
             arrowprops=dict(arrowstyle='->', color='blue'))
plt.annotate(f'Newell: {percent_change2:.2f}%', (mdates.date2num(data2['Date'].iloc[-1]), data2['Adj Close'].iloc[-1]), xytext=(-60, 40), textcoords='offset points',
             arrowprops=dict(arrowstyle='->', color='red'))

# Set labels and legend
plt.xlabel('Date')
plt.ylabel('Share Price')
plt.title('Lincoln and Newell Share Price')
plt.legend()

# Show the plot
plt.grid()
plt.show()
```
<img src="https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20230913/LNC%20NWL%20Share%20Price.png" width="600" />


[^1]: https://www.spglobal.com/spdji/en/documents/methodologies/methodology-sp-us-indices.pdf
