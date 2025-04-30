# Proyek Klasifikasi Gambar: Kucing vs Anjing (Dataset Besar dari Kaggle)

## Deskripsi Proyek

Proyek ini adalah implementasi klasifikasi gambar biner menggunakan dataset **Cat and Dog yang besar dari Kaggle**. Tujuan utamanya adalah untuk membangun model *Convolutional Neural Network (CNN)* menggunakan **Transfer Learning** dengan arsitektur **MobileNetV2** untuk secara akurat membedakan antara gambar kucing dan anjing. Proyek ini dijalankan sepenuhnya di lingkungan Google Colab dan menghasilkan model dalam berbagai format (SavedModel, TF-Lite, dan TFJS) untuk potensi deployment.

## Dataset

Dataset yang digunakan adalah **"Dogs vs. Cats Redux: Kernels Edition"** atau versi dataset Cat and Dog besar lainnya yang tersedia di Kaggle (seperti `tongpython/cat-and-dog`). Dataset ini terdiri dari puluhan ribu gambar (lebih dari 10.000 sampel total) dari kucing dan anjing, terbagi dalam dua kelas untuk pelatihan dan validasi/pengujian.

Dataset ini diakses langsung di Google Colab menggunakan **Kaggle API**, sehingga tidak perlu mengunduhnya secara manual ke komputer lokal.

## Metode

Proyek ini mengimplementasikan pendekatan **Transfer Learning**:
1.  Menggunakan model **MobileNetV2** yang sudah dilatih sebelumnya (pre-trained) pada dataset ImageNet sebagai *base model* untuk mengekstrak fitur dari gambar.
2.  Menambahkan layer klasifikasi baru di atas *base model* yang dibekukan (Frozen) untuk tugas klasifikasi biner (kucing vs anjing).
3.  Melatih layer klasifikasi baru terlebih dahulu.
4.  (Opsional, namun disarankan) Melakukan **Fine-tuning** dengan "mencairkan" (unfreeze) beberapa layer teratas dari *base model* dan melanjutkan pelatihan dengan *learning rate* yang sangat kecil untuk mengadaptasi model secara lebih spesifik ke dataset Cat and Dog.

Penggunaan **ImageDataGenerator** diimplementasikan untuk memuat gambar dari struktur direktori yang diekstrak dan melakukan augmentasi data selama pelatihan, yang membantu meningkatkan generalisasi model.

## Persyaratan

Untuk menjalankan proyek ini di Google Colab, Anda memerlukan:
* Akun Google.
* Akses ke Google Colab (dengan runtime GPU disarankan untuk pelatihan yang lebih cepat).
* File `kaggle.json` (API Token dari akun Kaggle Anda) untuk mengunduh dataset.
* Library Python yang dibutuhkan (terdaftar dalam sel instalasi di notebook).

## Menjalankan Proyek di Google Colab

1.  Buka Google Colab dan buat notebook baru, atau unggah file notebook Python (.ipynb) jika Anda sudah menyalin kode ke dalamnya.
2.  Copy dan paste kode dari skrip Colab yang disediakan (yang terakhir kita diskusikan, versi sederhana untuk dataset Cat and Dog Kaggle) ke dalam sel-sel di notebook Colab Anda.
3.  Ubah runtime notebook ke **GPU** (Runtime -> Change runtime type -> pilih GPU sebagai Hardware accelerator).
4.  Ikuti instruksi di notebook untuk mengunggah file `kaggle.json` Anda ke sesi Colab.
5.  Jalankan setiap sel kode secara berurutan dari atas ke bawah. Skrip akan:
    * Menginstal library yang diperlukan.
    * Mengkonfigurasi Kaggle API.
    * Mengunduh dan mengekstrak dataset Cat and Dog dari Kaggle.
    * Menyiapkan data generators menggunakan `ImageDataGenerator`.
    * Membangun model Transfer Learning (MobileNetV2 + Head).
    * Mengkompilasi model.
    * Melatih model (tahap Head dan opsional Fine-tuning).
    * Membuat plot akurasi dan loss pelatihan/validasi.
    * Mengevaluasi model.
    * Menyimpan model dalam format SavedModel, TF-Lite, dan TFJS.

## Output Proyek

Setelah menjalankan skrip, output utama yang dihasilkan adalah:
* **Plot Akurasi dan Loss:** Visualisasi performa model selama pelatihan.
* **Hasil Evaluasi:** Metrik akurasi dan loss pada data validasi/test.
* **File Model yang Disimpan:** Model terlatih disimpan dalam format:
    * SavedModel (`/content/catdog_kaggle_model_output/saved_model/`)
    * TF-Lite (`/content/catdog_kaggle_model_output/tflite_model/model.tflite`)
    * TFJS (`/content/catdog_kaggle_model_output/tfjs_model/`)

## Mengunduh Hasil

Folder-folder yang berisi model yang disimpan (`saved_model`, `tflite_model`, `tfjs_model`), checkpoint pelatihan (`catdog_kaggle_checkpoints`), dan dataset yang diunduh (`cat_dog_kaggle_data`) berada di lingkungan sementara Colab (`/content/`).

Untuk mengunduhnya ke komputer lokal Anda atau menyimpannya secara permanen, disarankan:
1.  **Menyalinnya ke Google Drive:** Ini adalah cara paling aman untuk menyimpan data besar secara persisten. Gunakan kode Python `shutil.copytree` setelah me-mount Google Drive. (Contoh kode untuk ini ada di diskusi sebelumnya).
2.  **Mengunduh Langsung ke Komputer (via Zip):** Untuk folder, Anda perlu mengompresnya terlebih dahulu menjadi file `.zip` menggunakan perintah `!zip` atau fungsi `shutil.make_archive` di Python, lalu menggunakan `google.colab.files.download()` pada file zip tersebut. (Contoh kode untuk ini juga ada di diskusi sebelumnya). Menggunakan File Browser di sidebar Colab dan klik kanan -> Download juga akan mengompres folder menjadi zip.

## Struktur Proyek
├── cat_dog_kaggle_data/        # Dataset yang diunduh dan diekstrak
│   ├── training_set/
│   └── test_set/
│       ├── cats/
│       └── dogs/
├── catdog_kaggle_checkpoints/  # Checkpoint model terbaik selama pelatihan
├── saved_model/                # SavedModel format
├── tflite_model/               # TF-Lite format (model.tflite)
├── tfjs_model/                 # TFJS format (model.json, shard files)
└── notebook.ipynb              # File notebook Colab Anda