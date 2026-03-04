## Menghitung Jarak Antar Data Numerik

Jika digunakan untuk clustering atau klasifikasi, jarak antar dua siswa dapat dihitung menggunakan nilai numerik (UTS, UAS, Nilai).

![Grafik Data](/gambar/scatterpelatihan.png)

### a. Euclidean Distance


\[
d(x,y) = \sqrt{(UTS_1-UTS_2)^2 + (UAS_1-UAS_2)^2 + (Nilai_1-Nilai_2)^2}
\]

Contoh:
Data A: (80, 85, 82)  
Data B: (70, 75, 72)

\[
d = \sqrt{(80-70)^2 + (85-75)^2 + (82-72)^2}
\]

\[
d = \sqrt{100 + 100 + 100} = \sqrt{300}
\]

#### Visualisasi Distance dalam  orange

![Grafik Data](/gambar/enclideanpelatihan.png)

---

### b. Manhattan Distance

\[
d(x,y) = |x_1-y_1| + |x_2-y_2| + |x_3-y_3|
\]

\[
d = |80-70| + |85-75| + |82-72|
\]

\[
d = 10 + 10 + 10 = 30
\]


#### Visualisasi Distance dalam  orange

![Grafik Data](/gambar/manhatpelatihan.png)

---