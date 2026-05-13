# Penjelasan Random Forest dan Tahapan Pengolahan Data di KNIME

## 1. Pengertian Random Forest

Random Forest adalah algoritma machine learning berbasis **ensemble learning** yang bekerja dengan membangun banyak pohon keputusan (*decision tree*) kemudian menggabungkan hasil dari seluruh pohon tersebut untuk menghasilkan prediksi yang lebih akurat dan stabil.

Algoritma ini dapat digunakan untuk:

* **Klasifikasi** → memprediksi kategori atau kelas.
* **Regresi** → memprediksi nilai numerik.

### Cara Kerja Random Forest

1. Dataset dibagi secara acak menjadi beberapa sampel.
2. Setiap sampel digunakan untuk membuat satu *decision tree*.
3. Setiap pohon memilih atribut secara acak ketika melakukan percabangan.
4. Hasil dari seluruh pohon digabungkan:

   * Klasifikasi → menggunakan voting terbanyak.
   * Regresi → menggunakan rata-rata hasil prediksi.

### Kelebihan Random Forest

* Akurasi tinggi.
* Mengurangi risiko *overfitting*.
* Dapat menangani data besar dan banyak atribut.
* Cocok untuk klasifikasi maupun regresi.

### Kekurangan Random Forest

* Proses pelatihan bisa lebih lambat dibanding satu decision tree.
* Membutuhkan sumber daya komputasi lebih besar.
* Interpretasi model lebih sulit.

---

# 2. Tahapan Pengolahan Data pada KNIME

Workflow pada gambar menunjukkan penggunaan algoritma Random Forest untuk dua jenis analisis:

1. **Klasifikasi**
2. **Regresi**

Berikut penjelasan setiap tahap yang digunakan.

---

## A. File Reader

Node **File Reader** digunakan untuk membaca dataset dari file eksternal, seperti file CSV.

### Fungsi:

* Mengimpor data ke KNIME.
* Membaca seluruh atribut dan isi dataset.
* Menentukan tipe data setiap kolom.

### Hasil:

Dataset berhasil dimasukkan ke dalam workflow KNIME.

---

## B. Table Partitioner

Node **Table Partitioner** digunakan untuk membagi dataset menjadi:

* **Data training**
* **Data testing**

### Fungsi:

* Data training digunakan untuk melatih model.
* Data testing digunakan untuk menguji performa model.

### Proses:

Umumnya pembagian dilakukan dengan rasio tertentu, misalnya:

* 70% data training
* 30% data testing

### Hasil:

Diperoleh dua bagian data:

1. Dataset pelatihan
2. Dataset pengujian

---

# 3. Proses Random Forest untuk Klasifikasi

## C. Random Forest Learner

Node **Random Forest Learner** digunakan untuk membuat model klasifikasi Random Forest.

### Fungsi:

* Melatih model menggunakan data training.
* Membentuk banyak decision tree.
* Menghasilkan model klasifikasi.

### Input:

* Data training dari Table Partitioner.

### Output:

* Model Random Forest.

---

## D. Random Forest Predictor

Node **Random Forest Predictor** digunakan untuk melakukan prediksi terhadap data testing.

### Fungsi:

* Menggunakan model Random Forest yang telah dilatih.
* Memprediksi kelas pada data testing.

### Input:

1. Model dari Random Forest Learner.
2. Data testing dari Table Partitioner.

### Output:

* Hasil prediksi klasifikasi.

---

## E. Scorer

Node **Scorer** digunakan untuk mengevaluasi hasil klasifikasi.

### Fungsi:

* Membandingkan hasil prediksi dengan data asli.
* Menghitung performa model.

### Hasil Evaluasi:

Beberapa metrik yang dihasilkan:

* Accuracy
* Precision
* Recall
* Confusion Matrix

### Tujuan:

Mengetahui seberapa baik model klasifikasi bekerja.

---

# 4. Proses Random Forest untuk Regresi

## F. Random Forest Learner (Regression)

Node **Random Forest Learner (Regression)** digunakan untuk membangun model Random Forest regresi.

### Fungsi:

* Melatih model regresi menggunakan data training.
* Memprediksi nilai numerik.

### Input:

* Data training.

### Output:

* Model Random Forest regresi.

---

## G. Random Forest Predictor (Regression)

Node **Random Forest Predictor (Regression)** digunakan untuk memprediksi nilai numerik pada data testing.

### Fungsi:

* Menggunakan model regresi yang telah dilatih.
* Menghasilkan nilai prediksi.

### Input:

1. Model regresi.
2. Data testing.

### Output:

* Hasil prediksi numerik.

---

## H. Column Filter

Node **Column Filter** digunakan untuk memilih kolom tertentu yang ingin ditampilkan atau dianalisis.

### Fungsi:

* Menyaring kolom yang diperlukan.
* Menghilangkan kolom yang tidak dibutuhkan.

### Tujuan:

Mempermudah visualisasi hasil prediksi.

---

## I. Line Plot (Legacy)

Node **Line Plot (Legacy)** digunakan untuk menampilkan grafik garis.

### Fungsi:

* Memvisualisasikan hasil prediksi regresi.
* Membandingkan nilai aktual dan nilai prediksi.

### Hasil:

Grafik garis yang menunjukkan pola hasil prediksi.

---

## J. Numeric Scorer

Node **Numeric Scorer** digunakan untuk mengevaluasi model regresi.

### Fungsi:

* Menghitung tingkat kesalahan prediksi.
* Menilai performa model regresi.

### Metrik yang Dihasilkan:

* RMSE (*Root Mean Square Error*)
* MAE (*Mean Absolute Error*)
* R² (*Coefficient of Determination*)

### Tujuan:

Mengetahui seberapa akurat model regresi dalam memprediksi nilai numerik.

---

# 5. Kesimpulan

Workflow KNIME pada gambar digunakan untuk melakukan:

1. Klasifikasi menggunakan Random Forest.
2. Regresi menggunakan Random Forest Regression.

Alur proses dimulai dari:

1. Membaca dataset.
2. Membagi data training dan testing.
3. Melatih model.
4. Melakukan prediksi.
5. Mengevaluasi hasil model.
6. Menampilkan visualisasi hasil regresi.

Metode Random Forest dipilih karena mampu menghasilkan prediksi yang cukup akurat dan stabil baik untuk klasifikasi maupun regresi.
