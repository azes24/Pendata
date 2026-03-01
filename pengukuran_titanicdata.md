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
