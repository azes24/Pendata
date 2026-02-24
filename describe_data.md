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

# Describe Data

Describing data atau mendeskripsikan data, adalah salah satu langkah penting dalam suatu penelitian dalam menyajikan data yang telah didapat agar lebih mudah dipahami oleh pembaca. Berbagai cara dapat dilakukan untuk mendapat hasil dari sebuah data, berupa rata-rata, data yang paling banyak keluar, dan sebagainya.

## IRIS FLower Case
Dari Dataset yang diambil dari IRIS Flower, dapat di visualisasikan menggunakan aplikasi Orange sehingga didapat pada bagian sepal_length-nya :
![Grafik Data](/gambar/iris01.png)
![Grafik Data](/gambar/iris02.png)

Dapat juga di implementasikan dengan menggunakan python, sehingga didapatkan data sebagai berikut
```{code-cell}
:tags: [hide-input]
import pandas as pd
from scipy import stats

df=pd.read_csv("IRIS.csv",usecols=[0])
kolom = df['sepal_length']

print("Jumlah data        :", kolom.count())
print("Rata-rata          :", round(kolom.mean(), 2))
print("Nilai minimal      :", kolom.min())
print("Q1                 :", kolom.quantile(0.25))
print("Q2 (Median)        :", kolom.quantile(0.5))
print("Q3                 :", kolom.quantile(0.75))
print("Nilai maksimal     :", kolom.max())
print("Kemencengan (skew) :", round(kolom.skew(), 2))
mode = stats.mode(kolom, keepdims=True)
print("Nilai modus        :", mode.mode[0])
print("Jumlah modus       :", mode.count[0])

print("Standar deviasi    :", round(kolom.std(), 2))
print("Variansi           :", round(kolom.var(), 2))
```

