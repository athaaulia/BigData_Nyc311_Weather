# Data Lake

Zona penampungan seluruh **data mentah** dari berbagai sumber dalam format aslinya (CSV & JSON), sebelum masuk ke proses transformasi pipeline (ETL/ELT).

Pada proyek ini, data lake menampung dua sumber mentah berikut (sama dengan isi folder `raw/`):

- `nyc311_raw.csv` — NYC 311 Service Requests (CSV, ~830 MB)
- `weather_raw.json` — Open-Meteo Historical Weather (JSON)

> Catatan: `raw/` menyimpan file fisik hasil extract, sedangkan `datalake/` adalah konsep zona penampung data mentah dari banyak sumber. Untuk skala proyek ini, keduanya merujuk pada data mentah yang sama.

File mentah lengkap disimpan di Google Drive (karena ukuran besar):

**Link data mentah:** https://drive.google.com/drive/folders/1rtflPT1ffWJuTZHfhqoU3A3-TvgjWqg4?usp=sharing
