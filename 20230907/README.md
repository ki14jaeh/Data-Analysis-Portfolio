# Economy and Data Analytics Portflio
## 2023/09/07: Inflation and US Economy Outlook
### Rising Oil Prices
Crude oil prices are beginning to upside as two of the largest crude exporters, China and Russia, prolongs their voluntary production cut[^1]. The existing fear of inflation makes the inflationary pressure of rising oil prices particulary pulpable. With the 2024 Presidential Election on the horizon, rising gas prices are critical for President Biden's re-election campaign. Last year, Biden has already utilized 180 mm barrels of the US Petroleum Reserve, which is now at the lowest level since 1983[^2]. The fund has declined by approximately 300 mm barrels since Biden took office[^3]. Though not as high as its 2022 peak, Crude Oil Futures (WTI) has seen a sharp increase in the last couple months.
```
import yfinance as yf
import matplotlib.pyplot as plt
from datetime import datetime, timedelta

# Define the ticker symbols
ticker1 = 'CL=F' #https://finance.yahoo.com/quote/CL=F/

# Calculate the end date as the current date and time
end_date = datetime.now()
start_date = end_date - timedelta(days = 365)

# Fetch data for WTI crude oil prices for the specified date range
data1 = yf.download(ticker1, start=start_date, end=end_date).reset_index()[['Date', 'Adj Close']]
plt.figure(1)
plt.plot(data1['Date'], data1['Adj Close'], label='WTI', color='blue')
plt.xlabel('Date')
plt.ylabel('Price')
plt.legend()
plt.show()
```

<img src="https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20230907/WTI%20Prices.png" width="600" />

The US 10Y Yield and oil prices are closely correlated. As seen below WTI Crude Oil and US 10-Year Tresury Yield has a relatively positive correlation. This is because increasing oil prices leads to an increase in the expected inflation rate, ultiamtely leading to interest rates and bond yields rising. Bonds with longer cash flows will see their yields rise and prices fall the most as seen in the figure below that compares US 10-Year Tresury Yield vs. iShares 20 Plus Year Treasury Bond ETF(TLT).

```
import matplotlib.dates as mdates
from datetime import datetime, timedelta

# Define the ticker symbols
ticker1 = 'WTI'
ticker2 = '%5ETNX'  # 10-Year Treasury Yield

# Calculate the end date as the current date and time
end_date = datetime.now()
start_date = end_date - timedelta(days=10 * 365)  # Assuming an average of 365 days per year

# Fetch data for WTI crude oil prices for the specified date range
data1 = yf.download(ticker1, start=start_date, end=end_date).reset_index()[['Date', 'Adj Close']]

# Fetch data for 10-Year Treasury Yield for the specified date range
data2 = yf.download(ticker2, start=start_date, end=end_date).reset_index()[['Date', 'Adj Close']]

# Create a new figure with two y-axes
fig, ax1 = plt.subplots(figsize=(12, 6))

# Plot the first dataset (WTI crude oil prices) using the left y-axis
ax1.set_xlabel('Year')
ax1.set_ylabel('WTI Crude Oil', color='blue')
ax1.plot(data1['Date'], data1['Adj Close'], label='WTI Crude Oil', color='blue')
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
plt.title('WTI Crude Oil vs. US 10-Year Treasury Yield')
plt.grid()
plt.show()
```
<img src="https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20230907/10YUSGG10YR%20vs.%20WTI.png" width="600" />

```
# Define the ticker symbols
ticker1 = '%5ETNX' # 10-Year Treasury Yield
ticker2 = 'TLT'  # iShares 20+ Year Treasury Bond ETF

# Calculate the end date as the current date and time
end_date = datetime.now()
start_date = end_date - timedelta(days=365)  # Assuming an average of 365 days per year

# Fetch data for # 10-Year Treasury Yield prices for the specified date range
data1 = yf.download(ticker1, start=start_date, end=end_date).reset_index()[['Date', 'Adj Close']]

# Fetch data for iShares 20+ Year Treasury Bond ETF for the specified date range
data2 = yf.download(ticker2, start=start_date, end=end_date).reset_index()[['Date', 'Adj Close']]

# Create a new figure with two y-axes
fig, ax1 = plt.subplots(figsize=(12, 6))

# Plot the first dataset (10-Year Treasury Yield) using the left y-axis
ax1.set_xlabel('Year')
ax1.set_ylabel('10-Year Treasury Yield', color='blue')
ax1.plot(data1['Date'], data1['Adj Close'], label='10-Year Treasury Yield', color='blue')
ax1.tick_params(axis='y', labelcolor='blue')

# Create a secondary y-axis on the right for the second dataset (iShares 20+ Year Treasury Bond ETF)
ax2 = ax1.twinx()
ax2.set_ylabel('iShares 20+ Year Treasury Bond ETF', color='red')
ax2.plot(data2['Date'], data2['Adj Close'], label='iShares 20+ Year Treasury Bond ETF(TLT)', color='red')
ax2.tick_params(axis='y', labelcolor='red')

# Add legends for both datasets
ax1.legend(loc='upper left')
ax2.legend(loc='upper right')

# Customize the x-axis labels for better readability
ax1.xaxis.set_major_locator(mdates.MonthLocator(interval=1))  # Display x-axis labels every month
ax1.xaxis.set_major_formatter(mdates.DateFormatter('%m %Y'))  # Format x-axis labels as "Jan 2020", "Feb 2020", etc.

# Show the plot
plt.title('10-Year Treasury Yield vs. iShares 20+ Year Treasury Bond ETF')
plt.grid()
plt.show()
```
<img src="https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20230907/10YUSGG10YR%20vs.%20TLT.png" width="600" />

### Declining Foreign Holdings of US Treasury
China and Saudi Arabia has been shaving off its holdings of US Treasuries. Though China still holds second most US Treasury Bonds in the world, they have been reducing its exposure to US currency by cutting its holdings[^4]. Now China , lowest since the 2008 financial crisis.

```
import requests
import pandas as pd
import matplotlib.pyplot as plt

url = "https://ticdata.treasury.gov/resource-center/data-chart-center/tic/Documents/slt_table5.html"
html = requests.get(url).content

# Read all tables from the HTML content (specift that first row is header)
tables = pd.read_html(html)

# Select the specific DataFrame you want from the list (e.g., the first one)
df = tables[0]

# Drop rows with missing values (NaN) and remove first column
df = df.dropna().reset_index()

#Change first row to header
df.columns = df.iloc[0]
df = df[df.columns[::-1]]
cols = list(df.columns)
cols = [cols[-1]] + cols[:-1]
df = df[cols]
df = df[1:].iloc[:, 1:]

country_name = "China, Mainland"
df2 = df.loc[df["Country"] == country_name]
df2 = df2.drop(columns=['Country']).astype(float)
df2 = df2.transpose()
ax2.plot(df2)

ax2.set_xlabel('Month')
ax2.set_ylabel('Holdings at end of time period (in Billion $)')
ax2.set_title(f"{country_name}: US Treasury Holdings")
```
<img src="https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20230907/China%20US%20Treasury%20Holdings.png" width="600" />

```
fig1, ax1 = plt.subplots(figsize=(12,8))

country_name = "Saudi Arabia"
df1 = df.loc[df["Country"] == country_name]
df1 = df1.drop(columns=['Country']).astype(float)
df1 = df1.transpose()
ax1.plot(df1)

ax1.set_xlabel('Month')
ax1.set_ylabel('Holdings at end of time period (in Billion $)')
ax1.set_title(f"{country_name}: US Treasury Holdings")
```

<img src="https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/Saudi%20Arabia%20US%20Treasury%20Holding.png" width="600" />

The selling pressures will lead to bond prices falling, which will inevitably lead to a increase in interest rates.

[^1]: https://www.cnn.com/2023/09/06/business/oil-price-goldman-sachs/index.html
[^2]: https://finance.yahoo.com/news/biden-replenish-oil-stockpile-3-143540728.html
[^3]: https://www.cnn.com/2023/05/15/business/biden-administration-oil-strategic-petroleum-reserve/index.html
['4]: https://www.globaltimes.cn/page/202308/1296371.shtml
