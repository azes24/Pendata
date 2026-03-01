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

# Pengukuran jarak Data TITANIC


```{code-cell}
:tags: [hide-input]
import pandas as pd

df = pd.read_csv("TITANIC.csv")
df.index = df.index + 1
df.head(len(df))
```
data Titanic diatas diambil dari kaggle.


## Klasifikasi Tipe Data

Setiap kolom dalam dataset diklasifikasikan ke dalam tipe data berikut:

| Kolom        | Tipe Data          | Penjelasan |
|:-------------|:-------------------|:-----------|
| `PassengerId`| **Numerik (Rasio)**| ID unik penumpang. Bersifat numerik kontinu, namun tidak memiliki makna analitik — hanya identifier. |
| `Survived`   | **Binary**         | Nilai hanya 0 (meninggal) atau 1 (selamat). Merupakan atribut biner simetris. |
| `Pclass`     | **Ordinal**        | Kelas tiket: 1 = First, 2 = Second, 3 = Third. Ada urutan/tingkatan yang bermakna (1 lebih baik dari 3). |
| `Name`       | **Nominal (Teks)** | Nama penumpang. Bersifat unik dan tidak memiliki urutan atau nilai numerik. |
| `Sex`        | **Nominal (Binary)**| Jenis kelamin: male/female. Tidak ada urutan, hanya dua kategori. |
| `Age`        | **Numerik (Rasio)**| Usia dalam tahun. Kontinu, memiliki titik nol absolut. Terdapat **86 nilai kosong (missing)**. |
| `SibSp`      | **Numerik (Diskrit)**| Jumlah saudara/pasangan di kapal. Bilangan bulat ≥ 0. |
| `Parch`      | **Numerik (Diskrit)**| Jumlah orang tua/anak di kapal. Bilangan bulat ≥ 0. |
| `Ticket`     | **Nominal (Teks)** | Nomor tiket. Kombinasi angka dan huruf, tidak ada urutan bermakna. |
| `Fare`       | **Numerik (Rasio)**| Harga tiket dalam poundsterling. Kontinu, memiliki titik nol. Terdapat **1 nilai kosong**. |
| `Cabin`      | **Nominal (Teks)** | Nomor kabin. Sangat banyak nilai kosong (**327 dari 418**), sehingga sering diabaikan. |
| `Embarked`   | **Nominal (Kategorik)**| Port keberangkatan: C = Cherbourg, Q = Queenstown, S = Southampton. Tidak ada urutan. |

### Ringkasan Klasifikasi

```
Numerik (Rasio/Kontinu) : Age, Fare
Numerik (Diskrit)       : SibSp, Parch, PassengerId
Ordinal                 : Pclass
Binary                  : Survived
Nominal / Kategorik     : Sex, Embarked, Name, Ticket, Cabin
```

---

## Pengukuran Jarak

Pengukuran jarak dilakukan pada **5 sampel pertama** yang memiliki data lengkap. Kolom yang digunakan disesuaikan dengan tipe datanya masing-masing.

```python
sample = df[['Survived','Pclass','Sex','Age','SibSp','Parch','Fare','Embarked']].dropna().head(5).reset_index(drop=True)
```

| P  | Survived | Pclass | Sex    |  Age | SibSp | Parch |    Fare | Embarked |
|:---|:--------:|:------:|:-------|-----:|------:|------:|--------:|:--------:|
| P0 |    0     |   3    | male   | 34.5 |     0 |     0 |  7.8292 | Q        |
| P1 |    1     |   3    | female | 47.0 |     1 |     0 |  7.0000 | S        |
| P2 |    0     |   2    | male   | 62.0 |     0 |     0 |  9.6875 | Q        |
| P3 |    0     |   3    | male   | 27.0 |     0 |     0 |  8.6625 | S        |
| P4 |    1     |   3    | female | 22.0 |     1 |     1 | 12.2875 | S        |

---

### Jarak Atribut Numerik — Euclidean & Manhattan

Kolom yang digunakan: `Age`, `Fare`, `SibSp`, `Parch`.

Data dinormalisasi terlebih dahulu ke rentang $[0, 1]$ menggunakan **Min-Max Normalization**:

$$z = \frac{x - x_{min}}{x_{max} - x_{min}}$$

```python
num_cols = ['Age', 'Fare', 'SibSp', 'Parch']
scaler  = MinMaxScaler()
num_norm = scaler.fit_transform(sample[num_cols])
```

**Data setelah normalisasi:**

|    |    Age |   Fare | SibSp | Parch |
|---:|-------:|-------:|------:|------:|
| P0 | 0.3125 | 0.1568 |   0.0 |   0.0 |
| P1 | 0.6250 | 0.0000 |   1.0 |   0.0 |
| P2 | 1.0000 | 0.5083 |   0.0 |   0.0 |
| P3 | 0.1250 | 0.3144 |   0.0 |   0.0 |
| P4 | 0.0000 | 1.0000 |   1.0 |   1.0 |

#### Euclidean Distance ($L_2$)

$$d(i, j) = \sqrt{\sum_{f=1}^{p}(x_{if} - x_{jf})^2}$$

```python
euc = euclidean_distances(num_norm)
```

|    |     P0 |     P1 |     P2 |     P3 |     P4 |
|:---|-------:|-------:|-------:|-------:|-------:|
| P0 | 0.0000 | 1.0594 | 0.7721 | 0.2449 | 1.6759 |
| P1 | 1.0594 | 0.0000 | 1.1828 | 1.1614 | 1.5462 |
| P2 | 0.7721 | 1.1828 | 0.0000 | 0.8962 | 1.8005 |
| P3 | 0.2449 | 1.1614 | 0.8962 | 0.0000 | 1.5766 |
| P4 | 1.6759 | 1.5462 | 1.8005 | 1.5766 | 0.0000 |

> **Interpretasi:** P0 dan P3 memiliki jarak Euclidean terkecil (0.2449), artinya kedua penumpang ini paling mirip secara numerik (usia dan fare berdekatan, sama-sama tanpa saudara/anak).

#### Manhattan Distance ($L_1$)

$$d(i, j) = \sum_{f=1}^{p} |x_{if} - x_{jf}|$$

```python
man = manhattan_distances(num_norm)
```

|    |     P0 |     P1 |     P2 |     P3 |     P4 |
|:---|-------:|-------:|-------:|-------:|-------:|
| P0 | 0.0000 | 1.4693 | 1.0390 | 0.3451 | 3.1557 |
| P1 | 1.4693 | 0.0000 | 1.8833 | 1.8144 | 2.6250 |
| P2 | 1.0390 | 1.8833 | 0.0000 | 1.0689 | 3.4917 |
| P3 | 0.3451 | 1.8144 | 1.0689 | 0.0000 | 2.8106 |
| P4 | 3.1557 | 2.6250 | 3.4917 | 2.8106 | 0.0000 |

> **Interpretasi:** Konsisten dengan Euclidean — P0 dan P3 tetap yang paling mirip. Manhattan lebih sensitif terhadap perbedaan besar pada satu dimensi. P4 cenderung jauh dari semua penumpang lain karena memiliki kombinasi unik (Fare tinggi, ada anak).

---

### Jarak Atribut Binary — Hamming Distance

Kolom yang digunakan: `Survived` dan `Sex` (diubah menjadi 0/1).

```python
sample['Sex_bin'] = (sample['Sex'] == 'female').astype(int)
bin_data = sample[['Survived', 'Sex_bin']].values
```

**Data biner:**

| P  | Survived | Sex_bin (female=1) |
|:---|:--------:|:------------------:|
| P0 |    0     |         0          |
| P1 |    1     |         1          |
| P2 |    0     |         0          |
| P3 |    0     |         0          |
| P4 |    1     |         1          |

#### Hamming Distance

$$d_{Hamming}(i, j) = \frac{\text{jumlah bit berbeda}}{\text{total bit}}$$

```python
from scipy.spatial.distance import hamming
hamming_mat[i,j] = hamming(bin_data[i], bin_data[j])
```

|    |   P0 |   P1 |   P2 |   P3 |   P4 |
|:---|-----:|-----:|-----:|-----:|-----:|
| P0 |  0.0 |  1.0 |  0.0 |  0.0 |  1.0 |
| P1 |  1.0 |  0.0 |  1.0 |  1.0 |  0.0 |
| P2 |  0.0 |  1.0 |  0.0 |  0.0 |  1.0 |
| P3 |  0.0 |  1.0 |  0.0 |  0.0 |  1.0 |
| P4 |  1.0 |  0.0 |  1.0 |  1.0 |  0.0 |

> **Interpretasi:** Nilai 0 berarti identik, nilai 1 berarti berbeda pada semua bit. P0, P2, P3 identik (laki-laki, tidak selamat). P1 dan P4 identik (perempuan, selamat). Ini mencerminkan kebijakan *"women and children first"* pada kapal Titanic.

---

### Jarak Atribut Ordinal — Pclass

Kolom yang digunakan: `Pclass` (nilai 1, 2, 3).

Atribut ordinal dinormalisasi ke $[0, 1]$ dengan rumus:

$$z_{if} = \frac{r_{if} - 1}{M_f - 1}$$

di mana $r_{if}$ adalah nilai rank dan $M_f$ adalah jumlah tingkatan (di sini $M_f = 3$).

```python
sample['Pclass_ord'] = (sample['Pclass'] - 1) / (3 - 1)
```

| P  | Pclass | Pclass (normalized) |
|:---|:------:|:-------------------:|
| P0 |   3    |        1.00         |
| P1 |   3    |        1.00         |
| P2 |   2    |        0.50         |
| P3 |   3    |        1.00         |
| P4 |   3    |        1.00         |

**Matriks Jarak Ordinal (Euclidean setelah normalisasi):**

|    |   P0 |   P1 |   P2 |   P3 |   P4 |
|:---|-----:|-----:|-----:|-----:|-----:|
| P0 | 0.00 | 0.00 | 0.50 | 0.00 | 0.00 |
| P1 | 0.00 | 0.00 | 0.50 | 0.00 | 0.00 |
| P2 | 0.50 | 0.50 | 0.00 | 0.50 | 0.50 |
| P3 | 0.00 | 0.00 | 0.50 | 0.00 | 0.00 |
| P4 | 0.00 | 0.00 | 0.50 | 0.00 | 0.00 |

> **Interpretasi:** P0, P1, P3, P4 semuanya di kelas 3 (kelas ekonomi), sehingga jaraknya 0. P2 berada di kelas 2, sehingga memiliki jarak 0.5 dari semua penumpang kelas 3.

---

### Jarak Atribut Nominal — Simple Matching Distance

Kolom yang digunakan: `Embarked` (C / Q / S).

Karena nominal tidak memiliki urutan, jarak diukur dengan **Simple Matching**:

$$d(i, j) = \begin{cases} 0 & \text{jika } x_i = x_j \\ 1 & \text{jika } x_i \neq x_j \end{cases}$$

```python
smatch[i,j] = 0 if emb[i] == emb[j] else 1
```

| P  | Embarked |
|:---|:--------:|
| P0 |    Q     |
| P1 |    S     |
| P2 |    Q     |
| P3 |    S     |
| P4 |    S     |

**Matriks Simple Matching Distance:**

|    |   P0 |   P1 |   P2 |   P3 |   P4 |
|:---|-----:|-----:|-----:|-----:|-----:|
| P0 |  0.0 |  1.0 |  0.0 |  1.0 |  1.0 |
| P1 |  1.0 |  0.0 |  1.0 |  0.0 |  0.0 |
| P2 |  0.0 |  1.0 |  0.0 |  1.0 |  1.0 |
| P3 |  1.0 |  0.0 |  1.0 |  0.0 |  0.0 |
| P4 |  1.0 |  0.0 |  1.0 |  0.0 |  0.0 |

> **Interpretasi:** P0 dan P2 berangkat dari Queenstown (Q), sehingga jaraknya 0. P1, P3, P4 berangkat dari Southampton (S), sehingga sesama mereka jaraknya 0. Antar grup Q dan S jaraknya 1 (tidak sama).

---

### Cosine Similarity (Atribut Numerik)

Cosine Similarity mengukur **kemiripan arah vektor**, bukan besarnya jarak. Sering digunakan untuk data berdimensi tinggi.

$$\cos(\mathbf{i}, \mathbf{j}) = \frac{\mathbf{i} \cdot \mathbf{j}}{\|\mathbf{i}\| \times \|\mathbf{j}\|}$$

Nilai mendekati **1** = sangat mirip, mendekati **0** = sangat berbeda.

```python
cos_sim = cosine_similarity(num_norm)
```

|    |     P0 |     P1 |     P2 |     P3 |     P4 |
|:---|-------:|-------:|-------:|-------:|-------:|
| P0 | 1.0000 | 0.4737 | 1.0000 | 0.7470 | 0.2590 |
| P1 | 0.4737 | 1.0000 | 0.4725 | 0.1958 | 0.4896 |
| P2 | 1.0000 | 0.4725 | 1.0000 | 0.7504 | 0.2616 |
| P3 | 0.7470 | 0.1958 | 0.7504 | 1.0000 | 0.5365 |
| P4 | 0.2590 | 0.4896 | 0.2616 | 0.5365 | 1.0000 |

> **Interpretasi:** P0 dan P2 memiliki cosine similarity = 1.0, artinya vektor atributnya mengarah ke arah yang sama meski berbeda magnitudo. P4 memiliki similarity rendah terhadap P0 dan P2, karena memiliki kombinasi atribut yang sangat berbeda (Fare sangat tinggi, ada Parch).

---

## Ringkasan Pengukuran Jarak

| Tipe Data | Kolom | Metode Jarak | Karakteristik |
|:----------|:------|:-------------|:--------------|
| **Numerik** | Age, Fare, SibSp, Parch | Euclidean ($L_2$) | Sensitif terhadap skala — perlu normalisasi |
| **Numerik** | Age, Fare, SibSp, Parch | Manhattan ($L_1$) | Lebih robust terhadap outlier dibanding Euclidean |
| **Binary** | Survived, Sex | Hamming Distance | Menghitung proporsi bit yang berbeda |
| **Ordinal** | Pclass | Euclidean (setelah normalisasi) | Rank dikonversi ke $[0,1]$ sebelum dihitung jaraknya |
| **Nominal** | Embarked | Simple Matching | Hanya 0 (sama) atau 1 (berbeda), tanpa urutan |
| **Numerik** | Age, Fare, SibSp, Parch | Cosine Similarity | Mengukur kemiripan arah vektor, bukan jarak absolut |