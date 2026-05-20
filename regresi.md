# Regresi Linier


# Analisis Regresi Linier — Data Koordinat Titik

## 1. Data Input

| Titik | Sumbu X | Sumbu Y |
|-------|---------|---------|
| A     | 2       | 2       |
| B     | 4       | 3       |
| C     | 5       | 5       |
| D     | 3       | 4       |
| E     | 3       | 3       |
| F     | 4       | 5       |
| G     | 5       | 6       |

---

## 2. Metode 1 — Analitik: β = (XᵀX)⁻¹XᵀY

### Kode

```python
import pandas as pd
import numpy as np

df = pd.read_excel("Data_Koordinat.xlsx")
X_vals = df["Sumbu X"].values
Y_vals = df["Sumbu Y"].values
n = len(X_vals)

X_mat   = np.column_stack([np.ones(n), X_vals])
Y_mat   = Y_vals.reshape(-1, 1)
XTX     = X_mat.T @ X_mat
XTY     = X_mat.T @ Y_mat
XTX_inv = np.linalg.inv(XTX)
beta    = XTX_inv @ XTY

print("X^T X =\n", XTX)
print("X^T Y =\n", XTY)
print("(X^T X)^-1 =\n", XTX_inv)
print("beta =\n", beta)
```

### Output

```
X^T X =
 [[  7.  26.]
 [ 26. 104.]]
X^T Y =
 [[ 28.]
 [112.]]
(X^T X)^-1 =
 [[ 2.         -0.5       ]
 [-0.5         0.13461538]]
beta =
 [[0.        ]
 [1.07692308]]
```

**β₀ = 0.0 &nbsp;|&nbsp; β₁ = 1.07692308**

---

## 3. Metode 2 — sklearn LinearRegression

### Kode

```python
from sklearn.linear_model import LinearRegression

model = LinearRegression()
model.fit(X_vals.reshape(-1, 1), Y_vals)

print("beta0 (intercept) =", model.intercept_)
print("beta1 (coef)      =", model.coef_[0])
```

### Output

```
beta0 (intercept) = -8.881784197001252e-16
beta1 (coef)      = 1.076923076923077
```

> β₀ sklearn = `-8.88e-16` ≈ 0 (floating-point precision, secara matematis sama)

---

## 4. Evaluasi Model

### Kode

```python
Y_pred = beta[0,0] + beta[1,0] * X_vals
SS_res = np.sum((Y_vals - Y_pred)**2)
SS_tot = np.sum((Y_vals - np.mean(Y_vals))**2)
R2     = 1 - SS_res / SS_tot

print("SS_res =", SS_res)
print("SS_tot =", SS_tot)
print("R2     =", R2)

for i in range(n):
    print(f"{df['Titik'].values[i]}: x={X_vals[i]}, y_aktual={Y_vals[i]}, "
          f"y_pred={Y_pred[i]:.4f}, residu={Y_vals[i]-Y_pred[i]:.4f}")
```

### Output

```
SS_res = 3.384615384615384
SS_tot = 12.0
R2     = 0.7179487179487181

A: x=2, y_aktual=2, y_pred=2.1538, residu=-0.1538
B: x=4, y_aktual=3, y_pred=4.3077, residu=-1.3077
C: x=5, y_aktual=5, y_pred=5.3846, residu=-0.3846
D: x=3, y_aktual=4, y_pred=3.2308, residu=0.7692
E: x=3, y_aktual=3, y_pred=3.2308, residu=-0.2308
F: x=4, y_aktual=5, y_pred=4.3077, residu=0.6923
G: x=5, y_aktual=6, y_pred=5.3846, residu=0.6154
```

---

## 5. Kesimpulan

| | Analitik | sklearn |
|---|---|---|
| β₀ | 0.0 | ≈ 0.0 |
| β₁ | 1.07692308 | 1.07692308 |
| R² | 0.7179 | 0.7179 |

**Model:** Ŷ = 1.0769x

R² = **0.718** → model menjelaskan **71.8%** variasi data Y.

---

## 6. Visualisasi di GeoGebra

Untuk menampilkan **garis regresi** (garis tengah) di GeoGebra, tambahkan persamaan berikut di kolom Algebra:

```
y = 1.0769x + 0

```
![Grafik Data](/gambar/regresi01.png)


