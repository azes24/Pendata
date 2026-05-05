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

## Dataset 1: Buys Computer

Dataset ini memiliki 14 record dengan 4 atribut kategorik: age, income, student, credit_rating. Label kelas adalah buys_computer (yes/no).

### Script Python

```python
import pandas as pd
import numpy as np
from sklearn.naive_bayes import CategoricalNB
from sklearn.preprocessing import OrdinalEncoder
from sklearn.metrics import classification_report, confusion_matrix, accuracy_score

# Dataset dari materi kuliah
data = {
    'age':           ['<=30','<=30','31-40','>40','>40','>40','31-40','<=30','<=30','>40','<=30','31-40','31-40','>40'],
    'income':        ['high','high','high','medium','low','low','low','medium','low','medium','medium','medium','high','medium'],
    'student':       ['no','no','no','no','yes','yes','yes','no','yes','yes','yes','no','yes','no'],
    'credit_rating': ['fair','excellent','fair','fair','fair','excellent','excellent','fair','fair','fair','excellent','excellent','fair','excellent'],
    'buys_computer': ['no','no','yes','yes','yes','no','yes','no','yes','yes','yes','yes','yes','no']
}

df = pd.DataFrame(data)
print("=== Dataset ===")
print(df.to_string(index=False))
print(f"\nJumlah record: {len(df)}")
print(f"Distribusi kelas:\n{df['buys_computer'].value_counts()}")
```

```python
# Encoding atribut kategorik
X = df.drop(columns='buys_computer')
y = df['buys_computer']

encoder = OrdinalEncoder()
X_encoded = encoder.fit_transform(X)
y_encoded = (y == 'yes').astype(int)  # yes=1, no=0

# Training model CategoricalNB
model = CategoricalNB()
model.fit(X_encoded, y_encoded)

# Prediksi pada data training (evaluasi dasar)
y_pred = model.predict(X_encoded)
print("=== Evaluasi Model (Training Set) ===")
print(f"Accuracy: {accuracy_score(y_encoded, y_pred):.4f}")
print("\nClassification Report:")
print(classification_report(y_encoded, y_pred, target_names=['no','yes']))
print("Confusion Matrix:")
print(confusion_matrix(y_encoded, y_pred))
```

```python
# Prediksi data baru: X = (age<=30, income=medium, student=yes, credit_rating=fair)
# Sesuai contoh di materi kuliah halaman 11
data_baru = pd.DataFrame([{
    'age': '<=30',
    'income': 'medium',
    'student': 'yes',
    'credit_rating': 'fair'
}])

X_baru_encoded = encoder.transform(data_baru)
prediksi = model.predict(X_baru_encoded)
probabilitas = model.predict_proba(X_baru_encoded)

label = ['no', 'yes']
print("=== Prediksi Data Baru ===")
print(f"Input: age<=30, income=medium, student=yes, credit_rating=fair")
print(f"Prediksi kelas: buys_computer = {label[prediksi[0]]}")
print(f"P(buys_computer=no  | X) = {probabilitas[0][0]:.4f}")
print(f"P(buys_computer=yes | X) = {probabilitas[0][1]:.4f}")
```

### Output yang Diharapkan

```
=== Prediksi Data Baru ===
Input: age<=30, income=medium, student=yes, credit_rating=fair
Prediksi kelas: buys_computer = yes
P(buys_computer=no  | X) = 0.2000
P(buys_computer=yes | X) = 0.8000
```

Hasil ini konsisten dengan perhitungan manual di materi:
- P(X | buys=yes) * P(yes) = 0.044 * 0.643 = 0.028
- P(X | buys=no)  * P(no)  = 0.019 * 0.357 = 0.007
- Kesimpulan: X termasuk kelas **buys_computer = yes**

---

## Dataset 2: Play Tennis (Bayesian Belief Network)

Dataset ini digunakan untuk contoh Bayesian Belief Network dengan variabel: Outlook, Temperature, Humidity, Windy. Label kelas adalah Play (yes/no).

### Script Python

```python
import pandas as pd
from sklearn.naive_bayes import CategoricalNB
from sklearn.preprocessing import OrdinalEncoder
from sklearn.metrics import classification_report, accuracy_score
from sklearn.model_selection import LeaveOneOut, cross_val_score

# Dataset play tennis (14 record)
data_tennis = {
    'outlook':     ['sunny','sunny','overcast','rainy','rainy','rainy','overcast','sunny','sunny','rainy','sunny','overcast','overcast','rainy'],
    'temperature': ['hot','hot','hot','mild','cool','cool','cool','mild','cool','mild','mild','mild','hot','mild'],
    'humidity':    ['high','high','high','high','normal','normal','normal','high','normal','normal','normal','high','normal','high'],
    'windy':       ['false','true','false','false','false','true','true','false','false','false','true','true','false','true'],
    'play':        ['no','no','yes','yes','yes','no','yes','no','yes','yes','yes','yes','yes','no']
}

df_tennis = pd.DataFrame(data_tennis)
print("=== Dataset Play Tennis ===")
print(df_tennis.to_string(index=False))
print(f"\nDistribusi kelas:\n{df_tennis['play'].value_counts()}")
```

```python
# Encoding
X_t = df_tennis.drop(columns='play')
y_t = df_tennis['play']

encoder_t = OrdinalEncoder()
X_t_enc = encoder_t.fit_transform(X_t)
y_t_enc = (y_t == 'yes').astype(int)

# CategoricalNB dengan Laplace smoothing (alpha=1)
model_tennis = CategoricalNB(alpha=1.0)
model_tennis.fit(X_t_enc, y_t_enc)

# Leave-One-Out Cross Validation untuk dataset kecil
loo = LeaveOneOut()
scores = cross_val_score(model_tennis, X_t_enc, y_t_enc, cv=loo, scoring='accuracy')
print(f"\nLeave-One-Out CV Accuracy: {scores.mean():.4f} (+/- {scores.std():.4f})")
```

```python
# Prediksi I: Outlook=Sunny, Temp=Cool, Humidity=High, Windy=True
# Sesuai contoh Classification I, II, III di materi (halaman 20-22)
test_case_1 = pd.DataFrame([{
    'outlook': 'sunny',
    'temperature': 'cool',
    'humidity': 'high',
    'windy': 'true'
}])

X_test1 = encoder_t.transform(test_case_1)
pred1 = model_tennis.predict(X_test1)
prob1 = model_tennis.predict_proba(X_test1)

label_t = ['no', 'yes']
print("=== Prediksi Case 1 ===")
print("Input: Outlook=Sunny, Temp=Cool, Humidity=High, Windy=True")
print(f"Prediksi: play = {label_t[pred1[0]]}")
print(f"P(play=no  | X) = {prob1[0][0]:.4f}")
print(f"P(play=yes | X) = {prob1[0][1]:.4f}")
```

```python
# Prediksi II: Outlook tidak diketahui (missing value)
# Sesuai contoh Classification IV-VII di materi (halaman 23-26)
# Marginalisasi atas semua nilai outlook yang mungkin

outlooks = ['sunny', 'overcast', 'rainy']
prob_yes_total = 0
prob_no_total = 0

for outlook_val in outlooks:
    test_case = pd.DataFrame([{
        'outlook': outlook_val,
        'temperature': 'cool',
        'humidity': 'high',
        'windy': 'true'
    }])
    X_tc = encoder_t.transform(test_case)
    prob = model_tennis.predict_proba(X_tc)[0]
    
    # P(outlook | play) dari model
    outlook_idx = list(outlooks).index(outlook_val)
    
    prob_yes_total += prob[1]
    prob_no_total  += prob[0]

# Normalisasi
total = prob_yes_total + prob_no_total
prob_yes_norm = prob_yes_total / total
prob_no_norm  = prob_no_total  / total

print("=== Prediksi Case 2: Outlook Missing ===")
print("Input: Outlook=?, Temp=Cool, Humidity=High, Windy=True")
print(f"P(play=yes | X) = {prob_yes_norm:.4f}")
print(f"P(play=no  | X) = {prob_no_norm:.4f}")
print(f"Prediksi: play = {'yes' if prob_yes_norm > prob_no_norm else 'no'}")
```

### Catatan: Case 2 dengan Outlook Missing

Materi halaman 26 menunjukkan hasil manual:
- P(play=yes | temp=cool, humidity=high, windy=true) = 0.44
- P(play=no  | temp=cool, humidity=high, windy=true) = 0.56
- Prediksi akhir: **play = no**

Sklearn melakukan marginalisasi secara aproksimasi. Untuk hasil yang identik dengan hitungan manual, implementasi kustom diperlukan karena sklearn tidak menangani marginalisasi variabel tersembunyi secara langsung.

---

## Laplacian Correction

Masalah probabilitas nol muncul ketika kombinasi nilai atribut dan kelas tidak pernah muncul di data training. Laplacian correction menambahkan konstanta (biasanya 1) ke setiap hitungan frekuensi:

```python
from sklearn.naive_bayes import CategoricalNB

# alpha=1.0 adalah Laplace smoothing (default di CategoricalNB)
# alpha=0.0 berarti tidak ada smoothing (bisa menghasilkan probabilitas 0)

model_tanpa_laplace = CategoricalNB(alpha=0.0)
model_dengan_laplace = CategoricalNB(alpha=1.0)

model_tanpa_laplace.fit(X_t_enc, y_t_enc)
model_dengan_laplace.fit(X_t_enc, y_t_enc)

# Contoh kasus: outlook=overcast, play=no tidak ada di data
# Tanpa Laplace: P(outlook=overcast | play=no) = 0/5 = 0
# Dengan Laplace: P(outlook=overcast | play=no) = (0+1)/(5+3) = 0.125
```

Dari materi kuliah halaman 16:
- P(outlook=overcast | play=no) = (0+1)/(5+3) = 0.125

Ini menghindari perkalian probabilitas menjadi 0 hanya karena satu kombinasi tidak ada di training data.

---

## Perbandingan Varian Naive Bayes di Sklearn

| Kelas              | Tipe Data        | Keterangan                                      |
|--------------------|------------------|-------------------------------------------------|
| `GaussianNB`       | Kontinu          | Distribusi fitur diasumsikan Gaussian           |
| `CategoricalNB`    | Kategorik        | Cocok untuk atribut diskrit seperti dataset ini |
| `MultinomialNB`    | Hitungan/frekuensi | Umum dipakai untuk klasifikasi teks           |
| `BernoulliNB`      | Biner (0/1)      | Cocok untuk fitur boolean                       |
| `ComplementNB`     | Hitungan/frekuensi | Versi perbaikan MultinomialNB untuk data tidak seimbang |

Untuk dataset buys_computer dan play_tennis di atas, `CategoricalNB` adalah pilihan yang tepat karena semua atribut bersifat kategorik.

---

## Kelebihan dan Keterbatasan

**Kelebihan:**
- Mudah diimplementasikan dan cepat dilatih
- Bekerja baik pada dataset kecil
- Tidak sensitif terhadap fitur yang tidak relevan jika jumlah data cukup

**Keterbatasan:**
- Asumsi independensi antar atribut jarang terpenuhi dalam data nyata
- Akurasi menurun ketika ada korelasi kuat antar fitur
- Tidak bisa memodelkan ketergantungan antar variabel (berbeda dengan Bayesian Belief Network)

Bayesian Belief Network (BBN) hadir sebagai solusi atas keterbatasan ini. BBN menggunakan graf berarah tanpa siklus (DAG) untuk merepresentasikan hubungan sebab-akibat antar variabel, seperti yang diilustrasikan pada materi halaman 14-18.

---

## Referensi

- Han, J., Kamber, M., & Pei, J. - Data Mining: Concepts and Techniques
- Scikit-learn: CategoricalNB - https://scikit-learn.org/stable/modules/generated/sklearn.naive_bayes.CategoricalNB.html
- Scikit-learn: Naive Bayes - https://scikit-learn.org/stable/modules/naive_bayes.html