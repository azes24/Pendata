# Decision Tree dalam Penambangan Data (Data Mining)

---

## 1. Pengertian Decision Tree

**Decision Tree** (Pohon Keputusan) adalah algoritma pembelajaran mesin yang digunakan untuk klasifikasi dan regresi. Dalam konteks **data mining**, decision tree membentuk model prediktif berbentuk pohon di mana:

- **Node internal** → merepresentasikan atribut/fitur yang diuji
- **Cabang (Branch)** → merepresentasikan hasil dari pengujian
- **Daun (Leaf Node)** → merepresentasikan label kelas atau nilai output

Decision tree bekerja dengan membagi dataset secara rekursif berdasarkan atribut yang paling informatif hingga mencapai kondisi berhenti (stopping criteria).

---

## 2. Konsep Dasar dan Perhitungan

### 2.1 Entropy (Ukuran Ketidakpastian)

Entropy mengukur ketidakmurnian atau ketidakpastian dalam dataset.

$$H(S) = - \sum_{i=1}^{c} p_i \log_2(p_i)$$

Dimana:
- `S` = dataset
- `c` = jumlah kelas
- `p_i` = proporsi sampel kelas ke-i

**Interpretasi:**
- Entropy = 0 → data murni (hanya satu kelas)
- Entropy = 1 → data seimbang sempurna (dua kelas sama banyak)

### 2.2 Information Gain (IG)

Information Gain mengukur seberapa besar pengurangan entropy setelah data dibagi berdasarkan atribut tertentu.

$$IG(S, A) = H(S) - \sum_{v \in Values(A)} \frac{|S_v|}{|S|} \cdot H(S_v)$$

Dimana:
- `A` = atribut yang dievaluasi
- `S_v` = subset data dengan nilai atribut A = v

**Atribut dengan IG tertinggi dipilih sebagai node pemisah.**

---

## 3. Dataset: Play Tennis

Data berikut diambil langsung dari file `play_tennis.xlsx`:

| Day | Outlook | Temp | Humidity | Wind | Play Tennis |
|-----|---------|------|----------|------|-------------|
| D1 | Sunny | Hot | High | False | **No** |
| D2 | Sunny | Hot | High | True | **No** |
| D3 | Overcast | Hot | High | False | **Yes** |
| D4 | Rain | Mild | High | False | **Yes** |
| D5 | Rain | Cool | Normal | False | **Yes** |
| D6 | Rain | Cool | Normal | True | **No** |
| D7 | Overcast | Cool | Normal | True | **Yes** |
| D8 | Sunny | Mild | High | False | **No** |
| D9 | Sunny | Cool | Normal | False | **Yes** |
| D10 | Rain | Mild | Normal | False | **Yes** |
| D11 | Sunny | Mild | Normal | True | **Yes** |
| D12 | Overcast | Mild | High | True | **Yes** |
| D13 | Overcast | Hot | Normal | False | **Yes** |
| D14 | Rain | Mild | High | True | **No** |

> **Keterangan kolom Wind:** `False` = Angin Lemah (Weak), `True` = Angin Kencang (Strong)

**Ringkasan:** Total 14 instance → **9 Yes**, **5 No**

---

## 4. Perhitungan Lengkap

### Langkah 1: Hitung Entropy Total H(S)

$$H(S) = -\frac{9}{14} \log_2\frac{9}{14} - \frac{5}{14} \log_2\frac{5}{14}$$

$$H(S) = -(0.6429 \times (-0.6374)) - (0.3571 \times (-1.4854))$$

$$\boxed{H(S) = 0.4097 + 0.5305 = 0.9403}$$

---

### Langkah 2: Hitung Information Gain Setiap Atribut

#### A. IG untuk Atribut **Outlook**

| Outlook | Yes | No | Total |
|---------|-----|----|-------|
| Sunny | 2 | 3 | 5 |
| Overcast | 4 | 0 | 4 |
| Rain | 3 | 2 | 5 |

$$H(Sunny) = -\frac{2}{5}\log_2\frac{2}{5} - \frac{3}{5}\log_2\frac{3}{5} = 0.9710$$

$$H(Overcast) = -\frac{4}{4}\log_2\frac{4}{4} = \mathbf{0} \quad \text{(murni!)}$$

$$H(Rain) = -\frac{3}{5}\log_2\frac{3}{5} - \frac{2}{5}\log_2\frac{2}{5} = 0.9710$$

$$IG(Outlook) = 0.9403 - \left(\frac{5}{14}(0.9710) + \frac{4}{14}(0) + \frac{5}{14}(0.9710)\right)$$

$$IG(Outlook) = 0.9403 - (0.3468 + 0 + 0.3468) = \boxed{0.2467}$$

---

#### B. IG untuk Atribut **Temp**

| Temp | Yes | No | Total |
|------|-----|----|-------|
| Hot | 2 | 2 | 4 |
| Mild | 4 | 2 | 6 |
| Cool | 3 | 1 | 4 |

$$H(Hot) = -\frac{2}{4}\log_2\frac{2}{4} - \frac{2}{4}\log_2\frac{2}{4} = 1.0000$$

$$H(Mild) = -\frac{4}{6}\log_2\frac{4}{6} - \frac{2}{6}\log_2\frac{2}{6} = 0.9183$$

$$H(Cool) = -\frac{3}{4}\log_2\frac{3}{4} - \frac{1}{4}\log_2\frac{1}{4} = 0.8113$$

$$IG(Temp) = 0.9403 - \left(\frac{4}{14}(1.0000) + \frac{6}{14}(0.9183) + \frac{4}{14}(0.8113)\right)$$

$$IG(Temp) = 0.9403 - (0.2857 + 0.3936 + 0.2318) = \boxed{0.0292}$$

---

#### C. IG untuk Atribut **Humidity**

| Humidity | Yes | No | Total |
|----------|-----|----|-------|
| High | 3 | 4 | 7 |
| Normal | 6 | 1 | 7 |

$$H(High) = -\frac{3}{7}\log_2\frac{3}{7} - \frac{4}{7}\log_2\frac{4}{7} = 0.9852$$

$$H(Normal) = -\frac{6}{7}\log_2\frac{6}{7} - \frac{1}{7}\log_2\frac{1}{7} = 0.5917$$

$$IG(Humidity) = 0.9403 - \left(\frac{7}{14}(0.9852) + \frac{7}{14}(0.5917)\right)$$

$$IG(Humidity) = 0.9403 - (0.4926 + 0.2959) = \boxed{0.1518}$$

---

#### D. IG untuk Atribut **Wind**

| Wind | Yes | No | Total |
|------|-----|----|-------|
| False (Weak) | 6 | 2 | 8 |
| True (Strong) | 3 | 3 | 6 |

$$H(Weak) = -\frac{6}{8}\log_2\frac{6}{8} - \frac{2}{8}\log_2\frac{2}{8} = 0.8113$$

$$H(Strong) = -\frac{3}{6}\log_2\frac{3}{6} - \frac{3}{6}\log_2\frac{3}{6} = 1.0000$$

$$IG(Wind) = 0.9403 - \left(\frac{8}{14}(0.8113) + \frac{6}{14}(1.0000)\right)$$

$$IG(Wind) = 0.9403 - (0.4636 + 0.4286) = \boxed{0.0481}$$

---

### Langkah 3: Pilih Atribut dengan IG Tertinggi

| Atribut | Information Gain | Ranking |
|---------|----------------|---------|
| **Outlook** | **0.2467** | **1 — dipilih sebagai Root Node** |
| Humidity | 0.1518 | 2 |
| Wind | 0.0481 | 3 |
| Temp | 0.0292 | 4 |

**→ Outlook menjadi Root Node karena memiliki IG tertinggi (0.2467)**

---

### Langkah 4: Rekursi pada Setiap Cabang Outlook

#### Cabang Overcast → D3, D7, D12, D13 → Semua Yes
Entropy = 0 → **Leaf Node: YES** 

#### Cabang Sunny → D1, D2, D8, D9, D11 → 2 Yes, 3 No
Hitung IG atribut tersisa, **Humidity** dipilih (IG tertinggi pada subset ini):

| Humidity (Sunny) | Yes | No | Hasil |
|------------------|-----|----|-------|
| High (D1, D2, D8) | 0 | 3 | → **NO** |
| Normal (D9, D11) | 2 | 0 | → **YES** |

#### Cabang Rain → D4, D5, D6, D10, D14 → 3 Yes, 2 No
Hitung IG atribut tersisa, **Wind** dipilih (IG tertinggi pada subset ini):

| Wind (Rain) | Yes | No | Hasil |
|-------------|-----|----|-------|
| False/Weak (D4, D5, D10) | 3 | 0 | → **YES** |
| True/Strong (D6, D14) | 0 | 2 | → **NO** |

---

### Hasil Akhir: Struktur Decision Tree

```
                         Outlook
                      /     |      \
               Sunny    Overcast    Rain
                 |          |          |
             Humidity      YES       Wind
             /     \               /      \
          High   Normal     Strong(T)  Weak(F)
           |        |             |         |
          NO       YES            NO       YES
```

---

## 5. Tahapan Workflow KNIME pada Gambar

Gambar menunjukkan workflow KNIME untuk membangun dan mengevaluasi **model Decision Tree** menggunakan dataset Play Tennis. Berikut penjelasan setiap node secara berurutan:

---

### Node 1: Excel Reader
![Grafik Data](/gambar/decission02.png)
**Fungsi:** Membaca dataset dari file Excel (`play_tennis.xlsx`).

- Mengimpor data dengan 6 kolom: `Day`, `Outlook`, `Temp`, `Humidity`, `Wind`, `Play Tennis`
- Seluruh 14 baris data termuat sebagai tabel input pipeline

---

### Node 2: Table Partitioner 
![Grafik Data](/gambar/decission03.png)
**Fungsi:** Membagi dataset menjadi *training set* dan *test set*.

- Label: *"split training set from test data"*
- Umumnya menggunakan rasio **70:30** atau **80:20**
- Output dua port:
  - Port atas → **Training Set** (melatih model)
  - Port bawah → **Test Set** (menguji model)

---

### Node 3: Color Manager

![Grafik Data](/gambar/decission04.png)
**Fungsi:** Memberi warna pada data berdasarkan kelas target (`Play Tennis`).

- Label: *"color by class"*
- Kelas `Yes` dan `No` mendapat warna berbeda untuk visualisasi
- Tidak mengubah struktur data, hanya menambahkan metadata warna

---

### Node 4: Decision Tree Learner

![Grafik Data](/gambar/decission05.png)
**Fungsi:** Membangun model Decision Tree dari training set.

- Label: *"build decision tree"*
- Menerapkan algoritma **ID3** dengan perhitungan Information Gain seperti di atas
- Menghasilkan struktur pohon dengan **Outlook** sebagai root node
- Parameter konfigurasi: metode split (Gini/IG), kedalaman pohon, min. sampel per node

---

### Node 5: Model Writer

**Fungsi:** Menyimpan model Decision Tree yang sudah dilatih ke dalam file.

- Berguna untuk deployment model tanpa melatih ulang
- Format: file model KNIME internal

---

### Node 6: Decision Tree View

![Grafik Data](/gambar/decission07.png)
**Fungsi:** Menampilkan visualisasi grafis struktur pohon keputusan.

- Memperlihatkan node split, cabang kondisi, dan leaf node kelas
- Untuk dataset ini menampilkan tree: Outlook → Humidity/YES/Wind → NO/YES/NO/YES
- Berguna untuk interpretasi dan presentasi model

---

### Node 7: Color Appender (deprecated)

**Fungsi:** Menambahkan metadata warna dari Color Manager ke data test set.

- Label: *"append color to test data"*
- Bertanda *deprecated* (tidak direkomendasikan di versi KNIME terbaru)
- Memastikan konsistensi warna antara training set dan test set untuk visualisasi

---

### Node 8: Decision Tree Predictor

**Fungsi:** Menggunakan model terlatih untuk memprediksi kolom `Play Tennis` pada test set.

- Label: *"use decision tree to predict classes"*
- Menerima dua input: model dari Learner + data test berwarna
- Output: tabel test set dengan kolom tambahan hasil prediksi (`Yes` / `No`)

---

### Node 9: Scorer (deprecated)

![Grafik Data](/gambar/decission10.png)
**Fungsi:** Mengevaluasi performa prediksi model.

- Label: *"score results"*
- Membandingkan prediksi dengan label asli `Play Tennis`
- Menghasilkan **Confusion Matrix** dan metrik evaluasi:
  - **Accuracy** = (TP + TN) / Total
  - **Precision** = TP / (TP + FP)
  - **Recall** = TP / (TP + FN)
  - **Kappa Coefficient**
- Bertanda *deprecated* → versi terbaru menggunakan node **Scorer** yang diperbarui

---

## 6. Alur Data Lengkap dalam Workflow

![Grafik Data](/gambar/decission01.png)
---
