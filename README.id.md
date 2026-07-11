# Proyek Analisis Data Penjualan Cafe

[🇬🇧 Read English version](README.md)

## Ringkasan
Proyek ini menunjukkan alur kerja analisis data yang lengkap dari ujung ke ujung — mulai dari data transaksi mentah yang berantakan sampai rekomendasi bisnis yang bisa langsung ditindaklanjuti — menggunakan Python dan Pandas. Mencakup data cleaning, exploratory data analysis (EDA), feature engineering, dan laporan eksekutif dari dataset penjualan cafe.

## Dataset
- **Sumber**: [Cafe Sales — Dirty Data for Cleaning Training](https://www.kaggle.com/datasets/ahmedmohamed2003/cafe-sales-dirty-data-for-cleaning-training) (Kaggle)
- **Ukuran data final**: 9.035 transaksi × 15 kolom (setelah proses cleaning dan feature engineering)
- **Deskripsi**: Data transaksi cafe sintetis yang awalnya berisi nilai kosong, nilai "menyamar" (`"UNKNOWN"`, `"ERROR"`), format tidak konsisten, dan ketidaksesuaian logika antar kolom.

## Alur Kerja Proyek
1. **Data Cleaning** (`01_data_cleaning.ipynb`) — mendeteksi nilai kosong yang "menyamar", memulihkan nilai `Item` yang hilang lewat pemetaan harga unik per item, menyelesaikan nilai numerik yang hilang lewat perhitungan silang matematis (`Quantity`, `Price Per Unit`, `Total Spent`), menstandarisasi format teks, dan memperbaiki tipe data
2. **Exploratory Data Analysis** (`02_eda_visualization.ipynb`) — memvisualisasikan pola penjualan menggunakan tiga metrik performa item yang saling melengkapi (frekuensi, quantity, revenue), distribusi metode pembayaran, dan tren berbasis waktu
3. **Feature Engineering** (`03_feature_engineering.ipynb`) — membuat kolom turunan baru (`Day of Week`, `Is Weekend`, `Spending Category`, `Effective Unit Price`) untuk mendukung analisis lebih dalam
4. **Insight & Reporting** (`04_insight_reporting.ipynb`) — merangkum temuan menjadi laporan eksekutif dengan rekomendasi berbasis data

## Tools & Library
- Python 3, Pandas, NumPy
- Matplotlib, Seaborn
- Google Colab / Jupyter Notebook

## Metrik Utama
| Metrik | Nilai |
|---|---|
| Total Transaksi | 9.035 |
| Total Revenue | 79.816,00 |
| Rata-rata Nilai Transaksi | 8,83 |
| Item Teratas (frekuensi) | Coffee (1.239 transaksi) |
| Item Teratas (quantity terjual) | Coffee (3.759 unit) |
| Item Teratas (revenue) | Salad (18.190,00) |
| Metode Pembayaran Dominan | Digital Wallet (54,7%) |
| Tipe Order Dominan | Takeaway (69,9%) |

## Temuan Utama

### Temuan 1: Coffee mendominasi popularitas, tapi Salad mendominasi profitabilitas
Coffee unggul di frekuensi transaksi (1.239 transaksi) maupun volume penjualan (3.759 unit), tapi **Salad justru menyumbang total revenue tertinggi** (18.190,00) meski frekuensinya lebih rendah (1.212 transaksi) — didorong oleh harga per unit yang lebih tinggi. Revenue per transaksi Salad (15,01) kira-kira **2,5x lebih tinggi** dari Coffee (6,07). Cookie menunjukkan revenue per transaksi terendah (2,99) di antara item teratas. Coffee dan Cookie berperan sebagai **traffic driver** berfrekuensi tinggi, sementara Salad adalah **penggerak profitabilitas** utama.

### Temuan 2: Transaksi bernilai Medium menyumbang lebih dari separuh revenue, meski minoritas dari segi jumlah
| Kategori | % Transaksi | % Revenue | Rasio Revenue:Transaksi |
|---|---|---|---|
| Low (< 10) | 62,36% | 34,43% | 0,55x |
| Medium (10–25) | 34,79% | **57,48%** | **1,65x** |
| High (≥ 25) | 2,86% | 8,08% | 2,82x |

Transaksi Low mendominasi dari segi jumlah tapi under-contribute terhadap revenue relatif dari porsinya. Transaksi Medium adalah segmen paling efisien secara agregat — 34,79% dari transaksi menghasilkan 57,48% revenue.

**Catatan metodologi**: Batas "High" (≥25) bertepatan dengan nilai transaksi maksimum yang mungkin terjadi di dataset ini (5 unit × item termahal, Salad seharga 5,0 per unit). Karena itu, ini merepresentasikan transaksi di titik puncak struktur harga×kuantitas, bukan rentang pengeluaran besar yang luas.

### Temuan 3: Preferensi Takeaway konsisten di semua metode pembayaran — tidak ada keterkaitan
Takeaway mewakili 69,25%–70,13% transaksi baik di Cash, Credit Card, maupun Digital Wallet — selisih kurang dari 1 poin persentase. Preferensi Takeaway adalah perilaku pelanggan yang umum, independen dari pilihan metode pembayaran.

### Temuan 4: Tidak ada pola mingguan yang kuat — permintaan relatif stabil sepanjang minggu
Revenue harian berkisar dari 10.790,50 (Rabu, terendah) sampai 11.701,00 (Kamis, tertinggi) — selisih sekitar 8,4%. Rata-rata nilai transaksi antara hari kerja (8,833) dan akhir pekan (8,837) hampir identik, selisihnya kurang dari 0,05%.

## Sintesis
Keempat temuan ini mengarah pada satu benang merah: **peluang pertumbuhan revenue terletak pada peningkatan nilai per transaksi (upsell), bukan pada peningkatan frekuensi kunjungan atau menyasar waktu/metode pembayaran tertentu.** Frekuensi kunjungan sudah tinggi dan stabil, dan pola pembayaran/tipe order tidak menunjukkan keterkaitan yang bisa dimanfaatkan. Peluang paling realistis ada pada menggeser komposisi transaksi — membundel item berfrekuensi tinggi bermarjin rendah (Coffee, Cookie) dengan item bernilai lebih tinggi (Salad), dan mendorong transaksi tier Low naik ke tier Medium yang lebih efisien.

## Rekomendasi
1. **Bundel Salad (atau item bermarjin tinggi lain) dengan item berfrekuensi tinggi namun revenue rendah seperti Coffee dan Cookie** untuk meningkatkan rata-rata nilai transaksi tanpa mengorbankan frekuensi kunjungan yang sudah ada.
2. **Rancang insentif upsell kecil yang menyasar transaksi tier Low** (62,36% dari seluruh transaksi, misalnya "tambah item dengan diskon kecil") untuk menggeser mereka ke tier Medium, yang secara proporsional merupakan segmen paling efisien menghasilkan revenue.
3. **Pertahankan investasi operasional yang merata untuk Takeaway** (packaging, kecepatan pengambilan) di semua metode pembayaran, karena preferensi Takeaway tidak berbeda berdasarkan cara pelanggan membayar.
4. **Hindari memusatkan anggaran promosi di akhir pekan atau hari tertentu**; karena permintaan stabil sepanjang minggu, anggaran lebih baik dialokasikan ke strategi upsell (Rekomendasi 1–2) dibanding penargetan berbasis waktu.

## Catatan Kualitas Data & Metodologi
- Nilai `Item` yang hilang dipulihkan lewat pemetaan harga-ke-item, hanya diterapkan pada harga yang unik (tidak ambigu); item yang berbagi harga sama (Cake/Juice seharga 3,0, Sandwich/Smoothie seharga 4,0) sengaja dikecualikan dari recovery ini untuk menghindari tebakan yang salah.
- Nilai `Quantity`, `Price Per Unit`, dan `Total Spent` yang hilang dipulihkan lewat perhitungan silang matematis dari dua kolom lain yang tersedia, alih-alih imputasi statistik (median), demi menjaga akurasi yang lebih tinggi.
- Deteksi duplikat sengaja menyertakan `Transaction ID`; transaksi dengan atribut identik namun ID berbeda dianggap sebagai transaksi sah dan independen, bukan kesalahan data — mengingat kardinalitas kategori yang rendah di dataset ini (8 item, 3 metode pembayaran, 2 lokasi, granularitas tanggal per hari) membuat kecocokan kebetulan secara statistik memang wajar terjadi.
- Fitur `Hour` dan `Time of Day` dikeluarkan dari analisis, karena kolom `Transaction Date` sumber hanya berisi informasi tanggal tanpa komponen jam.

## Struktur Proyek
```
cafe-sales-analysis/
├── README.md
├── README.id.md
├── data/
│   ├── raw/
│   │   └── dirty_cafe_sales.csv
│   └── cleaned/
│       ├── cafe_sales_cleaned_final.csv
│       └── cafe_sales_featured.csv
├── notebooks/
│   ├── 01_data_cleaning.ipynb
│   ├── 02_eda_visualization.ipynb
│   ├── 03_feature_engineering.ipynb
│   └── 04_insight_reporting.ipynb
├── images/
│   └── summary_dashboard.png
├── reports/
│   └── executive_report.md
└── requirements.txt
```

## Cara Menjalankan Ulang
```bash
git clone https://github.com/username-kamu/cafe-sales-analysis.git
cd cafe-sales-analysis
pip install -r requirements.txt
jupyter notebook notebooks/01_data_cleaning.ipynb
```

## Pelajaran yang Didapat
- Pengecekan missing value standar (`.isnull()`) saja tidak cukup — kolom kategori perlu diperiksa satu per satu untuk menemukan nilai "menyamar" yang tersembunyi.
- Kalau suatu kolom bisa dihitung ulang secara matematis dari kolom lain yang bersih (misalnya `Total Spent = Quantity × Price Per Unit`), perhitungan ulang lebih akurat dibanding imputasi statistik.
- Logika deteksi duplikat harus dipilih secara sadar sesuai konteks dataset — menyertakan kolom ID unik bisa jadi pilihan yang benar kalau kecocokan atribut secara kebetulan memang masuk akal, bukan otomatis dianggap "bug" yang harus diperbaiki.
- Item yang paling sering dibeli tidak selalu item yang paling bernilai — analisis revenue per item sering mengungkap prioritas yang berbeda dibanding analisis jumlah transaksi saja.
- Batas segmentasi yang bertepatan dengan nilai maksimum struktural dataset perlu didokumentasikan secara eksplisit, supaya pembaca tidak salah menafsirkan maknanya.

## Penulis
*Nama kamu di sini*
*Link LinkedIn / Portofolio (opsional)*
