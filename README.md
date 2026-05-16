# Ex.No: 02 LINEAR AND POLYNOMIAL TREND ESTIMATION
Date: 28/04/26
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
data = pd.read_csv("C:/Users/admin/Downloads/index_1.csv")

# Convert datetime column
data['datetime'] = pd.to_datetime(
    data['datetime'],
    errors='coerce'
)

data = data.dropna(subset=['datetime'])

daily_data = data.groupby(
    data['datetime'].dt.date
).size().reset_index(name='Orders')

daily_data.columns = ['Date', 'Orders']

daily_data = daily_data.sort_values('Date')

# Convert to numeric index
dates = np.arange(len(daily_data))
orders = daily_data['Orders'].values

# Center X
X = dates - len(dates)//2

```
A - LINEAR TREND ESTIMATION
```
x2 = X**2
xy = X * orders

n = len(dates)

b = (n*np.sum(xy) - np.sum(orders)*np.sum(X)) / (n*np.sum(x2) - (np.sum(X)**2))
a = (np.sum(orders) - b*np.sum(X)) / n

linear_trend = a + b*X
```

B- POLYNOMIAL TREND ESTIMATION
```
x3 = X**3
x4 = X**4
x2y = x2 * orders

coeff = [
    [n, np.sum(X), np.sum(x2)],
    [np.sum(X), np.sum(x2), np.sum(x3)],
    [np.sum(x2), np.sum(x3), np.sum(x4)]
]

Y = [
    np.sum(orders),
    np.sum(xy),
    np.sum(x2y)
]

A = np.array(coeff)
B = np.array(Y)

a_poly, b_poly, c_poly = np.linalg.solve(A, B)

poly_trend = a_poly + b_poly*X + c_poly*(X**2)
```
### VISUALIZING RESULTS
```
print(f"Linear Trend: y = {a:.2f} + {b:.2f}x")
print(f"Polynomial Trend: y = {a_poly:.2f} + {b_poly:.2f}x + {c_poly:.2f}x²")

plt.figure(figsize=(10,6))
plt.plot(dates, orders, marker='o', label='Actual Data')
plt.plot(dates, linear_trend, 'k--', label='Linear Trend')

plt.title("Daily Coffee Orders - Linear Trend")
plt.xlabel("Time Index")
plt.ylabel("Orders")
plt.legend()
plt.show()

plt.figure(figsize=(10,6))
plt.plot(dates, orders, marker='o', label='Actual Data')
plt.plot(dates, poly_trend, 'k--', label='Polynomial Trend')

plt.title("Daily Coffee Orders - Polynomial Trend")
plt.xlabel("Time Index")
plt.ylabel("Orders")
plt.legend()
plt.show()
```

### OUTPUT
## EQUATIONS:
<img width="420" height="56" alt="image" src="https://github.com/user-attachments/assets/5cc1421f-0994-4c69-a3b0-e6fa1c8d5a29" />

A - LINEAR TREND ESTIMATION
<img width="966" height="606" alt="image" src="https://github.com/user-attachments/assets/8eb85271-7529-4cb5-b2f3-a572ee65cb34" />


B- POLYNOMIAL TREND ESTIMATION

<img width="939" height="557" alt="image" src="https://github.com/user-attachments/assets/93154eb3-3fed-4ded-b051-a96b824a2460" />

### RESULT:
Thus the python program for linear and Polynomial Trend Estiamtion has been executed successfully.
