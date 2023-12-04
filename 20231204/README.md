# GDP and Spending Reports
## GDP
The following code grabs the Percent Change From Preceding Period in Real Gross Domestic Product (GDP). GDP and its growth rate is often used as an indicator of a country's economic health. It is the total market value of goods and services from that country. GDP(Y) is often denoted as a sum of consumption(C), investments(I), government expenditures(G), and net exports(X-M).

$Y = C + I +G +(X-M)$

```
import pandas as pd
import requests
import json

user_id = "YOUR_API_KEY"
url1 = f'https://apps.bea.gov/api/data/?UserID={user_id}&method=GetData&DataSetName=NIPA&TableName=T10101&Frequency=A,Q&Year=All&ResultFormat=JSON'
response = requests.get(url)
json_data  = json.loads(response.text)
bea_data1 = pd.json_normalize(json_data['BEAAPI']['Results']['Data'])
bea_data1.to_csv('nipa_T10101.csv', index=False)
bea_data1

import numpy as np
import matplotlib.pyplot as plt

GDP = bea_data1.loc[bea_data1['LineDescription'] == "Gross domestic product"].reset_index()
GDP_recent = GDP.loc[GDP['index'] >= 341]
i_values = np.arange(1, len(GDP_recent) + 1) 
GDP_recent_yearly = GDP_recent[~GDP_recent['TimePeriod'].str.contains('Q')]
GDP_recent_quarterly = GDP_recent[GDP_recent['TimePeriod'].str.contains('Q')]

PC = bea_data1.loc[bea_data1['LineDescription'] == "Personal consumption expenditures"].reset_index(drop=True).reset_index()
PC_recent = PC.loc[PC['index'] >= 341]
i_values = np.arange(1, len(PC_recent) + 1) 
PC_recent_yearly = PC_recent[~PC_recent['TimePeriod'].str.contains('Q')]
PC_recent_quarterly = PC_recent[PC_recent['TimePeriod'].str.contains('Q')]

# Convert 'DataValue' to numeric type
GDP_recent_yearly['DataValue'] = pd.to_numeric(GDP_recent_yearly['DataValue'])
GDP_recent_quarterly['DataValue'] = pd.to_numeric(GDP_recent_quarterly['DataValue'])
PC_recent_yearly['DataValue'] = pd.to_numeric(PC_recent_yearly['DataValue'])
PC_recent_quarterly['DataValue'] = pd.to_numeric(PC_recent_quarterly['DataValue'])

# Create subplots with a shared y-axis
fig, ax = plt.subplots(figsize=(20, 8))

# Plot Annual GDP Data
bars = ax.bar(GDP_recent_quarterly['TimePeriod'].tail(5), GDP_recent_quarterly['DataValue'].tail(5), alpha=0.7, color='blue',
              label="GDP")
# Plot Annual Personal Consumption Data on the same y-axis
plt.plot(PC_recent_quarterly['TimePeriod'].tail(5), PC_recent_quarterly['DataValue'].tail(5), color='red',
         label="Personal Consumption", marker='o')

# Set labels and title
ax.set_ylabel('Percent change, annual rate')
ax.set_title('GDP YoY (%) vs. Personal Consumption YoY (%)')

# Add data labels to the line plot
for i, txt in enumerate(PC_recent_quarterly['DataValue'].tail(5)):
    ax.annotate(f'{txt:.2f}%', (PC_recent_quarterly['TimePeriod'].tail(5).iloc[i], txt),
                textcoords="offset points", xytext=(0, 10), ha='center')

# Add data labels to the bar chart (GDP)
for bar in bars:
    yval = bar.get_height()
    ax.annotate(f'{yval:.2f}%', xy=(bar.get_x() + bar.get_width() / 2, yval),
                xytext=(0, 3), textcoords='offset points', ha='center', va='bottom')
# Display legends
ax.legend(loc='upper left')

# Display a horizontal line at y=0
plt.axhline(y=0, color='black')
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)
plt.show()
```
<img src="https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20231204/GDP%20and%20PC.png" width="800" />

2023Q3 GDP showed 5.20% growth at an annualized rate last quarter, which is the fastest it has grown since 2021Q4. Consumer spending also rose to 3.60%, which was lower than the expected 4.0%. 

```
url2 = f'https://apps.bea.gov/api/data/?UserID={user_id}&method=GetData&DataSetName=NIPA&TableName=T20303&Frequency=A,Q&Year=All&ResultFormat=JSON'
response = requests.get(url2)
json_data  = json.loads(response.text)
bea_data2 = pd.json_normalize(json_data['BEAAPI']['Results']['Data'])
bea_data2.to_csv('nipa_T10101.csv', index=False)
bea_data2
import numpy as np
import matplotlib.pyplot as plt

GDP = bea_data2.loc[bea_data2['LineDescription'] == "Gross domestic product"].reset_index()
GDP_recent = GDP.loc[GDP['index'] >= 341]
i_values = np.arange(1, len(GDP_recent) + 1) 
GDP_recent_yearly = GDP_recent[~GDP_recent['TimePeriod'].str.contains('Q')]
GDP_recent_quarterly = GDP_recent[GDP_recent['TimePeriod'].str.contains('Q')]

PC = bea_data2.loc[bea_data2['LineDescription'] == "Personal consumption expenditures (PCE)"].reset_index(drop=True).reset_index()
PC_recent = PC.loc[PC['index'] >= 341]
i_values = np.arange(1, len(PC_recent) + 1) 
PC_recent_yearly = PC_recent[~PC_recent['TimePeriod'].str.contains('Q')]
PC_recent_quarterly = PC_recent[PC_recent['TimePeriod'].str.contains('Q')]

services = bea_data2.loc[bea_data2['LineDescription'] == "Services"].reset_index(drop=True).reset_index()
services_recent = services.loc[services['index'] >= 341]
i_values = np.arange(1, len(services_recent) + 1) 
services_recent_yearly = services_recent[~services_recent['TimePeriod'].str.contains('Q')]
services_recent_quarterly = services_recent[services_recent['TimePeriod'].str.contains('Q')]

goods = bea_data2.loc[bea_data2['LineDescription'] == "Goods"].reset_index(drop=True).reset_index()
goods_recent = goods.loc[goods['index'] >= 341]
i_values = np.arange(1, len(goods_recent) + 1) 
goods_recent_yearly = goods_recent[~goods_recent['TimePeriod'].str.contains('Q')]
goods_recent_quarterly = goods_recent[goods_recent['TimePeriod'].str.contains('Q')]

# Convert 'DataValue' to numeric type
GDP_recent_yearly['DataValue'] = pd.to_numeric(GDP_recent_yearly['DataValue'])
GDP_recent_quarterly['DataValue'] = pd.to_numeric(GDP_recent_quarterly['DataValue'])
PC_recent_yearly['DataValue'] = pd.to_numeric(PC_recent_yearly['DataValue'])
PC_recent_quarterly['DataValue'] = pd.to_numeric(PC_recent_quarterly['DataValue'])
services_recent_yearly['DataValue'] = pd.to_numeric(services_recent_yearly['DataValue'])
services_recent_quarterly['DataValue'] = pd.to_numeric(services_recent_quarterly['DataValue'])
goods_recent_yearly['DataValue'] = pd.to_numeric(goods_recent_yearly['DataValue'])
goods_recent_quarterly['DataValue'] = pd.to_numeric(goods_recent_quarterly['DataValue'])

# Create subplots with a shared y-axis
fig, ax = plt.subplots()

# Plot Annual Personal Consumption Data on the same y-axis
plt.plot(goods_recent_quarterly['TimePeriod'].tail(10), goods_recent_quarterly['DataValue'].tail(10), color='red',
         label="Total", marker='o')
plt.plot(PC_recent_quarterly['TimePeriod'].tail(10), PC_recent_quarterly['DataValue'].tail(10), color='green',
         label="Goods", marker='o')
plt.plot(services_recent_quarterly['TimePeriod'].tail(10), services_recent_quarterly['DataValue'].tail(10), color='blue',
         label="Services", marker='o')

# Set labels and title
ax.set_xlabel('Year')
ax.set_xticklabels(ax.get_xticklabels(), rotation=45)
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)
plt.title('PCE, Goods and Services (last 10 quarters)')
plt.legend()
plt.show()
```
<img src="https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20231204/Consumer%20spending.png" width="800" />
Total Personal Consumption Expenditure as a result of increased spending from goods and services. Though interest rates remain high, personal consumption remains high, shrugging off the theoretical impact of a hawkish Fed policy. 

