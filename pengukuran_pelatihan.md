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

# Pengukuran jarak Data PELATIHAN

Dataset Pelatihan ini merupakan dataset yang berisi data nilai siswa/mahasiswa pada beberapa mata kuliah. Setiap baris merepresentasikan satu data peserta yang mencakup informasi identitas (ID dan Nama), jenis kelamin, mata kuliah yang diambil, nilai UTS, nilai UAS, nilai akhir, serta nilai dalam bentuk huruf.
berikut adalah datanya.


```{code-cell}
:tags: [hide-input]
import pandas as pd

df = pd.read_csv("Dataset_Pelatihan.csv")
df.index = df.index + 1
df.head(len(df))
```
data Titanic diatas diambil dari kaggle.

## Klasifikasi Tipe Data

Setiap kolom dalam dataset diklasifikasikan sebagai berikut:

| Kolom        | Tipe Data | Penjelasan |
|--------------|-----------|------------|
| `ID` | Numerik (Identifier / Diskrit) | Nomor identitas siswa. Bersifat numerik tetapi hanya sebagai penanda unik. |
| `Nama` | Nominal (Teks) | Nama siswa, tidak memiliki urutan atau nilai numerik. |
| `Gender` | Nominal (Kategorik) | Jenis kelamin (misal: Laki-laki, Perempuan), tidak memiliki urutan. |
| `Matkul` | Nominal (Kategorik) | Mata kuliah (Matematika, Kimia, Biologi, dll), tidak memiliki urutan. |
| `UTS` | Numerik (Rasio/Kontinu) | Nilai Ujian Tengah Semester (rentang 0–100). |
| `UAS` | Numerik (Rasio/Kontinu) | Nilai Ujian Akhir Semester (rentang 0–100). |
| `Nilai` | Numerik (Rasio/Kontinu) | Nilai akhir (umumnya hasil perhitungan dari UTS dan UAS). |
| `nilai huruf` | Ordinal | Nilai dalam bentuk huruf (A, B, C, D, E) yang memiliki tingkatan kualitas. |

### Ringkasan Klasifikasi Tipe Data
```
Numerik : UTS, UAS, Nilai
Ordinal : nilai huruf
Kategorikal : Matkul
binary :  Gender
```


---

## Pengukuran Jarak
### Normalisasi/Konversi

#### Konversi Atribut Binary
Atribut binary harus diubah menjadi nilai 0 dan 1.

| Gender    | Nilai |
| --------- | ----- |
| Laki-laki | 1     |
| Perempuan | 0     |

Contoh Hasil :
| ID | Nama | Gender       | Matkul     | UTS | UAS | Nilai | Nilai Huruf |
| -- | ---- | ------------ | ---------- | --- | --- | ----- | ----------- |
| 1  | Budi | 1            | Kimia      | 90  | 80  | 85    | A           |
| 2  | Ani  | 0            | Matematika | 75  | 80  | 77.5  | B           |
| 3  | Joko | 1            | Biologi    | 85  | 95  | 90    | A           |
| 4  | Siti | 0            | Matematika | 55  | 65  | 60    | D           |
| 5  | Agus | 1            | Fisika     | 80  | 75  | 77.5  | B           |
| 6  | Dewi | 0            | Biologi    | 75  | 95  | 85    | A           |
| 7  | Eka  | 0            | Matematika | 85  | 95  | 90    | A           |
| 8  | Adi  | 1            | Fisika     | 70  | 65  | 67.5  | C           |

#### Pengukuran Atribut Kategorikal

Atribut kategorikal menggunakan simple matching.

contoh pengukuran a=(2,3) b=(2,4)
| a      | b          | Jarak |
| ------ | ---------- | ----- |
| matematika  | matematika      | 0     |
| biologi  | Matematika | 1     |

#### Pengukuran Atribut Numerik

Atribut numerik dihitung menggunakan selisih yang dinormalisasi.

Normalisasi digunakan agar skala nilai tidak mendominasi perhitungan jarak.

Rumus normalisasi:

$$
x' = \frac{x - \min(x)}{\max(x) - \min(x)}
$$

Setelah normalisasi, jarak numerik dihitung dengan:

$$
d_{ij} = |x_i - x_j|
$$

Contoh:

$$
\min(UTS) = 55
$$

$$
\max(UTS) = 90
$$

Untuk data dengan nilai UTS = 90

$$
UTS_1 = \frac{90 - 55}{90 - 55} = 1
$$

Untuk data dengan nilai UTS = 75

$$
UTS_2 = \frac{75 - 55}{90 - 55} = 0.57
$$

Jadi, jarak dari 2 nilai UTS: 

$$
d = |1 - 0.57| = 0.43
$$

Contoh Hasil:
| ID | Nama | Gender       | Matkul     | UTS  | UAS  | Nilai | Nilai Huruf |
| -- | ---- | ------------ | ---------- | ---- | ---- | ----- | ----------- |
| 1  | Budi | 1            | Kimia      | 1.00 | 0.50 | 0.83  | A           |
| 2  | Ani  | 0            | Matematika | 0.57 | 0.50 | 0.58  | B           |
| 3  | Joko | 1            | Biologi    | 0.86 | 1.00 | 1.00  | A           |
| 4  | Siti | 0            | Matematika | 0.00 | 0.00 | 0.00  | D           |
| 5  | Agus | 1            | Fisika     | 0.71 | 0.33 | 0.58  | B           |
| 6  | Dewi | 0            | Biologi    | 0.57 | 1.00 | 0.83  | A           |
| 7  | Eka  | 0            | Matematika | 0.86 | 1.00 | 1.00  | A           |
| 8  | Adi  | 1            | Fisika     | 0.43 | 0.00 | 0.25  | C           |


#### Pengukuran Atribut Ordinal

Atribut ordinal memiliki urutan nilai.

Contoh Nilai Huruf:
| Nilai Huruf | Rank |
| ----------- | ---- |
| A           | 4    |
| B           | 3    |
| C           | 2    |
| D           | 1    |

Contoh Normalisasi Rank:

$$
A/4 = \frac{4 - 1}{4 - 1} = 1
$$

$$
B/3 = \frac{3 - 1}{4 - 1} = 0.67
$$

Jadi jarak ordinal antara 2 rank diatas:

$$
d = |1-0.67| = 0.33
$$ 

Contoh Hasil :
| ID | Nama | Gender       | Matkul     | UTS  | UAS  | Nilai | Nilai Huruf |
| -- | ---- | ------------ | ---------- | ---- | ---- | ----- | ----------- |
| 1  | Budi | 1            | Kimia      | 1.00 | 0.50 | 0.83  | 1.00           |
| 2  | Ani  | 0            | Matematika | 0.57 | 0.50 | 0.58  | 0.67           |
| 3  | Joko | 1            | Biologi    | 0.86 | 1.00 | 1.00  | 1.00          |
| 4  | Siti | 0            | Matematika | 0.00 | 0.00 | 0.00  | 0.00         |
| 5  | Agus | 1            | Fisika     | 0.71 | 0.33 | 0.58  |  0.67           |
| 6  | Dewi | 0            | Biologi    | 0.57 | 1.00 | 0.83  | 1.00          |
| 7  | Eka  | 0            | Matematika | 0.86 | 1.00 | 1.00  | 1.00          |
| 8  | Adi  | 1            | Fisika     | 0.43 | 0.00 | 0.25  | 0.33          |

### Menghitung Gower Distance

Setelah semua atribut dihitung, jarak total dihitung dengan Gower Distance.

Rumus:
$$
D_{ij} = \frac{\sum_{k=1}^{p} d_{ijk}}{p}
$$

dedngan:
- D_ij = jarak antara objek i dan j
- d_ij = jarak atribut ke-k antara objek i dan j
- p = jumlah atribut

Contoh Perhitungan

jarak tiap atribut antara Data 1 dan Data 2 adalah:

| Atribut     | Jarak |
| ----------- | ----- |
| Gender      | 1     |
| Matkul      | 0     |
| UTS         | 0.43  |
| UAS         | 0     |
| Nilai       | 0.27  |
| Nilai Huruf | 0.33  |

Maka:

$$
D_{12} = \frac{1 + 0 + 0.43 + 0 + 0.27 + 0.33}{6}
$$

$$
D_{12} = \frac{2.03}{6}
$$

$$
D_{12} = 0.338
$$