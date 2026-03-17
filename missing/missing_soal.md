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


# Prediksi Missing Value Data Soal

Terdapat Data dengan missing Value sebagai berikut.
![Grafik Data](../gambar/soal01.png)
Data diatas sudah dinormalisasi dengan cara min-max, sehingga tinggal menghitung jarak untuk memprediksi Missing Value.

## Jarak

Jarak dari data ke-7 dan yang lain bedasarkan 2 kolom IPK dan PO.
![Grafik Data](../gambar/jarak01soal.png)

## Missing Value

Dari jarak yang ditemukan, kemudian diurutkan dari yang paling kecil. Disini K = 3, dari ketiga data teratas maka didapat missing valuenya adalah 0,6.
![Grafik Data](../gambar/missing01soal.png)

jika dilihat dari data aslinya adalah 2 dan 3 dan kalau saat normalisasi adalah 0 dan 1 , sedangkan yang didapat disini adalah 0,6, maka bisa dibulatkan menjadi 1.

## Code 

```{code-cell}

import numpy as np

data = np.array([
    [0,   0,   0],
    [0.5, 0.5, 1],
    [1,   0,   0],
    [0,   0,   1],
    [0.5, 0.5, 0],
    [1,   1,   1],
])

query = np.array([0, 0.5])
k = 3

jarak = [(i+1, round(np.sqrt(np.sum((query - r[:2])**2)), 4), int(r[2])) for i, r in enumerate(data)]
knn   = sorted(jarak, key=lambda x: x[1])[:k]
bobot = [1/d**2 for _, d, _ in knn]
pred  = sum(b*l for b, (_, _, l) in zip(bobot, knn)) / sum(bobot)

print("Prediksi JML =", round(pred, 4))
print("Jika dibulatkan maka =", round(pred))
```
