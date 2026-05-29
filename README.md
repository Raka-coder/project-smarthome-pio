<div align="center">

# 🏠 Smart Home IoT - Lampu Pintar

**Sistem Kendali Lampu Pintar Berbasis IoT dengan Wemos D1 Mini, Sinric Pro, dan Google Home**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PlatformIO](https://img.shields.io/badge/build%20with-PlatformIO-orange?logo=platformio)](https://platformio.org/)
[![Arduino](https://img.shields.io/badge/Framework-Arduino-blue)](https://www.arduino.cc/)
[![ESP8266](https://img.shields.io/badge/Platform-ESP8266-orange)](https://www.espressif.com/en/products/socs/esp8266)

[![SinricPro](https://www.ardu-badge.com/badge/SinricPro.svg?)](https://www.arduino.cc/reference/en/libraries/sinricpro)
[![Build](https://github.com/sinricpro/esp8266-esp32-sdk/actions/workflows/build-esp8266-esp32.yml/badge.svg)](https://github.com/sinricpro/esp8266-esp32-sdk/actions/workflows/build-esp8266-esp32.yml)

</div>

---

## 📋 Daftar Isi

- [Deskripsi](#deskripsi)
- [Fitur](#fitur)
- [Arsitektur Sistem](#arsitektur-sistem)
- [Komponen Hardware](#komponen-hardware)
- [Komponen Software](#komponen-software)
- [Wiring Diagram](#wiring-diagram)
- [Konfigurasi](#konfigurasi)
- [Instalasi](#instalasi)
- [Penggunaan](#penggunaan)
- [Troubleshooting](#troubleshooting)
- [License](#license)

---

## Deskripsi

Projek ini adalah implementasi **Internet of Things (IoT)** pada sistem kendali lampu pintar menggunakan Wemos D1 Mini sebagai mikrokontroler utama. Sistem ini memungkinkan pengguna untuk mengontrol lampu secara remote melalui:

- **Google Home** / **Google Assistant** (kontrol suara)
- **Aplikasi Sinric Pro** (kontrol manual via smartphone)
- **Tombol fisik** (kontrol manual langsung)

Sistem bekerja dengan cara menerima perintah dari Google Home/Sinric Pro, memprosesnya di cloud, lalu mengirimkan sinyal kontrol ke relay untuk menyalakan/mematikan lampu. LCD I2C 16x2 menampilkan status sistem secara real-time.

---

## Fitur

| Fitur | Deskripsi |
|-------|-----------|
| 🔊 **Voice Control** | Kontrol lampu menggunakan suara via Google Assistant |
| 📱 **Remote Control** | Kontrol dari mana saja melalui aplikasi Sinric Pro |
| 🔘 **Manual Control** | Tombol fisik untuk kontrol langsung tanpa internet |
| 📟 **LCD Display** | Status sistem ditampilkan pada LCD 16x2 |
| 🔄 **Auto Reconnect** | WiFi otomatis reconnect jika koneksi terputus |
| 💾 **State Persistence** | Status lampu tersimpan dan dipulihkan saat restart |
| 🔌 **Multi-Device** | Mendukung multiple relay/switch dalam satu modul |

---

## Arsitektur Sistem

```
┌─────────────┐     ┌──────────────┐     ┌─────────────┐     ┌─────────────┐
│  Google Home │────▶│  Sinric Pro  │────▶│  Wemos D1   │────▶│   Relay     │
│  Assistant   │     │    Cloud     │     │    Mini     │     │  + Lampu    │
└─────────────┘     └──────────────┘     └─────────────┘     └─────────────┘
                                                │
                                                ▼
                                         ┌─────────────┐
                                         │  LCD I2C    │
                                         │   16x2      │
                                         └─────────────┘

                        ┌─────────────┐
                        │  Flip Switch │
                        │  (Tombol)   │
                        └──────┬──────┘
                               │
                               ▼
                        ┌─────────────┐
                        │  Wemos D1   │──── Relay ──── Lampu
                        │    Mini     │
                        └─────────────┘
```

---

## Komponen Hardware

| No | Komponen | Spesifikasi | Qty |
|----|----------|-------------|-----|
| 1 | Wemos D1 Mini | ESP8266 WiFi Microcontroller | 1 |
| 2 | Relay Module | 5V 2-Channel | 1 |
| 3 | LCD I2C | 16x2 Character Display | 1 |
| 4 | Breadboard | 830 point | 1 |
| 5 | Kabel Jumper | Male-to-Male / Male-to-Female | ~15 |
| 6 | Kabel USB | Micro USB | 1 |
| 7 | Adaptor USB | Charger 5V 1A | 1 |
| 8 | Fitting Lampu | E27 Socket | 2 |
| 9 | Lampu LED | 220V LED Bulb | 2 |
| 10 | Steker | AC Power Plug | 1 |
| 11 | Box Project | 3D Print Custom | 1 |

---

## Komponen Software

| Software | Keterangan | Link |
|----------|------------|------|
| VS Code | Code Editor | [Download](https://code.visualstudio.com/) |
| PlatformIO | Build System | [Install](https://platformio.org/install/ide?install=vscode) |
| Sinric Pro | IoT Platform | [Register](https://sinric.pro/) |
| Google Home | Voice Assistant | [Download](https://home.google.com/) |

### Library Dependencies

- **SinricPro** - IoT cloud platform library
- **LiquidCrystal_I2C** - LCD I2C communication library
- **ESP8266WiFi** - WiFi library (built-in)

---

## Wiring Diagram

### Pin Mapping

| Wemos D1 Mini | Relay | LCD I2C | Flip Switch |
|---------------|-------|---------|-------------|
| D1 (GPIO5) | - | SDA | - |
| D2 (GPIO4) | - | SCL | - |
| D5 (GPIO14) | Relay 2 IN | - | Switch 2 |
| D6 (GPIO16) | Relay 1 IN | - | Switch 1 |
| 5V | VCC | VCC | - |
| GND | GND | GND | GND |

### Koneksi Relay

```
Relay 1 (Lampu 1):
  - IN  → D6 (GPIO16)
  - VCC → 5V
  - GND → GND
  - COM → AC Live (Steker)
  - NO  → Lampu 1

Relay 2 (Lampu 2):
  - IN  → D5 (GPIO14)
  - VCC → 5V
  - GND → GND
  - COM → AC Live (Steker)
  - NO  → Lampu 2
```

### Koneksi LCD I2C

```
LCD I2C 16x2:
  - SDA → D1 (GPIO5)
  - SCL → D2 (GPIO4)
  - VCC → 5V
  - GND → GND
  - I2C Address: 0x27
```

> ⚠️ **PERINGATAN:** Hati-hati saat bekerja dengan listrik AC 220V. Pastikan semua koneksi terisolasi dengan baik. Jika tidak yakin, konsultasikan dengan teknisi listrik.

---

## Konfigurasi

### 1. Buat Akun Sinric Pro

1. Kunjungi [https://sinric.pro](https://sinric.pro/)
2. Daftar akun baru
3. Login ke dashboard
4. Buat Device baru (tipe: Switch)
5. Catat **APP_KEY** dan **APP_SECRET**

### 2. Buat Akun Google Home

1. Download aplikasi Google Home
2. Login dengan akun Google
3. Hubungkan Sinric Pro ke Google Home:
   - Buka Google Home App
   - Tap **+** → **Set up device** → **Works with Google**
   - Cari **Sinric Pro**
   - Login dengan akun Sinric Pro

### 3. Konfigurasi WiFi

Edit file `src/main.cpp` dan ganti nilai berikut:

```cpp
#define WIFI_SSID     "Nama_WiFi_Anda"
#define WIFI_PASS     "Password_WiFi_Anda"
```

### 4. Konfigurasi Sinric Pro

```cpp
#define APP_KEY       "your-app-key-here"
#define APP_SECRET    "your-app-secret-here"
```

### 5. Konfigurasi Device ID

Daftarkan device ID dari dashboard Sinric Pro:

```cpp
std::map<String, deviceConfig_t> devices = {
    //{deviceId, {relayPIN, flipSwitchPIN, activeLow}}
    {"your-device-id-1", {16, 0, true}},  // Relay 1
    {"your-device-id-2", {14, 4, true}},  // Relay 2
};
```

### 6. Konfigurasi Pin

```cpp
#define RELAYPIN_1  16   // GPIO16 = D6
#define RELAYPIN_2  14   // GPIO14 = D5
```

---

## Instalasi

### Prerequisites

- [Node.js](https://nodejs.org/) terinstall
- [VS Code](https://code.visualstudio.com/) terinstall
- [PlatformIO Extension](https://platformio.org/install/ide?install=vscode) terinstall di VS Code

### Langkah Instalasi

1. **Clone repository:**

   ```bash
   git clone https://github.com/username/project-smarthome-pio.git
   cd project-smarthome-pio
   ```

2. **Buka di VS Code:**

   ```bash
   code .
   ```

3. **Install dependencies** (otomatis oleh PlatformIO saat build pertama)

4. **Konfigurasi** sesuai panduan di atas

5. **Upload firmware:**

   ```bash
   # Via PlatformIO CLI
   pio run -t upload

   # Atau via VS Code
   # Klik tombol Upload (→) di taskbar PlatformIO
   ```

6. **Monitor serial output:**

   ```bash
   pio device monitor
   ```

---

## Penggunaan

### Via Google Home / Google Assistant

1. Pastikan WiFi terhubung
2. Gunakan perintah suara:
   - *"Hey Google, nyalakan lampu satu"*
   - *"Hey Google, matikan lampu dua"*
   - *"Hey Google, nyalakan semua lampu"*

### Via Aplikasi Sinric Pro

1. Buka aplikasi Sinric Pro
2. Pilih device yang ingin dikontrol
3. Tap tombol ON/OFF

### Via Tombol Fisik

1. Tekan tombol flip switch pada breadboard
2. Lampu akan toggle (ON↔OFF)
3. Status ter-update di Sinric Pro dan Google Home

---

## Troubleshooting

| Masalah | Solusi |
|---------|--------|
| WiFi tidak connect | Cek SSID dan password. Pastikan WiFi 2.4GHz |
| LCD tidak menyala | Cek koneksi I2C. Coba address `0x3F` atau gunakan I2C scanner |
| Relay tidak aktif | Cek wiring. Pastikan pin GPIO sesuai konfigurasi |
| Google Home tidak merespon | Pastikan Sinric Pro sudah di-link ke Google Home |
| Device offline di Sinric Pro | Cek koneksi internet. Restart Wemos D1 Mini |
| State tidak tersimpan | Pastikan `SinricPro.restoreDeviceStates(true)` terpanggil |

### Debug Mode

Untuk mengaktifkan debug output, pastikan baris berikut ada di `main.cpp`:

```cpp
#define ENABLE_DEBUG
```

Lalu buka Serial Monitor dengan baud rate `115200`.

---

## License

MIT License - Copyright (c) 2024 Raka

Lihat file [LICENSE](LICENSE) untuk informasi lebih lanjut.