# Big Data Analytics: Komparasi Pipeline ETL vs ELT (NYC 311 & Weather)

Proyek ini merupakan studi komparatif implementasi *data pipeline* menggunakan dua arsitektur berbeda: **ETL (Extract, Transform, Load)** dan **ELT (Extract, Load, Transform)**. Proyek ini ditujukan untuk menganalisis dampak cuaca terhadap pola permintaan layanan masyarakat di New York City (NYC 311) sebagai bagian dari pemenuhan Tugas Besar UAS Big Data.

**Tim Pengembang (Computer Engineering, Telkom University):**
1. **Atha Aulia Shidiq** - ELT Pipeline & Blue Dashboard
2. **[Nama Rekan Anda]** - ETL Pipeline & Pink Dashboard

---

## 📌 Deskripsi Proyek & Perbandingan Arsitektur
Proyek ini mengekstraksi dataset yang sama (CSV NYC 311 dan JSON Open-Meteo API), namun diproses melalui dua *pipeline* yang berbeda untuk membandingkan efisiensi dan metodenya:

*   **Arsitektur ETL (Oleh [Nama Rekan Anda]):**
    Data diekstrak, kemudian **ditransformasi secara ekstensif menggunakan Python (Pandas)** di memori Colab (pembersihan, normalisasi, dan *join*). Setelah data bersih dan membentuk tabel *Fact*, barulah data dimuat (*Load*) ke dalam *data warehouse* Neon DB.
*   **Arsitektur ELT (Oleh Atha Aulia Shidiq):**
    Data diekstrak dan **langsung dimuat (Load) dalam kondisi mentah (Raw)** ke dalam tabel *staging* Neon DB. Seluruh proses transformasi, *parsing* JSON, dan *feature engineering* dieksekusi secara native di dalam *database* menggunakan instruksi **SQL murni**.

## 🛠️ Cara Menjalankan Proyek (Reproducibility)
Proyek ini menyediakan dua *notebook* Google Colab yang berjalan secara independen.
1. Pastikan file raw `nyc311_raw.csv` dan `weather_raw.json` berada di direktori yang diatur dalam *notebook*.
2. **Menjalankan Pipeline ETL:** Buka file `ETL_Pipeline_NYC311.ipynb`, jalankan semua *cell*. Transformasi akan terlihat pada log proses Pandas sebelum masuk ke *database*.
3. **Menjalankan Pipeline ELT:** Buka file `ELT_Pipeline_NYC311.ipynb`, jalankan semua *cell*. Proses *Load* awal akan memuat data mentah utuh, dilanjutkan dengan eksekusi script SQL yang membangun *View/Table Fact* di dalam Neon DB.

*(Catatan: Kredensial Neon DB kami sembunyikan demi keamanan. Silakan gunakan connection string PostgreSQL Anda sendiri pada variabel `DATABASE_URL` jika ingin melakukan verifikasi run).*

## 📂 Dokumentasi Dataset
*   **NYC 311 Service Requests:** Data historis keluhan non-darurat warga NYC (Filter Area 5 Borough).
*   **Historical Weather API:** Data suhu, curah hujan, dan angin dari [Open-Meteo](https://open-meteo.com/).

## 🗄️ Dokumentasi Data Warehouse
Meskipun pendekatannya berbeda, kedua *pipeline* bermuara pada satu struktur analitik akhir (*Data Mart*) yang memiliki skema serupa. Detail *Data Lineage* dan ERD dapat dilihat pada file `architecture_diagram.png`.

**Struktur Tabel Analitik Final**
| Nama Kolom | Keterangan |
| :--- | :--- |
| `unique_key` | Primary Key, ID unik laporan 311 |
| `created_date` & `closed_date` | Waktu laporan dibuat dan ditutup |
| `complaint_type` & `borough` | Jenis pengaduan dan Nama wilayah |
| `temperature_2m_norm` | Suhu udara (Normalisasi Min-Max) |
| `precipitation` | Curah hujan (Imputasi & *clipping* IQR) |
| `wind_speed_10m_norm` | Kecepatan angin (Normalisasi Min-Max) |
| `hour_of_day` & `is_weekend` | Jam kejadian dan penanda akhir pekan |
| `response_time_hours` | Durasi penyelesaian laporan dalam jam |
| `weather_risk_score` | Kombinasi risiko curah hujan dan angin kencang |
| `rush_hour_bad_weather` | 1 jika jam sibuk DAN cuaca buruk/hujan, 0 jika tidak |

*(Untuk 8 Query SQL Analitik mendetail dari pipeline ELT, silakan rujuk ke file script/Colab ELT yang terlampir).*

## 📈 Dashboard Analitik
Kedua *pipeline* bermuara pada Dashboard yang dibuat secara terpisah berdasarkan instans Neon DB masing-masing.

### 1. Dashboard ELT (Nuansa Biru - Atha)
Menampilkan agregasi murni hasil transformasi SQL, difokuskan pada pemetaan *Borough* dan korelasi jam sibuk terhadap cuaca buruk.
*   **Tangkapan Layar:** `[Tambahkan link gambar dashboard biru di sini]`
*   **Link Dashboard:** `[Tambahkan link publik jika ada]`

### 2. Dashboard ETL (Nuansa Pink - [Nama Rekan])
Menampilkan agregasi hasil transformasi Pandas, difokuskan pada tren historis dan analisis proporsi tipe komplain.
*   **Tangkapan Layar:** `[Tambahkan link gambar dashboard pink di sini]`
*   **Link Dashboard:** `[Tambahkan link publik jika ada]`
