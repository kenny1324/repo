
# Tugas 5 — Morphology
**Mata Kuliah:** Pengolahan Citra dan Video (PCV)  
**Nama:** Kenny Gabriel Manurung  
**NRP:** 5024241022  
**Dosen Pengampu:** Arta Kusuma Hernanda  

---

## 1. Pengertian Morfologi Citra

Morfologi citra (*image morphology*) merupakan teknik pengolahan citra digital yang digunakan untuk mengubah bentuk, ukuran, dan struktur objek pada citra. Operasi morfologi umumnya diterapkan pada citra biner dengan menggunakan sebuah elemen struktur (*structuring element*) atau kernel.

Pada tugas ini, operasi morfologi yang digunakan adalah:

1. Erosi (*Erosion*)
2. Dilasi (*Dilation*)
3. Opening
4. Closing

Sebelum operasi morfologi dilakukan, gambar diubah menjadi grayscale dan citra biner.

## 2. Membaca Gambar

Gambar dibaca menggunakan fungsi `cv2.imread()` dari OpenCV. Gambar yang digunakan dalam tugas ini adalah `rb.png`.

```python
img = cv2.imread("rb.png")

if img is None:
    print("Error: gambar rb.png tidak ditemukan!")
    raise SystemExit
```

Pemeriksaan `img is None` digunakan untuk memastikan gambar berhasil dibaca. Jika gambar tidak ditemukan atau gagal dibuka, program akan dihentikan.

## 3. Mengubah Gambar Menjadi Grayscale

Grayscale adalah citra yang memiliki tingkat intensitas dari hitam hingga putih tanpa informasi warna RGB.

```python
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
```

Fungsi `cv2.cvtColor()` digunakan untuk mengubah gambar dari format BGR menjadi grayscale. Citra grayscale kemudian digunakan sebagai masukan untuk proses thresholding.

## 4. Mengubah Grayscale Menjadi Citra Biner

Citra biner hanya memiliki dua nilai piksel, yaitu 0 (hitam) dan 255 (putih). Proses thresholding digunakan untuk memisahkan piksel berdasarkan intensitasnya.

```python
_, binary = cv2.threshold(
    gray, 0, 255, cv2.THRESH_BINARY + cv2.THRESH_OTSU
)
```

Parameter `cv2.THRESH_BINARY` menghasilkan piksel hitam atau putih berdasarkan nilai ambang (*threshold*). Sementara itu, `cv2.THRESH_OTSU` secara otomatis menentukan nilai ambang berdasarkan distribusi intensitas gambar.

**Catatan penting:** Pada hasil citra biner tugas ini, objek atau *foreground* terlihat berwarna hitam, sedangkan latar belakang atau *background* berwarna putih. Operasi morfologi OpenCV secara matematis memperlakukan piksel putih (255) sebagai bagian aktif yang diproses. Oleh karena itu, efek visual terhadap objek hitam dapat terlihat berlawanan dengan penjelasan morfologi yang menggunakan objek putih sebagai foreground.

## 5. Membuat Kernel

Kernel atau elemen struktur menentukan area tetangga yang digunakan untuk memproses setiap piksel. Pada tugas ini, digunakan kernel berbentuk persegi dengan ukuran 5 × 5.

```python
kernel = cv2.getStructuringElement(cv2.MORPH_RECT, (5, 5))
```

Kernel tersebut memiliki ukuran 5 piksel × 5 piksel. Ukuran kernel memengaruhi seberapa besar perubahan bentuk yang terjadi pada citra. Kernel yang lebih besar umumnya memberikan efek morfologi yang lebih kuat.

## 6. Erosi (*Erosion*)

Erosi merupakan operasi morfologi yang mengikis atau memperkecil area aktif pada citra. Operasi ini dapat membantu menghilangkan bagian kecil dan mempersempit objek yang diproses.

```python
erosion = cv2.erode(binary, kernel, iterations=1)
```

Pada citra tugas ini, objek yang terlihat berwarna hitam merupakan foreground. Karena OpenCV memproses area putih sebagai bagian aktif, erosi akan mengikis area putih sehingga area hitam dapat terlihat meluas.

Dengan demikian, hasil visual pada tugas ini dapat tampak berlawanan dengan contoh erosi yang menggunakan objek putih sebagai foreground.

## 7. Dilasi (*Dilation*)

Dilasi merupakan operasi morfologi yang memperluas area aktif pada citra. Operasi ini dapat membantu menyambungkan bagian yang terputus dan memperbesar area yang diproses.

```python
dilation = cv2.dilate(binary, kernel, iterations=1)
```

Pada citra tugas ini, dilasi memperluas area putih. Akibatnya, objek hitam sebagai foreground dapat terlihat menyusut.

**Perbedaan utama pada hasil tugas ini:** erosi membuat area hitam terlihat meluas, sedangkan dilasi membuat area hitam terlihat menyusut.

## 8. Opening

Opening adalah operasi morfologi yang menggabungkan erosi dan dilasi secara berurutan. Operasi ini umumnya digunakan untuk menghilangkan komponen kecil atau noise tanpa mengubah terlalu banyak bentuk bagian yang lebih besar.

```python
opening = cv2.morphologyEx(binary, cv2.MORPH_OPEN, kernel)
```

Opening menjalankan erosi terlebih dahulu, kemudian dilasi. Karena citra tugas ini menggunakan objek hitam dan latar belakang putih, efeknya perlu diamati berdasarkan perubahan kedua area tersebut. Secara visual, opening pada citra ini merupakan operasi opening terhadap area putih yang diproses OpenCV, sehingga efeknya terhadap objek hitam dapat menyerupai efek closing.

## 9. Closing

Closing adalah operasi morfologi yang menggabungkan dilasi dan erosi secara berurutan. Operasi ini umumnya digunakan untuk menutup celah kecil dan lubang pada area yang diproses.

```python
closing = cv2.morphologyEx(binary, cv2.MORPH_CLOSE, kernel)
```

Closing menjalankan dilasi terlebih dahulu, kemudian erosi. Karena OpenCV memproses area putih sebagai bagian aktif, efeknya terhadap objek hitam dapat terlihat menyerupai opening jika dibandingkan dengan konvensi yang menggunakan objek putih sebagai foreground.

## 10. Perbedaan Operasi Morfologi

| Operasi | Urutan proses | Efek yang diharapkan pada citra tugas |
|---|---|---|
| Erosi | Erosi | Area putih menyusut dan area hitam dapat meluas |
| Dilasi | Dilasi | Area putih meluas dan area hitam dapat menyusut |
| Opening | Erosi → Dilasi | Perubahan area putih; efek terhadap objek hitam perlu diamati pada hasil |
| Closing | Dilasi → Erosi | Perubahan area putih; efek terhadap objek hitam perlu diamati pada hasil |

**Catatan:** Penjelasan efek di atas mengikuti citra biner yang digunakan pada tugas ini. Hasil akhir juga dipengaruhi oleh bentuk objek, ukuran kernel, dan susunan piksel di sekitarnya.

## 11. Visualisasi Hasil

Hasil setiap tahapan ditampilkan dalam jendela yang terpisah agar perubahan citra lebih mudah diamati.

### 11.1 Gambar Asli

Gambar awal yang digunakan sebelum proses pengolahan.

![Gambar Asli](image/1.png)

### 11.2 Grayscale

Gambar yang telah diubah menjadi citra berskala abu-abu.

![Grayscale](image/2.png)

### 11.3 Citra Biner

Gambar hasil thresholding yang hanya memiliki nilai piksel hitam dan putih. Pada hasil ini, objek terlihat hitam dan latar belakang putih.

![Citra Biner](image/3.png)

### 11.4 Erosi

Hasil operasi erosi menggunakan kernel persegi berukuran 5 × 5. Area putih menyusut sehingga area hitam dapat terlihat meluas.

![Erosi](image/4.png)

### 11.5 Dilasi

Hasil operasi dilasi menggunakan kernel persegi berukuran 5 × 5. Area putih meluas sehingga area hitam dapat terlihat menyusut.

![Dilasi](image/5.png)

### 11.6 Opening

Hasil operasi opening, yaitu erosi yang dilanjutkan dengan dilasi.

![Opening](image/6.png)

### 11.7 Closing

Hasil operasi closing, yaitu dilasi yang dilanjutkan dengan erosi.

![Closing](image/7.png)

## 12. Analisis Hasil

Berdasarkan hasil pengolahan citra, setiap operasi morfologi menghasilkan perubahan pada bentuk dan ukuran area dalam citra biner.

1. **Grayscale:** mengubah gambar berwarna menjadi gambar dengan tingkat intensitas abu-abu.
2. **Thresholding:** mengubah grayscale menjadi citra biner dengan piksel hitam dan putih. Metode Otsu menentukan ambang secara otomatis, tetapi tidak menjamin pemisahan objek dan latar belakang selalu sempurna.
3. **Erosi:** mengikis area putih yang diproses OpenCV. Karena foreground pada hasil tugas ini berwarna hitam, objek hitam dapat terlihat membesar.
4. **Dilasi:** memperluas area putih yang diproses OpenCV. Akibatnya, objek hitam dapat terlihat mengecil.
5. **Opening:** menjalankan erosi lalu dilasi untuk mengubah struktur area pada citra.
6. **Closing:** menjalankan dilasi lalu erosi untuk mengubah struktur area pada citra.

Perbedaan hasil erosi dan dilasi pada tugas ini tidak menunjukkan bahwa operasinya tertukar. Perbedaan tersebut terjadi karena foreground yang diamati berwarna hitam, sedangkan operasi morfologi OpenCV secara matematis bekerja terhadap area putih pada citra biner.

## 13. Struktur Folder

Susunan folder tugas yang digunakan adalah sebagai berikut.

```text
5-morphology/
├── README.md
├── 5-morphology.py
├── rb.png
└── image/
    ├── 1.png
    ├── 2.png
    ├── 3.png
    ├── 4.png
    ├── 5.png
    ├── 6.png
    └── 7.png
```

Keterangan:

- `README.md`: dokumentasi tugas dan analisis hasil.
- `5-morphology.py`: program Python untuk menjalankan operasi morfologi.
- `rb.png`: gambar masukan.
- `image/`: folder untuk menyimpan tujuh gambar hasil visualisasi.

Pastikan nama file dan struktur folder sesuai dengan dokumentasi agar gambar dapat ditampilkan dengan benar di GitHub.

## 14. Instalasi Library

Program menggunakan Python dan OpenCV. NumPy juga diimpor pada kode program, meskipun tidak digunakan secara langsung dalam operasi morfologi.

Instal library dengan perintah berikut:

```bash
pip install opencv-python numpy
```

## 15. Cara Menjalankan Program

Buka terminal pada folder tugas, kemudian jalankan:

```bash
python 5-morphology.py
```

Program akan menampilkan tujuh jendela, yaitu gambar asli, grayscale, citra biner, erosi, dilasi, opening, dan closing.

Tekan tombol apa saja ketika salah satu jendela gambar aktif untuk menutup seluruh jendela.

## 16. Kesimpulan

Morfologi citra merupakan teknik pengolahan citra yang digunakan untuk memodifikasi struktur dan bentuk area pada citra biner. Pada tugas ini, operasi erosi, dilasi, opening, dan closing diterapkan menggunakan kernel persegi berukuran 5 × 5.

Tahap grayscale dan thresholding digunakan untuk menyiapkan gambar sebelum operasi morfologi dilakukan. Metode Otsu membantu menentukan ambang secara otomatis, tetapi hasil pemisahan foreground dan background tetap bergantung pada karakteristik gambar.

Hal penting yang ditemukan pada tugas ini adalah bahwa objek berwarna hitam dapat menunjukkan efek visual yang berlawanan dengan contoh morfologi yang menggunakan objek putih sebagai foreground. OpenCV memproses area putih sebagai bagian aktif pada operasi tersebut. Oleh karena itu, pemahaman mengenai warna foreground dan background sangat penting ketika menganalisis hasil operasi morfologi.
