# 📊 Laporan Data Understanding

> **Analisis Kausal Berbasis Deep Learning untuk Edge Computing**

**Dataset:** NASA C-MAPSS (Commercial Modular Aero-Propulsion System Simulation)

**Penulis:** Alissa Dyah Ayu Permatasari – 25/575323/PPA/07279

---

## 📑 Daftar Isi

- [Ringkasan](#-ringkasan)
- [Business Understanding](#-business-understanding)
- [Data Understanding](#-data-understanding)
- [Data Quality Assessment](#-data-quality-assessment)
- [Methodology & Data Preparation](#-methodology--data-preparation)
- [Key Insight & Findings](#-key-insight--findings)
- [Recommendations](#-recommendations)
- [Conclusion](#-conclusion)
- [Tabel Referensi](#-tabel-referensi)

---

## 📝 Ringkasan

Laporan ini menyajikan analisis data understanding dari dataset **NASA C-MAPSS** (Commercial Modular Aero-Propulsion System Simulation) yang digunakan untuk mengembangkan model **Predictive Maintenance (PdM)**. Analisis berfokus pada pemahaman kualitas data, identifikasi fitur sensor kritis, dan persiapan dataset untuk aplikasi pembelajaran mesin tingkat lanjut dalam sistem siber-fisik industri.

### ✨ Temuan Utama

- ✅ Mengidentifikasi **7 sensor konstan/mati** yang tidak memberikan nilai informasi untuk pelatihan model
- ✅ Berhasil menghapus fitur non-informatif, mengurangi ruang fitur sambil mempertahankan integritas analitik
- ✅ Melakukan analisis korelasi yang mengungkapkan **ketergantungan sensor yang kuat** penting untuk penalaran kausal
- ✅ Dataset divalidasi untuk pendekatan **distilasi pengetahuan** yang memungkinkan deployment edge dengan sumber daya komputasi terbatas

---

## 🏢 Business Understanding

### Latar Belakang Masalah

Sistem **Predictive Maintenance (PdM)** berdasarkan **Deep Learning (DL)** secara tradisional bergantung pada arsitektur yang kompleks dan mahal secara komputasi seperti model **Transformer-GNN**. Meskipun pendekatan ini memberikan akurasi tinggi dalam deteksi anomali, mereka menimbulkan tantangan signifikan untuk deployment industri di dunia nyata:

1. **Overhead komputasi tinggi** yang tidak kompatibel dengan perangkat IoT edge
2. **Pemrosesan cloud berkelanjutan** dan persyaratan bandwidth jaringan tinggi
3. **Interpretabilitas terbatas** dalam hubungan kausal antar sensor
4. **Ketidakmampuan beroperasi** secara otonom dalam lingkungan dengan bandwidth terbatas

### 💡 Pendekatan Solusi

Penelitian ini menerapkan kerangka **Causal Knowledge Distillation** yang memungkinkan deployment penalaran kausal yang sophisticated pada perangkat edge dengan sumber daya terbatas. Metodologi ini mempertahankan **interpretabilitas** sambil secara dramatis mengurangi persyaratan komputasi, memungkinkan **Predictive Maintenance (PdM) real-time** di edge tanpa bergantung pada konektivitas cloud berkelanjutan.

---

## 📊 Data Understanding

### Tinjauan Dataset

| Aspek | Detail |
|-------|--------|
| **Nama Dataset** | NASA C-MAPSS (Commercial Modular Aero-Propulsion System Simulation) |
| **Tujuan Awal** | Kompetisi PHM08 Prognostics and Health Management |
| **Aplikasi Saat Ini** | Data understanding dan rekayasa fitur untuk Predictive Maintenance (PdM) |
| **Domain Data** | Telemetri sensor pesawat dan parameter operasional |

### 🔬 Metodologi Pengumpulan Data

Dataset terdiri dari **pembacaan sensor multivariat** dari operasi mesin turbofan yang disimulasikan dalam berbagai kondisi operasional dan mode kegagalan. Setiap record menangkap poin data **time-series** yang mewakili kondisi kesehatan mesin yang berbeda, memungkinkan pengembangan model prediksi **Remaining Useful Life (RUL)**.

### 📋 Inventori Fitur

Dataset awalnya berisi **26 fitur** yang terdiri dari pengaturan operasional dan pengukuran sensor. Setelah dilakukan **Exploratory Data Analysis (EDA)**, set fitur disempurnakan untuk menghapus variabel non-informatif.

**Kategori Fitur:**

- **Pengaturan Operasional:** Throttle mesin, ketinggian, dan parameter kontrol lainnya
- **Pengukuran Sensor:** Pembacaan temperatur, tekanan, kecepatan, dan getaran dari berbagai komponen mesin

---

## 🔍 Data Quality Assessment

### Identifikasi Sensor Konstan/Mati

**Exploratory Data Analysis (EDA)** mengungkapkan **7 sensor** yang menunjukkan nilai konstan di seluruh dataset. Fitur-fitur ini memberikan:

- **Varians nol** (tidak ada variabilitas)
- **Tidak ada gradient informasi** (tidak mengandung informasi)
- **Tidak cocok** untuk pemodelan prediktif

Penghapusan 7 sensor ini mengurangi beban komputasi sambil mempertahankan semua informasi analitik. Kemudian dilakukan penambahan fitur yaitu **Remaining Useful Life (RUL)** serta **RUL_clipped**. 

**Hasil Akhir:** Jumlah fitur akhir = **21 fitur**

#### Daftar Sensor yang Dihapus

| Sensor | Klasifikasi |
|--------|-------------|
| `setting_3` | Pengaturan Operasional |
| `T2` | Sensor Temperatur |
| `P2` | Sensor Tekanan |
| `epr` | Rasio Tekanan Mesin |
| `farB` | Rasio Aliran Bahan Bakar |
| `Nf_dmd` | Permintaan Kecepatan Fan |
| `PCNfR_dmd` | Permintaan Rasio Kecepatan Fan |

---

## 🔧 Methodology & Data Preparation

### Proses Pembersihan Data

**1. Deteksi Fitur Konstan**
- Menerapkan analisis varians untuk mengidentifikasi fitur dengan `nunique() ≤ 1`

**2. Penghapusan**
- Menghapus sensor mati yang diidentifikasi dari dataset

**3. Validasi**
- Mengkonfirmasi bahwa 21 fitur yang tersisa berisi data yang bervariasi dan informatif

### 📈 Analisis Fitur

Analisis korelasi dilakukan pada **21 fitur** yang tersisa untuk memahami ketergantungan sensor. **Korelasi kunci** yang diidentifikasi meliputi:

- 🔗 **Korelasi positif kuat** antara sensor kecepatan (Nf, NRf): **0.826**
- 🔗 **Korelasi tinggi** antara sensor tekanan (P30, Ps30): **0.780**
- 🔗 **Hubungan terbalik** antara sensor temperatur dan tekanan tertentu → **kausalitas fisik**
- 🔗 **Korelasi lemah** dengan fitur tertentu → **informasi sensor independen**

---

## 💡 Key Insight & Findings

### 1️⃣ Manajemen Redundansi Sensor
7 sensor yang dihapus mewakili pengurangan dimensionalitas fitur **20.6%** tanpa kehilangan informasi kritis.

### 2️⃣ Ketergantungan Multivariat
Korelasi kuat mengungkapkan bahwa pembacaan sensor mesin sangat saling bergantung, mendukung kebutuhan pendekatan **penalaran kausal**.

### 3️⃣ Kualitas Data
Fitur aktif menunjukkan pola varians realistis yang konsisten dengan dinamika mesin.

### 4️⃣ Kesiapan Edge
Dataset **21 fitur** yang dioptimalkan cocok untuk **distilasi pengetahuan** ke model edge yang dapat di-deploy.

---

## 🎯 Recommendations

### 1. Implementasi Causal Knowledge Distillation

Mengembangkan **arsitektur model guru-murid** menggunakan dataset 21 fitur di mana hubungan kausal kompleks yang dipelajari model guru didistilasi ke dalam model murid yang ringkas.

### 2. Deployment pada Perangkat Edge

Set fitur yang dioptimalkan memungkinkan **inferensi real-time** pada perangkat IoT dengan sumber daya komputasi terbatas.

### 3. Menetapkan Pemantauan Korelasi

Memantau **penyimpangan dari korelasi sensor** yang diharapkan sebagai mekanisme **deteksi anomali**.

### 4. Rekayasa Fitur Temporal

Mempertimbangkan **statistik jendela bergulir** dan **dekomposisi time series** untuk 21 fitur untuk menangkap pola degradasi.

---

## ✅ Conclusion

Dataset **NASA C-MAPSS** telah berhasil dianalisis dan disiapkan untuk pemodelan **Predictive Maintenance (PdM)**. 

**Pencapaian Utama:**
- ✓ Penghapusan 7 sensor non-informatif telah mengoptimalkan ruang fitur tanpa mengorbankan integritas data
- ✓ Korelasi dan ketergantungan yang teridentifikasi memberikan dasar untuk menerapkan model penalaran kausal sophisticated
- ✓ Dataset cocok untuk deployment edge

**Dataset 21 fitur aktif** yang dibersihkan sekarang siap untuk aplikasi pembelajaran mesin tingkat lanjut:

- 🚀 **Distilasi Pengetahuan**
- 🚀 **Penemuan Kausal**
- 🚀 **Prediksi Remaining Useful Life**

Analisis ini menunjukkan **kelayakan deployment** sistem **Predictive Maintenance (PdM) berbasis AI** pada perangkat IoT industri yang **resource-constrained** sambil mempertahankan **interpretabilitas** dan **kemampuan penalaran kausal**.

---


**📌 Catatan:** File ini dapat langsung di-copy ke GitHub Repository sebagai `README.md`
