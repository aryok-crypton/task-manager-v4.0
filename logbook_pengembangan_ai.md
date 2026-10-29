# 📝 LOGBOOK PENGEMBANGAN APLIKASI WEB (AI INTERACTION LOG)

**Nama Proyek:** Portal Belajar SMA & Smart Study Task Manager  
**Pengembang:** Siswa / Tim Pengembang  
**Teknologi:** HTML5, CSS3, JavaScript (ES6), LocalStorage, PWA (Service Worker)  
**Tanggal:** 29 Oktober 2026  

---

## 📌 TAHAP 1: Eksplorasi Ide & Perancangan Solusi
> **Fokus:** Menentukan konsep produk, arsitektur visual, serta penggabungan dua ide modul aplikasi (Portal Belajar SMA + Smart Study Task Manager).

| No | Prompt / Interaksi dengan AI | Saran / Output dari AI | Validasi & Modifikasi Mandiri (Oleh Siswa) |
|---|---|---|---|
| **1.1** | "Bagaimana cara menggabungkan modul Smart Study Task Manager ke dalam Portal Belajar Siswa SMA?" | Membantu memetakan tata letak UI, dengan merekomendasikan agar modul *Task Manager* diletakkan di dalam menu / section **To-Do List & Kalender** (`#planner-view`). | **Disetujui & Disesuaikan:** Menyetujui usulan AI untuk menaruh modul di `#planner-view`. Selain itu, siswa menambahkan ide mandiri agar data tugas yang diinput juga direfleksikan secara *real-time* ke widget ringkasan pada halaman **Dashboard Utama**. |
| **1.2** | "Skema warna dan layout seperti apa yang cocok agar kedua file CSS yang terpisah tidak bertabrakan (*conflict*)?" | Mengusulkan penggunaan Variabel CSS (`:root`) untuk mengunci skema warna utama (Indigo `#4f46e5`, Slate `#1e293b`) dan menggabungkan aturan `.task-card` ke dalam stylesheet terpadu. | **Dimodifikasi:** Menyesuaikan warna badge prioritas agar lebih kontras dan intuitif (Merah untuk Tinggi/Mendesak, Oranye untuk Sedang, Hijau untuk Chill/Rendah) agar mudah dibaca oleh siswa SMA. |

---

## 📌 TAHAP 2: Penyusunan Kerangka Kode (Scaffolding)
> **Fokus:** Menyusun struktur dasar HTML, penggabungan styling CSS, dan fungsi utama logika JavaScript.

| No | Prompt / Interaksi dengan AI | Saran / Output dari AI | Validasi & Modifikasi Mandiri (Oleh Siswa) |
|---|---|---|---|
| **2.1** | "Buatkan struktur form HTML untuk input tugas beserta kontainer daftar tugasnya." | Memberikan struktur kode HTML form dengan field: *Mata Pelajaran*, *Deskripsi Tugas*, *Tenggat Waktu*, dan *Tingkat Kesulitan (1-5)*. | **Diperbaiki & Divalidasi:** Menambahkan *attribute* `required` pada setiap input form serta membatasi `min="1"` dan `max="5"` pada input kesulitan untuk mencegah kesalahan penginputan (*user error*). |
| **2.2** | "Bagaimana logika JavaScript untuk menghitung skor prioritas tugas berbasis deadline dan kesulitan?" | Memberikan rumus fungsi `calculatePriority(deadlineStr, difficulty)` menggunakan pembobotan sisa hari dan skala kesulitan: `(difficulty * 2) + Math.max(1, 10 - diffDays)`. | **Dimodifikasi:** Memperbaiki perhitungan selisih hari (`diffDays`) dengan menyetel jam pada `today.setHours(0,0,0,0)` agar perhitungan sisa hari akurat tepat pada tengah malam. |
| **2.3** | "Buatkan satu kesatuan file baru yang menjalankan seluruh fitur secara bersamaan tanpa menghilangkan unsur dari file sebelumnya." | Menghasilkan kode aplikasi web terintegrasi (*Single-Page Application*) yang mencakup Sidebar, Dashboard, Grid 16 Mapel, Quiz Generator, dan Smart Task Manager dalam satu file `index.html`. | **Divalidasi:** Memeriksa seluruh event listener navigation (`.nav-item`) untuk memastikan transisi antar *view section* berjalan mulus tanpa merusak *layout* utama. |

---

## 📌 TAHAP 3: Pemecahan Masalah & Penanganan Galat (Debugging)
> **Fokus:** Memperbaiki *bug*, menangani galat pada LocalStorage, dan sinkronisasi data antar modul.

| No | Prompt / Interaksi dengan AI | Saran / Output dari AI | Validasi & Modifikasi Mandiri (Oleh Siswa) |
|---|---|---|---|
| **3.1** | **Masalah:** Saat menambah tugas baru di To-Do List, widget ringkasan di Dashboard tidak diperbarui secara langsung kecuali halaman di-*refresh*. | AI menyarankan pemanggilan ulang fungsi render secara terpisah atau menggunakan event `storage`. | **Modifikasi Mandiri (Solusi Terbaik):** Menyatukan fungsi pembaruan UI Dashboard ke dalam fungsi `renderTasks()`. Jadi, saat `renderTasks()` dipanggil setelah pendaftaran tugas baru, elemen `dashTodoCount` dan `dashReminderList` langsung diperbarui secara sinkron tanpa reload. |
| **3.2** | **Masalah:** Tombol "Hapus" tugas tidak merespons atau menyebabkan error `index undefined`. | AI menyarankan penggunaan penanganan event delegasi (*event delegation*). | **Divalidasi:** Memilih menggunakan argumen `index` langsung pada `onclick="deleteTask(${index})"` di dalam template string JS, lalu memperbarui `localStorage.setItem('tasks', JSON.stringify(tasks))` sebelum memanggil `renderTasks()` kembali. |
| **3.3** | **Masalah:** Tugas yang tenggat waktunya sudah lewat (*overdue*) menghasilkan angka sisa hari negatif (misal: `-2 hari lagi`). | AI menyarankan pengkondisian `if (diffDays < 0)`. | **Dimodifikasi:** Menambahkan status khusus `"TERLAMBAT 🚨"` dengan skor prioritas tertinggi (`999`) agar tugas yang terlambat otomatis melompat ke urutan paling atas daftar. |

---

## 📌 TAHAP 4: Refleksi & Penjelasan Kode
> **Fokus:** Verifikasi pemahaman siswa terhadap fungsi program, tipe data yang digunakan, dan arsitektur aplikasi.

| No | Topik Pembahasan / Pertanyaan | Penjelasan & Pemahaman Siswa | Analisis & Validasi Penerapan Kode |
|---|---|---|---|
| **4.1** | **Penggunaan Tipe Data Variabel Dasar** | Mengidentifikasi dan memahami pemanfaatan variabel dasar di JavaScript: <br>• **`String`**: Digunakan pada `mapel` dan `tugas` untuk menyimpan teks. <br>• **`Integer`**: Digunakan pada `kesulitan` (1-5) dan `diffDays` untuk perhitungan matematika. <br>• **`Boolean`**: Digunakan untuk penanda status seperti `isCompleted`. <br>• **`Array / Object`**: Digunakan pada variabel `tasks` untuk menampung kumpulan objek tugas. | **Penerapan:** Variabel-variabel ini diolah di dalam fungsi `calculatePriority()` untuk menghasilkan skor numerik, yang kemudian menentukan kelas CSS mana (`badge-tinggi`, `badge-sedang`, `badge-rendah`) yang akan ditampilkan di layar. |
| **4.2** | **Mekanisme Storage (Penyimpanan Permanen)** | Memahami penggunaan `localStorage.getItem()` dan `localStorage.setItem()` berserta `JSON.parse()` dan `JSON.stringify()`. | **Penerapan:** Karena LocalStorage hanya menerima data dalam bentuk string, data array dari objek `tasks` harus diubah menjadi JSON string saat disimpan, dan di-*parse* kembali menjadi array objek JS saat aplikasi dibuka. |
| **4.3** | **Arsitektur Single-Page Application (SPA)** | Memahami bagaimana perpindahan halaman dilakukan tanpa reload menggunakan manipulasi class CSS `.active`. | **Penerapan:** Ketika tombol navigasi disamping diklik, JavaScript menghapus class `.active` dari semua `.view-section` dan menambahkannya hanya pada elemen target (`data-target`), menciptakan pengalaman pengguna yang cepat dan responsif. |

---

### 🎓 Ringkasan Pembelajaran & Kesimpulan:
Penggunaan AI dalam proyek ini berfungsi sebagai **asisten kolaboratif** yang mempercepat proses penyusunan kerangka kode (*scaffolding*) dan pencarian opsi solusi masalah. Validasi serta modifikasi mandiri secara aktif memastikan bahwa seluruh struktur logika, batasan *input*, penanganan galat (*debugging*), dan keselarasan antarmuka tetap dikontrol sepenuhnya oleh siswa.