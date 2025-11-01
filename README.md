# C9MagangBanyubramanta_Hadryan-Rizky-Dimas-Saputra

# [Tugas Magang 1 - Controller]

Repositori ini berisi pengumpulan tugas untuk paket ROS 2 `controller` dan `interfaces`.

Proyek ini terdiri dari dua paket utama:
1.  **`interfaces`**: Berisi *custom message* `ControllerCommand.msg` yang digunakan untuk komunikasi antara *node*.
2.  **`controller`**: Berisi *node* `controller_node` yang menerjemahkan input dari *joystick* ke topik `/cmd_vel`.

---

## 🚀 Fungsionalitas Node Controller

`controller_node` membaca data mentah dari topik `/joy` (dari *package* `joy`) dan mempublikasikan data yang telah diproses ke topik `/cmd_vel` menggunakan *message* kustom `interfaces/msg/ControllerCommand`.

### Pemetaan Kontrol (Xbox 360 Controller)

* **`x_cmd` (Maju/Mundur)**
    * **Kontrol**: Stik Kiri (Vertikal)
    * **Logika**: *Stateless* (langsung merespons, kembali ke 0 saat dilepas).

* **`y_cmd` (Geser Kiri/Kanan)**
    * **Kontrol**: Stik Kiri (Horizontal)
    * **Logika**: *Stateless* (langsung merespons, kembali ke 0 saat dilepas).

* **`depth` (Kedalaman)**
    * **Kontrol**: Stik Kanan (Vertikal)
    * **Logika**: **Stateful**. Nilai bertambah/berkurang saat stik digerakkan dan **tetap (tersimpan)** saat stik dilepas.
    * **Batas**: `[0.0, 10.0]`

* **`yaw` (Belok Kiri/Kanan)**
    * **Kontrol**: Stik Kanan (Horizontal)
    * **Logika**: **Stateful**. Nilai bertambah/berkurang saat stik digerakkan dan **tetap (tersimpan)** saat stik dilepas.
    * **Batas**: `[-180.0, 180.0]`

---

## 📦 Struktur Repositori

. ├── controller/ # Paket node logika │ ├── CMakeLists.txt │ ├── package.xml │ └── src/ │ └── controller_node.cpp └── interfaces/ # Paket message kustom ├── CMakeLists.txt ├── package.xml └── msg/ └── ControllerCommand.msg


---

## 🛠️ Prasyarat

* ROS 2 Humble Hawksbill
* `ros-humble-joy-linux` (Untuk driver *joystick*)
* Kontroler Xbox 360 (atau yang kompatibel)

---

## ⚙️ Cara Menjalankan

1.  Pastikan Anda telah meng-kloning repositori ini ke dalam `src` dari *workspace* ROS 2 Anda.

2.  Bangun (*build*) *workspace* Anda dari direktori *root* (`ros2_ws`):
    ```bash
    # Pastikan interfaces di-build terlebih dahulu
    colcon build --packages-select interfaces
    
    # Source workspace
    source install/setup.bash
    
    # Build controller
    colcon build --packages-select controller
    ```

3.  *Source* *workspace* Anda di setiap terminal yang akan digunakan:
    ```bash
    source install/setup.bash
    ```

4.  **Terminal 1 (Jalankan Joy Node):**
    Hubungkan *joystick* Anda dan jalankan:
    ```bash
    ros2 run joy joy_node
    ```

5.  **Terminal 2 (Jalankan Controller Node):**
    ```bash
    ros2 run controller controller_node
    ```

6.  **Terminal 3 (Verifikasi Output):**
    Gunakan `ros2 topic echo` untuk melihat *output* dari *node* Anda saat *joystick* digerakkan.
    ```bash
    ros2 topic echo /cmd_vel
    ```

---

# [Tugas Magang 2 - Deteksi Warna OpenCV]

Repositori ini berisi pengumpulan tugas untuk program deteksi warna (HSV Color Tuner) yang dibuat menggunakan C++ dan OpenCV.

Proyek ini terdiri dari satu program C++ (`deteksi_warna.cpp`) yang berfungsi sebagai *tuner* HSV untuk sebuah file video input.

---

## 🚀 Fungsionalitas Program

Program ini akan membaca sebuah file video dari argumen *command-line* dan melakukan segmentasi warna berdasarkan rentang HSV yang dapat diatur secara *real-time* melalui *slider*.

Program akan menampilkan tiga jendela:
1.  **`Original Video`**: Menampilkan video asli yang sedang diputar.
2.  **`Masked Video`**: Menampilkan hasil *mask* biner (hitam-putih) dari deteksi warna.
3.  **`HSV Adjustments`**: Panel kontrol berisi 8 *slider* untuk mengatur nilai:
    * `Hue Min 1 / Hue Max 1` (Rentang Hue pertama)
    * `Hue Min 2 / Hue Max 2` (Rentang Hue kedua, untuk warna seperti merah)
    * `Sat Min / Sat Max` (Rentang Saturasi)
    * `Val Min / Val Max` (Rentang Value/Kecerahan)

---

## 📦 Struktur Repositori

. ├── deteksi_warna.cpp # File source code utama C++ └── README.md # File ini

---

## 🛠️ Prasyarat

* Sistem Operasi Ubuntu
* C++ Compiler (G++), bagian dari `build-essential`
* Pustaka Pengembangan OpenCV (`libopencv-dev`)
* Sebuah file video untuk diuji (misal: `.mp4`, `.avi`)

---

## ⚙️ Cara Menjalankan

1.  Pastikan Anda telah meng-kloning repositori ini dan semua prasyarat (terutama `libopencv-dev`) telah terinstal.

2.  Buka terminal di direktori repositori ini.

3.  Kompilasi program C++ menggunakan `g++` dan `pkg-config`:
    ```bash
    g++ deteksi_warna.cpp -o deteksi_app $(pkg-config --cflags --libs opencv4)
    ```

4.  Jalankan program yang sudah dikompilasi (`deteksi_app`), dengan memberikan nama file video Anda sebagai argumen.
    ```bash
    ./deteksi_app nama_video_anda.mp4
    ```

5.  Tiga jendela akan muncul. Atur *slider* di jendela "HSV Adjustments" untuk menyempurnakan deteksi warna di jendela "Masked Video".

6.  Tekan tombol **'q'** atau **ESC** saat jendela video aktif untuk keluar dari program.

---

:D
