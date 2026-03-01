---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.11.5
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

# Pengukuran jarak Data IRIS Flower

```{code-cell}
:tags: [hide-input]
import pandas as pd

df = pd.read_csv("IRIS.csv")
df.index = df.index + 1
df.head(len(df))
```

Data Iris di atas diambil dari Kaggle

---

## Klasifikasi Tipe Data

Dataset Iris memiliki struktur yang relatif bersih — tidak ada nilai kosong (*missing values*) pada seluruh kolom.

| Kolom          | Tipe Data              | Penjelasan |
|:---------------|:-----------------------|:-----------|
| `sepal_length` | **Numerik (Rasio, Kontinu)** | Panjang kelopak bunga dalam satuan cm. Nilai kontinu dengan titik nol absolut. |
| `sepal_width`  | **Numerik (Rasio, Kontinu)** | Lebar kelopak bunga dalam satuan cm. Nilai kontinu dengan titik nol absolut. |
| `petal_length` | **Numerik (Rasio, Kontinu)** | Panjang mahkota bunga dalam satuan cm. Nilai kontinu, sangat diskriminatif antar spesies. |
| `petal_width`  | **Numerik (Rasio, Kontinu)** | Lebar mahkota bunga dalam satuan cm. Nilai kontinu, sangat diskriminatif antar spesies. |
| `species`      | **Nominal (Kategorik)**     | Nama spesies: *Iris-setosa*, *Iris-versicolor*, *Iris-virginica*. Tidak ada urutan — ketiga spesies setara, bukan bertingkat. |

### Ringkasan Klasifikasi

```
Numerik (Rasio/Kontinu) : sepal_length, sepal_width, petal_length, petal_width
Nominal / Kategorik     : species
```

> **Catatan:** Dataset Iris tidak memiliki atribut ordinal maupun binary secara eksplisit. Kolom `species` bersifat nominal karena tidak ada hubungan urutan antar ketiga spesies.

---

## Pengukuran Jarak

Pengukuran jarak dilakukan pada **5 sampel representatif** — 2 dari *Iris-setosa*, 2 dari *Iris-versicolor*, dan 1 dari *Iris-virginica* — agar perbedaan antar spesies terlihat jelas.

```{code-cell}
:tags: [hide-input]
import pandas as pd
import numpy as np
from sklearn.preprocessing import MinMaxScaler
from sklearn.metrics.pairwise import euclidean_distances, manhattan_distances, cosine_similarity
from sklearn.metrics.pairwise import pairwise_distances

df = pd.read_csv("IRIS.csv")
sample = df.groupby('species').head(2).reset_index(drop=True).head(5)
sample.index = sample.index + 1
sample
```

| P  | sepal_length | sepal_width | petal_length | petal_width | species         |
|:---|:------------:|:-----------:|:------------:|:-----------:|:----------------|
| P0 |     5.1      |     3.5     |     1.4      |     0.2     | Iris-setosa     |
| P1 |     4.9      |     3.0     |     1.4      |     0.2     | Iris-setosa     |
| P2 |     7.0      |     3.2     |     4.7      |     1.4     | Iris-versicolor |
| P3 |     6.4      |     3.2     |     4.5      |     1.5     | Iris-versicolor |
| P4 |     6.3      |     3.3     |     6.0      |     2.5     | Iris-virginica  |

---

### Normalisasi Data Numerik

Sebelum menghitung jarak, seluruh atribut numerik dinormalisasi ke rentang $[0, 1]$ menggunakan **Min-Max Normalization** agar skala setiap atribut tidak mendominasi perhitungan:

$$z = \frac{x - x_{min}}{x_{max} - x_{min}}$$

```{code-cell}
:tags: [hide-input]
num_cols = ['sepal_length','sepal_width','petal_length','petal_width']
scaler = MinMaxScaler()
num_norm = scaler.fit_transform(sample[num_cols])
norm_df = pd.DataFrame(num_norm, columns=num_cols, index=[f'P{i}' for i in range(5)])
norm_df.round(4)
```

|    | sepal_length | sepal_width | petal_length | petal_width |
|:---|:------------:|:-----------:|:------------:|:-----------:|
| P0 |    0.0952    |    1.0000   |    0.0000    |    0.0000   |
| P1 |    0.0000    |    0.0000   |    0.0000    |    0.0000   |
| P2 |    1.0000    |    0.4000   |    0.7174    |    0.5217   |
| P3 |    0.7143    |    0.4000   |    0.6739    |    0.5652   |
| P4 |    0.6667    |    0.6000   |    1.0000    |    1.0000   |

---

### Euclidean Distance ($L_2$)

Euclidean Distance adalah jarak lurus antara dua titik dalam ruang multidimensi — paling umum digunakan untuk data numerik kontinu.

$$d(i, j) = \sqrt{\sum_{f=1}^{p}(x_{if} - x_{jf})^2}$$

```{code-cell}
:tags: [hide-input]
euc = euclidean_distances(num_norm)
pd.DataFrame(euc, columns=[f'P{i}' for i in range(5)], index=[f'P{i}' for i in range(5)]).round(4)
```

|    |     P0 |     P1 |     P2 |     P3 |     P4 |
|:---|-------:|-------:|-------:|-------:|-------:|
| P0 | 0.0000 | 1.0045 | 1.4019 | 1.2316 | 1.5769 |
| P1 | 1.0045 | 0.0000 | 1.3953 | 1.2016 | 1.6746 |
| P2 | 1.4019 | 1.3953 | 0.0000 | 0.2923 | 0.6780 |
| P3 | 1.2316 | 1.2016 | 0.2923 | 0.0000 | 0.5811 |
| P4 | 1.5769 | 1.6746 | 0.6780 | 0.5811 | 0.0000 |

> **Interpretasi:** P0 dan P1 (*Iris-setosa*) berjarak 1.0045 satu sama lain, sedangkan P2 dan P3 (*Iris-versicolor*) hanya berjarak 0.2923 — sangat mirip. Jarak antara spesies berbeda (misalnya P0 ke P2 = 1.4019) jauh lebih besar, menunjukkan pemisahan antar spesies yang jelas.

---

### Manhattan Distance ($L_1$)

Manhattan Distance menjumlahkan selisih absolut tiap dimensi — lebih robust terhadap outlier dibanding Euclidean karena tidak mengkuadratkan perbedaan.

$$d(i, j) = \sum_{f=1}^{p} |x_{if} - x_{jf}|$$

```{code-cell}
:tags: [hide-input]
man = manhattan_distances(num_norm)
pd.DataFrame(man, columns=[f'P{i}' for i in range(5)], index=[f'P{i}' for i in range(5)]).round(4)
```

|    |     P0 |     P1 |     P2 |     P3 |     P4 |
|:---|-------:|-------:|-------:|-------:|-------:|
| P0 | 0.0000 | 1.0952 | 2.7439 | 2.4582 | 2.9714 |
| P1 | 1.0952 | 0.0000 | 2.6391 | 2.3534 | 3.2667 |
| P2 | 2.7439 | 2.6391 | 0.0000 | 0.3727 | 1.2942 |
| P3 | 2.4582 | 2.3534 | 0.3727 | 0.0000 | 1.0085 |
| P4 | 2.9714 | 3.2667 | 1.2942 | 1.0085 | 0.0000 |

> **Interpretasi:** Pola konsisten dengan Euclidean — P2 dan P3 tetap paling mirip (0.3727). Nilai Manhattan secara umum lebih besar dari Euclidean karena tidak ada operasi akar kuadrat yang "memperkecil" penjumlahan.

---

### Minkowski Distance ($L_h$)

Minkowski adalah generalisasi dari Euclidean dan Manhattan. Dengan $h=3$, bobot diberikan lebih besar pada perbedaan dimensi yang besar.

$$d(i, j) = \left(\sum_{f=1}^{p} |x_{if} - x_{jf}|^h\right)^{1/h}$$

```{code-cell}
:tags: [hide-input]
mink = pairwise_distances(num_norm, metric='minkowski', p=3)
pd.DataFrame(mink, columns=[f'P{i}' for i in range(5)], index=[f'P{i}' for i in range(5)]).round(4)
```

|    |     P0 |     P1 |     P2 |     P3 |     P4 |
|:---|-------:|-------:|-------:|-------:|-------:|
| P0 | 0.0000 | 1.0003 | 1.1365 | 0.9795 | 1.3105 |
| P1 | 1.0003 | 0.0000 | 1.1635 | 0.9708 | 1.3594 |
| P2 | 1.1365 | 1.1635 | 0.0000 | 0.2864 | 0.5615 |
| P3 | 0.9795 | 0.9708 | 0.2864 | 0.0000 | 0.5000 |
| P4 | 1.3105 | 1.3594 | 0.5615 | 0.5000 | 0.0000 |

> **Interpretasi:** Dengan $h=3$, nilai jarak lebih kecil dari Manhattan namun lebih besar dari Euclidean — sesuai sifat $L_h$ norm. Urutan kemiripan antar sampel tetap konsisten.

---

### Supremum Distance ($L_\infty$)

Supremum (Chebyshev) mengambil **selisih maksimum** pada satu dimensi manapun — mencerminkan "worst-case" perbedaan antar dua objek.

$$d(i, j) = \max_f |x_{if} - x_{jf}|$$

```{code-cell}
:tags: [hide-input]
sup = pairwise_distances(num_norm, metric='chebyshev')
pd.DataFrame(sup, columns=[f'P{i}' for i in range(5)], index=[f'P{i}' for i in range(5)]).round(4)
```

|    |     P0 |     P1 |     P2 |     P3 |     P4 |
|:---|-------:|-------:|-------:|-------:|-------:|
| P0 | 0.0000 | 1.0000 | 0.9048 | 0.6739 | 1.0000 |
| P1 | 1.0000 | 0.0000 | 1.0000 | 0.7143 | 1.0000 |
| P2 | 0.9048 | 1.0000 | 0.0000 | 0.2857 | 0.4783 |
| P3 | 0.6739 | 0.7143 | 0.2857 | 0.0000 | 0.4348 |
| P4 | 1.0000 | 1.0000 | 0.4783 | 0.4348 | 0.0000 |

> **Interpretasi:** P1 terhadap banyak titik mencapai nilai maksimum 1.0, menandakan ada satu dimensi yang berbeda penuh. P2 dan P3 kembali paling mirip (0.2857), dominasi perbedaannya hanya pada satu atribut saja.

---

### Simple Matching Distance (Nominal — Species)

Karena `species` bersifat nominal (tanpa urutan), digunakan **Simple Matching Distance**:

$$d(i, j) = \begin{cases} 0 & \text{jika } x_i = x_j \\ 1 & \text{jika } x_i \neq x_j \end{cases}$$

```{code-cell}
:tags: [hide-input]
sp = sample['species'].values
n = len(sp)
smatch = np.zeros((n, n))
for i in range(n):
    for j in range(n):
        smatch[i, j] = 0 if sp[i] == sp[j] else 1
pd.DataFrame(smatch.astype(int), columns=[f'P{i}' for i in range(5)], index=[f'P{i}' for i in range(5)])
```

|    | P0 | P1 | P2 | P3 | P4 |
|:---|:--:|:--:|:--:|:--:|:--:|
| P0 |  0 |  0 |  1 |  1 |  1 |
| P1 |  0 |  0 |  1 |  1 |  1 |
| P2 |  1 |  1 |  0 |  0 |  1 |
| P3 |  1 |  1 |  0 |  0 |  1 |
| P4 |  1 |  1 |  1 |  1 |  0 |

> **Interpretasi:** P0 dan P1 sama-sama *Iris-setosa* (jarak = 0). P2 dan P3 sama-sama *Iris-versicolor* (jarak = 0). P4 (*Iris-virginica*) berbeda dari semua yang lain (jarak = 1 terhadap semua). Ini mempertegas bahwa ketiga spesies terdefinisi dengan baik.

---

### Cosine Similarity (Numerik)

Cosine Similarity mengukur **kemiripan arah vektor**, bukan besarnya jarak. Sangat berguna ketika magnitudo vektor tidak relevan.

$$\cos(\mathbf{i}, \mathbf{j}) = \frac{\mathbf{i} \cdot \mathbf{j}}{\|\mathbf{i}\| \times \|\mathbf{j}\|}$$

Nilai mendekati **1** = arah vektor sangat mirip, mendekati **0** = sangat berbeda.

```{code-cell}
:tags: [hide-input]
cos = cosine_similarity(num_norm)
pd.DataFrame(cos, columns=[f'P{i}' for i in range(5)], index=[f'P{i}' for i in range(5)]).round(4)
```

|    |     P0 |   P1 |     P2 |     P3 |     P4 |
|:---|-------:|-----:|-------:|-------:|-------:|
| P0 | 1.0000 | 0.0000 | 0.3533 | 0.3878 | 0.3944 |
| P1 | 0.0000 | 0.0000 | 0.0000 | 0.0000 | 0.0000 |
| P2 | 0.3533 | 0.0000 | 1.0000 | 0.9857 | 0.9183 |
| P3 | 0.3878 | 0.0000 | 0.9857 | 1.0000 | 0.9717 |
| P4 | 0.3944 | 0.0000 | 0.9183 | 0.9717 | 1.0000 |

> **Interpretasi:** P2, P3, P4 memiliki cosine similarity tinggi (0.92–0.99) meskipun berbeda spesies — menunjukkan arah vektor fiturnya mirip (sama-sama bunga dewasa dengan petal besar). P1 bernilai 0 karena setelah normalisasi semua nilainya = 0 (vektor nol), sehingga cosine tidak terdefinisi secara geometri.

---

## Ringkasan Pengukuran Jarak

| Tipe Data   | Kolom                                              | Metode            | Rumus Singkat | Karakteristik |
|:------------|:---------------------------------------------------|:------------------|:--------------|:--------------|
| **Numerik** | sepal_length, sepal_width, petal_length, petal_width | Euclidean ($L_2$) | $\sqrt{\sum(x_i - x_j)^2}$ | Jarak lurus, sensitif terhadap skala |
| **Numerik** | sepal_length, sepal_width, petal_length, petal_width | Manhattan ($L_1$) | $\sum\|x_i - x_j\|$ | Lebih robust terhadap outlier |
| **Numerik** | sepal_length, sepal_width, petal_length, petal_width | Minkowski ($L_3$) | $(\sum\|x_i - x_j\|^3)^{1/3}$ | Generalisasi $L_1$ dan $L_2$ |
| **Numerik** | sepal_length, sepal_width, petal_length, petal_width | Supremum ($L_\infty$) | $\max_f\|x_i - x_j\|$ | Perbedaan terbesar pada satu dimensi |
| **Nominal** | species                                            | Simple Matching   | $0$ jika sama, $1$ jika beda | Tanpa urutan, hanya cocok/tidak |
| **Numerik** | sepal_length, sepal_width, petal_length, petal_width | Cosine Similarity | $\frac{\mathbf{i}\cdot\mathbf{j}}{\|\mathbf{i}\|\|\mathbf{j}\|}$ | Kemiripan arah vektor, bukan jarak |