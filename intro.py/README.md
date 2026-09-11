# Dokumentasi

Project ini merupakan implementasi dasar pengolahan citra dan video menggunakan **Python dan OpenCV**.

Project ini mencakup beberapa proses, yaitu:

- Read Image
- Show Image
- Filter Color Image
- Perubahan Warna Menggunakan HSV
- Filter Color Video menggunakan Webcam

---

## 1. Read Image

Pada bagian ini, program membaca gambar menggunakan fungsi `cv2.imread()`.

Gambar yang digunakan adalah `rb.png`.

---

## 2. Show Image

Setelah gambar berhasil dibaca, program menampilkan gambar asli menggunakan `cv2.imshow()`.

---

## 3. Filter Color Image

Pada bagian ini dilakukan beberapa pengolahan warna pada gambar, yaitu:

- Grayscale
- Filter Green
- Filter Pink

OpenCV menggunakan format warna **BGR**, yaitu:

- Channel 0 = Blue
- Channel 1 = Green
- Channel 2 = Red

Pada filter Green, channel Blue dan Red dihilangkan sehingga warna Green menjadi lebih dominan.

Sedangkan pada filter Pink, channel Green dihilangkan sehingga kombinasi channel Red dan Blue menghasilkan warna yang cenderung pink atau magenta.

---

## 4. Perubahan Warna Menggunakan HSV

Program juga memiliki fitur tambahan untuk mengubah warna gambar secara dinamis menggunakan ruang warna **HSV**.

HSV terdiri dari:

- **H (Hue)** → menentukan jenis warna
- **S (Saturation)** → menentukan tingkat kejenuhan warna
- **V (Value)** → menentukan tingkat kecerahan

Nilai Hue diubah secara terus-menerus dari `0` sampai `179`, kemudian kembali ke `0`. Hal ini menyebabkan warna gambar berubah secara dinamis dan menghasilkan efek colorful/rainbow.

---

## 5. Filter Color Video

Pada bagian ini dilakukan filter warna pada **video secara real-time menggunakan webcam laptop**.

Setiap frame yang diperoleh dari webcam diproses menggunakan filter warna hijau.

Channel Blue dan Red dikurangi menjadi 30% dari nilai aslinya, sedangkan channel Green dipertahankan. Dengan demikian, hasil video menjadi lebih dominan berwarna hijau tanpa membuat seluruh piksel menjadi satu warna hijau yang sama.

---

## 6. Source Code

Berikut adalah source code lengkap `1-Intro.py` yang digunakan pada project ini.

```python
import cv2
import numpy as np


# READ IMAGE

image = cv2.imread("rb.png")

if image is None:
    print("Gambar tidak ditemukan!")
    exit()


# FILTER COLOR IMAGE

# Grayscale
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)


# Filter Green
green = image.copy()

green[:, :, 0] = 0
green[:, :, 2] = 0


# Filter Pink
pink = image.copy()

pink[:, :, 1] = 0


# SHOW IMAGE

cv2.imshow("Gambar Asli", image)
cv2.imshow("Grayscale", gray)
cv2.imshow("Green", green)
cv2.imshow("Pink", pink)


# PERUBAHAN WARNA MENGGUNAKAN HSV

hue = 0

while True:

    hsv = cv2.cvtColor(image, cv2.COLOR_BGR2HSV)

    hsv[:, :, 0] = hue

    colorful = cv2.cvtColor(hsv, cv2.COLOR_HSV2BGR)

    cv2.imshow("Colorful", colorful)

    hue += 1

    if hue >= 180:
        hue = 0

    if cv2.waitKey(30) == 27:
        break


cv2.destroyAllWindows()


# FILTER COLOR VIDEO - WEBCAM

cap = cv2.VideoCapture(0)

while True:

    ret, frame = cap.read()

    if not ret:
        print("Kamera tidak terbaca")
        break

    green = frame.copy()

    # Mengurangi channel Blue
    green[:, :, 0] = green[:, :, 0] * 0.3

    # Mempertahankan channel Green
    green[:, :, 1] = np.minimum(
        green[:, :, 1] * 1,
        255
    )

    # Mengurangi channel Red
    green[:, :, 2] = green[:, :, 2] * 0.3

    cv2.imshow("Kamera Asli", frame)
    cv2.imshow("Filter Hijau", green)

    # Tekan Q untuk keluar
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

```
---
## 7. Hasil Filter Color Image

### Gambar Asli

![Gambar Asli](image/gambar_asli.png)

### Grayscale

![Grayscale](image/grayscale.png)

### Filter Green

![Filter Green](image/filter_green.png)

### Filter Pink

![Filter Pink](image/filter_pink.png)

---

## 8. Hasil Perubahan Warna Menggunakan HSV

### Colorful Image

![Colorful Image](image/colorful.png)

Hasil menunjukkan perubahan warna gambar secara dinamis dengan mengubah nilai Hue pada ruang warna HSV.

---

## 9. Hasil Filter Color Video

### Kamera Asli

![Kamera Asli](image/kamera_asli.png)

### Filter Hijau

![Filter Hijau](image/filter_hijau.png)

Hasil menunjukkan bahwa filter warna dapat diterapkan pada video secara real-time menggunakan webcam. Kamera asli digunakan sebagai pembanding, sedangkan tampilan filter hijau merupakan hasil manipulasi channel warna pada setiap frame video.

