# Tugas 4 PCV

Tugas ini membahas **Model Warna pada Citra** menggunakan beberapa model warna, yaitu **RGB, CMYK, HSI, dan HSV**.

Pada tugas ini, gambar dibaca menggunakan OpenCV. Karena OpenCV membaca gambar dalam format **BGR**, gambar terlebih dahulu dikonversi menjadi **RGB**. Setelah itu, dilakukan konversi RGB ke CMYK, HSI, dan HSV secara manual menggunakan NumPy berdasarkan rumus masing-masing model warna.

Model warna yang digunakan:

* RGB (Red, Green, Blue)
* CMYK (Cyan, Magenta, Yellow, Black)
* HSI (Hue, Saturation, Intensity)
* HSV (Hue, Saturation, Value)

---

# 1. Model Warna RGB

**RGB (Red, Green, Blue)** merupakan model warna yang menggunakan tiga channel utama:

* Red (R)
* Green (G)
* Blue (B)

Setiap channel memiliki rentang nilai:

```text
0 - 255
```

Nilai dari ketiga channel tersebut menentukan warna suatu piksel.

Contoh:

```text
R = 255
G = 0
B = 0
```

menghasilkan warna merah.

Pada gambar yang digunakan dalam tugas ini, program mengambil **piksel yang berada di tengah gambar** sebagai contoh perhitungan.

Nilai piksel tersebut adalah:

```text
R = 203
G = 192
B = 190
```

Data RGB inilah yang digunakan sebagai input untuk contoh konversi ke CMYK, HSI, dan HSV pada bagian berikutnya.

---

# 2. Konversi BGR ke RGB

OpenCV secara default membaca gambar dengan urutan channel:

```text
BGR
```

Sedangkan perhitungan pada tugas ini menggunakan urutan:

```text
RGB
```

Gambar dibaca menggunakan:

```python
image_bgr = cv2.imread(image_path)
```

Kemudian dikonversi:

```python
image_rgb = cv2.cvtColor(
    image_bgr,
    cv2.COLOR_BGR2RGB
)
```

Sehingga urutan channel berubah dari:

```text
B → G → R
```

menjadi:

```text
R → G → B
```

Dengan demikian, nilai piksel tengah yang digunakan untuk perhitungan adalah:

```text
R = 203
G = 192
B = 190
```

---

# 3. Konversi RGB ke CMYK

**CMYK** terdiri dari empat komponen:

* Cyan (C)
* Magenta (M)
* Yellow (Y)
* Black (K)

Model CMYK banyak digunakan pada sistem pencetakan karena menggunakan kombinasi tinta.

Sebelum menghitung CMYK, nilai RGB terlebih dahulu dinormalisasi dari rentang `0-255` menjadi `0-1`.

## 3.1 Normalisasi RGB

Data piksel:

```text
R = 203
G = 192
B = 190
```

Rumus:

$$
R_n=\frac{R}{255}
$$

$$
G_n=\frac{G}{255}
$$

$$
B_n=\frac{B}{255}
$$

Perhitungannya:

$$
R_n=\frac{203}{255}=0.7961
$$

$$
G_n=\frac{192}{255}=0.7529
$$

$$
B_n=\frac{190}{255}=0.7451
$$

Sehingga:

```text
Rn = 0.7961
Gn = 0.7529
Bn = 0.7451
```

---

## 3.2 Menghitung Cyan

Rumus:

$$
C=1-R_n
$$

Substitusi:

$$
C=1-0.7961
$$

$$
C=0.2039
$$

Nilai ini masih merupakan nilai awal Cyan sebelum dikurangi komponen Black.

---

## 3.3 Menghitung Magenta

Rumus:

$$
M=1-G_n
$$

Substitusi:

$$
M=1-0.7529
$$

$$
M=0.2471
$$

---

## 3.4 Menghitung Yellow

Rumus:

$$
Y=1-B_n
$$

Substitusi:

$$
Y=1-0.7451
$$

$$
Y=0.2549
$$

---

## 3.5 Menghitung Black

Nilai Black diperoleh dari nilai minimum C, M, dan Y:

$$
K=\min(C,M,Y)
$$

Dengan:

```text
C = 0.2039
M = 0.2471
Y = 0.2549
```

maka:

$$
K=\min(0.2039,0.2471,0.2549)
$$

$$
K=0.2039
$$

Jika dikonversikan menjadi persen:

$$
K=0.2039\times100\%
$$

$$
K=20.39\%
$$

---

## 3.6 Menghitung C, M, dan Y Setelah K

Setelah nilai K diperoleh, nilai C, M, dan Y dihitung kembali menggunakan:

$$
C'=\frac{C-K}{1-K}
$$

$$
M'=\frac{M-K}{1-K}
$$

$$
Y'=\frac{Y-K}{1-K}
$$

### Cyan

$$
C'=
\frac{0.2039-0.2039}
{1-0.2039}
$$

$$
C'=0
$$

Sehingga:

```text
C = 0.00%
```

### Magenta

$$
M'=
\frac{0.2471-0.2039}
{1-0.2039}
$$

$$
M'\approx0.0542
$$

Sehingga:

```text
M = 5.42%
```

### Yellow

$$
Y'=
\frac{0.2549-0.2039}
{1-0.2039}
$$

$$
Y'\approx0.0640
$$

Sehingga:

```text
Y = 6.40%
```

### Hasil CMYK

Jadi, dari piksel RGB:

```text
R = 203
G = 192
B = 190
```

diperoleh:

```text
C = 0.00%
M = 5.42%
Y = 6.40%
K = 20.39%
```

---

# 4. Hasil Visualisasi CMYK

## 4.1 RGB

![RGB](image/1.png)

Gambar asli dalam model warna RGB yang digunakan sebagai input proses konversi.

---

## 4.2 CMYK - Cyan

![CMYK Cyan](image/2.png)

Visualisasi channel **Cyan (C)**.

Nilai Cyan yang lebih tinggi ditampilkan dengan intensitas grayscale yang lebih tinggi.

---

## 4.3 CMYK - Magenta

![CMYK Magenta](image/3.png)

Visualisasi channel **Magenta (M)**.

Nilai Magenta diperoleh berdasarkan kebalikan channel Green setelah dilakukan normalisasi.

---

## 4.4 CMYK - Yellow

![CMYK Yellow](image/4.png)

Visualisasi channel **Yellow (Y)**.

Nilai Yellow diperoleh berdasarkan kebalikan channel Blue setelah dilakukan normalisasi.

---

## 4.5 CMYK - Black

![CMYK Black](image/5.png)

Visualisasi channel **Black (K)**.

Nilai K diperoleh dari nilai minimum C, M, dan Y pada setiap piksel.

---

# 5. Konversi RGB ke HSI

**HSI** terdiri dari:

* Hue (H)
* Saturation (S)
* Intensity (I)

Berbeda dengan RGB, HSI memisahkan informasi warna dan tingkat intensitas sehingga jenis warna, kejenuhan, dan intensitas dapat dianalisis secara terpisah.

Contoh perhitungan berikut menggunakan piksel tengah gambar:

```text
R = 203
G = 192
B = 190
```

---

## 5.1 Intensity

Intensity menunjukkan nilai rata-rata dari R, G, dan B.

Rumus:

$$
I=\frac{R+G+B}{3}
$$

Substitusi:

$$
I=\frac{203+192+190}{3}
$$

$$
I=\frac{585}{3}
$$

$$
I=195
$$

Jadi:

```text
I = 195.00
```

---

## 5.2 Saturation

Rumus Saturation pada HSI:

$$
S=
1-
\frac{3\min(R,G,B)}
{R+G+B}
$$

Dari data:

```text
R = 203
G = 192
B = 190
```

nilai minimum:

$$
\min(203,192,190)=190
$$

Jumlah RGB:

$$
203+192+190=585
$$

Kemudian:

$$
S=
1-
\frac{3(190)}
{585}
$$

$$
S=
1-\frac{570}{585}
$$

$$
S=0.0256
$$

Untuk mendapatkan persen:

$$
S=0.0256\times100\%
$$

$$
S=2.56\%
$$

Jadi:

```text
S = 2.56%
```

Semakin kecil perbedaan nilai R, G, dan B, maka nilai Saturation semakin mendekati nol.

---

## 5.3 Hue

Hue menunjukkan jenis atau posisi warna pada lingkaran warna.

Rumus awal:

$$
\theta=
\cos^{-1}
\left(
\frac{
\frac{1}{2}[(R-G)+(R-B)]
}{
\sqrt{
(R-G)^2+(R-B)(G-B)
}
}
\right)
$$

Dengan:

```text
R = 203
G = 192
B = 190
```

Karena:

$$
G\geq B
$$

maka program menggunakan:

$$
H=\theta
$$

Setelah perhitungan dan konversi dari radian ke derajat diperoleh:

```text
H = 8.21°
```

Jadi hasil HSI untuk piksel tersebut adalah:

```text
H = 8.21°
S = 2.56%
I = 195.00
```

---

# 6. Hasil Visualisasi HSI

## 6.1 HSI - Hue

![HSI Hue](image/6.png)

Visualisasi channel **Hue** dari HSI.

Hue berada pada rentang:

```text
0° - 360°
```

Visualisasi menggunakan colormap `hsv` agar perbedaan posisi warna dapat terlihat.

---

## 6.2 HSI - Saturation

![HSI Saturation](image/7.png)

Visualisasi channel **Saturation**.

Saturation dihitung menggunakan:

$$
S=
1-
\frac{3\min(R,G,B)}
{R+G+B}
$$

Hasilnya ditampilkan menggunakan grayscale.

---

## 6.3 HSI - Intensity

![HSI Intensity](image/8.png)

Visualisasi channel **Intensity**.

Intensity dihitung menggunakan:

$$
I=\frac{R+G+B}{3}
$$

Nilai yang lebih tinggi menunjukkan intensitas yang lebih tinggi.

---

# 7. Konversi RGB ke HSV

**HSV** terdiri dari:

* Hue (H)
* Saturation (S)
* Value (V)

Contoh perhitungan menggunakan piksel tengah:

```text
R = 203
G = 192
B = 190
```

Karena HSV menggunakan RGB yang dinormalisasi, terlebih dahulu:

$$
R_n=\frac{203}{255}=0.7961
$$

$$
G_n=\frac{192}{255}=0.7529
$$

$$
B_n=\frac{190}{255}=0.7451
$$

---

## 7.1 Menentukan Maximum, Minimum, dan Delta

Nilai maksimum:

$$
MAX=\max(R_n,G_n,B_n)
$$

$$
MAX=\max(0.7961,0.7529,0.7451)
$$

$$
MAX=0.7961
$$

Nilai minimum:

$$
MIN=\min(R_n,G_n,B_n)
$$

$$
MIN=0.7451
$$

Selanjutnya:

$$
\Delta=MAX-MIN
$$

$$
\Delta=0.7961-0.7451
$$

$$
\Delta\approx0.0510
$$

---

## 7.2 Value

Value merupakan nilai maksimum dari RGB.

Rumus:

$$
V=\max(R,G,B)
$$

Karena nilai maksimum adalah:

$$
V=0.7961
$$

maka dalam persen:

$$
V=0.7961\times100\%
$$

$$
V=79.61\%
$$

Jadi:

```text
V = 79.61%
```

---

## 7.3 Saturation

Rumus:

$$
S=\frac{\Delta}{MAX}
$$

Substitusi:

$$
S=
\frac{0.0510}{0.7961}
$$

$$
S\approx0.0640
$$

Dalam persen:

$$
S=0.0640\times100\%
$$

$$
S=6.40\%
$$

Jadi:

```text
S = 6.40%
```

---

## 7.4 Hue

Karena nilai maksimum adalah channel Red:

$$
R=MAX
$$

maka rumus Hue yang digunakan adalah:

$$
H=
60
\left(
\frac{G-B}{\Delta}
\right)
$$

Dengan:

```text
G = 0.7529
B = 0.7451
Delta ≈ 0.0510
```

maka:

$$
H=
60
\left(
\frac{0.7529-0.7451}
{0.0510}
\right)
$$

Hasil perhitungan memberikan:

```text
H = 9.23°
```

Karena hasil Hue tidak negatif, tidak diperlukan penambahan 360°.

Jadi hasil HSV untuk piksel tersebut adalah:

```text
H = 9.23°
S = 6.40%
V = 79.61%
```

---

# 8. Hasil Visualisasi HSV

## 8.1 HSV - Hue

![HSV Hue](image/9.png)

Visualisasi channel **Hue** dari model HSV.

Hue berada pada rentang:

```text
0° - 360°
```

Visualisasi menggunakan colormap `hsv`.

---

## 8.2 HSV - Saturation

![HSV Saturation](image/10.png)

Visualisasi channel **Saturation** dari HSV.

Rumus:

$$
S=\frac{\Delta}{MAX}
$$

Semakin tinggi nilai Saturation, semakin kuat atau jenuh warna tersebut.

---

## 8.3 HSV - Value

![HSV Value](image/11.png)

Visualisasi channel **Value** dari HSV.

Rumus:

$$
V=\max(R,G,B)
$$

Value menunjukkan tingkat kecerahan berdasarkan nilai channel RGB terbesar.

---

# 9. Ringkasan Contoh Konversi Piksel

Program mengambil piksel tengah gambar sebagai contoh perhitungan.

Data awal:

```text
RGB
R = 203
G = 192
B = 190
```

Setelah dilakukan konversi diperoleh:

```text
CMYK
C = 0.00%
M = 5.42%
Y = 6.40%
K = 20.39%
```

```text
HSI
H = 8.21°
S = 2.56%
I = 195.00
```

```text
HSV
H = 9.23°
S = 6.40%
V = 79.61%
```

Ringkasannya:

| Model | Komponen |  Hasil |
| ----- | -------- | -----: |
| RGB   | R        |    203 |
| RGB   | G        |    192 |
| RGB   | B        |    190 |
| CMYK  | C        |  0.00% |
| CMYK  | M        |  5.42% |
| CMYK  | Y        |  6.40% |
| CMYK  | K        | 20.39% |
| HSI   | H        |  8.21° |
| HSI   | S        |  2.56% |
| HSI   | I        | 195.00 |
| HSV   | H        |  9.23° |
| HSV   | S        |  6.40% |
| HSV   | V        | 79.61% |

Contoh tersebut menunjukkan bagaimana **satu piksel RGB yang sama dapat direpresentasikan dalam tiga model warna yang berbeda**.

---

# 10. Perbedaan Model Warna

| Model Warna | Komponen                     | Karakteristik                              |
| ----------- | ---------------------------- | ------------------------------------------ |
| RGB         | Red, Green, Blue             | Representasi warna berbasis cahaya         |
| CMYK        | Cyan, Magenta, Yellow, Black | Representasi warna berbasis tinta          |
| HSI         | Hue, Saturation, Intensity   | Memisahkan warna dan intensitas            |
| HSV         | Hue, Saturation, Value       | Memisahkan warna, kejenuhan, dan kecerahan |

### RGB

```text
R + G + B
```

Digunakan untuk merepresentasikan warna pada perangkat seperti monitor dan kamera.

### CMYK

```text
C + M + Y + K
```

Lebih sesuai untuk sistem pencetakan yang menggunakan tinta.

### HSI

```text
H + S + I
```

Memisahkan informasi warna dari intensitas.

### HSV

```text
H + S + V
```

Memisahkan jenis warna, kejenuhan, dan nilai kecerahan.

---

# 11. Urutan Hasil Program

Program menampilkan hasil visualisasi satu per satu dalam urutan:

```text
1.png  → RGB
2.png  → CMYK - Cyan
3.png  → CMYK - Magenta
4.png  → CMYK - Yellow
5.png  → CMYK - Black
6.png  → HSI - Hue
7.png  → HSI - Saturation
8.png  → HSI - Intensity
9.png  → HSV - Hue
10.png → HSV - Saturation
11.png → HSV - Value
```

Setiap hasil ditampilkan menggunakan jendela Matplotlib secara bergantian. Jendela harus ditutup terlebih dahulu untuk melanjutkan ke hasil berikutnya.

Struktur folder:

```text
Tugas4/
├── README.md
├── tugas4.py
├── rb.png
└── image/
    ├── 1.png
    ├── 2.png
    ├── 3.png
    ├── 4.png
    ├── 5.png
    ├── 6.png
    ├── 7.png
    ├── 8.png
    ├── 9.png
    ├── 10.png
    └── 11.png
```

---

# 12. Kesimpulan

Pada tugas ini dilakukan konversi model warna dari **RGB ke CMYK, HSI, dan HSV** secara manual menggunakan NumPy.

Proses dimulai dengan membaca gambar menggunakan OpenCV. Karena OpenCV menggunakan format BGR, gambar dikonversi terlebih dahulu menjadi RGB sebelum digunakan dalam proses perhitungan.

Pada model **CMYK**, nilai RGB dinormalisasi terlebih dahulu, kemudian dihitung nilai Cyan, Magenta, Yellow, dan Black. Untuk contoh piksel dengan nilai:

```text
R = 203
G = 192
B = 190
```

diperoleh:

```text
C = 0.00%
M = 5.42%
Y = 6.40%
K = 20.39%
```

Pada model **HSI**, nilai RGB digunakan untuk menghitung Hue, Saturation, dan Intensity. Dari piksel yang sama diperoleh:

```text
H = 8.21°
S = 2.56%
I = 195.00
```

Sedangkan pada model **HSV**, nilai maksimum, minimum, dan selisih RGB digunakan untuk menghitung Hue, Saturation, dan Value. Hasilnya:

```text
H = 9.23°
S = 6.40%
V = 79.61%
```

Dari tugas ini dapat dilihat bahwa **satu piksel RGB dapat direpresentasikan dengan cara yang berbeda pada setiap model warna**. RGB berfokus pada tiga komponen warna dasar, CMYK menggunakan pendekatan warna berbasis tinta, sedangkan HSI dan HSV memisahkan informasi warna dari tingkat kejenuhan dan intensitas atau kecerahan.

Dengan demikian, setiap model warna memiliki karakteristik dan penggunaan yang berbeda dalam pengolahan citra.
