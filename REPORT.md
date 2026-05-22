# Systematic Evaluation of Data Augmentation Strategies for CNN-based Image Classification on CIFAR-10
*Catatan untuk dosen pengampu: Jika file .ipynb tidak dapat dimuat/dirender oleh GitHub, Bapak/Ibu dapat melihat dan menjalankan langsung kodingan eksperimen ini melalui tautan Google Colab berikut:*

[> Klik di sini untuk membuka kodingan di Google Colab <](https://colab.research.google.com/drive/1RVQWGu1zA4Fwl25tFVhvrLuyYzep41ia?usp=sharing)

## 1. Dataset yang Digunakan
Eksperimen ini menggunakan dataset **CIFAR-10**, yang terdiri dari 60.000 gambar berwarna berukuran 32x32 piksel yang terbagi ke dalam 10 kelas (pesawat, mobil, burung, kucing, rusa, anjing, katak, kuda, kapal, dan truk).
*   **Total Data Training:** 50.000 gambar
*   **Total Data Testing:** 10.000 gambar
*   **Ukuran Input:** 32 x 32 piksel (RGB)

---

## 2. Metode dan Arsitektur Model
Model yang digunakan dalam eksperimen ini adalah **Simple CNN (Custom 2-Layer Convolutional Network)** dengan konfigurasi hyperparameter sebagai berikut:
*   **Optimizer:** Adam
*   **Loss Function:** Cross-Entropy Loss
*   **Learning Rate:** 0.001
*   **Batch Size:** 128
*   **Epochs:** 15

---

## 3. Skema Eksperimen & Parameter Variabel
Untuk mengevaluasi efektivitas *data augmentation* dalam menekan *overfitting*, eksperimen dibagi menjadi tiga skema:

| Nama Skema | Jenis Data Augmentasi | Detail Parameter (PyTorch Transforms) |
| :--- | :--- | :--- |
| **Skema A (Baseline)** | Tanpa Augmentasi | Hanya normalisasi (`ToTensor()`) |
| **Skema B (Basic)** | Augmentasi Geometris Dasar | - `RandomHorizontalFlip(p=0.5)`<br>- `RandomCrop(size=32, padding=4)` |
| **Skema C (Advanced)**| Augmentasi Tingkat Lanjut | - Skema B ditambah:<br>- `ColorJitter(brightness=0.2, contrast=0.2)`<br>- `RandomRotation(degrees=15)` |

---

## 4. Hasil Eksperimen & Evaluasi Otomatis
Berikut adalah hasil pengujian performa model untuk ketiga skema setelah dilatih selama 15 epoch:

| Skema Eksperimen | Final Training Accuracy | Final Validation/Test Accuracy | Analisis Status Model |
| :--- | :--- | :--- | :--- |
| **Skema A (Baseline)** | 97.50% | 71.78% | **Overfitting Parah.** Model gagal menggeneralisasi data baru (gap 25.72%). |
| **Skema B (Basic)**| 75.49% | 75.16% | **Good Fit.** Sangat stabil. Gap nyaris 0%, performa pada data testing paling optimal. |
| **Skema C (Advanced)** | 69.70% | 72.99% | **Underfitting/Heavy Regularization.** Arsitektur model terlalu sederhana untuk mempelajari manipulasi gambar yang terlalu kompleks dalam 15 epoch. |

### Analisis Evaluasi:
1. **Dampak Baseline:** Tanpa augmentasi (Skema A), model dengan sangat cepat menghafal data *training*, namun gagal mengenali variasi gambar pada data *testing*.
2. **Titik Optimal:** Penerapan pemotongan dan pembalikan acak (Skema B) terbukti paling efektif untuk arsitektur CNN sederhana ini, memberikan generalisasi terbaik tanpa membebani kapasitas model.
3. **Batas Kapasitas:** Augmentasi ekstrem (Skema C) terbukti bertindak sebagai regularisasi yang sangat kuat, sehingga mencegah *overfitting* sepenuhnya. Namun, hal ini menuntut kapasitas model yang lebih dalam (*deeper network*) atau jumlah epoch yang lebih banyak agar akurasinya bisa menyusul Skema B.

---

## 5. Kesimpulan
Berdasarkan evaluasi sistematis ini, strategi *Data Augmentation* terbukti esensial dan mutlak diperlukan dalam klasifikasi citra. Untuk model CNN dengan arsitektur yang dangkal (*shallow*) dan durasi pelatihan yang singkat, strategi augmentasi geometris dasar (Skema B) adalah pilihan yang paling optimal dibandingkan memaksakan augmentasi tingkat lanjut (Skema C) yang justru dapat memicu *underfitting*.
