
# BanquetBook – Data Dictionary

## 1. Overview

Data Dictionary merupakan dokumentasi yang menjelaskan struktur tabel dan atribut pada database BanquetBook.

Database menggunakan PostgreSQL dengan schema `banquet_v2`. Dokumentasi ini disusun berdasarkan file migration SQL.

Informasi yang dijelaskan meliputi nama kolom, tipe data, key, aturan data, dan fungsi setiap atribut.

## 2. Database Tables

Database BanquetBook memiliki 9 tabel:

| No. | Table | Function |
|---|---|---|
| 1 | customers | Menyimpan data pelanggan atau perusahaan |
| 2 | customer_contacts | Menyimpan informasi kontak pelanggan |
| 3 | bookings | Menyimpan data utama pemesanan event |
| 4 | rooms | Menyimpan katalog ruangan banquet |
| 5 | packages | Menyimpan katalog paket event |
| 6 | booking_room_reservations | Menyimpan detail reservasi ruangan |
| 7 | addons | Menyimpan katalog layanan tambahan |
| 8 | booking_room_addons | Menyimpan detail add-ons pada reservasi |
| 9 | facilities | Menyimpan katalog fasilitas |

## 3. Table Structure

### 3.1 customers

Menyimpan identitas pelanggan atau perusahaan yang melakukan pemesanan.

| Column | Data Type | Key | Description |
|---|---|---|---|
| customer_id | bigint | PK | Identitas unik pelanggan |
| company | text | NOT NULL | Nama perusahaan atau pelanggan |
| customer_type | text | NOT NULL | Jenis pelanggan, default Corporate |
| notes | text | - | Catatan tambahan pelanggan |
| is_dummy | boolean | - | Penanda data simulasi |
| created_at | timestamptz | - | Waktu pembuatan data |

### 3.2 customer_contacts

Menyimpan informasi orang yang menjadi kontak dari suatu perusahaan atau pelanggan.

| Column | Data Type | Key | Description |
|---|---|---|---|
| contact_id | bigint | PK | Identitas unik kontak |
| customer_id | bigint | FK | Referensi ke customers.customer_id |
| contact_name | text | - | Nama orang yang dapat dihubungi |
| role_title | text | - | Jabatan atau posisi kontak |
| phone | text | - | Nomor telepon kontak |
| email | text | - | Alamat email kontak |
| is_primary | boolean | - | Penanda kontak utama |
| notes | text | - | Catatan tambahan |
| created_at | timestamptz | - | Waktu pembuatan data |

### 3.3 bookings

Menyimpan informasi utama pemesanan banquet, termasuk pelanggan, jadwal acara, paket, status, dan total biaya.

| Column | Data Type | Key | Description |
|---|---|---|---|
| booking_id | bigint | PK | Identitas unik booking |
| booking_code | text | UNIQUE | Kode unik pemesanan |
| customer_id | bigint | FK | Referensi ke customers.customer_id |
| event_name | text | NOT NULL | Nama acara |
| event_type | text | - | Jenis acara |
| booking_status | text | NOT NULL | Status pemesanan |
| starts_at | timestamptz | NOT NULL | Waktu mulai acara |
| ends_at | timestamptz | NOT NULL | Waktu selesai acara |
| expected_pax | integer | NOT NULL | Estimasi jumlah peserta |
| guaranteed_pax | integer | - | Jumlah peserta yang dijamin |
| hold_expires_at | timestamptz | - | Batas waktu penahanan booking |
| package_id | bigint | FK | Referensi ke packages.package_id |
| package_price_snapshot | numeric(14,2) | - | Salinan harga paket saat booking |
| package_days | integer | - | Jumlah hari paket |
| package_total_snapshot | numeric(14,2) | - | Salinan total harga paket |
| total_amount | numeric(14,2) | NOT NULL | Total biaya pemesanan |
| is_dummy | boolean | - | Penanda data simulasi |
| created_at | timestamptz | - | Waktu pembuatan booking |
| updated_at | timestamptz | - | Waktu pembaruan booking |

### 3.4 rooms

Menyimpan informasi katalog ruangan yang tersedia untuk kegiatan banquet.

| Column | Data Type | Key | Description |
|---|---|---|---|
| room_id | bigint | PK | Identitas unik ruangan |
| room_name | text | NOT NULL | Nama ruangan |
| room_type | text | - | Jenis ruangan |
| capacity | integer | - | Kapasitas maksimum ruangan |
| location | text | - | Lokasi ruangan |
| floor | text | - | Lantai tempat ruangan berada |
| base_price | numeric(14,2) | - | Harga dasar ruangan |
| currency | text | NOT NULL | Mata uang, default IDR |
| tax_per_room | numeric(14,2) | - | Nilai pajak per ruangan |
| service_per_room | numeric(14,2) | - | Biaya layanan per ruangan |
| notes | text | - | Catatan ruangan |
| description | text | - | Deskripsi ruangan |
| is_active | boolean | - | Status aktif ruangan |
| is_dummy | boolean | - | Penanda data simulasi |
| created_at | timestamptz | - | Waktu pembuatan data |
| updated_at | timestamptz | - | Waktu pembaruan data |

### 3.5 packages

Menyimpan katalog paket banquet atau meeting yang dapat dipilih pelanggan.

| Column | Data Type | Key | Description |
|---|---|---|---|
| package_id | bigint | PK | Identitas unik paket |
| package_name | text | NOT NULL | Nama paket |
| duration_type | text | - | Jenis durasi paket |
| price_per_pax | numeric(14,2) | - | Harga paket per peserta |
| price_is_net | boolean | NOT NULL | Penanda harga sudah net |
| includes_description | text | - | Penjelasan fasilitas paket |
| description | text | - | Deskripsi paket |
| includes | text[] | - | Daftar item yang termasuk dalam paket |
| image_url | text | - | Lokasi gambar paket |
| source_note | text | - | Catatan sumber informasi paket |
| is_active | boolean | - | Status aktif paket |
| is_dummy | boolean | - | Penanda data simulasi |
| created_at | timestamptz | - | Waktu pembuatan data |
| updated_at | timestamptz | - | Waktu pembaruan data |

### 3.6 booking_room_reservations

Menyimpan detail ruangan yang dipesan dalam suatu booking, termasuk jadwal, jumlah peserta, status reservasi, dan rincian biaya.

| Column | Data Type | Key | Description |
|---|---|---|---|
| reservation_id | bigint | PK | Identitas unik reservasi |
| booking_id | bigint | FK | Referensi ke bookings.booking_id |
| room_id | bigint | FK | Referensi ke rooms.room_id |
| starts_at | timestamptz | NOT NULL | Waktu mulai reservasi |
| ends_at | timestamptz | NOT NULL | Waktu selesai reservasi |
| days | integer | NOT NULL | Jumlah hari penggunaan |
| reservation_status | text | NOT NULL | Status reservasi ruangan |
| hold_expires_at | timestamptz | - | Batas waktu penahanan reservasi |
| expected_pax | integer | - | Estimasi peserta |
| guaranteed_pax | integer | - | Jumlah peserta yang dijamin |
| setup_style | text | - | Bentuk atau gaya penataan ruangan |
| rate_per_day | numeric(14,2) | - | Harga ruangan per hari |
| room_base_total | numeric(14,2) | - | Total harga dasar ruangan |
| tax_amount | numeric(14,2) | - | Nilai pajak |
| service_amount | numeric(14,2) | - | Biaya layanan |
| total_per_ruangan | numeric(14,2) | - | Total biaya satu ruangan |
| created_at | timestamptz | - | Waktu pembuatan reservasi |

### 3.7 addons

Menyimpan katalog layanan atau perlengkapan tambahan yang dapat dipilih pada reservasi ruangan.

| Column | Data Type | Key | Description |
|---|---|---|---|
| addon_id | bigint | PK | Identitas unik add-on |
| addon_name | text | NOT NULL | Nama layanan atau perlengkapan |
| category | text | - | Kategori add-on |
| unit | text | - | Satuan add-on |
| unit_price | numeric(14,2) | - | Harga per satuan |
| total_quantity | integer | - | Jumlah stok tersedia |
| stock_verified | boolean | NOT NULL | Penanda verifikasi stok |
| notes | text | - | Catatan tambahan |
| description | text | - | Deskripsi add-on |
| is_active | boolean | - | Status aktif add-on |
| is_dummy | boolean | - | Penanda data simulasi |
| created_at | timestamptz | - | Waktu pembuatan data |

### 3.8 booking_room_addons

Merupakan tabel penghubung antara reservasi ruangan dan katalog add-ons. Tabel ini juga menyimpan snapshot nama dan harga add-on ketika digunakan.

| Column | Data Type | Key | Description |
|---|---|---|---|
| reservation_id | bigint | PK, FK | Referensi ke booking_room_reservations.reservation_id |
| addon_id | bigint | PK, FK | Referensi ke addons.addon_id |
| addon_name_snapshot | text | NOT NULL | Salinan nama add-on saat transaksi |
| quantity | integer | NOT NULL | Jumlah add-on yang digunakan |
| unit_price_snapshot | numeric(14,2) | NOT NULL | Salinan harga satuan saat transaksi |
| total_snapshot | numeric(14,2) | NOT NULL | Total harga add-on saat transaksi |

**Catatan:** Primary Key pada tabel ini menggunakan kombinasi `reservation_id` dan `addon_id` (Composite Primary Key).

### 3.9 facilities

Menyimpan katalog fasilitas standar atau fasilitas tambahan yang tersedia dalam operasional banquet.

| Column | Data Type | Key | Description |
|---|---|---|---|
| facility_id | bigint | PK | Identitas unik fasilitas |
| facility_name | text | NOT NULL | Nama fasilitas |
| category | text | - | Kategori fasilitas |
| unit | text | - | Satuan fasilitas |
| is_standard | boolean | NOT NULL | Penanda fasilitas standar |
| is_chargeable | boolean | NOT NULL | Penanda fasilitas berbayar |
| default_unit_price | numeric(14,2) | - | Harga default per satuan |
| notes | text | - | Catatan tambahan |
| description | text | - | Deskripsi fasilitas |
| is_active | boolean | - | Status aktif fasilitas |
| is_dummy | boolean | - | Penanda data simulasi |
| created_at | timestamptz | - | Waktu pembuatan data |

## 4. Data Type Explanation

| Data Type | Function |
|---|---|
| bigint | Menyimpan bilangan bulat dengan kapasitas besar |
| integer | Menyimpan bilangan bulat |
| text | Menyimpan data teks |
| boolean | Menyimpan nilai TRUE atau FALSE |
| numeric(14,2) | Menyimpan angka desimal dengan 2 digit di belakang koma |
| timestamptz | Menyimpan tanggal dan waktu dengan informasi zona waktu |
| text[] | Menyimpan kumpulan nilai teks dalam bentuk array |

## 5. Key and Constraint Explanation

- **Primary Key (PK):** Identitas unik setiap record dalam tabel.
- **Foreign Key (FK):** Menghubungkan data antar tabel.
- **NOT NULL:** Kolom wajib memiliki nilai.
- **UNIQUE:** Mencegah nilai duplikat pada kolom yang ditentukan.
- **DEFAULT:** Memberikan nilai otomatis apabila nilai tidak diisi.
- **CHECK:** Memastikan data memenuhi aturan atau kondisi tertentu.
- **Composite Primary Key:** Primary Key yang terdiri dari lebih dari satu kolom.

## 6. Database Design Notes

1. Database menggunakan pendekatan relasional untuk mengelola informasi pelanggan, pemesanan, ruangan, paket, dan add-ons.
2. Tabel `bookings` berfungsi sebagai tabel utama transaksi pemesanan.
3. Tabel `booking_room_reservations` menyimpan detail ruangan untuk setiap booking.
4. Tabel `booking_room_addons` menghubungkan reservasi dengan add-ons yang dipilih.
5. Snapshot harga digunakan untuk mempertahankan informasi harga pada saat transaksi dilakukan.
6. Kolom `is_dummy` digunakan untuk membedakan data simulasi dari data lainnya.
7. Tabel `facilities` berdiri sendiri dan belum memiliki Foreign Key pada schema saat ini.

## 7. Source

Dokumentasi ini dibuat berdasarkan struktur SQL pada:

`supabase/migrations/20261102220000_banquet_v2_schema.sql`
