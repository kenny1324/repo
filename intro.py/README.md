Project Intro — Pengolahan Citra dan Video

Repository ini berisi implementasi dasar Pengolahan Citra dan Video menggunakan Python dan OpenCV.

Project ini mencakup beberapa proses dasar, yaitu:

Membaca gambar (read image)
Menampilkan gambar (show image)
Mengubah gambar menjadi grayscale
Melakukan filter warna pada gambar
Mengubah warna gambar secara dinamis menggunakan HSV
Melakukan filter warna pada video secara real-time menggunakan webcam
1. Read Image

Pada bagian ini, program digunakan untuk membaca gambar menggunakan fungsi cv2.imread().

import cv2

image = cv2.imread("rb.png")

if image is None:
    print("Gambar tidak ditemukan!")
    exit()

Fungsi cv2.imread() digunakan untuk membaca file gambar rb.png dan menyimpannya ke dalam variabel image.

Kemudian dilakukan pengecekan menggunakan:

if image is None:

Jika gambar tidak ditemukan atau gagal dibaca, program akan menampilkan pesan "Gambar tidak ditemukan!" dan menghentikan program.

2. Show Image

Setelah gambar berhasil dibaca, gambar dapat ditampilkan menggunakan fungsi cv2.imshow().

cv2.imshow("Gambar Asli", image)

Kode tersebut menampilkan gambar asli dengan judul "Gambar Asli".

Pada program utama, beberapa hasil pengolahan gambar juga ditampilkan secara bersamaan:

cv2.imshow("Gambar Asli", image)
cv2.imshow("Grayscale", gray)
cv2.imshow("Green", green)
cv2.imshow("Pink", pink)

Dengan demikian, dapat dibandingkan antara gambar asli dan gambar yang telah diproses.

3. Grayscale Image

Gambar asli kemudian diubah menjadi grayscale menggunakan:

gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

cv2.cvtColor() digunakan untuk mengubah ruang warna gambar.

Pada kode tersebut:

cv2.COLOR_BGR2GRAY

digunakan untuk mengubah gambar dari BGR menjadi grayscale.

Hasilnya adalah gambar yang hanya memiliki tingkat keabuan, dari hitam hingga putih.

Hasil Grayscale




4. Filter Color Image

Pada bagian ini dilakukan manipulasi terhadap channel warna pada gambar.

OpenCV membaca gambar dalam urutan channel:

BGR

yaitu:

B = Blue
G = Green
R = Red
Filter Green

Kode berikut membuat salinan gambar kemudian menghilangkan channel Blue dan Red:

green = image.copy()

green[:, :, 0] = 0
green[:, :, 2] = 0

Karena OpenCV menggunakan urutan BGR, maka:

green[:, :, 0] → Blue
green[:, :, 1] → Green
green[:, :, 2] → Red

Dengan mengubah channel Blue dan Red menjadi 0, warna yang tersisa pada gambar terutama berasal dari channel Green.

Hasil Filter Green




Filter Pink

Untuk membuat filter pink, channel Green dihilangkan:

pink = image.copy()

pink[:, :, 1] = 0

Channel Blue dan Red tetap dipertahankan, sedangkan channel Green dibuat menjadi 0.

Kombinasi Red dan Blue menghasilkan warna yang cenderung magenta/pink, tergantung warna asli pada setiap piksel gambar.

Hasil Filter Pink




5. Perubahan Warna Menggunakan HSV

Selain melakukan filter dengan menghilangkan channel BGR, program juga memiliki fitur tambahan untuk mengubah warna gambar secara dinamis.

Kode yang digunakan:

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

Pada bagian ini, gambar terlebih dahulu dikonversi dari BGR menjadi HSV:

hsv = cv2.cvtColor(image, cv2.COLOR_BGR2HSV)

HSV terdiri dari:

H (Hue) → menentukan jenis warna
S (Saturation) → menentukan tingkat kejenuhan warna
V (Value) → menentukan tingkat kecerahan

Program kemudian mengubah nilai Hue:

hsv[:, :, 0] = hue

Nilai Hue terus bertambah:

hue += 1

dan ketika mencapai 180, nilainya kembali menjadi 0.

Akibatnya, warna pada gambar berubah secara terus-menerus sehingga menghasilkan efek colorful/rainbow.

Hasil Perubahan Warna




Hasilnya adalah gambar yang warna keseluruhannya berubah secara dinamis berdasarkan perubahan nilai Hue.

6. Filter Color Video — Webcam

Selain gambar, program juga melakukan filter warna pada video secara real-time menggunakan webcam laptop.

Video dari webcam sebenarnya terdiri dari banyak gambar (frame) yang ditampilkan secara berurutan dengan cepat.

Kode yang digunakan:

import cv2
import numpy as np

cap = cv2.VideoCapture(0)

while True:
    ret, frame = cap.read()

    if not ret:
        print("Kamera tidak terbaca")
        break

    green = frame.copy()

    green[:, :, 0] = green[:, :, 0] * 0.3

    green[:, :, 1] = np.minimum(green[:, :, 1] * 1 , 255)

    green[:, :, 2] = green[:, :, 2] * 0.3

    cv2.imshow("Kamera Asli", frame)
    cv2.imshow("Filter Hijau", green)

    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()
Penjelasan

Webcam dibuka menggunakan:

cap = cv2.VideoCapture(0)

Angka 0 menunjukkan kamera utama/default yang tersedia pada komputer.

Kemudian program mengambil frame secara terus-menerus:

ret, frame = cap.read()

Setiap frame merupakan satu gambar yang berasal dari webcam.

Frame tersebut kemudian disalin:

green = frame.copy()

Selanjutnya dilakukan pengaturan intensitas masing-masing channel warna.

Channel Blue dikurangi:

green[:, :, 0] = green[:, :, 0] * 0.3

Channel Green dipertahankan:

green[:, :, 1] = np.minimum(green[:, :, 1] * 1, 255)

Channel Red juga dikurangi:

green[:, :, 2] = green[:, :, 2] * 0.3

Karena channel Blue dan Red dikurangi, sedangkan channel Green dipertahankan, hasil frame terlihat lebih dominan berwarna hijau.

Program kemudian menampilkan dua video secara bersamaan:

cv2.imshow("Kamera Asli", frame)
cv2.imshow("Filter Hijau", green)

Sehingga dapat dibandingkan antara kamera asli dan hasil filter hijau secara real-time.

Untuk menghentikan program, tombol:

Q

dapat ditekan.

Hasil Webcam Asli




Hasil Filter Hijau Webcam




Kesimpulan

Pada project ini telah dilakukan beberapa operasi dasar pengolahan citra dan video menggunakan OpenCV, yaitu membaca dan menampilkan gambar, mengubah gambar menjadi grayscale, melakukan manipulasi channel warna BGR, mengubah warna menggunakan HSV, serta menerapkan filter warna pada video secara real-time menggunakan webcam.

Dari project ini dapat dipahami bahwa gambar digital tersusun dari piksel dan setiap piksel memiliki nilai warna. Dengan memanipulasi nilai channel warna tersebut, tampilan gambar maupun video dapat diubah sesuai dengan filter yang diinginkan.
