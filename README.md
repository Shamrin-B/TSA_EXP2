# Ex.No: 02 LINEAR AND POLYNOMIAL TREND ESTIMATION
Date:13.05.2026
### AIM:
To Implement Linear and Polynomial Trend Estiamtion Using Python.

### ALGORITHM:
Import necessary libraries (NumPy, Matplotlib)

Load the dataset

Calculate the linear trend values using least square method

Calculate the polynomial trend values using least square method

End the program
### PROGRAM:
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# Load dataset
data = pd.read_csv('/content/gold_rate_history.csv')

# Convert Date column into datetime
data['Date'] = pd.to_datetime(data['Date'])

# Set Date as index
data.set_index('Date', inplace=True)

# Yearly average gold rate
resampled_data = data['Standard Gold (22 K)'].resample('YE').mean().to_frame()

# Convert index to year
resampled_data.index = resampled_data.index.year

# Reset index
resampled_data.reset_index(inplace=True)

# Rename Date column to Year
resampled_data.rename(columns={'Date': 'Year'}, inplace=True)

# Convert into list
years = resampled_data['Year'].tolist()

gold_rate = resampled_data['Standard Gold (22 K)'].tolist()

# --------------------------------
# Linear Trend Estimation
# --------------------------------

X = [i - years[len(years)//2] for i in years]

x2 = [i**2 for i in X]

xy = [i*j for i, j in zip(X, gold_rate)]

n = len(years)

b = (n * sum(xy) - sum(gold_rate) * sum(X)) / \
    (n * sum(x2) - (sum(X)**2))

a = (sum(gold_rate) - b * sum(X)) / n

linear_trend = [a + b * X[i] for i in range(n)]

# --------------------------------
# Polynomial Trend (Degree 2)
# --------------------------------

x3 = [i**3 for i in X]

x4 = [i**4 for i in X]

x2y = [i*j for i, j in zip(x2, gold_rate)]

coeff = [
    [len(X), sum(X), sum(x2)],
    [sum(X), sum(x2), sum(x3)],
    [sum(x2), sum(x3), sum(x4)]
]

Y = [sum(gold_rate), sum(xy), sum(x2y)]

A = np.array(coeff)

B = np.array(Y)

a_poly, b_poly, c_poly = np.linalg.solve(A, B)

poly_trend = [
    a_poly + b_poly * X[i] + c_poly * (X[i]**2)
    for i in range(n)
]

# --------------------------------
# Add Trend Values
# --------------------------------

resampled_data['Linear Trend'] = linear_trend

resampled_data['Polynomial Trend'] = poly_trend

# Set Year as index
resampled_data.set_index('Year', inplace=True)

# --------------------------------
# Plot Graph
# --------------------------------

plt.figure(figsize=(10,6))

plt.plot(
    resampled_data.index,
    resampled_data['Standard Gold (22 K)'],
    marker='o',
    label='Original Data'
)

plt.plot(
    resampled_data.index,
    resampled_data['Linear Trend'],
    linestyle='--',
    label='Linear Trend'
)

plt.plot(
    resampled_data.index,
    resampled_data['Polynomial Trend'],
    marker='o',
    label='Polynomial Trend'
)

plt.xlabel('Year')

plt.ylabel('Gold Rate')

plt.title('Trend Analysis of Gold Rate')

plt.legend()

plt.grid(True)

plt.show()

# --------------------------------
# Equations
# --------------------------------

print(f"Linear Trend: y = {a:.2f} + {b:.2f}x")

print(
    f"Polynomial Trend: y = {a_poly:.2f} + "
    f"{b_poly:.2f}x + {c_poly:.2f}x²"
)
```

### OUTPUT
<img width="859" height="547" alt="image" src="https://github.com/user-attachments/assets/79434ec4-0250-4984-9b35-07d03e330d93" />


### RESULT:
Thus the python program for linear and Polynomial Trend Estiamtion has been executed successfully.
