# Halloween Strategy

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
| Year Range    | Oct.31 to May 1 Change (%) |Jan.1 to Dec. 31 Change (%) |
| ------------- |:--------------------------:|:--------------------------:|
| 2000-2001     | -11.40                     | -10.22                     |
| 2001-2002     | 2.52                       | 8.33                       |
| 2002-2003     | 3.45                       | -0.67                      |
| 2003-2004     | 5.39                       |  5.83                      |
| 2004-2005     | 2.80                       |  7.20                      |
| 2005-2006     | 8.13                       |  3.42                      |
| 2006-2007     | 7.86                       |  2.81                      |
| 2007-2008     | -9.04                      |  -5.23                     |
| 2008-2009     | --9.42                     |  -6.76                    |
| 2009-2010     | 14.52                      |   7.62                     |
| 2010-2011     | 14.93                      |   6.19                     |
| 2011-2012     | 12.17                      |              0.34          |
| 2012-2013     | 12.08                      |       0.99                 |
| 2013-2014     | 7.24                       |          5.23              |
| 2014-2015     | 4.47                       |           2.02             |
| 2015-2016     | 0.10                       |              -1.70         |
| 2016-2017     | 12.33                      |               5.30         |
| 2017-2018     | 3.09                       |               4.68         |
| 2018-2019     | 7.82                       |           -7.56            |
| 2019-2020     | -6.81                      |          6.36              |
| 2020-2021     | 27.87                      |         14.87              |
| 2021-2022     | -9.93                      |           3.31             |
| 2022-2023     | 7.64                       |           -0.84            |


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
