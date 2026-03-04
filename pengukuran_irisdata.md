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
Numerik : sepal_length, sepal_width, petal_length, petal_width
Kategorikal     : species
```

## Pengukuran Jarak

Pengukuran jarak dilakukan pada **5 sampel representatif** — 2 dari *Iris-setosa*, 2 dari *Iris-versicolor*, dan 1 dari *Iris-virginica* — agar perbedaan antar spesies terlihat jelas.

### Manhattan

![Grafik Data](/gambar/manhattaniris.png)
