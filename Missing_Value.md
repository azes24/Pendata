# Missing Value dengan Metode KNN (K-Nearest Neighbor)

## Pengertian

**Missing value** adalah nilai yang tidak tersedia pada suatu atribut dalam dataset. Salah satu metode penanganannya adalah **KNN Imputation**, yaitu mengisi missing value berdasarkan kemiripan dengan data lain (tetangga terdekat).

---

## Konsep KNN Imputation

KNN bekerja dengan mencari **K data terdekat** (tetangga) dari baris yang memiliki missing value, lalu mengisi nilai yang hilang menggunakan **rata-rata nilai tetangga** tersebut.

> Semakin kecil jarak antar dua data, semakin mirip keduanya.

---

## Rumus Jarak (Euclidean Distance)

Jarak antara dua data dihitung menggunakan rumus Euclidean:

$$d(a, b) = \sqrt{\sum_{i=1}^{n} (a_i - b_i)^2}$$

- $a$ dan $b$ = dua baris data yang dibandingkan
- $i$ = atribut/kolom yang tersedia (bukan yang missing)
- $n$ = jumlah atribut yang digunakan

---

## Rumus Imputasi (Pengisian Nilai)

Setelah K tetangga terdekat ditemukan, missing value diisi dengan:

$$\hat{x} = \frac{\sum_{j=1}^{K} x_j}{K}$$

- $\hat{x}$ = nilai yang akan diisikan
- $x_j$ = nilai atribut dari tetangga ke-$j$
- $K$ = jumlah tetangga yang digunakan

---

## Contoh Perhitungan (K = 2)

Dataset nilai ujian siswa:

| No | Matematika | IPA | Bahasa |
|----|-----------|-----|--------|
| 1  | 80        | 75  | 90     |
| 2  | 70        | 60  | 85     |
| 3  | 90        | 92  | 88     |
| 4  | 65        | 70  | **?**  |
| 5  | 85        | 78  | 80     |

Baris **No. 4** memiliki missing value pada kolom **Bahasa**. Atribut yang digunakan untuk menghitung jarak adalah **Matematika** dan **IPA**.

---

### Langkah 1 — Hitung Jarak dari No. 4 ke Semua Baris Lain

**d(4,1):**
$$d = \sqrt{(65-80)^2 + (70-75)^2} = \sqrt{225 + 25} = \sqrt{250} \approx 15{,}81$$

**d(4,2):**
$$d = \sqrt{(65-70)^2 + (70-60)^2} = \sqrt{25 + 100} = \sqrt{125} \approx 11{,}18$$

**d(4,3):**
$$d = \sqrt{(65-90)^2 + (70-92)^2} = \sqrt{625 + 484} = \sqrt{1109} \approx 33{,}30$$

**d(4,5):**
$$d = \sqrt{(65-85)^2 + (70-78)^2} = \sqrt{400 + 64} = \sqrt{464} \approx 21{,}54$$

---

### Langkah 2 — Urutkan Jarak Terkecil

| Tetangga | Jarak  |
|----------|--------|
| No. 2    | 11,18  |
| No. 1    | 15,81  |
| No. 5    | 21,54  |
| No. 3    | 33,30  |

Dengan **K = 2**, dipilih **2 tetangga terdekat** yaitu **No. 2** dan **No. 1**.

---

### Langkah 3 — Hitung Nilai Imputasi

Nilai Bahasa dari tetangga terpilih:
- No. 2 → Bahasa = **85**
- No. 1 → Bahasa = **90**

$$\hat{x} = \frac{85 + 90}{2} = \frac{175}{2} = 87{,}5$$

**Maka missing value Bahasa pada No. 4 diisi dengan 87,5.**

---

## Pemilihan Nilai K

| Nilai K | Karakteristik |
|---------|--------------|
| K kecil (misal K=1) | Sensitif terhadap noise, hasil kurang stabil |
| K sedang (K=3~5) | Paling umum digunakan, hasil lebih stabil |
| K besar | Terlalu umum, bisa mengabaikan pola lokal |

> Nilai **K = 3 atau K = 5** paling sering direkomendasikan dalam praktik.

---