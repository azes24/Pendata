# Pengukuran Data Ordinal pada Dataset Pelatihan

## 1. Identifikasi Tipe Data Ordinal

Pada dataset **Dataset_Pelatihan.csv**, variabel yang termasuk **data ordinal** adalah:

- **nilai huruf**

Variabel ini memiliki **tingkatan yang berurutan**, tetapi jarak antar kategori tidak pasti sama.

Urutan kategori:

| Nilai Huruf | Makna | Rank |
|---|---|---|
| D | Terendah | 1 |
| C | Rendah | 2 |
| B | Baik | 3 |
| A | Sangat Baik | 4 |

---

# 2. Transformasi Ordinal ke Rank

Setiap kategori diubah menjadi **peringkat (rank)** agar dapat dihitung secara numerik.

r(x) =
D = 1 
C = 2 
B = 3 
A = 4

Contoh hasil transformasi:

| Nama | Nilai Huruf | Rank |
|---|---|---|
| Budi | A | 4 |
| Ani | B | 3 |
| Joko | A | 4 |
| Siti | D | 1 |
| Agus | B | 3 |

---

# 3. Normalisasi Data Ordinal

Agar nilai berada dalam **rentang 0 – 1**, digunakan normalisasi:


z_i = {r_i - 1}{K - 1}


di mana:

- r_i = rank kategori
- K = jumlah kategori ordinal
- pada dataset ini **K = 4**

Sehingga:

z_i = {r_i - 1}{3}

---

# 4. Hasil Normalisasi

| Nama | Nilai Huruf | Rank | Normalisasi |
|---|---|---|---|
| Budi | A | 4 | 1.00 |
| Ani | B | 3 | 0.67 |
| Joko | A | 4 | 1.00 |
| Siti | D | 1 | 0.00 |
| Agus | B | 3 | 0.67 |

Interpretasi:

- **0** → kategori terendah  
- **1** → kategori tertinggi  
- nilai di antaranya menunjukkan posisi relatif dalam skala ordinal.

---

# 5. Pengukuran Jarak Antar Data (Ordinal Distance)

Setelah normalisasi, jarak antar data dapat dihitung dengan:

## Manhattan Distance

d(x,y) = |z_x - z_y|


Contoh:

- Budi (1.00) dan Ani (0.67)


d = |1.00 - 0.67| = 0.33


- Ani (0.67) dan Siti (0.00)


d = |0.67 - 0.00| = 0.67


Semakin besar nilai jarak → semakin berbeda tingkat ordinalnya.

---

# 6. Kesimpulan

1. Variabel ordinal pada dataset adalah **nilai huruf**.
2. Data ordinal diubah menjadi **rank numerik**.
3. Rank kemudian **dinormalisasi ke skala 0–1** menggunakan:


z_i = {r_i - 1}{K - 1}
