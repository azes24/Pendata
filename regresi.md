# Regresi Linier

# Analisis Regresi Linier — Data Koordinat Titik

## 1. Data Input

| Titik | X | Y |
|-------|---|---|
| A     | 2 | 2 |
| B     | 4 | 3 |
| C     | 5 | 5 |
| D     | 3 | 4 |
| E     | 3 | 3 |
| F     | 4 | 5 |
| G     | 5 | 6 |

---

## 2. Tujuan Analisis

1. Menghitung koefisien regresi menggunakan **library `sklearn`**.
2. Menghitung koefisien regresi secara **analitik** menggunakan rumus matriks:

$$\hat{\beta} = (X^T X)^{-1} X^T Y$$

---

## 3. Metode 1 — Perhitungan Analitik

### Kode Python

```python
import numpy as np

X_vals = np.array([2, 4, 5, 3, 3, 4, 5])
Y_vals = np.array([2, 3, 5, 4, 3, 5, 6])
n = len(X_vals)

# Bentuk matrix X (dengan kolom bias) dan Y
X_mat = np.column_stack([np.ones(n), X_vals])
Y_mat = Y_vals.reshape(-1, 1)

# Hitung komponen formula
XTX     = X_mat.T @ X_mat
XTY     = X_mat.T @ Y_mat
XTX_inv = np.linalg.inv(XTX)
beta    = XTX_inv @ XTY

print("X^T X =\n", XTX)
print("X^T Y =\n", XTY)
print("(X^T X)^-1 =\n", XTX_inv)
print("β =\n", beta)
```

### Output Python

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

β =
[[0.        ]
 [1.07692308]]
```

### Hasil Koefisien (Analitik)

| Koefisien | Nilai |
|-----------|-------|
| β₀ (intercept) | **0.0** |
| β₁ (slope)     | **1.07692308** |

---

## 4. Metode 2 — Menggunakan sklearn

### Kode Python

```python
from sklearn.linear_model import LinearRegression

model = LinearRegression()
model.fit(X_vals.reshape(-1, 1), Y_vals)

print("β0 (intercept) =", model.intercept_)
print("β1 (coef)      =", model.coef_[0])
```

### Output Python

```
β0 (intercept) = -8.881784197001252e-16
β1 (coef)      = 1.076923076923077
```

> **Catatan:** Nilai β₀ dari sklearn adalah `-8.88e-16`, yang secara numerik sama dengan **0** (floating-point precision error yang sangat kecil).

### Hasil Koefisien (sklearn)

| Koefisien | Nilai |
|-----------|-------|
| β₀ (intercept) | **≈ 0.0** |
| β₁ (slope)     | **1.076923076923077** |

---

## 5. Model Regresi Final

$$\hat{Y} = 1.0769 \cdot X$$

---

## 6. Evaluasi Model

### Kode Python

```python
Y_pred = beta[0, 0] + beta[1, 0] * X_vals
SS_res = np.sum((Y_vals - Y_pred) ** 2)
SS_tot = np.sum((Y_vals - np.mean(Y_vals)) ** 2)
R2     = 1 - SS_res / SS_tot

print("SS_res =", SS_res)
print("SS_tot =", SS_tot)
print("R²     =", R2)

points = ['A','B','C','D','E','F','G']
for i in range(n):
    print(f"{points[i]}: x={X_vals[i]}, y_aktual={Y_vals[i]}, "
          f"y_pred={Y_pred[i]:.4f}, residu={Y_vals[i]-Y_pred[i]:.4f}")
```

### Output Python

```
SS_res = 3.384615384615384
SS_tot = 12.0
R²     = 0.7179487179487181

A: x=2, y_aktual=2, y_pred=2.1538, residu=-0.1538
B: x=4, y_aktual=3, y_pred=4.3077, residu=-1.3077
C: x=5, y_aktual=5, y_pred=5.3846, residu=-0.3846
D: x=3, y_aktual=4, y_pred=3.2308, residu=0.7692
E: x=3, y_aktual=3, y_pred=3.2308, residu=-0.2308
F: x=4, y_aktual=5, y_pred=4.3077, residu=0.6923
G: x=5, y_aktual=6, y_pred=5.3846, residu=0.6154
```

---

## 7. Perbandingan Kedua Metode

| Aspek | Analitik (X^T X)^-1 X^T Y | sklearn LinearRegression |
|-------|---------------------------|--------------------------|
| β₀   | 0.0                       | -8.88e-16 (≈ 0)          |
| β₁   | 1.07692308                | 1.07692308               |
| R²   | 0.7179                    | 0.7179                   |

Kedua metode menghasilkan koefisien yang **identik** — `sklearn` secara internal mengimplementasikan rumus analitik yang sama.

---