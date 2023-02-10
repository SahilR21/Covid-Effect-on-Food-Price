# Covid-Effect-on-Food-Price
Aim to check the effect of covid lockdown on the commodity prices during the first covid lockdown
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
from scipy.stats import linregress

# Load the data from the excel file
df = pd.read_excel("food_prices.xlsx")

# Convert the date column to numerical representation (number of days since a certain date)
df["Date"] = (df["Date"] - pd.Timestamp("2020-01-01")).dt.days

# Create a binary variable representing whether the observation is before or after the lockdown
df["lockdown"] = (df["Date"] >= (pd.Timestamp("2020-03-24") - pd.Timestamp("2020-01-01")).days).astype(int)

# Plot the multiple food prices against the binary variable representing lockdown
plt.figure(figsize=(10, 6))
for i, food_price in enumerate(df.columns[:-1]):
    plt.subplot(5, 5, i+1)

    # Scatter plot of food price and lockdown
    plt.scatter(df["lockdown"], df[food_price], s=10)
    plt.xlabel("Lockdown")
    plt.ylabel(food_price)

    # Fit the trendline
    slope, intercept, r_value, p_value, std_err = linregress(df["lockdown"], df[food_price])
    x = np.array([0, 1])
    y = slope * x + intercept

    # Plot the trendline
    plt.plot(x, y, color="red")

    # Annotate the R^2 value
    plt.annotate("R^2 = {:.2f}".format(r_value ** 2), (0.05, 0.95), xycoords="axes fraction", fontsize=10, ha="left",
                 va="top")

plt.tight_layout()
plt.show()
