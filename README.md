# Koreksi Bias Data CHIRPS: Metode Regresi Linear (Time-Variant)

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Research-orange)

## Deskripsi Proyek

Repositori ini memuat implementasi kode Python untuk melakukan **koreksi bias** pada data estimasi curah hujan satelit **CHIRPS v3.0** di wilayah Provinsi Jambi. Metode yang digunakan adalah **Regresi Linear (Linear Regression)** yang bersifat *time-variant*.

Berbeda dengan metode faktor koreksi statis, metode ini membangun model persamaan regresi yang unik untuk setiap langkah waktu (bulan) berdasarkan hubungan antara data satelit dan data stasiun observasi pada bulan tersebut.

## Metodologi

Koreksi dilakukan dengan memodelkan hubungan linear antara curah hujan satelit ($P_{sat}$) sebagai variabel independen dan curah hujan observasi ($P_{obs}$) sebagai variabel dependen:

$$P_{corr} = (a \times P_{sat}) + b$$

Dimana:
- $P_{corr}$: Curah hujan terkoreksi.
- $P_{sat}$: Data mentah CHIRPS (Raw).
- $a$: Gradien (*Slope*) dari model regresi bulan $t$.
- $b$: Konstanta (*Intercept*) dari model regresi bulan $t$.

Model dibentuk menggunakan algoritma *Ordinary Least Squares* (OLS) melalui pustaka `scikit-learn`. Jika pada bulan tertentu korelasi data tidak mencukupi, script akan mempertahankan nilai asli data satelit.

## Prasyarat Instalasi

Script ini dirancang untuk dijalankan di **Google Colab**. Berikut adalah dependensi pustaka yang digunakan:

### Daftar Pustaka
* **Statistik & Machine Learning:** `scikit-learn` (LinearRegression), `scipy`
* **Geospasial:** `rasterio`, `geopandas`, `shapely`
* **Analisis Data:** `pandas`, `numpy`
* **Visualisasi:** `matplotlib`, `seaborn`

### Instalasi
```bash
pip install rasterio geopandas shapely matplotlib scikit-learn pandas numpy seaborn tqdm folium

```

## Struktur Direktori Data

Pastikan data di Google Drive Anda disusun dengan struktur berikut:

```text
/Direktori_Project/
├── CHIRPS v3/                  # File raster .tif (input)
│   └── bias_corrected_reg/     # (Output) Hasil koreksi
├── Batas Adm Jambi/            # Shapefile batas wilayah
├── Data Stasiun BWS/           # File .csv data curah hujan observasi
└── script_regresi.ipynb        # Script utama

```

## Cara Penggunaan

1. **Persiapan Data**: Unggah data CHIRPS, Shapefile, dan data stasiun ke Google Drive.
2. **Konfigurasi**: Sesuaikan path `BASE_DIR` dan daftar stasiun (`bias_stations` untuk training, `validation_stations` untuk uji) di dalam script.
3. **Eksekusi**: Jalankan script. Program akan melakukan iterasi bulanan:
* *Clipping* raster ke area studi.
* *Training* model regresi berdasarkan stasiun kalibrasi.
* Menerapkan model ke seluruh piksel raster.
* Melakukan validasi pada stasiun independen.


4. **Hasil**: Statistik validasi (NSE, R, RSR) akan disimpan dalam format CSV.

## Contoh Hasil Validasi

Metode regresi umumnya memberikan hasil yang responsif terhadap pola musiman. Berikut contoh ringkasan statistik output:

| Stasiun Validasi | NSE (Raw) | NSE (Corrected) | R (Raw) | R (Corrected) |
| --- | --- | --- | --- | --- |
| **Muara Tembesi** | -0.45 | **0.65** | 0.55 | **0.82** |
| **Rantau Pandan** | 0.12 | **0.55** | 0.42 | **0.75** |

> *Script juga akan menghasilkan Scatter Plot perbandingan NSE dan Bar Chart RSR secara otomatis.*

## Penulis

**Jariyan Arifudin** Mahasiswa Geografi Lingkungan

Universitas Gadjah Mada (UGM)

## Lisensi & Sitasi

Kode ini didistribusikan di bawah **MIT License**.
Jika Anda menggunakan metode ini untuk penelitian, silakan sitasi repositori ini:

> Arifudin, J. (2026). *Koreksi Bias Data CHIRPS Menggunakan Regresi Linear*. GitHub Repository.
