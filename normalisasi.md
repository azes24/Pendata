# Preprocessing
# Normalisasi Data

Normalisasi data merupakan salah satu teknik preprocessing (pra-pemrosesan) data yang digunakan untuk mengubah nilai-nilai fitur atau atribut dalam dataset ke dalam skala atau rentang tertentu, sehingga setiap fitur memiliki kontribusi yang setara dalam proses analisis atau pemodelan.

## Metode Normalisasi Data

### 1. Min-Max Normalization

Min-Max Normalization adalah metode normalisasi yang mengubah nilai data ke dalam rentang **[0, 1]** (atau rentang [a, b] yang ditentukan). Metode ini memetakan nilai minimum menjadi 0 dan nilai maksimum menjadi 1.

**Rumus:**


$$x' = \frac{x - x_{\min}}{x_{\max} - x_{\min}}$$
 
Jika ingin dinormalisasi ke rentang $[a, b]$:
 
$$x' = a + \frac{(x - x_{\min})(b - a)}{x_{\max} - x_{\min}}$$
 
**Keterangan:**
- $x$ = nilai asli
- $x'$ = nilai hasil normalisasi
- $x_{\min}$ = nilai minimum dalam dataset
- $x_{\max}$ = nilai maksimum dalam dataset
- $a, b$ = batas bawah dan atas rentang tujuan
 
**Contoh Perhitungan**
 
Diketahui data nilai ujian: $\{40, 60, 80, 100\}$
 
$$x'_{60} = \frac{60 - 40}{100 - 40} = \frac{20}{60} \approx 0.333$$

### 2. Z-Score Standardization (Standarisasi)

Z-Score Standardization mengubah data sehingga memiliki **rata-rata (mean) = 0** dan **standar deviasi = 1**. Metode ini tidak membatasi rentang output pada interval tertentu.
 
**Rumus**
 
$$x' = \frac{x - \mu}{\sigma}$$
 
**Keterangan:**
- $x$ = nilai asli
- $x'$ = nilai hasil standarisasi (disebut juga *z-score*)
- $\mu$ = rata-rata (*mean*) dari dataset
- $\sigma$ = standar deviasi dari dataset
 
$$\mu = \frac{1}{n} \sum_{i=1}^{n} x_i$$
 
$$\sigma = \sqrt{\frac{1}{n} \sum_{i=1}^{n} (x_i - \mu)^2}$$
 
**Contoh Perhitungan**
 
Diketahui data: $\{2, 4, 6, 8, 10\}$
 
$$\mu = \frac{2+4+6+8+10}{5} = 6$$
 
$$\sigma = \sqrt{\frac{(2-6)^2 + (4-6)^2 + (6-6)^2 + (8-6)^2 + (10-6)^2}{5}} = \sqrt{8} \approx 2.83$$
 
$$x'_{4} = \frac{4 - 6}{2.83} \approx -0.707$$
 
### 3. Decimal Scaling
 
Decimal Scaling melakukan normalisasi dengan membagi setiap nilai pada dataset dengan **pangkat 10** tertentu, sehingga nilai absolut dari semua data menjadi kurang dari 1.
 
**Rumus**
 
$$x' = \frac{x}{10^j}$$
 
**Keterangan:**
- $x$ = nilai asli
- $x'$ = nilai hasil normalisasi
- $j$ = bilangan bulat terkecil sehingga $\max(|x'|) < 1$
 
Nilai $j$ ditentukan dengan:
 
$$j = \lceil \log_{10}(\max(|x|)) \rceil$$
 
**Contoh Perhitungan**
 
Diketahui data: $\{-500, -200, 0, 300, 800\}$
 
Nilai absolut maksimum = $800$
 
$$j = \lceil \log_{10}(800) \rceil = \lceil 2.903 \rceil = 3$$
 
$$x'_{800} = \frac{800}{10^3} = \frac{800}{1000} = 0.8$$
 
$$x'_{-500} = \frac{-500}{1000} = -0.5$$
 


## Manfaat Normalisasi Data

- Mempercepat proses pelatihan model (training)
- Meningkatkan akurasi dan performa algoritma
- Menghindari dominasi fitur dengan skala besar
- Membantu algoritma berbasis jarak (seperti KNN, SVM, dan K-Means) bekerja lebih optimal

## Kesimpulan

Normalisasi data merupakan tahap preprocessing yang sangat penting sebelum data dimasukkan ke dalam model, karena memastikan bahwa seluruh fitur berada pada skala yang sebanding sehingga proses pembelajaran mesin dapat berjalan secara optimal dan menghasilkan model yang lebih akurat.