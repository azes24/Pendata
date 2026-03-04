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

# Eksplorasi Data IRIS

Eksplorasi data adalah proses awal dalam analisis data yang bertujuan untuk memahami karakteristik, struktur, pola, dan kualitas suatu dataset sebelum dilakukan pemodelan atau analisis lanjutan. Tahap ini membantu peneliti atau analis mengenali isi data secara menyeluruh agar dapat menentukan metode analisis yang tepat. Disini saya menggunakan studi kasus IRIS Flower.

Dari sumber dataset IRIS Flower didapatkan sebanyak 5 fitur dengan 150 data, diantaranya sebagai berikut :

```{code-cell}
:tags: [hide-input]
import pandas as pd

df = pd.read_csv("IRIS.csv")
df.index = df.index + 1
df.head(150)
```
data IRIS Flower diatas diambil dari kaggle.

