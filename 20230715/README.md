## US Debt Level Reaches New High

```
import pandas as pd
from fredapi import Fred
import matplotlib.pyplot as plt

# replace 'YOUR_API_KEY' with your actual FRED API key
key = 'fe9f42b07c731caccc44b8a79c0d16df'
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