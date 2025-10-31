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

:D
