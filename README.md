# Dokumentasi Tugas Template Hotspot MikroTik

**Judul Project:** Spektakuler Sengguruh 2026 (Karnaval Budaya)
**Dibuat oleh:** [Isi Nama Kakak / Kelompok]

---

## 1. Konsep & Alasan Desain

Pada template hotspot ini, saya menggunakan pendekatan _Single-Page Experience_ (Pengalaman Satu Halaman) menggunakan **Pop-up / Modal Overlay**.

Alasannya, halaman login WiFi di perangkat _mobile_ (Captive Portal) sangat sensitif dan sering kali langsung tertutup sendiri (_force close_) jika _user_ berpindah-pindah antar halaman HTML sebelum proses login berhasil. Dengan sistem Pop-up, warga bisa melihat informasi jadwal dan rute karnaval tanpa harus meninggalkan halaman login utama, sehingga koneksi tetap stabil.

---

## 2. Fitur Tambahan Utama

- **Splash Screen Sponsor Utama (5 Detik):** Mengakomodasi kebijakan RouterOS v7 yang menonaktifkan fitur `radvert.html` (iklan _interstitial_), saya memodifikasi alur otentikasi. Warga baru akan disambut oleh _Splash Screen_ elegan dari sponsor (PT. JSN) selama 5 detik di awal sebelum memasuki halaman login utama.
- **Smart Bypass (Jalan Tol Warga Lama):** Sistem dioptimalkan dengan fitur _MAC Cookie_. Bagi pengunjung yang sudah pernah login, sistem akan mendeteksi MAC Address perangkat secara otomatis. Pengunjung lama tidak perlu lagi melihat halaman login atau iklan; begitu masuk area karnaval, internet langsung aktif tanpa interupsi.
- **Pinch-to-Zoom Peta:** Terdapat _script_ khusus agar gambar peta rute dan titik kumpul evakuasi di dalam pop-up bisa diperbesar menggunakan dua jari langsung dari layar HP pengunjung.
- **Penerjemah Error Otomatis:** Pesan _error_ bawaan MikroTik otomatis diterjemahkan ke dalam kalimat Bahasa Indonesia yang ramah dan mudah dipahami warga.

---

## 3. Penjelasan Struktur File

Berikut adalah file-file inti yang menyusun sistem template hotspot ini:

| Nama File       | Fungsi & Modifikasi di dalam Sistem                                                                   |
| :-------------- | :---------------------------------------------------------------------------------------------------- |
| `style.css`     | File CSS utama pengatur tata letak visual. Didesain dengan pendekatan _Mobile-First_.                 |
| `rlogin.html`   | Modifikasi total menjadi **Panggung Sponsor (Splash Screen)** yang menampilkan hitung mundur 5 detik. |
| `login.html`    | Halaman Pintu Utama _Captive Portal_. Berisi tombol login dan fitur Pop-up Jadwal.                    |
| `redirect.html` | Halaman transisi (3 detik) yang memunculkan animasi centang hijau setelah berhasil login.             |
| `status.html`   | Halaman _dashboard_ saat _user_ sukses terhubung, menampilkan sisa waktu dan tombol logout.           |
| `logout.html`   | Halaman akhir berisi ringkasan waktu pemakaian setelah _user_ memutuskan koneksi.                     |
| `error.html`    | Penampil pesan gangguan atau limitasi sistem dalam Bahasa Indonesia.                                  |
| `md5.js`        | _Script_ enkripsi keamanan _password_ bawaan standar dari MikroTik.                                   |
| `images/`       | Folder penyimpan seluruh aset visual (Logo Desa, Logo JSN, Background, Peta Rute).                    |

---

## 4. Cara Pemasangan ke Router MikroTik

1. Buka aplikasi WinBox dan login ke router.

2. Masuk ke menu Files.

3. Drag and drop (seret) folder proyek hotspot ini ke dalam jendela Files.

4. Masuk ke menu IP > Hotspot > Server Profiles.

5. Buka profile yang dipakai (misal: default), lalu pada tab General di bagian HTML Directory, arahkan ke folder yang baru saja di-upload.

6. Pada tab Login, pastikan opsi MAC atau Login By Trial sudah dicentang.

7. Pada tab User Profiles (IP > Hotspot > User Profiles), pastikan opsi Add MAC Cookie dicentang agar fitur Smart Bypass (Jalan Tol Warga Lama) dapat berfungsi.

````

## 5. Alur Logika Sistem (Flowchart)

Sistem membedakan perlakuan (_User Experience_) berdasarkan status perangkat pengguna di dalam _database_ router:

### A. Alur Warga Baru (First-Time User)

```text
[Warga Connect WiFi]
        ↓
(Tampil rlogin.html) ──> Halaman Sambutan Sponsor (JSN) hitung mundur 5 Detik
        ↓
(Masuk login.html)   ──> Warga dapat membaca Info Jadwal & Rute (Pop-up)
        ↓
[Warga Klik Tombol "Mulai Internet Gratis"]
        ↓
(Proses Otentikasi Router) ──(Jika Kuota Habis)──> Tampil Pesan Error (B. Indonesia)
        ↓
(Jika Sukses)
        ↓
(Tampil redirect.html) ──> Transisi Sukses (Animasi Centang Hijau 3 detik)
        ↓
[Masuk status.html] ──> Warga sukses terhubung ke Internet

B. Alur Warga Lama (Returning VIP User)
Plaintext

[Warga Kembali ke Area Karnaval & Connect WiFi]
        ↓
(Router Membaca MAC Cookie) ──> Perangkat Dikenali sebagai Warga Terdaftar dan masih masuk masa aktif
        ↓
[Internet Langsung Aktif] ──> Bebas hambatan. Tidak ada pop-up halaman login/iklan yang muncul.


````
