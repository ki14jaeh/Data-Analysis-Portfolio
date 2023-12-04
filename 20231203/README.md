# Halloween Strategy
The Halloween effect is a strategy based on the hypothesis that the market performs better from November to next year April compared to the market from May through October. You can find academic studies done on this hypothesis (e.g. [The Halloween effect: Trick or Treat](https://www.sciencedirect.com/science/article/abs/pii/S1057521910000608)). Today we will explore whether the performance difference is statistically significant. 

```
import yfinance as yf
import pandas as pd
from datetime import datetime
import matplotlib.pyplot as plt

# Define the ticker symbols
SPX = '%5EGSPC'  # S&P500

# Calculate the end date as the current date and time
end_date = datetime.now()
start_date_str = "2000-10-31"
start_date = datetime.strptime(start_date_str, "%Y-%m-%d")

# Download historical data for the given ticker symbol
df = yf.download(SPX, start=start_date, end=end_date).reset_index()[['Date', 'Adj Close']]

# Create a DataFrame with all days in the range
all_dates = pd.date_range(start=start_date, end=end_date, freq='D')
all_data = pd.DataFrame({'Date': all_dates})

# Merge the available data for May 1st and October 31st with the complete date range
merged_data = pd.merge_asof(all_data, df, on='Date', direction='nearest')

# Filter data for May 1st and October 31st after the merge
halloween_data = merged_data.loc[
    ((merged_data['Date'].dt.day == 1) & (merged_data['Date'].dt.month == 5)) | 
    ((merged_data['Date'].dt.day == 31) & (merged_data['Date'].dt.month == 10)), 
    ['Date', 'Adj Close']
]

# Calculate differences between alternate rows directly
halloween = (halloween_data['Adj Close'].iloc[1::2].reset_index(drop=True) / halloween_data['Adj Close'].iloc[::2].reset_index(drop=True) - 1) * 100
halloween = halloween.reset_index()

start_year = 2000
end_year = start_year + len(halloween)

separate_df = pd.DataFrame({
    'Year Range': [f"{year}-{year + 1}" for year in range(start_year, end_year)],
})

final1 = pd.merge(separate_df, halloween, left_index=True, right_index=True, suffixes=('_left', '_right'))
final1 = final1[['Year Range', 'Adj Close']]

# Filter data for Jan 1st and December 31st after the merge
fullyear_data = merged_data.loc[
    ((merged_data['Date'].dt.day == 1) & (merged_data['Date'].dt.month == 1)) | 
    ((merged_data['Date'].dt.day == 31) & (merged_data['Date'].dt.month == 12)), 
    ['Date', 'Adj Close']
]

# Calculate differences between alternate rows directly
fullyear = (fullyear_data['Adj Close'].iloc[1::2].reset_index(drop=True) / halloween_data['Adj Close'].iloc[::2].reset_index(drop=True) - 1) * 100
fullyear = fullyear.reset_index()

final2 = pd.merge(separate_df, fullyear, left_index=True, right_index=True, suffixes=('_left', '_right'))
final2 = final2[['Year Range', 'Adj Close']]
```
| Year     | Halloween Investing (Nov. - Apr., %) | Rest (May - Oct., %) |
| -------- |:--------------------------:|:------------------:|
| 1985     | 22.97                      | 3.75               |
| 1986     | 18.19                      | -12.58             |
| 1987     | 2.18                       | 6.66               |
| 1988     | 10.77                      | 10.11              |
| 1989     | -3.05                      | -8.5               |
| 1990     | 22.25                      | 3.20               |
| 1991     | 6.04                       | 1.49               |
| 1992     | 4.13                       | 6.57               |
| 1993     | -3.88                      | 4.27               |
| 1994     | 9.79                       | 13.08              |
| 1995     | 11.97                      | 7.74               |
| 1996     | 13.86                      | 14.54              |
| 1997     | 21.55                      | -1.99              |
| 1998     | 20.11                      | 1.42               |
| 1999     | 8.43                       | -2.65              |
| 2000     | -12.09                     | -16.32             |
| 2001     | -0.66                      | -18.47             |
| 2002     | 1.77                       | 14.67              |
| 2003     | 5.39                       | 2.10               |
| 2004     | 2.33                       | 3.86               |
| 2005     | 8.52                       | 5.57               |
| 2006     | 8.38                       | 4.24               |
| 2007     | -8.14                      | -31.26             |
| 2008     | -9.90                      | 18.08              |
| 2009     | 13.79                      | -0.19              |
| 2010     | 15.13                      | -7.93              |
| 2011     | 14.74                      | 0.45               |
| 2012     | 11.91                      | 10.98              |
| 2013     | 6.94                       | 7.13               |
| 2014     | 3.34                       | -1.37              |
| 2015     | -1.84                      | 2.15               |
| 2016     | 13.10                      | 7.83               |
| 2017     | 2.66                       | 2.14               |
| 2018     | 7.50                       | 3.89               |
| 2019     | -5.04                      | 15.52              |
| 2020     | 26.31                      | 10.34              |
| 2021     | -10.45                     | -6.82              |
| 2022     | 8.09                       | 0.62               |


|                | Halloween Investing (Nov. - Apr., %) | Rest (May - Oct., %) |
| -------------- |:--------------------------:|:------------------------------:|
|Bull Market     | 29                         | 27                             |
|Bear Market     | 9                          | 11                             |
|Average Gain    | 7.03%                      | 1.96%                          |
|Bull Market Gain| 11.11%                     | 6.76%                          |
|Bear Market Gain| -6.12%                     | -9.83%                         |

```
# Create subplots with a shared y-axis
fig, ax = plt.subplots()

# Plot 'final1' DataFrame as a bar chart with rounded data labels
bars = ax.bar(final1['Year Range'], final1['Adj Close'], label='Oct 31st and May 1st Differences', alpha=0.7, color='blue')
ax.set_xlabel('Year Range')
ax.set_ylabel('Percentage Difference (%)')
ax.set_title('Bar Chart of Percentage Differences Over Time')
plt.xticks(rotation=45, ha='right')

# Add rounded data labels with adjusted positions
for bar, value in zip(bars, final1['Adj Close']):
    va = 'bottom' if value >= 0 else 'top'  # Set va based on the sign of the value
    ax.text(bar.get_x() + bar.get_width() / 2, value, f'{round(value, 1)}', 
            ha='center', va=va, fontsize=8, rotation=45, color='black')  # Adjust fontsize, rotation, and color

# Overlay 'final2' data as a line chart on the same plot
ax.plot(final2['Year Range'], final2['Adj Close'], color='red', label='Jan 1st and Dec 31st Differences', marker='o')

# Display legends
ax.legend(loc='upper left')

# Automatically adjust subplot parameters to prevent overlap
plt.tight_layout()

plt.show()
```
<img src="https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20231203/Halloween%20investing.png" width="600" />
