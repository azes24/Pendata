# Mengukur Jarak

---

## Similarity dan Dissimilarity

### Similarity
- Mengukur secara Numerik bagaimana kesamaan dua objek data
- Tinggi nilainya bila benda yang lebih mirip
- Range $[0, 1]$

### Dissimilarity (e.g., distance/jarak)
- Ukuran numerik dari perbedaan dua objek
- Sangat rendah bila benda yang lebih mirip
- Minimum dissimilarity $= 0$

---

## Data Matrix and Dissimilarity Matrix

### Data Matrix
- $n$ titik data dengan $p$ dimensi
- Two modes

### Dissimilarity Matrix
- $n$ titik data yang didata adalah distance/jarak
- Matrik segitiga
- Single mode

---

## Mengukur Jarak untuk Atribut Nominal

- Misal terdapat 2 atau lebih nilai, misal., red, yellow, blue, green (generalisasi dari atribut binary)
- **Metode Simple Matching:**

$$d(i, j) = \frac{p - m}{p}$$

  - $m$: jumlah atribut yang sesuai (cocok)
  - $p$: jumlah variabel (total atribut)

---

## Mengukur Jarak untuk Atribut Binary

- Tabel kontingensi untuk data biner

### Variabel Biner Simetris (Symmetric)

$$d(i, j) = \frac{r + s}{q + r + s + t}$$

### Variabel Biner Tidak Simetris (Asymmetric)

$$d(i, j) = \frac{r + s}{q + r + s}$$

### Jaccard Coefficient
- Similarity mengukur variabel asymmetric binary:

$$\text{sim}(i, j) = \frac{q}{q + r + s}$$

- Jaccard coefficient sama dengan "coherence"

---

## Contoh: Atribut Binary

- Jenis kelamin → atribut symmetric
- Atribut lain adalah asymmetric binary
- Nilai Y dan P adalah 1, dan nilai N adalah 0

---

## Atribut Ordinal

- Atribut ordinal dapat bernilai diskrit atau kontinu
- Mengandung urutan/tingkatan, misal., ranking
- Dapat dinyatakan dengan tipe data interval-scaled
- Gantikan $x_{if}$ dengan urutan rankingnya $r_{if} \in \{1, \ldots, M_f\}$
- Petakan setiap nilai variabel ke dalam $[0, 1]$:

$$z_{if} = \frac{r_{if} - 1}{M_f - 1}$$

- Hitung ketidaksamaan (dissimilarity) menggunakan metode variabel skala interval (numerik)

---

## Standarisasi Data Numerik (Normalisasi)

### Z-score

$$z = \frac{x - \mu}{\sigma}$$

- $x$: data yang akan dinormalisasi
- $\mu$: mean
- $\sigma$: standard deviation

### Mean Absolute Deviation

$$s_f = \frac{1}{n} \sum_{i=1}^{n} |x_{if} - \bar{x}_f|$$

Ukuran standarisasi dengan mean absolute deviation:

$$z_{if} = \frac{x_{if} - \bar{x}_f}{s_f}$$

Menggunakan mean absolute deviation lebih robust/handal daripada menggunakan standard deviation.

---

## Contoh: Data Matrix and Dissimilarity Matrix

- **Data Matrix**
- **Dissimilarity Matrix** (dengan Euclidean Distance)

---

## Jarak pada Data Numerik: Minkowski Distance

Minkowski distance adalah ukuran jarak secara umum:

$$d(i, j) = \left( \sum_{f=1}^{p} |x_{if} - x_{jf}|^h \right)^{1/h}$$

- $i = (x_{i1}, x_{i2}, \ldots, x_{ip})$ dan $j = (x_{j1}, x_{j2}, \ldots, x_{jp})$ adalah dua objek dengan $p$-dimensional data
- $h$ adalah pangkat (disebut juga dengan $L_h$ norm)

### Sifat-sifat
- **Positive Definiteness:** $d(i, j) > 0$ jika $i \neq j$, dan $d(i, i) = 0$
- **Symmetry:** $d(i, j) = d(j, i)$
- **Triangle Inequality:** $d(i, j) \leq d(i, k) + d(k, j)$

---

## Special Cases of Minkowski Distance

| $h$ | Nama | Rumus |
|-----|------|-------|
| $h = 1$ | Manhattan ($L_1$ norm) | $d(i,j) = \sum_{f=1}^{p} \|x_{if} - x_{jf}\|$ |
| $h = 2$ | Euclidean ($L_2$ norm) | $d(i,j) = \sqrt{\sum_{f=1}^{p} (x_{if} - x_{jf})^2}$ |
| $h \to \infty$ | Supremum ($L_\infty$ norm) | $d(i,j) = \max_f \|x_{if} - x_{jf}\|$ |

- **Manhattan ($h=1$):** Misal., the Hamming distance — jumlah bit yang berbeda antara dua vektor biner
- **Supremum ($h \to \infty$):** Selisih maksimum di antara atribut-atributnya dalam suatu vektor

---

## Contoh: Minkowski Distance

**Dissimilarity Matrices:**
- Manhattan ($L_1$)
- Euclidean ($L_2$)
- Supremum ($L_\infty$)

---

## Atribut Campuran

- Database mungkin mengandung tipe campuran (Nominal, symmetric binary, asymmetric binary, numeric, ordinal)
- Dapat menggunakan pembobotan untuk menggabungkan:

$$d(i, j) = \frac{\sum_{f=1}^{p} \delta_{ij}^{(f)} \cdot d_{ij}^{(f)}}{\sum_{f=1}^{p} \delta_{ij}^{(f)}}$$

### Aturan per tipe:
- **Binary atau nominal:** $d_{ij}^{(f)} = 0$ jika $x_{if} = x_{jf}$, atau $d_{ij}^{(f)} = 1$ untuk yang lainnya
- **Numerik:** gunakan normalisasi
- **Ordinal:** hitung ranking $r_{if}$ dan cari $z_{if}$ sebagai skala interval

---

## Cosine Similarity

- Dokumen dapat dinyatakan dengan ribuan atribut yang masing-masing menyatakan kemunculan kata-kata dalam suatu dokumen

### Formula
Jika $\mathbf{d_1}$ dan $\mathbf{d_2}$ adalah dua vektor (e.g., term-frequency vectors), maka:

$$\cos(\mathbf{d_1}, \mathbf{d_2}) = \frac{\mathbf{d_1} \cdot \mathbf{d_2}}{\|\mathbf{d_1}\| \times \|\mathbf{d_2}\|}$$

- $\cdot$ menyatakan perkalian titik (dot product) antar vektor
- $\|\mathbf{d}\|$ adalah panjang vektor $\mathbf{d}$

---

## Contoh: Cosine Similarity

Cari kemiripan (similarity) antara dokumen 1 dan 2.

$$\mathbf{d_1} = (5, 0, 3, 0, 2, 0, 0, 2, 0, 0)$$

$$\mathbf{d_2} = (3, 0, 2, 0, 1, 1, 0, 1, 0, 1)$$

$$\mathbf{d_1} \cdot \mathbf{d_2} = 5{\times}3 + 3{\times}2 + 2{\times}1 + 2{\times}1 = 25$$

$$\|\mathbf{d_1}\| = \sqrt{25+9+4+4} = \sqrt{42} \approx 6{,}481$$

$$\|\mathbf{d_2}\| = \sqrt{9+4+1+1+1+1} = \sqrt{17} \approx 4{,}12$$

$$\cos(\mathbf{d_1}, \mathbf{d_2}) = \frac{25}{6{,}481 \times 4{,}12} \approx 0{,}94$$