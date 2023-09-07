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

html1 = requests.get(url1).content
df_list1 = pd.read_html(html1)
df1 = df_list1[1].iloc[:,[0,2,6]]
df1.rename(columns = {df1.columns[2]: '2021/03'}, inplace = True)

html2 = requests.get(url2).content
df_list2 = pd.read_html(html2)
df2 = df_list2[1].iloc[:,[0,6]]
df2.rename(columns = {df2.columns[1]: '2021/06'}, inplace = True)

html3 = requests.get(url3).content
df_list3 = pd.read_html(html3)
df3 = df_list3[1].iloc[:,[0,6]]
df3.rename(columns = {df3.columns[1]: '2021/09'}, inplace = True)

html4 = requests.get(url4).content
df_list4 = pd.read_html(html4)
df4 = df_list4[1].iloc[:,[0,6]]
df4.rename(columns = {df4.columns[1]: '2021/12'}, inplace = True)

html5 = requests.get(url5).content
df_list5 = pd.read_html(html5)
df5 = df_list5[1].iloc[:,[0,6]]
df5.rename(columns = {df5.columns[1]: '2022/03'}, inplace = True)

html6 = requests.get(url6).content
df_list6 = pd.read_html(html6)
df6 = df_list6[1].iloc[:,[0,6]]
df6.rename(columns = {df6.columns[1]: '2022/06'}, inplace = True)

html7 = requests.get(url7).content
df_list7 = pd.read_html(html7)
df7 = df_list7[1].iloc[:,[0,6]]
df7.rename(columns = {df7.columns[1]: '2022/09'}, inplace = True)

html8 = requests.get(url8).content
df_list8 = pd.read_html(html8)
df8 = df_list8[1].iloc[:,[0,6]]
df8.rename(columns = {df8.columns[1]: '2022/12'}, inplace = True)

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
