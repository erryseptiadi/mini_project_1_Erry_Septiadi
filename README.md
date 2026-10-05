# Mini Project 1 — Weather Data Pipeline

**Author:** Erry Septiadi  
**Language:** Python  
**Data Source:** Tomorrow.io Weather Forecast API

## 📌 Project Overview

Mini Project 1 ini merupakan latihan pengambilan, ekstraksi, pembersihan, dan penyimpanan data cuaca menggunakan Python.

Data cuaca diambil dari **Tomorrow.io Weather Forecast API** berdasarkan koordinat **42.3478,-71.0466 (Boston)**. Data kemudian diolah menggunakan `pandas` hingga menghasilkan dataset yang lebih siap digunakan untuk analisis.

Project ini juga menerapkan konsep **Object-Oriented Programming (OOP)** melalui class `KlienCuaca` serta beberapa function untuk proses data cleaning.

## 🎯 Objectives

Project ini bertujuan untuk:

- Mengambil data cuaca melalui API.
- Memahami cara menggunakan API dengan Python.
- Mengekstrak data JSON menjadi struktur tabular.
- Mengubah data menjadi `pandas DataFrame`.
- Melakukan pengecekan kualitas data.
- Menangani missing values.
- Menangani duplicate data.
- Mengubah tipe data tanggal dan numerik.
- Menyimpan dataset hasil cleaning ke dalam file CSV.

## 🛠️ Technologies & Libraries

Project dibuat menggunakan:

- **Python 3.13**
- `requests` — melakukan request ke API.
- `python-dotenv` — membaca API key dari file `.env`.
- `pandas` — mengolah dan membersihkan data.
- `time`
- `os`

## 🔐 Environment Setup

API key tidak ditulis langsung di dalam source code. Project menggunakan file `.env`.

Buat file `.env`:

```env
WEATHER_API_KEY=your_api_key_here
```

> **Important:** Jangan upload file `.env` ke GitHub karena berisi API key.

Tambahkan `.env` ke `.gitignore`:

```gitignore
.env
```

## 📂 Project Structure

Contoh struktur repository:

```text
mini-project-1/
│
├── mini_project_1_[Erry_Septiadi].ipynb
├── dataset_cuaca_tomorrow.csv
├── .env
├── .gitignore
└── README.md
```

## 📊 Data Source

Data diperoleh dari:

**Tomorrow.io — Weather Forecast API**

Endpoint yang digunakan:

```text
https://api.tomorrow.io/v4/weather/forecast
```

Lokasi yang digunakan:

```text
42.3478,-71.0466
```

Lokasi tersebut digunakan sebagai sumber data cuaca untuk area **Boston**.

## 🔄 Workflow

Alur utama project:

```text
Tomorrow.io API
       ↓
Request menggunakan Python
       ↓
JSON Response
       ↓
Ekstraksi data hourly
       ↓
Pandas DataFrame
       ↓
Data Quality Check
       ↓
Data Cleaning
       ↓
Validasi Dataset
       ↓
CSV
```

## 🧱 OOP Implementation

Project membuat class:

```python
class KlienCuaca:
```

Class tersebut digunakan untuk menangani proses pengambilan data cuaca dari API.

Method utama:

```python
ambil_data_cuaca(lokasi, unit="metric")
```

Fungsi method tersebut adalah mengirim request ke Tomorrow.io API berdasarkan lokasi dan unit yang dipilih, kemudian mengembalikan response dalam bentuk JSON.

Contoh penggunaan:

```python
klien = KlienCuaca(API_KEY)

koordinat_lokasi = "42.3478,-71.0466"

hasil = klien.ambil_data_cuaca(
    lokasi=koordinat_lokasi,
    unit="metric"
)
```

## 🧹 Data Cleaning

Beberapa proses cleaning yang dilakukan:

### 1. Mengubah tipe data tanggal

Kolom `Tanggal_Waktu` yang berasal dari API diubah menjadi tipe `datetime` menggunakan:

```python
pd.to_datetime()
```

### 2. Menangani missing values

Nilai kosong pada `Indeks_UV` diisi dengan `0`.

```python
df_cuaca["Indeks_UV"] = df_cuaca["Indeks_UV"].fillna(0)
```

### 3. Menghapus duplicate

Data duplicate berdasarkan `Tanggal_Waktu` dihapus:

```python
df_cuaca = df_cuaca.drop_duplicates(
    subset=["Tanggal_Waktu"]
)
```

### 4. Memastikan tipe data numerik

Kolom numerik dikonversi menggunakan:

```python
pd.to_numeric(..., errors="coerce")
```

## 🧩 Functions

Dua function utama yang digunakan dalam proses cleaning:

### `ubah_tipe_tanggal()`

Mengubah kolom tanggal dari format teks menjadi `datetime`.

```python
def ubah_tipe_tanggal(df, kolom):
    df[kolom] = pd.to_datetime(df[kolom])
    return df
```

### `perbaiki_tipe_numerik()`

Memastikan kolom yang seharusnya berisi angka memiliki tipe numerik.

```python
def perbaiki_tipe_numerik(df, daftar_kolom):
    for kolom in daftar_kolom:
        df[kolom] = pd.to_numeric(
            df[kolom],
            errors="coerce"
        )
    return df
```

## 📋 Dataset

Dataset akhir memiliki:

- **120 baris**
- **10 kolom**
- Tidak terdapat duplicate berdasarkan `Tanggal_Waktu`
- Tidak terdapat missing value setelah proses cleaning

Kolom yang tersedia:

| Kolom | Deskripsi |
|---|---|
| `Tanggal_Waktu` | Waktu pengamatan/prakiraan cuaca |
| `Sumber_Koordinat` | Koordinat lokasi sumber data |
| `Suhu_C` | Suhu dalam Celsius |
| `Suhu_Dirasakan_C` | Suhu yang dirasakan |
| `Kelembaban` | Tingkat kelembaban |
| `Kecepatan_Angin` | Kecepatan angin |
| `Arah_Angin` | Arah angin |
| `Indeks_UV` | Indeks UV |
| `Tutupan_Awan` | Persentase tutupan awan |
| `Visibilitas_KM` | Jarak visibilitas dalam kilometer |

## 📈 Statistik Singkat

Hasil statistik deskriptif menunjukkan:

| Metric | Suhu (°C) | Kelembaban | Indeks UV |
|---|---:|---:|---:|
| Count | 120 | 120 | 120 |
| Mean | 14.25 | 70.83 | 0.74 |
| Minimum | 11.42 | 55 | 0 |
| Maximum | 17.91 | 97 | 5 |

## ✅ Final Validation

Hasil pengecekan akhir:

```text
Jumlah baris              : 120
Sudah lebih dari 100?     : True
Indeks_UV masih ada kosong: 0
Waktu masih ada kembar    : 0
```

Dataset berhasil memenuhi target minimal jumlah data dan telah melalui proses cleaning.

## 💾 Output

Dataset hasil pengolahan disimpan sebagai:

```text
dataset_cuaca_tomorrow.csv
```

File kemudian dibaca kembali menggunakan `pandas` untuk memastikan file berhasil tersimpan dan dapat digunakan kembali.

## ▶️ How to Run

### 1. Clone repository

```bash
git clone <repository-url>
cd <repository-folder>
```

### 2. Install dependencies

```bash
pip install requests python-dotenv pandas
```

### 3. Buat file `.env`

```env
WEATHER_API_KEY=your_api_key_here
```

### 4. Jalankan Notebook

Buka:

```text
mini_project_1_[Erry_Septiadi].ipynb
```

Kemudian jalankan cell secara berurutan.

## 📚 Learning Outcomes

Melalui mini project ini, beberapa konsep yang dipraktikkan adalah:

- API integration
- HTTP request
- JSON data extraction
- Environment variables
- Pandas DataFrame
- Data cleaning
- Missing value handling
- Duplicate handling
- Data type conversion
- Object-Oriented Programming
- CSV data storage
- Basic data validation

## 👤 Author

**Erry Septiadi**

Mini Project — Python & Data Processing
