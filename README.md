# Kopi Ulee Kareng — Project Laravel 12

Aplikasi web profil usaha (landing page) dan sistem manajemen produk (CRUD) sederhana untuk **Kopi Ulee Kareng**, kopi tradisional legendaris khas Aceh. Dikembangkan sebagai materi dan instrumen persiapan Asesmen Sertifikasi Kompetensi (SERKOM) Rekayasa Perangkat Lunak (RPL) untuk Skema **Pemrogram Junior (Junior Coder)** berdasarkan standar SKKNI Nomor 282 Tahun 2016.

---

## 1. Tujuan Aplikasi

1. **Promosi Publik:** Memperkenalkan profil usaha, sejarah, keunggulan, serta daftar produk Kopi Ulee Kareng secara dinamis langsung dari database.
2. **Pengelolaan Produk (CRUD):** Menyediakan antarmuka bagi pengelola untuk melihat, menambah, mengubah, dan menghapus produk tanpa perlu login/autentikasi (sesuai batasan skema Pemrogram Junior).
3. **Standar Kompetensi:** Memenuhi kriteria unit kompetensi struktur data, spesifikasi program, pemrograman terstruktur, penulisan kode sesuai pedoman, dokumentasi, debugging, dan pengujian unit program.

---

## 2. Teknologi yang Digunakan

- **Framework:** Laravel 12.x
- **Bahasa Pemrograman:** PHP 8.2.x
- **Dependency Manager:** Composer 2.x
- **Database:** MySQL 8.x (Laragon / XAMPP)
- **Frontend Template Engine:** Laravel Blade
- **Styling:** CSS Murni Lokal (`public/css/style.css`, tanpa NPM/Vite/CDN)
- **Testing Framework:** PHPUnit 11 & Laravel Feature Testing

---

## 3. Persyaratan Lingkungan

- Sistem Operasi: Windows 10/11
- Web Server / PHP CLI: PHP >= 8.2
- Database Server: MySQL Laragon (Port 3306)
- Web Browser: Google Chrome / Microsoft Edge / Mozilla Firefox
- Git (opsional)

---

## 4. Konfigurasi MySQL

Konfigurasi koneksi database pada file `.env`:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=kopi_ulee_kareng
DB_USERNAME=root
DB_PASSWORD=
```

Database default dibuat dengan karakter encoding `utf8mb4_unicode_ci`.

---

## 5. Struktur Folder MVC

```text
new-project-kopi/
├── app/
│   ├── Http/
│   │   └── Controllers/
│   │       └── ProductController.php      # Controller logika bisnis landing & CRUD
│   └── Models/
│       └── Product.php                    # Model Eloquent entitas produk
│
├── database/
│   ├── migrations/
│   │   └── 2024_01_01_000000_create_products_table.php # Skema tabel products
│   └── seeders/
│       ├── DatabaseSeeder.php             # Pemanggil seeder utama
│       └── ProductSeeder.php              # Data awal 3 produk kopi wajib
│
├── public/
│   ├── css/
│   │   └── style.css                      # Styling lokal responsif
│   └── images/
│       ├── hero-kopi.jpg                  # Gambar banner hero
│       ├── kopi-original.jpg              # Gambar produk Kopi Original
│       ├── kopi-sanger.jpg                # Gambar produk Kopi Sanger
│       ├── kopi-tubruk.jpg                # Gambar produk Kopi Tubruk
│       ├── kopi-default.jpg               # Gambar fallback cadangan
│       └── kopi-hitam.jpg                 # Gambar cadangan varian
│
├── resources/
│   └── views/
│       ├── layouts/
│       │   └── app.blade.php              # Layout induk HTML, navbar, footer
│       ├── landing.blade.php              # Landing page publik (7 bagian)
│       └── produk/
│           ├── index.blade.php            # Tabel daftar produk & aksi CRUD
│           ├── create.blade.php           # Halaman formulir tambah produk
│           ├── edit.blade.php             # Halaman formulir edit produk
│           └── _form.blade.php            # Partial formulir bersama (DRY)
│
├── routes/
│   └── web.php                            # Definisi rute landing & resource produk
│
├── tests/
│   └── Feature/
│       └── ProductFlowTest.php            # Feature test otomatis skenario TC-01 s/d TC-10
│
└── README.md                              # Dokumentasi teknis proyek
```

---

## 6. Cara Menjalankan Project di Laragon

1. Buka aplikasi **Laragon** lalu klik **Start All** (pastikan modul MySQL aktif pada port 3306).
2. Buat database baru bernama `kopi_ulee_kareng` via HeidiSQL atau Terminal MySQL:
   ```bash
   mysql -u root -e "CREATE DATABASE IF NOT EXISTS kopi_ulee_kareng CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;"
   ```
3. Buka Terminal pada root direktori project (`new-project-kopi`):
   ```bash
   composer install
   ```
4. Pastikan file `.env` sudah sesuai (koneksi database `kopi_ulee_kareng`).
5. Jalankan migration dan seeder data awal:
   ```bash
   php artisan migrate --seed
   ```
6. Jalankan development server bawaan Laravel:
   ```bash
   php artisan serve --host=127.0.0.1 --port=8000
   ```
7. Buka browser:
   - **Landing Page Publik:** `http://127.0.0.1:8000`
   - **Halaman Kelola CRUD:** `http://127.0.0.1:8000/produk`

---

## 7. Penjelasan Landing Page

Landing page memuat 7 bagian utama:
1. **Navbar:** Memuat logo brand "Kopi Ulee Kareng", tautan navigasi anchor (#beranda, #tentang, #keunggulan, #produk, #kontak), serta tombol cepat menuju "Kelola Produk". Dilengkapi toggle responsif untuk perangkat mobile.
2. **Hero Section:** Judul utama yang merefleksikan identitas Kopi Ulee Kareng khas Aceh, deskripsi singkat, tombol ajakan "Lihat Produk Kami", dan gambar latar belakang `hero-kopi.jpg`.
3. **Tentang (About):** Mengulas latar belakang Kopi Ulee Kareng sebagai salah satu kopi legendaris asal Banda Aceh dan tradisi ngopi masyarakat Aceh.
4. **Keunggulan (Features):** 4 kartu pilar utama: Aroma Khas, Cita Rasa Kuat, Penyajian Tradisional (saringan kain khas Aceh), dan Pilihan Varian.
5. **Produk Dinamis:** Menampilkan seluruh koleksi kopi langsung dari database MySQL menggunakan query `Product::latest()->get()`. Dilengkapi format mata uang Rupiah (`number_format`) dan fallback gambar otomatis jika file kosong/tidak ditemukan.
6. **Kontak:** Informasi alamat lokasi di Ulee Kareng, Banda Aceh, alamat email, dan jam operasional.
7. **Footer:** Identitas hak cipta, ringkasan informasi, dan navigasi balik.

---

## 8. Penjelasan CRUD Produk

- **Index (`/produk`):** Menampilkan data produk dalam format tabel (Nomor urut iterasi, Thumbnail Gambar, Nama Produk, Deskripsi ringkas, Harga terformat, dan Kolom Aksi). Menyediakan tombol Tambah Produk dan Kembali ke Beranda.
- **Create (`/produk/create`):** Formulir input produk baru (Nama, Deskripsi, Harga, Nama file gambar opsional). Memiliki proteksi token CSRF (`@csrf`).
- **Store (`POST /produk`):** Melakukan validasi input di server, menyimpan ke tabel `products`, lalu melakukan redirect ke halaman index dengan pesan notifikasi sukses.
- **Edit (`/produk/{produk}/edit`):** Menampilkan data lama produk ke dalam formulir menggunakan helper `old('field', $produk->field)`.
- **Update (`PUT /produk/{produk}`):** Menggunakan method spoofing (`@method('PUT')`), memvalidasi pembaruan data, dan menyimpan perubahan ke database.
- **Destroy (`DELETE /produk/{produk}`):** Menghapus entitas produk yang dipilih setelah pengguna menyetujui dialog konfirmasi JavaScript (`confirm()`).

---

## 9. Penjelasan Validasi

Aturan validasi didefinisikan pada `ProductController`:

```php
$validated = $request->validate([
    'nama_produk' => ['required', 'string', 'max:100'],
    'deskripsi'   => ['required', 'string'],
    'harga'       => ['required', 'integer', 'min:0'],
    'gambar'      => ['nullable', 'string', 'max:255'],
]);
```

Pesan kesalahan kustom berbahasa Indonesia:
- `nama_produk.required`: "Nama produk wajib diisi."
- `nama_produk.max`: "Nama produk maksimal 100 karakter."
- `deskripsi.required`: "Deskripsi wajib diisi."
- `harga.required`: "Harga wajib diisi."
- `harga.integer`: "Harga harus berupa bilangan bulat."
- `harga.min`: "Harga tidak boleh negatif."
- `gambar.max`: "Nama file gambar maksimal 255 karakter."

Error ditampilkan tepat di bawah masing-masing elemen formulir dengan penanda kelas `.error`.

---

## 10. Pengelolaan Gambar & Keamanan Fallback

- Gambar disimpan di direktori `public/images/`.
- Tidak menggunakan form upload file untuk menjaga kesederhanaan skema serkom. Pengguna cukup memasukkan nama file (contoh: `kopi-original.jpg`).
- Model `Product` mengimplementasikan accessor `nama_gambar` yang membersihkan nama file menggunakan `basename()` untuk mencegah **Path Traversal Attack** (misal: `../../etc/passwd`).
- Jika kolom `gambar` kosong (`null`) atau file gambar tidak ditemukan di disk `public/images/`, sistem secara otomatis mengembalikan `kopi-default.jpg`.

---

## 11. Cara Menjalankan Pengujian

Eksekusi seluruh unit dan feature test otomatis menggunakan Artisan:

```bash
php artisan test
```

Atau eksekusi spesifik suite pengujian alur produk:

```bash
php artisan test tests/Feature/ProductFlowTest.php
```

*Catatan:* Pengujian otomatis berjalan pada database SQLite `:memory:` yang terisolasi sesuai pengaturan `phpunit.xml`, sehingga data pada database utama MySQL `kopi_ulee_kareng` tetap aman dan tidak terhapus.

---

## 12. Hasil Pengujian Nyata

Semua pengujian telah diverifikasi pada lingkungan Windows 10/11, PHP 8.2.12, Laravel 12.71.0:

| ID | Skenario Pengujian | Hasil Aktual | Status |
|---|---|---|---|
| TC-01 | Membuka landing page publik (`/`) | Halaman menampilkan status HTTP 200 dan teks identitas "Kopi Ulee Kareng" | **PASS** |
| TC-02 | Membuka index saat produk kosong | Menampilkan empty state "Belum ada produk yang tersedia" | **PASS** |
| TC-03 | Menambah produk valid via POST | Produk tersimpan ke database dan pengguna diarahkan ke index dengan notifikasi flash | **PASS** |
| TC-04 | Validasi nama produk kosong | Validasi gagal, sesi memuat error `nama_produk`, data tidak tersimpan | **PASS** |
| TC-05 | Validasi harga berupa huruf non-angka | Validasi gagal, sesi memuat error `harga` ("Harga harus berupa bilangan bulat") | **PASS** |
| TC-06 | Validasi harga bernilai negatif | Validasi gagal, sesi memuat error `harga` ("Harga tidak boleh negatif") | **PASS** |
| TC-07 | Mengedit dan memperbarui data produk | Data produk berhasil diperbarui di database dan redirect sukses | **PASS** |
| TC-08 | Menghapus produk via DELETE | Produk terhapus dari tabel database dan redirect ke index sukses | **PASS** |
| TC-09 | Fallback gambar saat gambar kosong/null | Komponen Blade merender `kopi-default.jpg` sebagai pengganti | **PASS** |
| TC-10 | Landing page membaca produk database | Produk yang baru disimpan langsung muncul pada kartu produk di landing page | **PASS** |

Total: **13 tests passed, 31 assertions, 0 failed**.

---

## 13. Catatan Debugging

1. **Masalah Cache Permissions di Windows Sandbox:**
   - *Gejala:* Perintah `php artisan optimize:clear` atau `test` memunculkan `The bootstrap/cache directory must be present and writable`.
   - *Penyebab:* Direktori `bootstrap/cache` belum dibuat secara eksplisit setelah instalasi awal.
   - *Perbaikan:* Membuat direktori `bootstrap/cache` dan memastikan izin penulisan lokal.
2. **Koneksi Database pada Environment Testing:**
   - *Gejala:* Default `ExampleTest` gagal dengan pesan `no such table: products`.
   - *Penyebab:* `ExampleTest.php` bawaan Laravel belum menyertakan trait `RefreshDatabase` saat menguji rute yang memerlukan tabel database pada SQLite in-memory.
   - *Perbaikan:* Menambahkan `use RefreshDatabase;` pada `ExampleTest.php`. Pengujian ulang menghasilkan PASS 100%.

---

## 14. Batasan Aplikasi (Scope Constraints)

Sesuai instruksi jobsheet persiapan SERKOM Pemrogram Junior:
- Hanya menggunakan 1 tabel domain (`products`).
- Tidak menggunakan autentikasi, registrasi, hak akses, atau multi-role.
- Tidak ada fitur keranjang belanja, checkout, atau payment gateway.
- Tidak ada modul API, web service, maupun pelaporan grafik statistik.
- Tidak menggunakan framework JavaScript kompleks (React/Vue/Angular) atau build tool Node.js (Vite/NPM) untuk styling.
- Aplikasi dirancang untuk berjalan di lingkungan laboratorium mandiri dan siap dipresentasikan.
