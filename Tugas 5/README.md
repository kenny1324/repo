
# Tugas 5 - Morphology

## Pengertian Morphology

Morfologi citra (*image morphology*) adalah teknik pengolahan citra digital yang digunakan untuk mengubah bentuk dan struktur objek pada citra. Operasi ini umumnya diterapkan pada citra biner dengan menggunakan kernel atau *structuring element*.

Pada tugas ini, operasi morfologi yang digunakan meliputi:

- Erosi (*Erosion*)
- Dilasi (*Dilation*)
- Opening
- Closing

Sebelum operasi morfologi dilakukan, gambar diubah menjadi grayscale dan citra biner.

## 1. Membaca Gambar

Gambar dibaca menggunakan fungsi `cv2.imread()` dari OpenCV.

```python
img = cv2.imread("rb.png")

if img is None:
    print("Error: gambar rb.png tidak ditemukan!")
    raise SystemExit
```

Pemeriksaan `img is None` digunakan untuk memastikan gambar berhasil dibaca oleh program.

## 2. Grayscale

Grayscale adalah proses mengubah gambar berwarna menjadi gambar dengan tingkat intensitas abu-abu.

```python
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
```

Fungsi `cv2.cvtColor()` digunakan untuk mengubah gambar dari format BGR menjadi grayscale. Tahap ini mempermudah proses thresholding karena pemisahan piksel dilakukan berdasarkan intensitas.

## 3. Thresholding

Thresholding merupakan proses mengubah citra grayscale menjadi citra biner yang hanya memiliki dua nilai piksel, yaitu hitam (0) dan putih (255).

```python
_, binary = cv2.threshold(
    gray, 0, 255, cv2.THRESH_BINARY + cv2.THRESH_OTSU
)
```

Metode Otsu digunakan untuk menentukan nilai ambang secara otomatis berdasarkan distribusi intensitas gambar.

**Catatan:** Pada citra biner yang digunakan dalam tugas ini, objek atau foreground terlihat berwarna hitam, sedangkan latar belakang atau background berwarna putih. Karena itu, efek visual operasi morfologi terhadap objek hitam dapat berlawanan dengan contoh yang menggunakan foreground putih.

## 4. Kernel

Kernel atau *structuring element* adalah elemen struktur yang digunakan untuk memproses piksel beserta area tetangganya.

```python
kernel = cv2.getStructuringElement(cv2.MORPH_RECT, (5, 5))
```

Kernel yang digunakan berbentuk persegi dengan ukuran 5 × 5 piksel. Ukuran dan bentuk kernel memengaruhi perubahan yang terjadi pada citra.

## 5. Erosi (Erosion)

Erosi merupakan operasi morfologi yang mengikis atau memperkecil area aktif pada citra.

```python
erosion = cv2.erode(binary, kernel, iterations=1)
```

Dalam operasi morfologi OpenCV, area putih merupakan area aktif yang diproses. Oleh karena foreground pada citra ini berwarna hitam, erosi mengikis area putih sehingga objek hitam dapat terlihat meluas.


## 6. Dilasi (Dilation)

Dilasi merupakan operasi morfologi yang memperluas area aktif pada citra.

```python
dilation = cv2.dilate(binary, kernel, iterations=1)
```

Dilasi memperluas area putih pada citra. Akibatnya, objek hitam yang menjadi foreground dapat terlihat menyusut.


## 7. Opening

Opening merupakan gabungan operasi erosi yang dilanjutkan dengan dilasi.

```python
opening = cv2.morphologyEx(binary, cv2.MORPH_OPEN, kernel)
```

Opening biasanya digunakan untuk menghilangkan komponen kecil atau noise pada area foreground. Pada tugas ini, OpenCV memproses area putih sehingga efek terhadap objek hitam perlu diamati berdasarkan hasil citra.


## 8. Closing

Closing merupakan gabungan operasi dilasi yang dilanjutkan dengan erosi.

```python
closing = cv2.morphologyEx(binary, cv2.MORPH_CLOSE, kernel)
```

Closing biasanya digunakan untuk menutup celah kecil atau lubang pada area foreground. Pada citra tugas ini, karena area putih diproses sebagai bagian aktif, efek terhadap objek hitam dapat berbeda dari contoh dengan foreground putih.


## 9. Visualisasi Hasil

### Gambar Asli

![Gambar Asli](image/1.png)

### Grayscale

![Grayscale](image/2.png)

### Citra Biner

![Citra Biner](image/3.png)

### Erosi

![Erosi](image/4.png)

### Dilasi

![Dilasi](image/5.png)

### Opening

![Opening](image/6.png)

### Closing

![Closing](image/7.png)

## 10. Analisis Hasil

Berdasarkan hasil pengolahan citra, setiap tahapan menghasilkan perubahan yang berbeda.

1. **Grayscale:** mengubah gambar berwarna menjadi gambar dengan intensitas abu-abu.
2. **Thresholding:** mengubah grayscale menjadi citra biner menggunakan metode Otsu.
3. **Erosi:** mengikis area putih sehingga area hitam dapat terlihat meluas.
4. **Dilasi:** memperluas area putih sehingga area hitam dapat terlihat menyusut.
5. **Opening:** menjalankan erosi kemudian dilasi untuk mengubah struktur area pada citra.
6. **Closing:** menjalankan dilasi kemudian erosi untuk mengubah struktur area pada citra.

Perbedaan visual antara erosi dan dilasi terjadi karena objek pada citra biner ini berwarna hitam, sedangkan operasi morfologi OpenCV memproses area putih sebagai bagian aktif. Dengan demikian, operasi yang dilakukan tidak tertukar; perbedaan tersebut berasal dari warna foreground dan background yang digunakan.

## 11. Struktur Folder

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

## 12. Instalasi Library

Pastikan Python sudah terpasang. Kemudian instal library yang dibutuhkan menggunakan perintah berikut:

```bash
pip install opencv-python numpy
```

## 13. Cara Menjalankan Program

Jalankan program melalui terminal pada folder tugas:

```bash
python 5-morphology.py
```

Program akan menampilkan gambar asli, grayscale, citra biner, erosi, dilasi, opening, dan closing pada jendela yang terpisah.

## Kesimpulan

Morfologi citra merupakan teknik pengolahan citra untuk mengubah struktur dan bentuk area pada citra biner. Pada tugas ini, operasi erosi, dilasi, opening, dan closing dilakukan menggunakan kernel persegi berukuran 5 × 5.

Tahap grayscale dan thresholding digunakan sebagai preprocessing sebelum operasi morfologi dilakukan. Metode Otsu membantu menentukan nilai ambang secara otomatis, sedangkan operasi morfologi mengubah struktur area berdasarkan kernel yang digunakan.

Karena objek pada citra biner ini berwarna hitam dan latar belakangnya putih, hasil erosi dan dilasi terlihat berlawanan dengan contoh yang menggunakan foreground putih. Hal ini penting untuk diperhatikan ketika menganalisis hasil pengolahan citra.
