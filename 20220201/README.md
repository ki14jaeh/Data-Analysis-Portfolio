```
import requests
import pandas as pd
import matplotlib.pyplot as plt

url8 = 'https://www.federalreserve.gov/releases/lbr/20221231/default.htm'
url7 = 'https://www.federalreserve.gov/releases/lbr/20220930/default.htm'
url6 = 'https://www.federalreserve.gov/releases/lbr/20220630/default.htm'
url5 = 'https://www.federalreserve.gov/releases/lbr/20220331/default.htm'
url4 = 'https://www.federalreserve.gov/releases/lbr/20211231/default.htm'
url3 = 'https://www.federalreserve.gov/releases/lbr/20210930/default.htm'
url2 = 'https://www.federalreserve.gov/releases/lbr/20210630/default.htm'
url1 = 'https://www.federalreserve.gov/releases/lbr/20210331/default.htm'

url_column_pairs = [
    (url1, '2021/03'),
    (url2, '2021/06'),
    (url3, '2021/09'),
    (url4, '2021/12'),
    (url5, '2022/03'),
    (url6, '2022/06'),
    (url7, '2022/09'),
    (url8, '2022/12')
]

# Initialize an empty dictionary to store the dataframes
dfs = {}

# Loop through the URL-column pairs
for url, column_name in url_column_pairs:
    html = requests.get(url).content
    df_list = pd.read_html(html)
    df = df_list[1].iloc[:, [0, 6]]
    df.rename(columns={df.columns[1]: column_name}, inplace=True)
    dfs[column_name] = df

df_list = [df1, df2, df3, df4, df5, df6, df7, df8]
from functools import reduce
df_final = reduce(lambda x, y: pd.merge(x,y, on='Bank Name / Holding Co Name'), df_list)

# Create a figure and axis object
fig_bb, ax_bb = plt.subplots(figsize=(12,8))

# Loop through the dictionary and plot each bank's deposit trend
for i in range(4):
    bank_name = df_final.iloc[i, 0]
    data = pd.to_numeric(df_final.iloc[i, 2:], errors='coerce')
    ax_bb.plot(data, label=bank_name)

# Set the axis labels and title
ax_bb.set_xlabel('Quarter')
ax_bb.set_ylabel('Deposit Amount ($)')
ax_bb.set_title('Big Bank Loan Trends')

# Add a legend and display the plot
ax_bb.legend()
plt.show()
```
d
```
# Create a figure and axis object
fig_rb, ax_rb = plt.subplots(figsize=(12,8))

# Loop through the dictionary and plot each bank's deposit trend
rb_index = [20, 23, 30, 32, 33, 41, 53]
#20 - First Republic Bank
#23 - Silicon Valley Bank
#30 - First Horizon Bank
#32 - Signature Bank
#33 - Zions Bank
#41 - Western Alliance Bank
#53 - Pacific Western Bank
for i in rb_index:
    bank_name = df_final.iloc[i, 0]
    data = pd.to_numeric(df_final.iloc[i, 2:], errors='coerce')
    ax_rb.plot(data, label=bank_name)

# Set the axis labels and title
ax_rb.set_xlabel('Quarter')
ax_rb.set_ylabel('Deposit Amount ($)')
ax_rb.set_title('Regional Bank Deposit Trends')

# Add a legend and display the plot
ax_rb.legend()
plt.show()
```
