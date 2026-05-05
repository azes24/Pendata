# Analisis Klasifikasi Naive Bayes dengan Scikit-Learn

## Pendahuluan

Naive Bayes adalah metode klasifikasi berbasis probabilitas yang bekerja dari Teorema Bayes. Asumsi utamanya adalah setiap atribut bersifat *conditionally independent* terhadap atribut lain jika kelas diketahui. Artinya:

```
P(X | Ci) = P(x1 | Ci) * P(x2 | Ci) * ... * P(xn | Ci)
```

Klasifikasi dilakukan dengan memilih kelas Ci yang memaksimumkan nilai posterior:

```
P(Ci | X) = P(X | Ci) * P(Ci) / P(X)
```

Karena P(X) konstan untuk semua kelas, kita hanya perlu memaksimumkan:

```
P(Ci | X) ∝ P(X | Ci) * P(Ci)
```

---

## Dataset: Buys Computer

Dataset `buys_computer.csv` memiliki **14 record** dengan **4 atribut kategorik**: `age`, `income`, `student`, `credit_rating`. Label kelas adalah `buys_computer` (yes/no).

### Isi Dataset

| age   | income | student | credit_rating | buys_computer |
|-------|--------|---------|---------------|---------------|
| <=30  | high   | no      | fair          | no            |
| <=30  | high   | no      | excellent     | no            |
| 31-40 | high   | no      | fair          | yes           |
| >40   | medium | no      | fair          | yes           |
| >40   | low    | yes     | fair          | yes           |
| >40   | low    | yes     | excellent     | no            |
| 31-40 | low    | yes     | excellent     | yes           |
| <=30  | medium | no      | fair          | no            |
| <=30  | low    | yes     | fair          | yes           |
| >40   | medium | yes     | fair          | yes           |
| <=30  | medium | yes     | excellent     | yes           |
| 31-40 | medium | no      | excellent     | yes           |
| 31-40 | high   | yes     | fair          | yes           |
| >40   | medium | no      | excellent     | no            |

**Distribusi kelas:** `yes` = 9 record, `no` = 5 record

---

## Script Python

### Import Library dan Baca Data

```python
import pandas as pd
import numpy as np
from sklearn.naive_bayes import CategoricalNB
from sklearn.preprocessing import OrdinalEncoder
from sklearn.metrics import classification_report, confusion_matrix, accuracy_score

# Baca dari CSV
df = pd.read_csv('buys_computer.csv')

print("=== Dataset ===")
print(df.to_string(index=False))
print(f"\nJumlah record: {len(df)}")
print(f"Distribusi kelas:\n{df['buys_computer'].value_counts()}")
```

**Output:**
```
=== Dataset ===
  age income student credit_rating buys_computer
 <=30   high      no          fair            no
 <=30   high      no     excellent            no
31-40   high      no          fair           yes
  >40 medium      no          fair           yes
  >40    low     yes          fair           yes
  >40    low     yes     excellent            no
31-40    low     yes     excellent           yes
 <=30 medium      no          fair            no
 <=30    low     yes          fair           yes
  >40 medium     yes          fair           yes
 <=30 medium     yes     excellent           yes
31-40 medium      no     excellent           yes
31-40   high     yes          fair           yes
  >40 medium      no     excellent            no

Jumlah record: 14
Distribusi kelas:
buys_computer
yes    9
no     5
```

---

### Encoding dan Training Model

```python
# Pisah fitur dan label
X = df.drop(columns='buys_computer')
y = df['buys_computer']

# OrdinalEncoder mengubah kategori menjadi angka
enc = OrdinalEncoder()
X_enc = enc.fit_transform(X)
y_enc = (y == 'yes').astype(int)  # yes=1, no=0

print("=== Kategori per Kolom (setelah encoding) ===")
for i, col in enumerate(X.columns):
    print(f"  {col}: {list(enc.categories_[i])}")

# Training CategoricalNB dengan Laplace smoothing (alpha=1)
model = CategoricalNB(alpha=1.0)
model.fit(X_enc, y_enc)
```

**Output:**
```
=== Kategori per Kolom (setelah encoding) ===
  age: ['31-40', '<=30', '>40']
  income: ['high', 'low', 'medium']
  student: ['no', 'yes']
  credit_rating: ['excellent', 'fair']
```

> **Catatan:** `OrdinalEncoder` secara default mengurutkan kategori secara alfabetis. Indeks numerik ini digunakan secara internal oleh `CategoricalNB`.

---

### Evaluasi Model pada Training Set

```python
y_pred = model.predict(X_enc)

print("=== Evaluasi Model (Training Set) ===")
print(f"Accuracy: {accuracy_score(y_enc, y_pred):.4f}")
print("\nClassification Report:")
print(classification_report(y_enc, y_pred, target_names=['no', 'yes']))
print("Confusion Matrix:")
print(confusion_matrix(y_enc, y_pred))
```

**Output:**
```
=== Evaluasi Model (Training Set) ===
Accuracy: 0.9286

Classification Report:
              precision    recall  f1-score   support

          no       1.00      0.80      0.89         5
         yes       0.90      1.00      0.95         9

    accuracy                           0.93        14
   macro avg       0.95      0.90      0.92        14
weighted avg       0.94      0.93      0.93        14

Confusion Matrix:
[[4 1]
 [0 9]]
```

**Interpretasi Confusion Matrix:**

|                  | Prediksi: no | Prediksi: yes |
|------------------|:------------:|:-------------:|
| **Aktual: no**   | 4 (TN)       | 1 (FP)        |
| **Aktual: yes**  | 0 (FN)       | 9 (TP)        |

Model memprediksi 13 dari 14 record dengan benar (accuracy 92.86%). Terdapat 1 kesalahan: 1 record berlabel `no` diprediksi `yes`.

---

### Prediksi Data Baru

```python
# Prediksi X = (age<=30, income=medium, student=yes, credit_rating=fair)
# Contoh dari materi kuliah
data_baru = pd.DataFrame([{
    'age': '<=30',
    'income': 'medium',
    'student': 'yes',
    'credit_rating': 'fair'
}])

X_baru_enc = enc.transform(data_baru)
prediksi   = model.predict(X_baru_enc)
probabilitas = model.predict_proba(X_baru_enc)

label = ['no', 'yes']
print("=== Prediksi Data Baru ===")
print("Input: age<=30, income=medium, student=yes, credit_rating=fair")
print(f"Prediksi kelas: buys_computer = {label[prediksi[0]]}")
print(f"P(buys_computer=no  | X) = {probabilitas[0][0]:.4f}")
print(f"P(buys_computer=yes | X) = {probabilitas[0][1]:.4f}")
```

**Output:**
```
=== Prediksi Data Baru ===
Input: age<=30, income=medium, student=yes, credit_rating=fair
Prediksi kelas: buys_computer = yes
P(buys_computer=no  | X) = 0.2322
P(buys_computer=yes | X) = 0.7678
```

---

## Verifikasi Perhitungan Manual

### Prior Probability

Dari 14 data training:

| Kelas | Count | P(kelas) |
|-------|-------|----------|
| yes   | 9     | 9/14 = **0.6429** |
| no    | 5     | 5/14 = **0.3571** |

### Tabel Probabilitas Kondisional P(fitur \| kelas) dengan Laplace α=1

| Atribut       | Nilai     | P(\|no)  | P(\|yes) |
|---------------|-----------|----------|----------|
| age           | 31-40     | 0.1250   | 0.4167   |
|               | <=30      | 0.5000   | 0.2500   |
|               | >40       | 0.3750   | 0.3333   |
| income        | high      | 0.3750   | 0.2500   |
|               | low       | 0.2500   | 0.3333   |
|               | medium    | 0.3750   | 0.4167   |
| student       | no        | 0.7143   | 0.3636   |
|               | yes       | 0.2857   | 0.6364   |
| credit_rating | excellent | 0.5714   | 0.3636   |
|               | fair      | 0.4286   | 0.6364   |

### Perhitungan untuk X = (age<=30, income=medium, student=yes, credit_rating=fair)

**Skor kelas `yes`:**

```
P(yes|X) ∝ P(age<=30|yes) × P(medium|yes) × P(student=yes|yes) × P(fair|yes) × P(yes)
         = 0.2500 × 0.4167 × 0.6364 × 0.6364 × 0.6429
         = 0.027118
```

**Skor kelas `no`:**

```
P(no|X) ∝ P(age<=30|no) × P(medium|no) × P(student=yes|no) × P(fair|no) × P(no)
        = 0.5000 × 0.3750 × 0.2857 × 0.4286 × 0.3571
        = 0.008200
```

**Normalisasi:**

```
P(yes|X) = 0.027118 / (0.027118 + 0.008200) = 0.7678
P(no|X)  = 0.008200 / (0.027118 + 0.008200) = 0.2322
```

**Kesimpulan: X diprediksi sebagai buys\_computer = `yes`** ✓

---

## Laplacian Correction

Masalah probabilitas nol muncul ketika suatu kombinasi nilai atribut dan kelas tidak pernah muncul di data training. Laplacian correction menambahkan konstanta α ke setiap hitungan frekuensi:

```
P(xi | Cj) = (count(xi, Cj) + α) / (count(Cj) + α × |Vi|)
```

di mana |Vi| adalah jumlah nilai unik atribut i.

```python
# alpha=0.0: tanpa smoothing (bisa menghasilkan probabilitas = 0)
# alpha=1.0: Laplace smoothing (default yang direkomendasikan)

model_tanpa_laplace = CategoricalNB(alpha=0.0)
model_dengan_laplace = CategoricalNB(alpha=1.0)

model_tanpa_laplace.fit(X_enc, y_enc)
model_dengan_laplace.fit(X_enc, y_enc)
```

**Contoh kasus:** Pada data training, tidak ada record dengan `age=31-40` dan `buys_computer=no`.

| Metode          | P(age=31-40 \| no)          |
|-----------------|-----------------------------|
| Tanpa Laplace   | 0 / 5 = **0.0000** ⚠        |
| Dengan Laplace  | (0+1) / (5+3) = **0.1250** ✓ |

Tanpa Laplace, skor kelas `no` akan selalu nol untuk input apapun yang mengandung `age=31-40`, sehingga hasil prediksi menjadi tidak andal.

---

## Perbandingan Varian Naive Bayes di Sklearn

| Kelas           | Tipe Data           | Keterangan                                               |
|-----------------|---------------------|----------------------------------------------------------|
| `GaussianNB`    | Kontinu             | Distribusi fitur diasumsikan Gaussian (normal)           |
| `CategoricalNB` | Kategorik           | Cocok untuk atribut diskrit seperti dataset ini          |
| `MultinomialNB` | Hitungan/frekuensi  | Umum dipakai untuk klasifikasi teks (bag-of-words)       |
| `BernoulliNB`   | Biner (0/1)         | Cocok untuk fitur boolean                                |
| `ComplementNB`  | Hitungan/frekuensi  | Versi perbaikan MultinomialNB untuk data tidak seimbang  |

Untuk dataset `buys_computer` yang semua atributnya bersifat kategorik, **`CategoricalNB`** adalah pilihan yang paling tepat.

---

## Kelebihan dan Keterbatasan

**Kelebihan:**

- Mudah diimplementasikan dan sangat cepat dilatih
- Bekerja baik pada dataset kecil
- Efektif bahkan ketika dimensi fitur tinggi
- Tidak sensitif terhadap fitur yang tidak relevan jika jumlah data cukup

**Keterbatasan:**

- Asumsi independensi antar atribut jarang terpenuhi dalam data nyata
- Akurasi menurun ketika ada korelasi kuat antar fitur
- Tidak bisa memodelkan ketergantungan antar variabel (berbeda dengan Bayesian Belief Network)

Bayesian Belief Network (BBN) hadir sebagai solusi atas keterbatasan ini. BBN menggunakan graf berarah tanpa siklus (DAG) untuk merepresentasikan hubungan sebab-akibat antar variabel.

---
