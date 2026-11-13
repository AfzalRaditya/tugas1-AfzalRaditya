# Data Tugas 1

## Dataset yang Dipilih

| Item | Isi |
|---|---|
| Nama dataset | APBD Pemda Indonesia (2011–2026) |
| Sumber | [pohonmaple/apbd-pemda-indonesia — Hugging Face](https://huggingface.co/datasets/pohonmaple/apbd-pemda-indonesia) |
| Lisensi/ketentuan pakai | Belum terverifikasi; metadata sumber yang diperiksa belum mencantumkan lisensi eksplisit. Akses publik tidak berarti Public Domain. |
| Ukuran | **14,24 MB / 13,58 MiB** (14.244.671 byte), **3.132.863 baris**, 11 kolom; memenuhi kriteria lebih dari 1 juta baris. |
| Periode data | Januari 2011–September 2026 pada file lokal |
| Unit analisis | Laporan APBD per tahun, bulan laporan, provinsi, pemda, dan akun keuangan |

MB menggunakan pembagi 1.000.000 byte; MiB menggunakan pembagi 1.048.576 byte.
Kunci laporan adalah `tahun`, `bulan`, `id_provinsi`, `id_pemda`, dan `no_akun`.

## Tempat Mencari Dataset

Dataset proyek ini diperoleh dari Hugging Face. Situs berikut merupakan referensi sumber dataset Indonesia.

| Situs | Kegunaan |
|---|---|
| [Satu Data Indonesia](https://data.go.id/) | Portal data terbuka lintas instansi pemerintah Indonesia. |
| [Badan Pusat Statistik](https://www.bps.go.id/) | Statistik sosial, ekonomi, kependudukan, dan data wilayah. |
| [BMKG Data Online](https://dataonline.bmkg.go.id/) | Data cuaca, iklim, gempa bumi, dan observasi meteorologi. |
| [Hugging Face Datasets](https://huggingface.co/datasets) | Dataset publik yang dapat dicari berdasarkan topik, bahasa, atau ukuran. |
| [Kaggle Datasets](https://www.kaggle.com/datasets) | Katalog dataset publik; periksa lisensi dan dokumentasi pembuatnya. |
| [Google Dataset Search](https://datasetsearch.research.google.com/) | Mesin pencari untuk menemukan dataset dari berbagai portal. |

## Cara Memperoleh Data

1. Buka [halaman sumber APBD](https://huggingface.co/datasets/pohonmaple/apbd-pemda-indonesia)
   dan tab [Files and versions](https://huggingface.co/datasets/pohonmaple/apbd-pemda-indonesia/tree/main).
2. Periksa file dan riwayat versi. File lokal diperoleh melalui unduhan Parquet
   langsung menurut pemilik proyek, tetapi tautan unduh dan nama file aslinya tidak
   diingat. Notebook membutuhkan file gabungan yang sesuai dengan identitas di bawah;
   satu file laporan bulanan bukan penggantinya.
3. Jika file asli/arsip yang sesuai telah diperoleh, letakkan tanpa mengubah isinya
   pada `data/raw/apbd_indonesia_2011_2026.parquet`.
4. Verifikasi ukuran, jumlah baris, dan checksum. Ketiga notebook telah menggunakan
   nama file tersebut; tidak perlu mengubah `DATA_PATH` jika lokasi dan namanya sama.
5. Jalankan `notebooks/01_data_profiling.ipynb`, lalu
   `notebooks/02_data_cleaning.ipynb`, kemudian `notebooks/03_eda_and_insights.ipynb`.
   Notebook 02 menghasilkan `data/processed/apbd_indonesia_bersih.parquet`.

**Identitas file lokal:**

- Nama: `apbd_indonesia_2011_2026.parquet`.
- Ukuran: 14.244.671 byte.
- SHA-256: `a8da276c8e7936453b1cbcef9b0b7c8e80c952f4c87bb522d0c6c25662419ac9`.

Verifikasi dari root proyek menggunakan PowerShell:

```powershell
Get-FileHash -LiteralPath data/raw/apbd_indonesia_2011_2026.parquet -Algorithm SHA256
python -c "import polars as pl; p='data/raw/apbd_indonesia_2011_2026.parquet'; d=pl.scan_parquet(p); print(d.collect_schema()); print(d.select(pl.len()).collect())"
```

**Status sumber dan reproduksi:** [metadata sumber](https://huggingface.co/api/datasets/pohonmaple/apbd-pemda-indonesia)
 diperiksa pada 13 November 2026, revision `999eb215073d248ef89360fc0c833f6a8098e208`.
Daftar file mencakup laporan Desember 2011–2022, sejumlah laporan bulanan 2023–2026,
serta `ringkas_tahunan.parquet`, `index.json`, dan `pemda.json`. File gabungan
bernama seperti file lokal tidak tercantum. Tautan unduh identik belum terverifikasi;
menggabungkan file sumber saat ini tidak dijamin menghasilkan dataset yang sama.
Checksum di atas mengidentifikasi file lokal, bukan membuktikan keasliannya terhadap sumber.

Metadata yang diperiksa belum memuat deklarasi lisensi dan daftar file tidak memuat
README/license. Ketentuan penggunaan perlu dikonfirmasi kepada pengunggah sebelum
distribusi ulang; klaim Public Domain tidak digunakan.

## Aturan Penyimpanan

- Jangan commit dataset mentah atau hasil olahan berukuran besar ke Git.
- File pada `data/raw/` adalah data asli dan tidak boleh diubah.
- Simpan hasil transformasi yang dapat direproduksi pada `data/processed/`.
- Cleaning mempertahankan nominal dan persentase sumber, menghitung rasio pada
  anggaran positif, serta menandai anomali tanpa clipping. Rasio lainnya null.
- Jangan menyalin laporan tahunan ke bulan lain atau mengisi bulan yang tidak tersedia
  untuk menambah jumlah baris. Snapshot bulanan tidak dijumlahkan menjadi total tahunan.
