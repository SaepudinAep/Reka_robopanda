# Robopanda - REKA:BIT Extended Library for micro:bit

Library ekstensi MakeCode untuk papan **Cytron REKA:BIT** yang telah ditingkatkan (*extended*) untuk mendukung kontrol **4 Motor DC (M1, M2, M3, M4)**, kontrol **4 Servo**, **RGB LED bawaan**, serta pembacaan **sensor analog aman**.

---

## 🌟 Fitur Utama

- **4-Channel DC Motor Driver:**
  - **M1 & M2:** Dikendalikan melalui chip *coprocessor* bawaan REKA:BIT via protokol **I2C**.
  - **M3 & M4:** Dikendalikan secara langsung melalui pin PWM micro:bit dengan frekuensi **5 kHz** dan pengaturan kecepatan halus (**0 – 255**).
  - **Sistem Pengereman Aman (*Coast Brake*):** Logika rem menggunakan status pin `0 0` (*LOW, LOW*) untuk mencegah lonjakan arus dan *short-circuit*.
- **4x Servo Controller (S1, S2, S3, S4, All):** Pengaturan sudut derajat 0° – 180° dengan pembulatan integer presisi untuk I2C.
- **2x Onboard NeoPixel RGB LED:** Kontrol warna dan kecerahan pada pin P8.
- **Blok Pembaca Sensor Aman (*Safe Analog Read*):** Pembacaan sensor analog khusus pada pin yang aman (P0, P1, P2) untuk mencegah bentrok dengan motor dan I2C.

---

## 🔌 Pemetaan Pin Perangkat Keras (*Hardware Pinout*)

| Komponen / Port | Pin micro:bit | Jalur Komunikasi | Keterangan |
|---|---|---|---|
| **Motor M1** | Internal | I2C (PIC16F1827) | Penggerak motor bawaan REKA:BIT |
| **Motor M2** | Internal | I2C (PIC16F1827) | Penggerak motor bawaan REKA:BIT |
| **Motor M3** | **P14** (IN1), **P13** (IN2) | PWM (5 kHz) | Port motor tambahan dengan kontrol speed |
| **Motor M4** | **P16** (IN1), **P15** (IN2) | PWM (5 kHz) | Port motor tambahan dengan kontrol speed |
| **Servo S1 - S4** | Port Servo 1 - 4 | I2C (PIC16F1827) | 5V Servo Header |
| **RGB LED** | **P8** | Digital (NeoPixel) | 2x WS2812B LED di papan REKA:BIT |
| **I2C Bus** | **P19** (SCL), **P20** (SDA) | I2C | Komunikasi chip coprocessor REKA:BIT |
| **Sensor Port Aman** | **P0, P1, P2** | Analog / Digital Read | Rekomendasi utama untuk sensor garis/jarak |

> [!IMPORTANT]
> Jangan menggunakan pin **P13, P14, P15, dan P16** untuk sensor karena sudah dialokasikan untuk jalur sinyal penggerak Motor M3 dan M4.

---

## 📦 Cara Memasang Ekstensi di MakeCode

1. Buka [Microsoft MakeCode for micro:bit](https://makecode.microbit.org/).
2. Buat proyek baru atau buka proyek yang sudah ada.
3. Klik menu **Extensions** (atau ikon roda gigi ⚙️ -> *Extensions*).
4. Masukkan URL repositori GitHub ini pada kolom pencarian:
   ```text
   https://github.com/SaepudinAep/bkp_Robopanda_library
   ```
5. Klik pada hasil pencarian untuk menambahkan ekstensi ini ke lembar kerja Anda.

---

## 💻 Contoh Penggunaan Blok (*Code Examples*)

### 1. Kontrol Motor M1 & M2 (Bawaan REKA:BIT)
Menjalankan Motor 1 maju dengan kecepatan 50% saat tombol A ditekan, dan mengerem saat tombol B ditekan:

```blocks
input.onButtonPressed(Button.A, function () {
    rekabit.runMotor(MotorChannel.M1, MotorDirection.Forward, 128)
})
input.onButtonPressed(Button.B, function () {
    rekabit.brakeMotor(MotorChannel.M1)
})
```

### 2. Kontrol Motor M3 & M4 (Port Ekstensi dengan Speed)
Menjalankan Motor M3 maju dan Motor M4 mundur dengan kecepatan penuh, lalu melakukan pengereman aman:

```blocks
input.onButtonPressed(Button.A, function () {
    rekabit.runMotor34(rekabit.MotorChannelM3M4.M3, MotorDirection.Forward, 255)
    rekabit.runMotor34(rekabit.MotorChannelM3M4.M4, MotorDirection.Backward, 255)
})
input.onButtonPressed(Button.B, function () {
    rekabit.brakeMotor34(rekabit.MotorChannelM3M4.M3)
    rekabit.brakeMotor34(rekabit.MotorChannelM3M4.M4)
})
```

### 3. Kontrol Servo
Menggerakkan Servo 1 ke posisi 0° dan 180°, atau menonaktifkannya:

```blocks
input.onButtonPressed(Button.A, function () {
    rekabit.setServoPosition(ServoChannel.S1, 0)
})
input.onButtonPressed(Button.B, function () {
    rekabit.setServoPosition(ServoChannel.S1, 180)
})
input.onButtonPressed(Button.AB, function () {
    rekabit.disableServo(ServoChannel.S1)
})
```

### 4. Pembacaan Sensor Analog Aman (*Safe Analog Read*)
Membaca nilai sensor garis atau sensor cahaya pada pin P0 (nilai 0 – 1023):

```blocks
basic.forever(function () {
    let sensorValue = rekabit.readAnalogPin(rekabit.RekabitAnalogPin.P0)
    if (sensorValue < 500) {
        // Garis gelap terdeteksi
        rekabit.runMotor(MotorChannel.All, MotorDirection.Forward, 100)
    } else {
        // Area terang
        rekabit.brakeMotor(MotorChannel.All)
    }
})
```

### 5. Lampu RGB (NeoPixel)
Mengatur warna lampu RGB di papan REKA:BIT:

```blocks
// Nyalakan semua LED dengan warna hijau
rekabit.setAllRgbPixelsColor(0x00ff00)
basic.pause(1000)

// Ubah LED 0 jadi merah dan LED 1 jadi biru
rekabit.setRgbPixelColor(0, 0xff0000)
rekabit.setRgbPixelColor(1, 0x0000ff)
basic.pause(1000)

// Matikan semua LED
rekabit.clearAllRgbPixels()
```

---

## 🛠️ Kompatibilitas Target
* **micro:bit V1 & micro:bit V2**
* **Microsoft MakeCode for micro:bit (PXT)**

---

## 📄 Lisensi
[MIT License](LICENSE)
