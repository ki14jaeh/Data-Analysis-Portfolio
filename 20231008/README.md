# U.S. 10 Year Treasury Rate
Rapid sell-off of U.S. government bonds sent yields to its highest level in 16 years.

'''
import yfinance as yf
import matplotlib.pyplot as plt
from datetime import datetime, timedelta

TenYear = '%5ETNX' #CBOE Interest Rate 10 Year Treasury Yield Index
end_date = datetime.now()
start_date = end_date - timedelta(days=180)

data1 = yf.download(TenYear, start=start_date, end=end_date).reset_index()[['Date', 'Adj Close']]

plt.plot(data1['Date'], data1['Adj Close'], label='CBOE Interest Rate 10 Year Treasury Yield Index', color='blue')
plt.ylabel('Yield (%)')
plt.legend()

max_value = max(data1['Adj Close'])
max_index = data1['Adj Close'].idxmax()
plt.annotate(f'Max: {max_value:.2f}',
             xy=(data1['Date'][max_index], max_value),
             xytext=(data1['Date'][max_index] + timedelta(days=7), max_value + 0.1),
             arrowprops=dict(arrowstyle='->'))
plt.gca().spines['top'].set_visible(False)
plt.gca().spines['right'].set_visible(False)
plt.show()
'''
<img src="https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20231008/US%2010YR.png" width="600" />

'''
# Define the ticker symbols
ticker1 = 'IEF'
ticker2 = '%5ETNX'  # 10-Year Treasury Yield

# Calculate the end date as the current date and time
end_date = datetime.now()
start_date = end_date - timedelta(days=300) 

# Fetch data for WTI crude oil prices for the specified date range
data1 = yf.download(ticker1, start=start_date, end=end_date).reset_index()[['Date', 'Adj Close']]

# Fetch data for 10-Year Treasury Yield for the specified date range
data2 = yf.download(ticker2, start=start_date, end=end_date).reset_index()[['Date', 'Adj Close']]

# Create a new figure with two y-axes
fig, ax1 = plt.subplots(figsize=(12, 6))

# Plot the first dataset (WTI crude oil prices) using the left y-axis
ax1.set_xlabel('Year')
ax1.set_ylabel('iShares 7-10 Year Treasury Bond ETF', color='blue')
ax1.plot(data1['Date'], data1['Adj Close'], label='iShares 7-10 Year Treasury Bond ETF', color='blue')
ax1.tick_params(axis='y', labelcolor='blue')

# Create a secondary y-axis on the right for the second dataset (10-Year Treasury Yield)
ax2 = ax1.twinx()
ax2.set_ylabel('US 10-Year Treasury Yield', color='red')
ax2.plot(data2['Date'], data2['Adj Close'], label='US 10-Year Treasury Yield', color='red')
ax2.tick_params(axis='y', labelcolor='red')

# Add legends for both datasets
ax1.legend(loc='upper left')
ax2.legend(loc='upper right')

# Customize the x-axis labels for better readability
ax1.xaxis.set_major_locator(mdates.YearLocator(base=1))  # Display x-axis labels every 1 year
ax1.xaxis.set_major_formatter(mdates.DateFormatter('%Y'))  # Format x-axis labels as years

# Show the plot
plt.title('iShares 7-10 Year Treasury Bond ETF vs. US 10-Year Treasury Yield')
plt.grid()
plt.show()
'''
<img src="https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20231008/Bond%20Price%20Yield.png" width="600" />
'''
USD = 'DX-Y.NYB' #ICE US Dollar Index
start_date = end_date - timedelta(days=365)

data1 = yf.download(USD, start=start_date, end=end_date).reset_index()[['Date', 'Adj Close']]

plt.plot(data1['Date'], data1['Adj Close'], label='ICE US Dollar Index', color='blue')
plt.legend()

plt.gca().spines['top'].set_visible(False)
plt.gca().spines['right'].set_visible(False)
plt.show()
'''
<img src="https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20231008/USD.png" width="600" />
'''
VIX = '^VIX' #Volatility Index
start_date = end_date - timedelta(days=365)

data1 = yf.download(VIX, start=start_date, end=end_date).reset_index()[['Date', 'Adj Close']]

plt.plot(data1['Date'], data1['Adj Close'], label='CBOE Volatility Index', color='blue')
plt.legend()

plt.gca().spines['top'].set_visible(False)
plt.gca().spines['right'].set_visible(False)
plt.show()
'''
<img src="https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20231008/VIX.png" width="600" />
