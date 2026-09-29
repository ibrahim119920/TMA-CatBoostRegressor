# TMA-CatBoostRegressor

Prediksi tinggi muka air dengan ensemble CatBoost.

Pipeline notebook untuk memprediksi tinggi muka air (`tma_mdpl`) pada 30 pos pengamatan. Model memadukan data lingkungan, kalender, dan fitur jaringan sungai HydroRIVERS dalam ensemble `CatBoostRegressor` tanpa memakai lag target yang tidak tersedia pada horizon prediksi.

## Hasil yang tercatat

| Versi | RMSE leaderboard | RMSE validasi pooled OOF |
|---|---:|---:|
| V1 | 1,61079 m | 1,66058 m |
| V2 (ensemble terbaru) | **1,60422 m** | **1,60029 m** |

Angka V2 berasal dari [`logs.md`](logs.md) dan [manifest koreksi handoff](handoff/handoff_20260715T065403Z_all_manifest_correction.json). RMSE leaderboard dilaporkan dari run Kaggle; dataset, submission, model terlatih, dan arsip handoff asli tidak tersedia di repository ini untuk verifikasi ulang secara mandiri. Manifest koreksi juga mencatat bahwa hash notebook pada run lama tidak terekam, sehingga versi sumber yang persis digunakan tidak dapat dibuktikan dari checkout ini. Metode validasi dan keterbatasannya dijelaskan di [`architecture.md`](architecture.md).

## Menjalankan notebook

1. Siapkan lingkungan Python dengan Jupyter, lalu pasang dependensi model: `python -m pip install -r requirements.txt`.
2. Dapatkan dataset kompetisi dari sumber resminya. Untuk eksekusi lokal, simpan `train.csv`, `test.csv`, `sample_submission.csv`, serta folder `data_pendukung/` di dalam `datasets/`. Folder pendukung mencakup data lingkungan, koordinat pos, dan HydroRIVERS; rincian path ada di [`architecture.md`](architecture.md#8-artefak-dan-cara-menjalankan).
3. Buka [`notebook-150726.ipynb`](notebook-150726.ipynb) dari root repository. Nilai `MODE = "audit"` adalah default dan tidak melatih model. Pilih `validate` untuk menghasilkan validasi dan tuning, `fit` untuk final fit setelah tuning tersedia, atau `all` untuk validasi dan final fit.
4. Output lokal ditulis ke `artifacts/`. Di Kaggle, notebook mendeteksi dataset kompetisi dan menulis output ke `/kaggle/working/`.

Training penuh memerlukan dataset dan sumber daya komputasi yang sesuai; hasil tabel di atas adalah catatan run terdahulu, bukan hasil yang dihitung ulang saat membuka repository.

## Peta repository

| File atau folder | Fungsi |
|---|---|
| [`notebook-150726.ipynb`](notebook-150726.ipynb) | Audit data, rekayasa fitur, validasi rolling-origin, training, dan submission V2 |
| [`notebook-template.ipynb`](notebook-template.ipynb) | Template dan baseline awal |
| [`compare_submission.ipynb`](compare_submission.ipynb) | Perbandingan submission |
| [`architecture.md`](architecture.md) | Desain model, input, validasi, dan artefak |
| [`logs.md`](logs.md) | Riwayat eksperimen dan hasil |
| `handoff/` | Manifest koreksi historis; arsip model/submission tidak disertakan |
| `datasets/` | Tempat input lokal; file data diabaikan Git |

Notebook perbandingan memakai `train.csv` dan file `submission_*.csv` dari root kerja. Dependensi visualisasinya tersedia melalui `requirements.txt`; file submission harus disediakan sendiri.

Notebook utama tetap berada di root karena konfigurasi path dan provenance pipeline mengacu pada lokasi dan source notebook tersebut.
