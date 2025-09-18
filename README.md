# PredictiveAnalytics

# 🏥 Prediksi Biaya Asuransi dengan Machine Learning

## 📌 Domain Proyek
Biaya asuransi medis sangat dipengaruhi oleh faktor-faktor seperti usia, indeks massa tubuh (BMI), jumlah anak, jenis kelamin, status perokok, dan wilayah tempat tinggal. Penentuan biaya asuransi yang akurat penting bagi perusahaan asuransi untuk mengurangi risiko kerugian sekaligus memberikan premi yang adil bagi pelanggan.  

Dataset yang digunakan dalam proyek ini adalah **Medical Insurance Cost Dataset** dari Kaggle:  
👉 [Medical Insurance Cost Dataset](https://www.kaggle.com/datasets/mirichoi0218/insurance)

Dataset ini berisi **1338 sampel** data pelanggan dengan variabel-variabel seperti `age`, `sex`, `bmi`, `children`, `smoker`, `region`, dan `charges` (biaya asuransi).

---

## 🎯 Business Understanding

### Problem Statements
1. Bagaimana memprediksi biaya asuransi pelanggan secara akurat?  
2. Fitur apa saja yang paling memengaruhi besarnya biaya asuransi?

### Goals
1. Menghasilkan model machine learning yang dapat memprediksi biaya asuransi dengan akurat.  
2. Memberikan insight mengenai faktor-faktor utama yang memengaruhi biaya asuransi.  

### Solution Statements
- Mengimplementasikan tiga algoritma machine learning:
  - **K-Nearest Neighbor (KNN)**
  - **Random Forest Regressor**
  - **Gradient Boosting Regressor**
- Mengevaluasi performa model dengan metrik **MAE, MSE, RMSE, dan R²**.
- Menentukan model terbaik berdasarkan hasil evaluasi.
- Melakukan analisis **feature importance** untuk mengetahui variabel paling berpengaruh.

---

## 📊 Data Understanding

### Variabel pada Dataset
- **age**: Usia pelanggan (numerik)  
- **sex**: Jenis kelamin (kategori: male/female)  
- **bmi**: Body Mass Index (numerik)  
- **children**: Jumlah anak/tanggungan (numerik)  
- **smoker**: Status perokok (kategori: yes/no)  
- **region**: Wilayah tempat tinggal (kategori: northeast, northwest, southeast, southwest)  
- **charges**: Biaya asuransi medis (numerik, target/label)  

### Exploratory Data Analysis
- Tidak ada missing values.  
- Outlier ditemukan pada `bmi` (9 sampel) dan `charges` (139 sampel).  
- Distribusi menunjukkan bahwa **smoker** dan **BMI tinggi** cenderung memiliki biaya asuransi yang lebih besar.  

---

## 🛠️ Data Preparation

Langkah-langkah persiapan data:
1. **Penanganan Outlier**  
   Menggunakan metode **Interquartile Range (IQR)**. Data di luar [Q1 - 1.5 * IQR, Q3 + 1.5 * IQR] dianggap outlier dan dihapus, menghasilkan dataset baru `data_clean`.

2. **Encoding Fitur Kategori**  
   Menggunakan **One-Hot Encoding** pada fitur `sex`, `smoker`, dan `region`.  

3. **Standarisasi**  
   Fitur numerik (`age`, `bmi`, `children`) dinormalisasi menggunakan **StandardScaler** agar skala antar variabel sebanding.  

4. **Pembagian Data**  
   Dataset dibagi menjadi data latih dan data uji dengan perbandingan **80:20** menggunakan `train_test_split`.

---

## 🤖 Modeling

### Algoritma yang Digunakan
1. **K-Nearest Neighbors (KNN)**  
   - Prinsip: mencari K tetangga terdekat lalu merata-ratakan nilai target.  
   - Parameter: `n_neighbors=5` (default).  
   - Kelebihan: sederhana dan mudah dipahami.  
   - Kekurangan: kurang efektif pada data dengan dimensi tinggi.

2. **Random Forest Regressor**  
   - Prinsip: membangun banyak decision tree secara acak dan mengambil rata-rata prediksi.  
   - Parameter: `n_estimators=100`, `random_state=42`.  
   - Kelebihan: tahan terhadap overfitting, mampu menangani non-linearitas.  
   - Kekurangan: hasil lebih sulit diinterpretasikan dibanding single tree.

3. **Gradient Boosting Regressor**  
   - Prinsip: membangun model bertahap, setiap tree baru memperbaiki kesalahan tree sebelumnya.  
   - Parameter: `n_estimators=100`, `learning_rate=0.1`, `random_state=42`.  
   - Kelebihan: akurasi tinggi.  
   - Kekurangan: waktu pelatihan lebih lama dibanding Random Forest.

---

## 📈 Evaluation

### Metrik Evaluasi
- **MAE (Mean Absolute Error)**: rata-rata selisih absolut antara prediksi dan aktual.  
- **MSE (Mean Squared Error)**: rata-rata kuadrat error, penalti besar untuk kesalahan besar.  
- **RMSE (Root Mean Squared Error)**: akar dari MSE, lebih mudah dipahami karena dalam satuan yang sama dengan target.  
- **R² (R-Squared)**: proporsi variasi data target yang dapat dijelaskan oleh model.

### Hasil Evaluasi

| Model            | MAE    | MSE        | RMSE   | R²    |
|------------------|--------|------------|--------|-------|
| **KNN**          | 2969.30 | 23,051,721 | 4801.22 | 0.518 |
| **Random Forest**| 2573.10 | 20,426,721 | 4519.59 | 0.573 |
| **Gradient Boosting** | 2370.90 | 17,879,299 | 4228.39 | 0.626 |

➡️ **Model terbaik adalah Gradient Boosting** karena memiliki **RMSE terendah (4228.39)** dan **R² tertinggi (0.626)**.

---

## 🔍 Feature Importance

Dari Random Forest & Gradient Boosting, fitur yang paling berpengaruh adalah:
1. **age**  
2. **bmi**  
3. **smoker_yes**

Hal ini sesuai dengan intuisi: usia, indeks massa tubuh, dan kebiasaan merokok sangat memengaruhi premi asuransi.

---

## 📊 Contoh Hasil Prediksi

| Index | y_true (Biaya Aktual) | Prediksi KNN | Prediksi Random Forest | Prediksi Gradient Boosting |
|-------|------------------------|--------------|-------------------------|-----------------------------|
| 660   | 6435.62                | 5875.43      | 8471.37                 | 6173.53                     |
| 754   | 17128.43               | 8873.41      | 5743.82                 | 5920.88                     |
| 487   | 1253.94                | 1597.16      | 1441.00                 | 2042.17                     |
| 429   | 18804.75               | 8880.85      | 7605.73                 | 7965.80                     |
| 559   | 1646.43                | 1832.26      | 2034.42                 | 3287.62                     |

---

## ✅ Kesimpulan

1. Proyek ini berhasil menjawab kedua problem statement:
   - Prediksi biaya asuransi dapat dilakukan dengan model Gradient Boosting sebagai model terbaik.  
   - Fitur yang paling memengaruhi biaya asuransi adalah **age**, **bmi**, dan **smoker_yes**.  

2. Model Gradient Boosting memiliki performa terbaik (RMSE: 4228.39, R²: 0.626).  

3. Hasil ini dapat membantu perusahaan asuransi dalam:  
   - Menetapkan premi yang lebih adil dan akurat.  
   - Mengidentifikasi faktor risiko utama pelanggan.  

---

## 📚 Referensi
- Dataset: [Medical Insurance Cost Dataset - Kaggle](https://www.kaggle.com/datasets/mirichoi0218/insurance)  
- Hastie, T., Tibshirani, R., & Friedman, J. (2009). *The Elements of Statistical Learning*.  
- Géron, A. (2019). *Hands-On Machine Learning with Scikit-Learn, Keras, and TensorFlow*.  
