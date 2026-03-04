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
Numerik (Rasio/Kontinu) : UTS, UAS, Nilai
Numerik (Diskrit/ID) : ID
Ordinal : nilai huruf
Kategorikal : Nama, Matkul
binary :  Gender
```


---

## Pengukuran Jarak

Pengukuran jarak dilakukan pada **5 sampel pertama** yang memiliki data lengkap. Kolom yang digunakan disesuaikan dengan tipe datanya masing-masing.
