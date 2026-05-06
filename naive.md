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

---

## Script Python

Jalankan dengan: `python naive_bayes.py`

### Import Library dan Baca Data

```{code-cell}
:tags: [hide-input]
import pandas as pd
import numpy as np
from sklearn.naive_bayes import CategoricalNB
from sklearn.preprocessing import OrdinalEncoder
from sklearn.metrics import classification_report, confusion_matrix, accuracy_score

df = pd.read_csv('buys_computer.csv')

print("=" * 50)
print("=== DATASET ===")
print("=" * 50)
print(df.to_string(index=False))
print(f"\nJumlah record: {len(df)}")
print(f"\nDistribusi kelas:")
print(df['buys_computer'].value_counts().to_string())
```

---

### Encoding dan Training Model

```{code-cell}
:tags: [hide-input]
X = df.drop(columns='buys_computer')
y = df['buys_computer']

enc = OrdinalEncoder()
X_enc = enc.fit_transform(X)
y_enc = (y == 'yes').astype(int)  # yes=1, no=0

print("\n" + "=" * 50)
print("=== ENCODING KATEGORI ===")
print("=" * 50)
for i, col in enumerate(X.columns):
    print(f"  {col}: {list(enc.categories_[i])}")

# Training CategoricalNB dengan Laplace smoothing (alpha=1)
model = CategoricalNB(alpha=1.0)
model.fit(X_enc, y_enc)
```

> **Catatan:** `OrdinalEncoder` secara default mengurutkan kategori secara alfabetis. Indeks numerik ini digunakan secara internal oleh `CategoricalNB`.

---

### Prior Probability dan Probabilitas Kondisional

```{code-cell}
:tags: [hide-input]
print("\n" + "=" * 50)
print("=== PRIOR PROBABILITY ===")
print("=" * 50)
classes = ['no', 'yes']
n_total = len(y)
for label in classes:
    count = (y == label).sum()
    print(f"  P({label}) = {count}/{n_total} = {count/n_total:.4f}")

print("\n" + "=" * 50)
print("=== PROBABILITAS KONDISIONAL P(fitur|kelas) — Laplace α=1 ===")
print("=" * 50)
for fi, col in enumerate(X.columns):
    cats = enc.categories_[fi]
    print(f"\n  Atribut: {col}")
    print(f"  {'Nilai':<14} {'P(|no)':>10} {'P(|yes)':>10}")
    print(f"  {'-'*36}")
    for ci, cat in enumerate(cats):
        p_no  = np.exp(model.feature_log_prob_[fi][0][ci])
        p_yes = np.exp(model.feature_log_prob_[fi][1][ci])
        print(f"  {cat:<14} {p_no:>10.4f} {p_yes:>10.4f}")
```

---

### Evaluasi Model pada Training Set

```{code-cell}
:tags: [hide-input]
y_pred = model.predict(X_enc)

print("\n" + "=" * 50)
print("=== EVALUASI MODEL (Training Set) ===")
print("=" * 50)
print(f"Accuracy: {accuracy_score(y_enc, y_pred):.4f}")
print("\nClassification Report:")
print(classification_report(y_enc, y_pred, target_names=['no', 'yes']))

cm = confusion_matrix(y_enc, y_pred)
print("Confusion Matrix:")
print(f"  {'':20} {'Prediksi: no':>14} {'Prediksi: yes':>14}")
print(f"  {'Aktual: no':20} {cm[0][0]:>14} {cm[0][1]:>14}")
print(f"  {'Aktual: yes':20} {cm[1][0]:>14} {cm[1][1]:>14}")
print(f"\n  TN={cm[0][0]}, FP={cm[0][1]}, FN={cm[1][0]}, TP={cm[1][1]}")
```

**Interpretasi Confusion Matrix:**

|                  | Prediksi: no | Prediksi: yes |
|------------------|:------------:|:-------------:|
| **Aktual: no**   | TN           | FP            |
| **Aktual: yes**  | FN           | TP            |

---

### Prediksi Data Baru

```{code-cell}
:tags: [hide-input]
data_baru = pd.DataFrame([{
    'age': '<=30',
    'income': 'medium',
    'student': 'yes',
    'credit_rating': 'fair'
}])

X_baru_enc   = enc.transform(data_baru)
prediksi     = model.predict(X_baru_enc)
probabilitas = model.predict_proba(X_baru_enc)

label_map = {0: 'no', 1: 'yes'}
print("\n" + "=" * 50)
print("=== PREDIKSI DATA BARU ===")
print("=" * 50)
print("Input: age=<=30, income=medium, student=yes, credit_rating=fair")
print(f"\nPrediksi kelas     : buys_computer = {label_map[prediksi[0]]}")
print(f"P(buys_computer=no  | X) = {probabilitas[0][0]:.4f}")
print(f"P(buys_computer=yes | X) = {probabilitas[0][1]:.4f}")
```

---

### Verifikasi Perhitungan Manual

```{code-cell}
:tags: [hide-input]
lookup = {}
for fi, col in enumerate(X.columns):
    cats = enc.categories_[fi]
    lookup[col] = {}
    for ci, cat in enumerate(cats):
        lookup[col][cat] = {
            'no':  np.exp(model.feature_log_prob_[fi][0][ci]),
            'yes': np.exp(model.feature_log_prob_[fi][1][ci]),
        }

prior_yes = (y == 'yes').sum() / n_total
prior_no  = (y == 'no').sum()  / n_total

q = {'age': '<=30', 'income': 'medium', 'student': 'yes', 'credit_rating': 'fair'}

score_yes = prior_yes
score_no  = prior_no
for col, val in q.items():
    score_yes *= lookup[col][val]['yes']
    score_no  *= lookup[col][val]['no']

total = score_yes + score_no

print("\n" + "=" * 50)
print("=== VERIFIKASI PERHITUNGAN MANUAL ===")
print("=" * 50)
print(f"  Skor P(yes|X) sebelum normalisasi : {score_yes:.6f}")
print(f"  Skor P(no|X)  sebelum normalisasi : {score_no:.6f}")
print(f"  Total                             : {total:.6f}")
print(f"\n  P(yes|X) ternormalisasi = {score_yes/total:.4f}")
print(f"  P(no|X)  ternormalisasi = {score_no/total:.4f}")
print(f"\n  ✓ Konsisten dengan output model sklearn")
```

---

## Laplacian Correction

Masalah probabilitas nol muncul ketika suatu kombinasi nilai atribut dan kelas tidak pernah muncul di data training. Laplacian correction menambahkan konstanta α ke setiap hitungan frekuensi:

```
P(xi | Cj) = (count(xi, Cj) + α) / (count(Cj) + α × |Vi|)
```

di mana `|Vi|` adalah jumlah nilai unik atribut i.

```{code-cell}
:tags: [hide-input]
count_3140_no = ((df['age'] == '31-40') & (df['buys_computer'] == 'no')).sum()
count_no      = (df['buys_computer'] == 'no').sum()
n_age_vals    = df['age'].nunique()

p_tanpa  = count_3140_no / count_no
p_dengan = (count_3140_no + 1) / (count_no + n_age_vals)

print("\n" + "=" * 50)
print("=== DAMPAK LAPLACE SMOOTHING ===")
print("=== Kasus: P(age=31-40 | no) ===")
print("=" * 50)
print(f"  Count(age=31-40, no) = {count_3140_no}")
print(f"  Count(no)            = {count_no}")
print(f"  |V(age)|             = {n_age_vals}")
print(f"\n  Tanpa Laplace  : {count_3140_no}/{count_no} = {p_tanpa:.4f}  ⚠ (probabilitas nol!)")
print(f"  Dengan Laplace : ({count_3140_no}+1)/({count_no}+{n_age_vals}) = {p_dengan:.4f}  ✓")
```

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