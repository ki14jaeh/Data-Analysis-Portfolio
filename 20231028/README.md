# Bank Sector Beating Amid Higher-for-Longer Rate Environment
## The "Big 4" Banks: JP Morgan, Bank of America, Wells Fargo, Citi Group
### Stock
As interest rates continue to rise and the Fed remains hawkish, investors are expecting loan demands to weaken, narrowing banks' net interest income. Bank of America stocks fell to its three-year-low to $25.17 (lowest since Covid-19 shock) due to its biggest sell off since March of this year (2023 U.S. Banking Crisis). J.P. Morgan stocks also sank 3.5% to $135.69 as news broke that CEO Jamie Dimon and his family plan to sell about 1 million shares "for financial diversification and tax-planning purposes."
```
import yfinance as yf
import matplotlib.pyplot as plt
import matplotlib.dates as mdates
from datetime import datetime, timedelta
import numpy as np
import pandas as pd

# Define the ticker symbols
ticker1 = 'JPM'
ticker2 = 'BAC'
ticker3 = 'WFC'
ticker4 = 'C'

# Calculate the end date as the current date and time
end_date = datetime.now()
start_date = end_date - timedelta(days=120)

# Fetch data for Blackstone and Airbnb for the specified date range
data1 = yf.download(ticker1, start=start_date, end=end_date).reset_index()[['Date', 'Adj Close']]
data2 = yf.download(ticker2, start=start_date, end=end_date).reset_index()[['Date', 'Adj Close']]
data3 = yf.download(ticker3, start=start_date, end=end_date).reset_index()[['Date', 'Adj Close']]
data4 = yf.download(ticker4, start=start_date, end=end_date).reset_index()[['Date', 'Adj Close']]

# Create a figure and plot both datasets on the same y-axis
plt.figure(figsize=(12, 6))

plt.plot(data1['Date'], data1['Adj Close'], label='JP Morgan', color='blue')
plt.plot(data2['Date'], data2['Adj Close'], label='Bank of America', color='red')
plt.plot(data3['Date'], data3['Adj Close'], label='Wells Fargo', color='green')
plt.plot(data4['Date'], data4['Adj Close'], label='Citi', color='black')

# Set labels and legend
plt.xlabel('Date')
plt.ylabel('Share Price')
plt.title('Major U.S. Bank Stock Price')
plt.legend()

# Show the plot
plt.grid()
plt.show()
```
<img src="https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20231028/Stock%20B4.png" width="600" />

```
from datetime import datetime, timedelta
from matplotlib.ticker import FuncFormatter

# Define the ticker symbols
ticker1 = 'JPM'
ticker2 = 'BAC'
ticker3 = 'WFC'
ticker4 = 'C'

# Calculate the end date as the current date and time
end_date = datetime.now()
start_date = end_date - timedelta(days=120)

# Fetch data for the specified date range
data1 = yf.download(ticker1, start=start_date, end=end_date).reset_index()
data2 = yf.download(ticker2, start=start_date, end=end_date).reset_index()
data3 = yf.download(ticker3, start=start_date, end=end_date).reset_index()
data4 = yf.download(ticker4, start=start_date, end=end_date).reset_index()

# Calculate the percentage changes from the start date
data1['Percent Change'] = (data1['Adj Close'] / data1['Adj Close'].iloc[0] - 1) * 100
data2['Percent Change'] = (data2['Adj Close'] / data2['Adj Close'].iloc[0] - 1) * 100
data3['Percent Change'] = (data3['Adj Close'] / data3['Adj Close'].iloc[0] - 1) * 100
data4['Percent Change'] = (data4['Adj Close'] / data4['Adj Close'].iloc[0] - 1) * 100

# Plot the percentage changes
plt.figure(figsize=(12, 6))
plt.plot(data1['Date'], data1['Percent Change'], label=ticker1)
plt.plot(data2['Date'], data2['Percent Change'], label=ticker2)
plt.plot(data3['Date'], data3['Percent Change'], label=ticker3)
plt.plot(data4['Date'], data4['Percent Change'], label=ticker4)

plt.title('Percentage Change from Start Date')
plt.xlabel('Date')
plt.ylabel('Percentage Change')
plt.legend()
plt.grid(True)
plt.gca().yaxis.set_major_formatter(FuncFormatter(lambda x, _: f'{x:.0f}%'))

plt.show()
```
<img src="https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20231028/Stock%20%25%20change%20B4.png" width="600" />

### Price-to-Book Ratio
A price-to-book (P/B) ratio is often used as a valuation metric in the bankinjg industry because it is more relevant than the commonly used price-to-earnings (P/E) ratio. A P/B ratio below 1 suggests that a company's stock is trading at a discount to its book value. For banks, loans are considered assets while deposits are considered liabilities. Due to this unique business model, a bank's book value tells us more of a bank's financial health than its earnings. Furthermore, because banks' earnings are significantly impacted by exogenous macroeconomic factors such as interest rates and inflation, P/B ratio is considered more reliable than a P/E ratio. 
```
data = {
    "Bank": ["JP Morgan", "Bank of America", "Wells Fargo", "Citi"],
    "P/B Ratio Current": [1.26, 0.71, 0.78, 0.35],
    "P/B Ratio 4 Months Ago": [1.33, 0.80, 0.85, 0.42],
} 
#data from https://www.macrotrends.net/ as of 10/28/2023
#P/B ratio 4 months ago using data as of 6/30/2023

# Convert the data to a DataFrame
df = pd.DataFrame(data)

# Set the bank names as the index for easier plotting
df.set_index("Bank", inplace=True)

# Calculate the change in P/B ratio
df["P/B Ratio Change"] = df["P/B Ratio Current"] - df["P/B Ratio 4 Months Ago"]

# Plot the data
fig, ax = plt.subplots(figsize=(12, 6))
width = 0.4  # Bar width

# Bar positions
x = range(len(df.index))

# Bar chart for current P/B ratio
ax.bar(x, df["P/B Ratio Current"], width, label="Current P/B Ratio")

# Bar chart for P/B ratio 6 months ago
ax.bar([i + width for i in x], df["P/B Ratio 4 Months Ago"], width, label="P/B Ratio 4 Months Ago")

# Add data labels
for i, v in enumerate(df["P/B Ratio Current"]):
    ax.text(i, v + 0.01, f"{v:.2f}", ha="center", va="bottom", fontsize=10)

for i, v in enumerate(df["P/B Ratio 4 Months Ago"]):
    ax.text(i + width, v + 0.05, f"{v:.2f}", ha="center", va="bottom", fontsize=10)

# Set the x-axis labels
ax.set_xticks([i + width / 2 for i in x])
ax.set_xticklabels(df.index)

# Set axis labels and chart title
ax.set_xlabel("Banks")
ax.set_ylabel("P/B Ratio")
plt.title("P/B Ratio Comparison: Current vs. 4 Months Ago")

# Show the legend
plt.legend()
plt.gca().spines['top'].set_visible(False)
plt.gca().spines['right'].set_visible(False)

# Display the chart
plt.show()
```
<img src="https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20231028/PB%20Big%204.png" width="600" />
