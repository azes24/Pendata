# Prediksi Missing Value Data IRIS

# Prediksi Missing Value Data ke-40 IRIS dengan KNN

## Data ke-40 (yang Memiliki Missing Value)

| sepal_length | sepal_width | petal_length | petal_width |
|-------------|-------------|-------------|-------------|
| 5.1         | 3.4         | 1.5         | **?**       |

Diasumsikan **petal_width** pada data ke-40 hilang. Kolom lain (sepal_length, sepal_width, petal_length) digunakan untuk menghitung jarak ke data lain.

---

## Langkah 1 — Pilih K

Digunakan **K = 3**, artinya akan dicari **3 data terdekat** sebagai tetangga.

---

## Langkah 2 — Hitung Jarak Euclidean

Rumus jarak:

$$d(a,b) = \sqrt{(a_1 - b_1)^2 + (a_2 - b_2)^2 + (a_3 - b_3)^2}$$

Diambil 5 data pembanding dari dataset:

| No | sepal_length | sepal_width | petal_length | petal_width |
|----|-------------|-------------|-------------|-------------|
| 1  | 5.1         | 3.5         | 1.4         | 0.2         |
| 8  | 5.0         | 3.4         | 1.5         | 0.2         |
| 18 | 5.1         | 3.5         | 1.4         | 0.3         |
| 38 | 4.9         | 3.1         | 1.5         | 0.1         |
| 39 | 4.4         | 3.0         | 1.3         | 0.2         |

### Perhitungan Jarak ke Data ke-40 (5.1, 3.4, 1.5):

**d(40, 1):**
$$d = \sqrt{(5.1-5.1)^2 + (3.4-3.5)^2 + (1.5-1.4)^2} = \sqrt{0 + 0.01 + 0.01} = \sqrt{0.02} \approx 0.141$$

**d(40, 8):**
$$d = \sqrt{(5.1-5.0)^2 + (3.4-3.4)^2 + (1.5-1.5)^2} = \sqrt{0.01 + 0 + 0} = \sqrt{0.01} = 0.100$$

**d(40, 18):**
$$d = \sqrt{(5.1-5.1)^2 + (3.4-3.5)^2 + (1.5-1.4)^2} = \sqrt{0 + 0.01 + 0.01} = \sqrt{0.02} \approx 0.141$$

**d(40, 38):**
$$d = \sqrt{(5.1-4.9)^2 + (3.4-3.1)^2 + (1.5-1.5)^2} = \sqrt{0.04 + 0.09 + 0} = \sqrt{0.13} \approx 0.361$$

**d(40, 39):**
$$d = \sqrt{(5.1-4.4)^2 + (3.4-3.0)^2 + (1.5-1.3)^2} = \sqrt{0.49 + 0.16 + 0.04} = \sqrt{0.69} \approx 0.831$$

---

## Langkah 3 — Urutkan Jarak & Pilih K Terdekat

| No Data | Jarak  | petal_width |
|---------|--------|-------------|
| 8       | 0.100  | 0.2         |
| 1       | 0.141  | 0.2         |
| 18      | 0.141  | 0.3         |
| 38      | 0.361  | 0.1         |
| 39      | 0.831  | 0.2         |

Dengan **K = 3**, dipilih: **No. 8, No. 1, dan No. 18**

---

## Langkah 4 — Hitung Nilai Imputasi

$$\hat{x} = \frac{x_1 + x_2 + x_3}{K} = \frac{0.2 + 0.2 + 0.3}{3} = \frac{0.7}{3} \approx 0.233$$

---

## Hasil

| sepal_length | sepal_width | petal_length | petal_width (hasil KNN) |
|-------------|-------------|-------------|------------------------|
| 5.1         | 3.4         | 1.5         | **0.233 ≈ 0.2**        |

> Nilai asli petal_width data ke-40 pada dataset IRIS adalah **0.2**, sehingga hasil prediksi KNN sangat akurat.


![Grafik Data](/gambar/xxx.png)