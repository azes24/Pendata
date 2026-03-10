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
### Euclidean

Disini saya mengambil perhitungan jarak dari data ke-1 dan ke-40 dalam data IRIS,

$$
d=\sqrt{(5.1-5.1)^2+(3.5-3.4)^2+(1.4-1.5)^2+(0.2-0.2)^2}
$$

$$
d=\sqrt{0^2+0.1^2+(-0.1)^2+0^2}
$$

$$
d=\sqrt{0+0.01+0.01+0}
$$

$$
d=\sqrt{0.02}=0.1414
$$

![Grafik Data](/gambar/enclideaniris.png)


### Manhattan

Disini saya mengambil perhitungan jarak dari data ke-1 dan ke-40 dalam data IRIS,

$$
d=|5.1-5.1|+|3.5-3.4|+|1.4-1.5|+|0.2-0.2|
$$

$$
d=0+0.1+0.1+0=0.2
$$


![Grafik Data](/gambar/manhattaniris.png)
