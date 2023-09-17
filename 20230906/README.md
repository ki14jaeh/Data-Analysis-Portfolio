# Consumer Spending, Deficit, and Job Market
## Increasing Deliquency Rate of Credit Card Loans

```
from fredapi import Fred
import matplotlib.pyplot as plt
from datetime import datetime, timedelta

# Replace 'YOUR_API_KEY' with your FRED API key
api_key = 'YOUR_API_KEY'

# Create a Fred object with your API key
fred = Fred(api_key=api_key)

# Calculate the start date as three years ago from the current date
end_date = datetime.now()
start_date = end_date - timedelta(days=365 * 3)  # Three years ago

# Fetch data for DRCCLACBS and BOGZ1FA176007025Q within the past 3 years
data1 = fred.get_series("DRCCLACBS", start_date=start_date, end_date=end_date).loc['2019-10-01':]
data2 = fred.get_series("BOGZ1FA176007025Q", start_date=start_date, end_date=end_date).loc['2019-10-01':]

# Create a figure and two subplots with different y-axes
fig, ax1 = plt.subplots(figsize=(10, 6))

# Plot the first series (DRCCLACBS) on the first axis (ax1)
ax1.plot(data1.index, data1.values, label="Delinquency Rate (Credit Card, All Commercial Banks)", color="blue")
ax1.set_xlabel("Date")
ax1.set_ylabel("Percent", color="blue")
ax1.tick_params(axis='y', labelcolor="blue")
ax1.grid(True)

# Create a second axis (ax2) that shares the same x-axis
ax2 = ax1.twinx()

# Plot the second series (BOGZ1FA176007025Q) on the second axis (ax2)
ax2.plot(data2.index, data2.values, label="Personal Saving", color="green")
ax2.set_ylabel("Saving in Millions of Dollars", color="green")
ax2.tick_params(axis='y', labelcolor="green")

# Add a legend for both series
lines, labels = ax1.get_legend_handles_labels()
lines2, labels2 = ax2.get_legend_handles_labels()
ax2.legend(lines + lines2, labels + labels2, loc="upper left")

# Set the title
plt.title("Increasing Delinquency Rate and Falling Savings Rate")

# Ensure that labels fit within the figure
plt.tight_layout()

# Show the plot
plt.show()
```
<img src= 
"https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20230906/Delinquency%20and%20Savings.png"
 width="600" 
  />
  
## Deliquency Growth Rate YoY at All-Time High
```
from fredapi import Fred
import matplotlib.pyplot as plt
from datetime import datetime, timedelta

# Replace 'YOUR_API_KEY' with your FRED API key
api_key = 'fe9f42b07c731caccc44b8a79c0d16df'

# Create a Fred object with your API key
fred = Fred(api_key=api_key)

# Calculate the start date as three years ago from the current date
end_date = datetime.now()
start_date = end_date - timedelta(days=365 * 3)  # Three years ago

# Fetch data for DRCCLACBS and BOGZ1FA176007025Q within the past 3 years
data1 = fred.get_series("DRCCLACBS", start_date=start_date, end_date=end_date).loc['2019-10-01':]
data2 = fred.get_series("BOGZ1FA176007025Q", start_date=start_date, end_date=end_date).loc['2019-10-01':]

# Create a figure and two subplots with different y-axes
fig, ax1 = plt.subplots(figsize=(10, 6))

# Plot the first series (DRCCLACBS) on the first axis (ax1)
ax1.plot(data1.index, data1.values, label="Delinquency Rate (Credit Card, All Commercial Banks)", color="blue")
ax1.set_xlabel("Date")
ax1.set_ylabel("Percent", color="blue")
ax1.tick_params(axis='y', labelcolor="blue")
ax1.grid(True)

# Create a second axis (ax2) that shares the same x-axis
ax2 = ax1.twinx()

# Plot the second series (BOGZ1FA176007025Q) on the second axis (ax2)
ax2.plot(data2.index, data2.values, label="Personal Saving", color="green")
ax2.set_ylabel("Saving in Millions of Dollars", color="green")
ax2.tick_params(axis='y', labelcolor="green")

# Add a legend for both series
lines, labels = ax1.get_legend_handles_labels()
lines2, labels2 = ax2.get_legend_handles_labels()
ax2.legend(lines + lines2, labels + labels2, loc="upper left")

# Set the title
plt.title("Increasing Delinquency Rate and Falling Savings Rate")

# Ensure that labels fit within the figure
plt.tight_layout()

# Show the plot
plt.show()
```
<img src= 
"https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20230906/YoY%20Growth%20in%20Credit%20Card%20Delinquency.png"
 width = "600"
  />

## Job Market: Stagnant layoffs and declining quitting
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
<img src= 
"https://github.com/ki14jaeh/Data-Analysis-Portfolio/blob/main/20230906/Quit%20vs%20Layoffs.png"
width = "600"
  />



