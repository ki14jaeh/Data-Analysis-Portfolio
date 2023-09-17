# What is S&P 500
Companies included in the S&P 500 index are selected by the index committee who asses the company's merit using the following criteria[^1]:
* Company should be from the U.S.
* Market Capitalization should exceed a certain value ($14.5 B as of July 2023) 
* Shares must be liquid (i.e. listed in a U.S. exchange such as NYSE, Nasdaq, etc.)
* At least 50% of outstanding shares must be available for public trading
* Must report positive earnings in the most recent quarter
* Sum of its earnings in the previous four quarters must be positive

The S&P 500 tracks the performance of about 500 companies in the U.S. and offers a picture of the health of the 11 sectors it covers and the broader economy. It is a prominent indicator that gauges American equities' performance and often utilized as the benchmark growth rate.

# S&P Quarterly Rebalance
On September 1, index provider S&P Dow Jones Indices (SPDJI) announced changes in their U.S. large-cap benchmark. Notable changes include the entrance of world's largest PE firm Blackstone ($BX) and Airbnb ($ABNB) and deletion of insurance company Lincoln National ($LNC) and consumer conglomerate Newell Brands ($NWL)[^2].

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

# VOO: S&P 500 as an investment in 2023 Q3
VOO, a popular S&P 500 index fund, is considered top-heavy at the moment. The "S&P 7" - Apple, Microsoft, Amazon, Alphabet, Meta, Tesla, and Nvidia accounts for 27.6% of its total holdings, combined to more than $1 trillion in assets[^3]. 

```
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib.ticker as mtick

VOO_top = pd.Series([0.077, 0.068, 0.036, 0.031, 0.028, 0.019, 0.017, 0.016, 0.012, 0.012], 
                    index=["Apple", "Microsoft", "Alphabet", "Amazon", "Nvidia", "Tesla", "Meta", "Berkshire", "United Health", "Exxon Mobil"]).reset_index()

## Data as of 9/13/2023 via https://institutional.vanguard.com/iippdf/pdfs/FS968R.pdf

df = pd.DataFrame(VOO_top)
df.columns = ['Name', '% Asset']

# Sort the DataFrame in descending order by '% Asset'
df = df.sort_values(by='% Asset', ascending=False)

fig, ax = plt.subplots()

# Plot the data
bars = ax.bar(df['Name'], df['% Asset'], width=0.8, edgecolor="white")

# Format y-axis as percentages
ax.yaxis.set_major_formatter(mtick.PercentFormatter(1.0, decimals=2))

plt.xticks(rotation='vertical')

# Add data labels on top of the bars
for bar in bars:
    height = bar.get_height()
    ax.annotate(f'{height:.2%}',  # The text label
                xy=(bar.get_x() + bar.get_width() / 2, height),  # Position of the label
                xytext=(0, 2),  # Offset text by 3 points vertically
                textcoords="offset points",
                ha='center',  # Horizontal alignment
                va='bottom',   # Vertical alignment
                fontsize=10,   # Font size
                color='black')  # Font color

# Remove the top and right spines
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)

ax.set_title("Ten largest holdings and % of total net assets")

plt.show()
```
<img src="https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20230913/10%20largest%20voo.png" width="600" />

Though stock prices consider a company's growth potential, PE ratios of Amazon and Nvidia seem unrealistically high with a PER of 111.01 and 105.96 when compared to the S&P 500 PE ratio of 25.41 (as of Sep. 15, 2023). Amazon stock prices are up 63.59% YTD and Nvidia stock up 206.67% and it is questionable if these mega cap companies are projected to grow at a similar rate with major headwinds like U.S.-China tensions and high interest rates.
```
# Data as of 9/15/2023 4:00 PM EDT
SP7 = pd.Series([29.40, 34.11, 29.10, 111.01, 105.96, 78.04, 35.17], 
                index=["Apple", "Microsoft", "Alphabet", "Amazon", "Nvidia", "Tesla", "Meta"])

# Create a DataFrame
df = pd.DataFrame({'Company': SP7.index, 'P/E TTM': SP7.values})

# Sort the DataFrame in descending order by 'P/E TTM'
df = df.sort_values(by='P/E TTM', ascending=False)

# Create a horizontal bar chart
fig, ax = plt.subplots(figsize=(8, 6))  # Set the figure size

bars = ax.barh(df['Company'], df['P/E TTM'], color='skyblue')  # Use barh for horizontal bars

# Add labels to the bars
for bar in bars:
    width = bar.get_width()
    ax.annotate(f'{width:.2f}',  # Format the label with two decimal places
                xy=(width, bar.get_y() + bar.get_height() / 2),
                xytext=(5, 0),  # 5 points horizontal offset for better alignment
                textcoords="offset points",
                ha='left', va='center')

# Set the title and labels
ax.set_title("S&P 7 P/E TTM (as of Sep 15, 2023)")
ax.set_xlabel("P/E TTM")
ax.set_ylabel("Company")

# Remove the top and right spines
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)

# Add a red vertical line at S&P 500 PE ratio as of 9/15/2023 4:00 PM EDT (as of 9/15/2023 4:00 PM EDT)
ax.axvline(x=27, color='red', linestyle='--', label='S&P 500 PE Ratio')
ax.text(15, 6.8, 'S&P 500 PE Ratio', color='red', fontsize=7)

# Show the plot
plt.tight_layout()  # Ensure that labels fit within the figure
plt.show()
```
<img src="https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20230913/sp7%20pe%20ttm.png" width="600" />


[^1]: https://www.spglobal.com/spdji/en/documents/methodologies/methodology-sp-us-indices.pdf
[^2]: https://www.reuters.com/markets/us/blackstone-airbnb-set-join-sp-500-shares-climb-2023-09-01/
[^3]: https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20230913/2023Q3%20VOO%20Holdings.pdf
