# Mengukur Jarak

## Similarity dan Dissimilarity

### Similarity
- Mengukur secara Numerik bagaimana kesamaan dua objek data
- Tinggi nilainya bila benda yang lebih mirip
- Range [0, 1]

### Dissimilarity (e.g., distance/jarak)
- Ukuran numerik dari perbedaan dua objek
- Sangat rendah bila benda yang lebih mirip
- Minimum dissimilarity = 0

## Data Matrix and Dissimilarity Matrix

### Data Matrix
- n titik data dengan p dimensi
- Two modes

### Dissimilarity Matrix
- n titik data yang didata adalah distance/jarak
- Matrik segitiga
- Single mode

## Mengukur Jarak untuk Atribut Nominal

- Misal terdapat 2 atau lebih nilai, misal., red, yellow, blue, green (generalisasi dari atribut binary)
- **Metode Simple Matching:**
  - m: jumlah yang sesuai
  - p: jumlah variabel

## Mengukur Jarak untuk Atribut Binary

- Tabel kontingensi untuk data biner

### Mengukur jarak untuk variabel biner simetris (symmetric binary variables)

### Mengukur jarak untuk variabel biner tidak simetris (asymmetric binary variables)

### Jaccard Coefficient
- Similarity mengukur variabel asymmetric binary
- Jaccard coefficient sama dengan "coherence"

## Contoh: Atribut Binary

- Jenis kelamin → atribut symmetric
- Atribut lain adalah asymmetric binary
- Nilai Y dan P adalah 1, dan nilai N adalah 0

## Atribut Ordinal

- Atribut ordinal dapat bernilai diskrit atau kontinu
- Mengandung urutan/tingkatan, misal., ranking
- Dapat dinyatakan dengan tipe data interval-scaled
- Gantikan x_if dengan urutan rankingnya
- Petakan setiap nilai variabel ke dalam [0, 1] dengan menggantikan objek ke-i dalam variabel ke-f
- Hitung ketidaksamaan (dissimilarity) menggunakan metode variabel skala interval (numerik)

## Standarisasi Data Numerik (Normalisasi)

### Z-score
- X: data yang akan dinormalisasi
- μ: mean
- σ: standard deviation

### Mean Absolute Deviation
- Cara lain untuk normalisasi
- Menggunakan mean absolute deviation lebih robust/handal daripada menggunakan standard deviation

## Contoh: Data Matrix and Dissimilarity Matrix

- **Data Matrix**
- **Dissimilarity Matrix** (dengan Euclidean Distance)

## Jarak pada Data Numerik: Minkowski Distance

Minkowski distance adalah ukuran jarak secara umum:

- i = (x_i1, x_i2, …, x_ip) dan j = (x_j1, x_j2, …, x_jp) adalah dua objek dengan p-dimensional data
- h adalah pangkat (disebut juga dengan L-h norm)

### Sifat-sifat
- **Positive Definiteness:** d(i, j) > 0 jika i ≠ j, dan d(i, i) = 0
- **Symmetry:** d(i, j) = d(j, i)
- **Triangle Inequality:** d(i, j) ≤ d(i, k) + d(k, j)

## Special Cases of Minkowski Distance

| h | Nama | Keterangan |
|---|------|------------|
| h = 1 | Manhattan (city block, L1 norm) | Misal., the Hamming distance: jumlah bit yang berbeda antara dua vektor biner |
| h = 2 | Euclidean distance (L2 norm) | Jarak Euclidean standar |
| h → ∞ | Supremum (L_max norm, L_∞ norm) | Selisih maksimum di antara atribut-atributnya dalam suatu vektor |

## Contoh: Minkowski Distance

**Dissimilarity Matrices:**
- Manhattan (L1)
- Euclidean (L2)
- Supremum

## Atribut Campuran

- Database mungkin mengandung tipe campuran (Nominal, symmetric binary, asymmetric binary, numeric, ordinal)
- Dapat menggunakan pembobotan untuk menggabungkan

### Aturan per tipe:
- **Binary atau nominal:** d_ij(f) = 0 jika x_if = x_jf, atau d_ij(f) = 1 untuk yang lainnya
- **Numerik:** gunakan normalisasi
- **Ordinal:** hitung ranking r_if dan cari z_if sebagai skala interval

## Cosine Similarity

- Dokumen dapat dinyatakan dengan ribuan atribut yang masing-masing menyatakan kemunculan kata-kata dalam suatu dokumen

### Formula
Jika d1 dan d2 adalah dua vektor (e.g., term-frequency vectors), maka:

```
cos(d1, d2) = (d1 · d2) / (||d1|| × ||d2||)
```

- `·` menyatakan perkalian titik (dot product) antar vektor
- `||d||` adalah panjang vektor d

## Contoh: Cosine Similarity

Cari kemiripan (similarity) antara dokumen 1 dan 2.

```
d1 = (5, 0, 3, 0, 2, 0, 0, 2, 0, 0)
d2 = (3, 0, 2, 0, 1, 1, 0, 1, 0, 1)

d1·d2 = 5×3 + 0×0 + 3×2 + 0×0 + 2×1 + 0×1 + 0×1 + 2×1 + 0×0 + 0×1 = 25

||d1|| = √(25+0+9+0+4+0+0+4+0+0) = √42 ≈ 6.481

||d2|| = √(9+0+4+0+1+1+0+1+0+1) = √17 ≈ 4.12

cos(d1, d2) = 25 / (6.481 × 4.12) ≈ 0.94
```