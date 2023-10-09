# U.S. 10 Year Treasury Rate
Rapid sell-off of U.S. government bonds sent yields to its highest level in 16 years. Yields for U.S. 10 Year Treasuries reached 4.80%, which is higher than that of the 2008 Financial Crisis. Federal Reserve won’t stop raising interest rates until it sees a cooling labor market.

```
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
```
<img src="https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20231008/US%2010YR.png" width="600" />

Bond prices and yield have an inverse relationship because bonds are sold with a face value and fixed coupon rate. If a bond's price increases, then its yield - the bond's coupon yield divided by its current market price - decreases. The graph below displays the inverse relationship between the U.S. 10 Year Treasury Rate and iShares 7-10 Year Treasury Bond ETF (IEF)

```
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
```
<img src="https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20231008/Bond%20Price%20Yield.png" width="600" />

# Bond Yields and Foreign Exchange
Lower bond prices and higher interest rates often attract foreign investors, increasing the demand for U.S. dollars on the foreign exchange market to purchase U.S. bonds. Empiracally, the U.S. Dollar Index has significantly increased in the past couple months. The dollar has seen a strong decline in 1H 2023 as there have been internaional movement to de-dollarize global trade, fueling concerns that the U.S. dollar may lose its status as the world's reserve currency. For instance, during the 2023 BRICS (Brazil, Russia, India, China, and South Africa) Summit in Johannesburg this past August, Brazilan president Luiz Inacio Lula da Silva proposed to create a common currency for trade and investment between each other to reduce its vulnerability to U.S. dollar rate fluctuations. 

```
USD = 'DX-Y.NYB' #ICE US Dollar Index
start_date = end_date - timedelta(days=365)

data1 = yf.download(USD, start=start_date, end=end_date).reset_index()[['Date', 'Adj Close']]

plt.plot(data1['Date'], data1['Adj Close'], label='ICE US Dollar Index', color='blue')
plt.legend()

plt.gca().spines['top'].set_visible(False)
plt.gca().spines['right'].set_visible(False)
plt.show()
```
<img src="https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20231008/USD.png" width="600" />

# High Volatility and Selloff of U.S. Stocks
High interest rates and strong dollar weighs down on multinational companies. 
```
VIX = '^VIX' #Volatility Index
start_date = end_date - timedelta(days=365)

data1 = yf.download(VIX, start=start_date, end=end_date).reset_index()[['Date', 'Adj Close']]

plt.plot(data1['Date'], data1['Adj Close'], label='CBOE Volatility Index', color='blue')
plt.legend()

plt.gca().spines['top'].set_visible(False)
plt.gca().spines['right'].set_visible(False)
plt.show()
```
<img src="https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20231008/VIX.png" width="600" />

# Inverted Yield Curve
```
# Replace 'YOUR_API_KEY' with your actual FRED API key
key = 'YOUR_API_KEY'
fred = Fred(api_key=key)

# Retrieve data for the series 'T10Y3M'
T10Y3M = fred.get_series('T10Y3M')  # 10-Year Treasury Constant Maturity Minus 3-Month Treasury Constant Maturity

# Create a DataFrame from the data
df = pd.DataFrame({'Date': T10Y3M.index, 'Value': T10Y3M.values})

# Extract the year from the date
df['Year'] = df['Date'].dt.year

# Define recession years
recession_years = [2020, 2008, 2001, 1990]

# Create the plot
fig, ax = plt.subplots(figsize=(12, 8))
ax.plot(df['Date'], df['Value'])
plt.axhline(y=0, color='black')
ax.set_ylabel('Percent')
ax.set_title("10-Year Treasury Constant Maturity Minus 3-Month Treasury Constant Maturity")

# Highlight recession years
for year in recession_years:
    recession_df = df[df['Year'] == year]
    plt.fill_between(recession_df['Date'], 0, recession_df['Value'], alpha=0.3, label=f'Recession Year {year}')

# Show the legend
plt.legend()
plt.show()
```
<img src="https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20231008/10Yr%203%20Mo.png" width="600" />
