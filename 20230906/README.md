# Consumer Spending, Deficit, and Job Market
## Increasing Deliquency Rate of Credit Card Loans
```
import pandas as pd
from fredapi import Fred
import matplotlib.pyplot as plt

# replace 'YOUR_API_KEY' with your actual FRED API key
fred = Fred(api_key=YOUR_API_KEY)

# retrieve data for the series 'DRCCLACBS'
delinquency = fred.get_series('DRCCLACBS') # Delinquency Rate on Credit Card Loans, All Commercial Banks 

fig, ax = plt.subplots(figsize=(12,8))

ax.plot(delinquency)
ax.set_ylabel('Percent')
ax.set_title(" Delinquency Rate on Credit Card Loans, All Commercial Banks ")
```
<img src= 
"https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20230906/Delinquency%20Rate%20on%20Credit%20Card%20Loans.png"
 width="600" 
  />
  
## Deliquency Growth Rate YoY at All-Time High
```
# Calculate the rate of change using the 'pct_change' method
rate_of_change = delinquency.pct_change(periods=4) * 100

# Create a DataFrame with the original Series and the rate of change as a new column
delinquency1 = pd.DataFrame({'Delinquency': delinquency, 'Rate_of_Change': rate_of_change})
delinquency1.iloc[1,1] = 0

fig1, ax1 = plt.subplots(figsize=(12,8))

ax1.plot(delinquency1['Rate_of_Change'])
ax1.set_ylabel('Rate of Change')
ax1.set_title("YoY Growth in Credit Card Delinquency")

plt.axhline(y=0, color = 'black')
plt.axhline(y=delinquency1['Rate_of_Change'].max(), color='red', linestyle='--')
plt.annotate('Source: FRED', (0,0), (-80,-20), fontsize=6, xycoords='axes fraction', textcoords='offset points', va='top')
```
<img src= 
"https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20230906/YoY%20Growth%20in%20Credit%20Card%20Delinquency.png" 
  />

```
jolts2 = fred.get_series('JTSQUR') #Quits: Total Nonfarm
jolts2 = jolts2.loc["2019-09-01":]
jolts3 = fred.get_series('JTSLDR') #Layoffs and Discharges: Total Nonfarm
jolts3 = jolts3.loc["2019-09-01":]

# Create a new figure with two y-axes
fig2, ax2 = plt.subplots(figsize=(12, 6))

# Plot the first dataset (Personal Savings Rate) using the left y-axis
ax2.set_xlabel('Year')
ax2.set_ylabel('Rate', color='black')
ax2.plot(jolts2, label='Quits', color='blue')
ax2.tick_params(axis='y', labelcolor='blue')

# Create a secondary y-axis on the right for the second dataset (Consumer Loans)
ax3 = ax2.twinx()
ax3.set_ylabel('Rate', color='red')
ax3.plot(jolts3, label='Layoffs and Discharges', color='red')
ax3.tick_params(axis='y', labelcolor='red')

# Add legends for both datasets
ax2.legend(loc='upper left')
ax3.legend(loc='upper right')

# Show the plot
plt.title('Quit vs. Layoffs (Total Non-Farm)')
plt.annotate('Source: FRED JOLT', (0,0), (-80,-20), fontsize=6, xycoords='axes fraction', textcoords='offset points', va='top')
plt.grid()
plt.show()
```

** Job Market: Stagnant layoffs and declining quitting

