## US Debt Level Reaches New High

```
import pandas as pd
from fredapi import Fred
import matplotlib.pyplot as plt

# replace 'YOUR_API_KEY' with your actual FRED API key
key = 'YOUR_API_KEY'
fred = Fred(api_key=key)

# retrieve data for the series 'FYFSD'
fedsd = fred.get_series('FYFSD') # Delinquency Rate on Credit Card Loans, All Commercial Banks 
fedsd = fedsd.loc["1980-06-30":]
fig, ax = plt.subplots(figsize=(12,8))

plt.axhline(y=0, color = 'black')
ax.plot(fedsd)
ax.set_ylabel('Percent')
ax.set_title("Federal Surplus or Deficit")
```

<img src= 
"https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20230715/US%20Debt%20Deficit%20Surplus.png"
 width="600" 
  />

```
# retrieve data for the series 'A091RC1Q027SBEA'
fedintpay = fred.get_series('A091RC1Q027SBEA') # Federal government current expenditures: Interest payments
fedintpay = fedintpay.loc["1980-01-01":]
fig1, ax1 = plt.subplots(figsize=(12,8))

ax1.plot(fedintpay)
ax1.set_ylabel('Percent')
ax1.set_title("Cost of US Debt (Estimated annualized deby payment)")

```
<img src= 
"https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20230715/Cost%20of%20US%20Debt.png"
 width="600" 
  />
