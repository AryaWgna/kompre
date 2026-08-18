# 📘 Panduan Belajar Ujian Komprehensif — Teknik Informatika

Panduan lengkap untuk mempersiapkan ujian komprehensif yang mencakup **5 mata kuliah**. Setiap bagian berisi ringkasan teori, penjelasan konsep mendalam, contoh konkret, dan contoh pertanyaan beserta jawabannya.

| No | Mata Kuliah | Waktu |
|----|-------------|-------|
| 1 | [Database](#1️⃣-database) | 08.00 – 08.30 |
| 2 | [Programming](#2️⃣-programming) | 08.30 – 09.00 |
| 3 | [Information Technology](#3️⃣-information-technology) | 09.00 – 09.30 |
| 4 | [Web Systems and Technologies](#4️⃣-web-systems-and-technologies) | 09.30 – 10.00 |
| 5 | [Enterprise Networking](#5️⃣-enterprise-networking) | 09.30 – 10.00 |

---

# 1️⃣ Database

## Daftar Materi

- Database & DBMS
- Relational Database
- Primary Key & Foreign Key
- ERD (Entity Relationship Diagram)
- Normalisasi (1NF, 2NF, 3NF)
- SQL (SELECT, INSERT, UPDATE, DELETE)
- JOIN
- Relasi antar tabel
- Index
- Transaction
- MySQL

---

## Ringkasan Teori

### Apa itu Database dan DBMS?

**Database** adalah kumpulan data yang tersimpan secara terstruktur dan terorganisasi sehingga mudah diakses, dikelola, dan diperbarui.

**DBMS (Database Management System)** adalah software yang digunakan untuk mengelola database. DBMS menjadi perantara antara pengguna dengan database.

| Istilah | Penjelasan | Contoh |
|---------|------------|--------|
| Database | Kumpulan data terstruktur | Data mahasiswa, data nilai |
| DBMS | Software pengelola database | MySQL, PostgreSQL, Oracle, SQL Server |
| Relational Database | Database yang menyimpan data dalam bentuk **tabel** yang saling berhubungan | MySQL, PostgreSQL |

**Contoh analogi:**
- **Database** = Lemari arsip yang berisi banyak folder
- **Tabel** = Masing-masing folder (folder mahasiswa, folder nilai, folder dosen)
- **Baris (Row)** = Satu dokumen/data di dalam folder
- **Kolom (Column)** = Informasi spesifik dalam dokumen (nama, nim, alamat)
- **DBMS** = Petugas arsip yang membantu mencari, menambah, mengubah, dan menghapus data

**Contoh tabel database:**

```
Tabel: mahasiswa
+------+-----------+-------+----------+
| nim  | nama      | prodi | angkatan |
+------+-----------+-------+----------+
| 001  | Andi      | TI    | 2022     |
| 002  | Budi      | SI    | 2022     |
| 003  | Citra     | TI    | 2023     |
+------+-----------+-------+----------+
```

---

### Primary Key & Foreign Key

| Jenis Key | Definisi | Aturan |
|-----------|----------|--------|
| **Primary Key (PK)** | Kolom yang secara unik mengidentifikasi setiap baris | Harus **unik** dan **tidak boleh NULL** |
| **Foreign Key (FK)** | Kolom yang mereferensikan Primary Key di tabel lain | Menjaga **integritas referensial** |
| **Candidate Key** | Kolom yang berpotensi menjadi Primary Key | Bisa ada lebih dari satu |
| **Composite Key** | Primary Key yang terdiri dari **lebih dari satu kolom** | Contoh: (nim, kode_mk) |

**Contoh lengkap dengan 3 tabel:**

```sql
-- Tabel 1: mahasiswa (PK: nim)
CREATE TABLE mahasiswa (
    nim VARCHAR(10) PRIMARY KEY,        -- Primary Key
    nama VARCHAR(100) NOT NULL,
    prodi VARCHAR(50)
);

-- Tabel 2: mata_kuliah (PK: kode_mk)
CREATE TABLE mata_kuliah (
    kode_mk VARCHAR(10) PRIMARY KEY,    -- Primary Key
    nama_mk VARCHAR(100),
    sks INT
);

-- Tabel 3: nilai (PK: composite, FK: nim dan kode_mk)
CREATE TABLE nilai (
    nim VARCHAR(10),
    kode_mk VARCHAR(10),
    nilai CHAR(1),
    PRIMARY KEY (nim, kode_mk),                        -- Composite PK
    FOREIGN KEY (nim) REFERENCES mahasiswa(nim),        -- FK ke mahasiswa
    FOREIGN KEY (kode_mk) REFERENCES mata_kuliah(kode_mk) -- FK ke mata_kuliah
);
```

**Penjelasan hubungannya:**
```
mahasiswa (nim = PK)          mata_kuliah (kode_mk = PK)
  │                               │
  └──────── nilai ────────────────┘
       nim (FK)    kode_mk (FK)
       
Artinya: Tabel "nilai" menghubungkan mahasiswa dengan mata_kuliah.
Jika nim "001" tidak ada di tabel mahasiswa, maka kita TIDAK BISA
memasukkan nilai untuk nim "001" → ini disebut integritas referensial.
```

---

### ERD (Entity Relationship Diagram)

ERD adalah diagram yang menggambarkan **hubungan antar entitas** dalam database.

**Komponen ERD:**

| Komponen | Simbol | Penjelasan |
|----------|--------|------------|
| **Entitas** | Persegi panjang | Objek yang datanya disimpan (Mahasiswa, Dosen) |
| **Atribut** | Elips | Properti dari entitas (nim, nama, alamat) |
| **Relasi** | Belah ketupat | Hubungan antar entitas (mengambil, mengajar) |
| **Kardinalitas** | Garis + notasi | 1:1, 1:N, M:N |
| **Atribut Key** | Elips dengan garis bawah | Atribut yang menjadi Primary Key |
| **Atribut Multivalued** | Elips ganda | Atribut yang bisa punya banyak nilai (hobi, telepon) |
| **Atribut Derived** | Elips dengan garis putus-putus | Atribut yang dihitung dari atribut lain (umur dari tgl_lahir) |

**Jenis Kardinalitas dengan contoh detail:**

| Kardinalitas | Artinya | Contoh | Implementasi |
|-------------|---------|--------|-------------|
| **One-to-One (1:1)** | 1 entitas A berhubungan dengan tepat 1 entitas B | 1 mahasiswa punya 1 KTM | FK di salah satu tabel |
| **One-to-Many (1:N)** | 1 entitas A berhubungan dengan banyak entitas B | 1 dosen membimbing banyak mahasiswa | FK di tabel "banyak" (mahasiswa) |
| **Many-to-Many (M:N)** | Banyak entitas A berhubungan dengan banyak entitas B | Banyak mahasiswa mengambil banyak mata kuliah | Butuh **tabel perantara** (tabel nilai/enrollment) |

**Contoh ERD dalam bentuk teks:**

```
┌───────────────┐          ┌─────────────┐          ┌────────────────┐
│   MAHASISWA   │          │  MENGAMBIL  │          │  MATA_KULIAH   │
│───────────────│    M     │             │    N     │────────────────│
│ *nim          │──────────│   nilai     │──────────│ *kode_mk       │
│  nama         │          │   semester  │          │  nama_mk       │
│  prodi        │          │             │          │  sks           │
│  alamat       │          └─────────────┘          │  dosen         │
└───────────────┘                                   └────────────────┘

* = Primary Key
M:N = Many-to-Many → butuh tabel perantara "mengambil"
```

**Langkah membuat ERD:**
1. Identifikasi **entitas** (mahasiswa, dosen, mata kuliah, ruangan)
2. Tentukan **atribut** setiap entitas
3. Tentukan **Primary Key** (atribut unik)
4. Tentukan **relasi** antar entitas
5. Tentukan **kardinalitas** (1:1, 1:N, M:N)

---

### Normalisasi

Normalisasi adalah proses **mengorganisasi data** dalam database untuk mengurangi **redundansi** (data berulang) dan meningkatkan **integritas data**.

**Kenapa normalisasi penting?**
- Menghindari **anomali insert** (tidak bisa tambah data karena data lain belum ada)
- Menghindari **anomali update** (harus update data di banyak tempat)
- Menghindari **anomali delete** (hapus satu data malah kehilangan data lain)

| Bentuk Normal | Syarat | Masalah yang Dihilangkan |
|---------------|--------|--------------------------|
| **1NF** | Setiap kolom berisi nilai **atomik** (tunggal, tidak ada multi-value) | Data ganda dalam satu sel |
| **2NF** | Sudah 1NF + **tidak ada ketergantungan parsial** (semua atribut non-key bergantung sepenuhnya pada seluruh PK) | Ketergantungan parsial |
| **3NF** | Sudah 2NF + **tidak ada ketergantungan transitif** (atribut non-key tidak bergantung pada atribut non-key lain) | Ketergantungan transitif |

---

### 📌 Contoh Lengkap Normalisasi: 1NF → 2NF → 3NF

**Studi Kasus:** Data pemesanan barang di sebuah toko.

#### ❌ Tabel Awal (Belum Normal / UNF)

| no_pesanan | tgl_pesan | nama_pelanggan | alamat | kode_barang | nama_barang | harga | qty |
|------------|-----------|----------------|--------|-------------|-------------|-------|-----|
| P001 | 2026-01-10 | Andi | Jakarta | B01, B02 | Buku, Pensil | 15000, 3000 | 2, 5 |
| P002 | 2026-01-11 | Budi | Bandung | B01 | Buku | 15000 | 1 |

**Masalah:** Kolom `kode_barang`, `nama_barang`, `harga`, dan `qty` mengandung **banyak nilai dalam satu sel** → melanggar 1NF.

**Anomali yang terjadi:**
- **Anomali Insert:** Tidak bisa menambah barang baru tanpa ada pesanan
- **Anomali Update:** Jika harga Buku berubah, harus update di semua baris
- **Anomali Delete:** Jika pesanan P001 dihapus, data barang Pensil juga hilang

---

#### ✅ Tahap 1: Normalisasi ke 1NF

> **Aturan 1NF:** Setiap kolom hanya berisi **satu nilai (atomik)**. Tidak boleh ada multi-value atau repeating group.

**Cara:** Pecah baris yang mengandung banyak nilai menjadi baris terpisah.

| no_pesanan | tgl_pesan | nama_pelanggan | alamat | kode_barang | nama_barang | harga | qty |
|------------|-----------|----------------|--------|-------------|-------------|-------|-----|
| P001 | 2026-01-10 | Andi | Jakarta | B01 | Buku | 15000 | 2 |
| P001 | 2026-01-10 | Andi | Jakarta | B02 | Pensil | 3000 | 5 |
| P002 | 2026-01-11 | Budi | Bandung | B01 | Buku | 15000 | 1 |

✅ Sekarang setiap sel berisi **satu nilai saja** → sudah **1NF**.

**Primary Key (Composite):** `(no_pesanan, kode_barang)`

**Masalah yang masih ada:**
- `nama_pelanggan` dan `alamat` hanya bergantung pada `no_pesanan` saja (bukan pada `kode_barang`) → **ketergantungan parsial**
- `nama_barang` dan `harga` hanya bergantung pada `kode_barang` saja → **ketergantungan parsial**

**Ketergantungan parsial artinya:**
```
PK = (no_pesanan, kode_barang)

nama_pelanggan bergantung pada no_pesanan SAJA ← parsial (tidak butuh kode_barang)
nama_barang bergantung pada kode_barang SAJA   ← parsial (tidak butuh no_pesanan)
qty bergantung pada (no_pesanan + kode_barang)  ← full dependency (OK)
```

---

#### ✅ Tahap 2: Normalisasi ke 2NF

> **Aturan 2NF:** Sudah 1NF + **tidak ada ketergantungan parsial**. Semua atribut non-key harus bergantung pada **seluruh** Primary Key (bukan hanya sebagian PK).

**Cara:** Pisahkan atribut yang hanya bergantung pada **sebagian PK** ke tabel terpisah.

**Tabel Pesanan:**

| no_pesanan | tgl_pesan | nama_pelanggan | alamat |
|------------|-----------|----------------|--------|
| P001 | 2026-01-10 | Andi | Jakarta |
| P002 | 2026-01-11 | Budi | Bandung |

> PK: `no_pesanan` — `tgl_pesan`, `nama_pelanggan`, `alamat` bergantung sepenuhnya pada `no_pesanan`.

**Tabel Barang:**

| kode_barang | nama_barang | harga |
|-------------|-------------|-------|
| B01 | Buku | 15000 |
| B02 | Pensil | 3000 |

> PK: `kode_barang` — `nama_barang` dan `harga` bergantung sepenuhnya pada `kode_barang`.

**Tabel Detail Pesanan:**

| no_pesanan | kode_barang | qty |
|------------|-------------|-----|
| P001 | B01 | 2 |
| P001 | B02 | 5 |
| P002 | B01 | 1 |

> PK: `(no_pesanan, kode_barang)` — `qty` bergantung pada seluruh composite key.

✅ Sekarang **tidak ada ketergantungan parsial** → sudah **2NF**.

**Masalah yang masih ada:**
- Di tabel Pesanan: `alamat` bergantung pada `nama_pelanggan`, bukan langsung pada `no_pesanan` → **ketergantungan transitif** (`no_pesanan → nama_pelanggan → alamat`)

**Ketergantungan transitif artinya:**
```
no_pesanan → nama_pelanggan → alamat

"alamat" tidak langsung bergantung pada PK (no_pesanan),
tapi bergantung pada "nama_pelanggan" yang juga bukan key.
Jika Andi pindah alamat, kita harus update di SEMUA pesanan Andi.
```

---

#### ✅ Tahap 3: Normalisasi ke 3NF

> **Aturan 3NF:** Sudah 2NF + **tidak ada ketergantungan transitif**. Atribut non-key tidak boleh bergantung pada atribut non-key lain.

**Cara:** Pisahkan atribut yang mengalami ketergantungan transitif ke tabel terpisah.

**Tabel Pesanan (diperbarui):**

| no_pesanan | tgl_pesan | id_pelanggan |
|------------|-----------|-------------|
| P001 | 2026-01-10 | C01 |
| P002 | 2026-01-11 | C02 |

> PK: `no_pesanan`, FK: `id_pelanggan`

**Tabel Pelanggan (baru):**

| id_pelanggan | nama_pelanggan | alamat |
|-------------|----------------|--------|
| C01 | Andi | Jakarta |
| C02 | Budi | Bandung |

> PK: `id_pelanggan` — `nama_pelanggan` dan `alamat` bergantung langsung pada `id_pelanggan`.

**Tabel Barang (tetap):**

| kode_barang | nama_barang | harga |
|-------------|-------------|-------|
| B01 | Buku | 15000 |
| B02 | Pensil | 3000 |

**Tabel Detail Pesanan (tetap):**

| no_pesanan | kode_barang | qty |
|------------|-------------|-----|
| P001 | B01 | 2 |
| P001 | B02 | 5 |
| P002 | B01 | 1 |

✅ Tidak ada ketergantungan transitif → sudah **3NF**!

**Keuntungan setelah 3NF:**
- ✅ Andi pindah alamat? Cukup update 1 baris di tabel Pelanggan
- ✅ Tambah barang baru? Langsung insert ke tabel Barang (tanpa perlu ada pesanan)
- ✅ Hapus pesanan? Data pelanggan dan barang tetap aman

---

#### 🔑 Ringkasan Proses Normalisasi

```
UNF  → 1NF : Hilangkan multi-value (setiap sel hanya 1 nilai)
1NF  → 2NF : Hilangkan ketergantungan parsial (pisahkan ke tabel sesuai dependensi PK)
2NF  → 3NF : Hilangkan ketergantungan transitif (pisahkan atribut non-key yang bergantung pada non-key lain)
```

**Hasil akhir (4 tabel):**

```
┌─────────────┐     ┌──────────────────┐     ┌────────────┐
│  Pelanggan  │←─FK─│     Pesanan      │     │   Barang   │
│ id_pelanggan│     │ no_pesanan       │     │kode_barang │
│ nama        │     │ tgl_pesan        │     │nama_barang │
│ alamat      │     │ id_pelanggan(FK) │     │harga       │
└─────────────┘     └───────┬──────────┘     └─────┬──────┘
                            │                      │
                     ┌──────┴──────────────────────┴──────┐
                     │        Detail Pesanan              │
                     │ no_pesanan (FK)                     │
                     │ kode_barang (FK)                    │
                     │ qty                                 │
                     └────────────────────────────────────┘
```

**SQL untuk membuat hasil akhir 3NF:**

```sql
CREATE TABLE pelanggan (
    id_pelanggan VARCHAR(5) PRIMARY KEY,
    nama_pelanggan VARCHAR(100),
    alamat VARCHAR(200)
);

CREATE TABLE pesanan (
    no_pesanan VARCHAR(10) PRIMARY KEY,
    tgl_pesan DATE,
    id_pelanggan VARCHAR(5),
    FOREIGN KEY (id_pelanggan) REFERENCES pelanggan(id_pelanggan)
);

CREATE TABLE barang (
    kode_barang VARCHAR(5) PRIMARY KEY,
    nama_barang VARCHAR(100),
    harga INT
);

CREATE TABLE detail_pesanan (
    no_pesanan VARCHAR(10),
    kode_barang VARCHAR(5),
    qty INT,
    PRIMARY KEY (no_pesanan, kode_barang),
    FOREIGN KEY (no_pesanan) REFERENCES pesanan(no_pesanan),
    FOREIGN KEY (kode_barang) REFERENCES barang(kode_barang)
);
```

---

### SQL (Structured Query Language)

SQL adalah bahasa standar untuk mengelola dan memanipulasi database relasional.

**Kategori SQL:**

| Kategori | Singkatan | Fungsi | Perintah |
|----------|-----------|--------|----------|
| **DDL** | Data Definition Language | Mendefinisikan struktur database | `CREATE`, `ALTER`, `DROP`, `TRUNCATE` |
| **DML** | Data Manipulation Language | Memanipulasi data | `SELECT`, `INSERT`, `UPDATE`, `DELETE` |
| **DCL** | Data Control Language | Mengatur hak akses | `GRANT`, `REVOKE` |
| **TCL** | Transaction Control Language | Mengatur transaksi | `COMMIT`, `ROLLBACK`, `SAVEPOINT` |

---

**DDL (Data Definition Language) — contoh lengkap:**

```sql
-- ========== CREATE ==========
-- Membuat database
CREATE DATABASE toko_online;
USE toko_online;

-- Membuat tabel dengan berbagai constraint
CREATE TABLE mahasiswa (
    nim VARCHAR(10) PRIMARY KEY,           -- Primary Key
    nama VARCHAR(100) NOT NULL,            -- Tidak boleh kosong
    email VARCHAR(100) UNIQUE,             -- Harus unik
    prodi VARCHAR(50) DEFAULT 'TI',        -- Nilai default
    angkatan YEAR,
    ipk DECIMAL(3,2) CHECK (ipk >= 0 AND ipk <= 4.00)  -- Validasi range
);

-- ========== ALTER ==========
-- Menambah kolom baru
ALTER TABLE mahasiswa ADD COLUMN telepon VARCHAR(15);

-- Mengubah tipe data kolom
ALTER TABLE mahasiswa MODIFY COLUMN telepon VARCHAR(20);

-- Menghapus kolom
ALTER TABLE mahasiswa DROP COLUMN telepon;

-- Menambah constraint
ALTER TABLE mahasiswa ADD CONSTRAINT chk_angkatan CHECK (angkatan >= 2000);

-- ========== DROP ==========
-- Menghapus tabel (HATI-HATI: data & struktur hilang semua)
DROP TABLE mahasiswa;

-- Menghapus database
DROP DATABASE toko_online;

-- ========== TRUNCATE ==========
-- Menghapus semua data, tapi struktur tabel tetap ada
TRUNCATE TABLE mahasiswa;
```

**Perbedaan DROP, TRUNCATE, DELETE:**

| Perintah | Yang Dihapus | Bisa WHERE? | Bisa ROLLBACK? | Kecepatan |
|----------|-------------|-------------|----------------|-----------|
| **DELETE** | Baris data (bisa pilih) | ✅ Ya | ✅ Ya | Lambat |
| **TRUNCATE** | Semua data (struktur tetap) | ❌ Tidak | ❌ Tidak | Cepat |
| **DROP** | Data + struktur tabel | ❌ Tidak | ❌ Tidak | Cepat |

---

**DML (Data Manipulation Language) — contoh lengkap:**

```sql
-- ========== INSERT ==========
-- Insert satu baris
INSERT INTO mahasiswa (nim, nama, prodi, angkatan) 
VALUES ('001', 'Andi', 'TI', 2022);

-- Insert banyak baris sekaligus
INSERT INTO mahasiswa (nim, nama, prodi, angkatan) VALUES 
('002', 'Budi', 'SI', 2022),
('003', 'Citra', 'TI', 2023),
('004', 'Dina', 'TI', 2023);

-- ========== SELECT ==========
-- Ambil semua data
SELECT * FROM mahasiswa;

-- Ambil kolom tertentu
SELECT nim, nama FROM mahasiswa;

-- Filter dengan WHERE
SELECT * FROM mahasiswa WHERE prodi = 'TI';
SELECT * FROM mahasiswa WHERE angkatan = 2022 AND prodi = 'TI';
SELECT * FROM mahasiswa WHERE prodi = 'TI' OR prodi = 'SI';
SELECT * FROM mahasiswa WHERE angkatan BETWEEN 2022 AND 2023;
SELECT * FROM mahasiswa WHERE nama LIKE 'A%';    -- nama diawali 'A'
SELECT * FROM mahasiswa WHERE nama LIKE '%di';   -- nama diakhiri 'di'
SELECT * FROM mahasiswa WHERE nama LIKE '%nd%';  -- nama mengandung 'nd'
SELECT * FROM mahasiswa WHERE prodi IN ('TI', 'SI', 'MI');

-- Urutkan hasil
SELECT * FROM mahasiswa ORDER BY nama ASC;    -- A-Z
SELECT * FROM mahasiswa ORDER BY angkatan DESC; -- terbaru dulu

-- Batasi jumlah hasil
SELECT * FROM mahasiswa LIMIT 10;
SELECT * FROM mahasiswa LIMIT 5 OFFSET 10;  -- skip 10, ambil 5

-- Hilangkan duplikat
SELECT DISTINCT prodi FROM mahasiswa;

-- ========== UPDATE ==========
-- Update dengan kondisi (SELALU pakai WHERE!)
UPDATE mahasiswa SET nama = 'Andi Wijaya' WHERE nim = '001';
UPDATE mahasiswa SET prodi = 'SI', angkatan = 2023 WHERE nim = '002';

-- ⚠️ BAHAYA: tanpa WHERE → semua data berubah!
-- UPDATE mahasiswa SET prodi = 'TI';  ← JANGAN!

-- ========== DELETE ==========
-- Hapus dengan kondisi
DELETE FROM mahasiswa WHERE nim = '004';

-- ⚠️ BAHAYA: tanpa WHERE → semua data terhapus!
-- DELETE FROM mahasiswa;  ← JANGAN!
```

---

**Klausa penting dalam SELECT:**

| Klausa | Fungsi | Contoh |
|--------|--------|--------|
| `WHERE` | Filter baris sebelum grouping | `WHERE prodi = 'TI'` |
| `ORDER BY` | Mengurutkan hasil | `ORDER BY nama ASC` |
| `GROUP BY` | Mengelompokkan data | `GROUP BY prodi` |
| `HAVING` | Filter setelah GROUP BY | `HAVING COUNT(*) > 5` |
| `LIMIT` | Membatasi jumlah hasil | `LIMIT 10` |
| `DISTINCT` | Menghilangkan duplikat | `SELECT DISTINCT prodi` |
| `LIKE` | Pencarian pola | `WHERE nama LIKE '%andi%'` |
| `BETWEEN` | Range nilai | `WHERE angkatan BETWEEN 2020 AND 2024` |
| `IN` | Daftar nilai | `WHERE prodi IN ('TI', 'SI')` |
| `IS NULL` | Cek nilai kosong | `WHERE email IS NULL` |

**Urutan eksekusi klausa SQL:**
```
FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT
(bukan sesuai urutan penulisan!)
```

---

**Aggregate Function — contoh penggunaan:**

| Fungsi | Kegunaan | Contoh |
|--------|----------|--------|
| `COUNT()` | Menghitung jumlah baris | `SELECT COUNT(*) FROM mahasiswa` |
| `SUM()` | Menjumlahkan nilai | `SELECT SUM(sks) FROM mata_kuliah` |
| `AVG()` | Menghitung rata-rata | `SELECT AVG(ipk) FROM mahasiswa` |
| `MAX()` | Nilai tertinggi | `SELECT MAX(ipk) FROM mahasiswa` |
| `MIN()` | Nilai terendah | `SELECT MIN(ipk) FROM mahasiswa` |

```sql
-- Hitung jumlah mahasiswa per prodi
SELECT prodi, COUNT(*) AS jumlah_mhs
FROM mahasiswa
GROUP BY prodi;

-- Hasil:
-- TI  | 15
-- SI  | 10
-- MI  | 8

-- Hitung rata-rata IPK per prodi, hanya tampilkan yang > 3.0
SELECT prodi, AVG(ipk) AS rata_rata_ipk
FROM mahasiswa
GROUP BY prodi
HAVING AVG(ipk) > 3.0;

-- Perbedaan WHERE vs HAVING:
-- WHERE  → filter SEBELUM GROUP BY (filter baris individual)
-- HAVING → filter SESUDAH GROUP BY (filter hasil agregat/grup)
```

---

### JOIN

JOIN digunakan untuk **menggabungkan data dari dua atau lebih tabel** berdasarkan kolom yang berhubungan.

**Contoh data untuk memahami JOIN:**

```
Tabel: mahasiswa                  Tabel: nilai
+------+--------+               +------+---------+-------+
| nim  | nama   |               | nim  | kode_mk | nilai |
+------+--------+               +------+---------+-------+
| 001  | Andi   |               | 001  | MK01    | A     |
| 002  | Budi   |               | 001  | MK02    | B     |
| 003  | Citra  |               | 002  | MK01    | A     |
+------+--------+               | 004  | MK01    | C     |  ← nim 004 tidak ada di mahasiswa
                                +------+---------+-------+
                                  ↑ nim 003 tidak ada di nilai
```

| Tipe JOIN | Penjelasan | Hasil dengan data di atas |
|-----------|------------|---------------------------|
| **INNER JOIN** | Hanya data yang **cocok di kedua tabel** | Andi (001), Budi (002) — tanpa Citra (003) dan tanpa 004 |
| **LEFT JOIN** | **Semua data tabel kiri** + data cocok dari kanan | Andi, Budi, **Citra (NULL)** — tanpa 004 |
| **RIGHT JOIN** | **Semua data tabel kanan** + data cocok dari kiri | Andi, Budi, **004 (NULL)** — tanpa Citra |
| **FULL OUTER JOIN** | **Semua data dari kedua tabel** | Andi, Budi, **Citra (NULL)**, **004 (NULL)** |
| **CROSS JOIN** | Semua kombinasi baris (kartesian) | 3 × 4 = 12 baris kombinasi |

```sql
-- INNER JOIN: hanya mahasiswa yang PUNYA nilai
SELECT m.nim, m.nama, n.kode_mk, n.nilai
FROM mahasiswa m
INNER JOIN nilai n ON m.nim = n.nim;
-- Hasil: Andi-MK01-A, Andi-MK02-B, Budi-MK01-A

-- LEFT JOIN: SEMUA mahasiswa, meskipun belum ada nilai
SELECT m.nim, m.nama, n.kode_mk, n.nilai
FROM mahasiswa m
LEFT JOIN nilai n ON m.nim = n.nim;
-- Hasil: Andi-MK01-A, Andi-MK02-B, Budi-MK01-A, Citra-NULL-NULL

-- RIGHT JOIN: SEMUA nilai, meskipun nim tidak ada di mahasiswa
SELECT m.nim, m.nama, n.kode_mk, n.nilai
FROM mahasiswa m
RIGHT JOIN nilai n ON m.nim = n.nim;
-- Hasil: Andi-MK01-A, Andi-MK02-B, Budi-MK01-A, NULL-MK01-C (nim 004)

-- JOIN lebih dari 2 tabel
SELECT m.nama, mk.nama_mk, n.nilai
FROM mahasiswa m
INNER JOIN nilai n ON m.nim = n.nim
INNER JOIN mata_kuliah mk ON n.kode_mk = mk.kode_mk;
```

**Self JOIN — tabel join dengan dirinya sendiri:**

```sql
-- Contoh: tabel karyawan di mana setiap karyawan punya manager_id
-- yang merujuk ke id karyawan lain

SELECT k.nama AS karyawan, m.nama AS manager
FROM karyawan k
LEFT JOIN karyawan m ON k.manager_id = m.id;
```

---

### Subquery (Query di dalam Query)

**Subquery** adalah query SELECT yang **diletakkan di dalam query lain**.

```sql
-- Subquery di WHERE: cari mahasiswa yang punya nilai A
SELECT nama FROM mahasiswa
WHERE nim IN (
    SELECT nim FROM nilai WHERE nilai = 'A'
);

-- Subquery di FROM (Derived Table)
SELECT prodi, avg_ipk
FROM (
    SELECT prodi, AVG(ipk) AS avg_ipk
    FROM mahasiswa
    GROUP BY prodi
) AS sub
WHERE avg_ipk > 3.0;

-- Subquery dengan EXISTS: cari mahasiswa yang sudah punya nilai
SELECT nama FROM mahasiswa m
WHERE EXISTS (
    SELECT 1 FROM nilai n WHERE n.nim = m.nim
);
```

---

### Index

Index adalah **struktur data** (biasanya B-Tree) yang mempercepat pencarian data pada tabel. Analogi: seperti **daftar isi** di buku yang membantu menemukan halaman tertentu tanpa membaca seluruh buku.

| Aspek | Tanpa Index | Dengan Index |
|-------|-------------|-------------|
| **SELECT/WHERE** | Full table scan — lambat | Langsung ke lokasi — cepat |
| **INSERT/UPDATE/DELETE** | Normal | Lebih lambat (index harus diupdate) |
| **Storage** | Normal | Membutuhkan ruang tambahan |

```sql
-- Membuat index pada kolom yang sering dicari
CREATE INDEX idx_nama ON mahasiswa(nama);

-- Index unik (tidak boleh ada duplikat)
CREATE UNIQUE INDEX idx_email ON mahasiswa(email);

-- Composite index (2+ kolom)
CREATE INDEX idx_prodi_angkatan ON mahasiswa(prodi, angkatan);

-- Melihat semua index di tabel
SHOW INDEX FROM mahasiswa;

-- Menghapus index
DROP INDEX idx_nama ON mahasiswa;
```

**Kapan PAKAI index:**
- Kolom yang sering dipakai di `WHERE`, `JOIN`, atau `ORDER BY`
- Kolom dengan banyak nilai unik (high cardinality)
- Tabel dengan data besar (ribuan/jutaan baris)

**Kapan JANGAN pakai index:**
- Tabel kecil (< 1000 baris) → full scan lebih cepat
- Kolom yang jarang dipakai di query
- Kolom yang sering di-update
- Kolom dengan sedikit nilai unik (contoh: kolom `jenis_kelamin` hanya L/P)

---

### Transaction & ACID

Transaction adalah serangkaian operasi database yang dieksekusi sebagai **satu kesatuan yang utuh**. Jika satu operasi gagal, semua operasi dibatalkan.

| Properti ACID | Penjelasan | Contoh |
|---------------|------------|--------|
| **Atomicity** | Semua berhasil, atau semua dibatalkan | Transfer uang: kurangi saldo A DAN tambah saldo B (tidak boleh setengah) |
| **Consistency** | Database tetap valid sebelum dan sesudah | Total saldo A+B tetap sama sebelum dan sesudah transfer |
| **Isolation** | Setiap transaksi berjalan independen | Dua transfer bersamaan tidak saling mempengaruhi |
| **Durability** | Data yang sudah COMMIT tersimpan permanen | Meskipun server mati setelah COMMIT, data tetap ada |

**Contoh studi kasus — Transfer uang dari rekening A ke B:**

```sql
-- Saldo awal: A = Rp 1.000.000, B = Rp 500.000
-- Transfer Rp 300.000 dari A ke B

START TRANSACTION;

-- Step 1: Kurangi saldo A
UPDATE rekening SET saldo = saldo - 300000 WHERE id = 'A';
-- Saldo A sekarang: 700.000

-- Step 2: Tambah saldo B
UPDATE rekening SET saldo = saldo + 300000 WHERE id = 'B';
-- Saldo B sekarang: 800.000

-- Cek: apakah saldo A cukup?
-- Jika semua berhasil:
COMMIT;    -- Simpan permanen. Saldo A=700K, B=800K

-- Jika ada error di tengah jalan:
ROLLBACK;  -- Batalkan semua. Saldo kembali A=1M, B=500K
```

**SAVEPOINT — titik simpan sementara:**

```sql
START TRANSACTION;

INSERT INTO pesanan VALUES ('P001', '2026-01-10', 'C01');
SAVEPOINT sp1;  -- tandai titik ini

INSERT INTO detail_pesanan VALUES ('P001', 'B01', 2);
-- Oops, ternyata ada error di detail
ROLLBACK TO sp1;  -- kembali ke SAVEPOINT, pesanan tetap ada

INSERT INTO detail_pesanan VALUES ('P001', 'B02', 3);  -- coba lagi
COMMIT;
```

---

### MySQL — Hal-hal Spesifik

| Fitur | Penjelasan |
|-------|------------|
| **AUTO_INCREMENT** | ID otomatis bertambah 1 setiap insert |
| **ENGINE** | InnoDB (default, support transaction) vs MyISAM (cepat, no transaction) |
| **SHOW DATABASES** | Melihat semua database |
| **SHOW TABLES** | Melihat semua tabel dalam database |
| **DESCRIBE** | Melihat struktur tabel |
| **EXPLAIN** | Melihat rencana eksekusi query (untuk optimasi) |

```sql
-- Melihat semua database
SHOW DATABASES;

-- Menggunakan database tertentu
USE nama_database;

-- Melihat semua tabel
SHOW TABLES;

-- Melihat struktur tabel
DESCRIBE mahasiswa;
-- atau
SHOW COLUMNS FROM mahasiswa;

-- Melihat bagaimana MySQL mengeksekusi query (untuk optimasi)
EXPLAIN SELECT * FROM mahasiswa WHERE nama = 'Andi';

-- View: tabel virtual dari query
CREATE VIEW v_mhs_ti AS
SELECT nim, nama FROM mahasiswa WHERE prodi = 'TI';

SELECT * FROM v_mhs_ti;  -- gunakan seperti tabel biasa
```

---

## Contoh Pertanyaan & Jawaban — Database

<details>
<summary><b>Q: Apa perbedaan WHERE dan HAVING?</b></summary>

| Aspek | WHERE | HAVING |
|-------|-------|--------|
| **Waktu filter** | **Sebelum** GROUP BY | **Sesudah** GROUP BY |
| **Digunakan dengan** | Kolom biasa | Aggregate function |
| **Contoh** | `WHERE prodi = 'TI'` | `HAVING COUNT(*) > 5` |

```sql
-- WHERE: filter baris individual sebelum grouping
SELECT prodi, COUNT(*) as jumlah
FROM mahasiswa
WHERE angkatan = 2022     -- filter: hanya angkatan 2022
GROUP BY prodi;

-- HAVING: filter hasil GROUP setelah grouping
SELECT prodi, COUNT(*) as jumlah
FROM mahasiswa
GROUP BY prodi
HAVING COUNT(*) > 10;     -- filter: hanya grup yang > 10 orang

-- Gabungan WHERE + HAVING
SELECT prodi, AVG(ipk) as rata_ipk
FROM mahasiswa
WHERE angkatan = 2022     -- 1. filter baris: angkatan 2022
GROUP BY prodi             -- 2. kelompokkan per prodi
HAVING AVG(ipk) > 3.0;    -- 3. filter grup: rata IPK > 3.0
```
</details>

<details>
<summary><b>Q: Apa perbedaan Primary Key dan Unique Key?</b></summary>

| Aspek | Primary Key | Unique Key |
|-------|-------------|------------|
| **Jumlah per tabel** | Hanya **1** | Bisa **banyak** |
| **NULL** | **Tidak boleh** NULL | **Boleh** NULL (sekali) |
| **Fungsi** | Identitas utama baris | Menjamin keunikan kolom |
| **Index** | Otomatis clustered index | Non-clustered index |

```sql
CREATE TABLE mahasiswa (
    nim VARCHAR(10) PRIMARY KEY,    -- PK: hanya 1, tidak boleh NULL
    email VARCHAR(100) UNIQUE,      -- UK: boleh NULL, tapi jika diisi harus unik
    nik VARCHAR(16) UNIQUE          -- UK kedua: boleh banyak UNIQUE
);
```
</details>

<details>
<summary><b>Q: Apa itu View dan apa manfaatnya?</b></summary>

**View** adalah **tabel virtual** yang dibuat dari query `SELECT`. View **tidak menyimpan data** sendiri, hanya menyimpan query-nya.

```sql
-- Membuat view
CREATE VIEW v_mahasiswa_aktif AS
SELECT nim, nama, prodi
FROM mahasiswa
WHERE status = 'aktif';

-- Menggunakan view (seperti tabel biasa)
SELECT * FROM v_mahasiswa_aktif WHERE prodi = 'TI';
```

**Manfaat View:**
1. **Menyederhanakan query kompleks** — query JOIN panjang bisa dijadikan view
2. **Keamanan** — user tertentu hanya boleh akses view, bukan tabel asli
3. **Konsistensi** — semua user melihat data yang sama melalui view yang sama
4. **Abstraksi** — struktur tabel bisa berubah tanpa mempengaruhi query yang pakai view
</details>

<details>
<summary><b>Q: Jelaskan perbedaan SQL vs NoSQL!</b></summary>

| Aspek | SQL (Relasional) | NoSQL (Non-Relasional) |
|-------|-----------------|----------------------|
| **Struktur** | Tabel dengan baris & kolom (schema fixed) | Document, key-value, graph, column-family |
| **Schema** | Rigid (harus didefinisikan dulu) | Flexible (bisa berubah-ubah) |
| **Relasi** | JOIN antar tabel | Biasanya embedded/nested document |
| **Skalabilitas** | Vertical scaling (server lebih kuat) | Horizontal scaling (tambah server) |
| **Contoh** | MySQL, PostgreSQL, Oracle | MongoDB, Redis, Cassandra |
| **Cocok untuk** | Data terstruktur, transaksi keuangan | Big data, real-time app, data tidak terstruktur |
</details>

---

# 2️⃣ Programming

## Daftar Materi

- Algoritma & Pemrograman
- Variabel & Tipe Data
- Percabangan (if-else, switch)
- Perulangan (for, while, do-while)
- Function / Method
- OOP (Object-Oriented Programming)
- Class & Object
- Inheritance, Encapsulation, Polymorphism, Abstraction
- Error Handling
- Struktur Data Dasar
- Konsep API / Programming Modern

---

## Ringkasan Teori

### Algoritma & Pemrograman

**Algoritma** adalah langkah-langkah logis dan sistematis untuk menyelesaikan suatu masalah.

**Ciri-ciri algoritma yang baik:**
1. **Finite** — Memiliki akhir (berhenti setelah langkah tertentu)
2. **Definite** — Setiap langkah jelas dan tidak ambigu
3. **Input** — Memiliki nol atau lebih input
4. **Output** — Menghasilkan minimal satu output
5. **Effective** — Setiap langkah bisa dikerjakan dalam waktu terbatas

**Contoh algoritma dalam kehidupan sehari-hari — Membuat kopi:**
```
1. Siapkan gelas
2. Masukkan kopi 1 sendok ke dalam gelas
3. Masukkan gula 2 sendok ke dalam gelas
4. Tuang air panas ke dalam gelas
5. Aduk sampai rata
6. Kopi siap diminum
```

**Cara merepresentasikan algoritma:**

| Metode | Penjelasan | Contoh |
|--------|------------|--------|
| **Pseudocode** | Ditulis menyerupai bahasa pemrograman tapi dalam bahasa manusia | `JIKA umur >= 17 MAKA cetak "Boleh SIM"` |
| **Flowchart** | Menggunakan simbol diagram alir | Oval (mulai/selesai), Kotak (proses), Belah ketupat (keputusan) |
| **Kode program** | Langsung ditulis dalam bahasa pemrograman | `if (umur >= 17) print("Boleh SIM");` |

**Simbol flowchart yang wajib diketahui:**

| Simbol | Nama | Fungsi |
|--------|------|--------|
| Oval | Terminator | Mulai / Selesai |
| Persegi panjang | Process | Proses/instruksi |
| Belah ketupat | Decision | Percabangan (if-else) |
| Jajaran genjang | Input/Output | Menerima input atau menampilkan output |
| Panah | Flow line | Menunjukkan arah alur |

---

### Variabel & Tipe Data

**Variabel** adalah tempat penyimpanan data di memori yang memiliki **nama**, **tipe**, dan **nilai**.

| Tipe Data | Contoh Nilai | Ukuran | Keterangan |
|-----------|-------------|--------|------------|
| **int** | 1, 42, -7 | 4 byte | Bilangan bulat |
| **long** | 9999999999 | 8 byte | Bilangan bulat besar |
| **float** | 3.14f | 4 byte | Desimal (presisi rendah) |
| **double** | 3.141592653 | 8 byte | Desimal (presisi tinggi) |
| **char** | 'A', 'z' | 2 byte | Satu karakter |
| **String** | "Hello World" | Variabel | Kumpulan karakter |
| **boolean** | true, false | 1 bit | Nilai kebenaran |

```java
// Deklarasi variabel
int umur;              // deklarasi saja
umur = 21;             // inisialisasi

// Deklarasi + inisialisasi langsung
int umur = 21;
double ipk = 3.75;
String nama = "Andi";
boolean lulus = true;
char grade = 'A';

// Konstanta (tidak bisa diubah)
final double PI = 3.14159;
// PI = 3.0;  ← ERROR! konstanta tidak bisa diubah
```

**Aturan penamaan variabel:**
- Diawali huruf, underscore (_), atau dollar sign ($)
- Tidak boleh diawali angka
- Tidak boleh menggunakan reserved word (int, class, return, dll)
- Case-sensitive (`nama` ≠ `Nama`)
- Konvensi: camelCase (`namaLengkap`, `tanggalLahir`)

**Konversi tipe data (Type Casting):**

```java
// Implicit casting (otomatis): tipe kecil → tipe besar
int angka = 10;
double desimal = angka;  // 10 → 10.0 (otomatis)

// Explicit casting (manual): tipe besar → tipe kecil
double harga = 99.99;
int hargaBulat = (int) harga;  // 99.99 → 99 (desimal hilang)

// String → int
String teks = "123";
int angka = Integer.parseInt(teks);  // "123" → 123

// int → String
int angka = 123;
String teks = String.valueOf(angka);  // 123 → "123"
```

---

### Operator

| Kategori | Operator | Contoh |
|----------|----------|--------|
| **Aritmatika** | `+`, `-`, `*`, `/`, `%` (modulus) | `10 % 3 = 1` (sisa bagi) |
| **Perbandingan** | `==`, `!=`, `>`, `<`, `>=`, `<=` | `5 == 5` → true |
| **Logika** | `&&` (AND), `||` (OR), `!` (NOT) | `true && false` → false |
| **Assignment** | `=`, `+=`, `-=`, `*=`, `/=` | `x += 5` sama dengan `x = x + 5` |
| **Increment/Decrement** | `++`, `--` | `i++` sama dengan `i = i + 1` |

```java
// Contoh operator logika
int umur = 20;
boolean punyaSIM = true;

if (umur >= 17 && punyaSIM) {
    System.out.println("Boleh menyetir");
}

// Modulus (%) — sering dipakai untuk cek genap/ganjil
if (angka % 2 == 0) {
    System.out.println("Genap");
} else {
    System.out.println("Ganjil");
}
```

---

### Percabangan

Percabangan digunakan untuk membuat **keputusan** berdasarkan kondisi tertentu.

**if-else — contoh lengkap:**

```java
// if sederhana
if (nilai >= 80) {
    System.out.println("Lulus");
}

// if-else
if (nilai >= 80) {
    System.out.println("Lulus");
} else {
    System.out.println("Tidak lulus");
}

// if-else if-else (bertingkat)
int nilai = 75;
if (nilai >= 85) {
    System.out.println("A");
} else if (nilai >= 70) {
    System.out.println("B");      // ← ini yang dieksekusi (75 >= 70)
} else if (nilai >= 55) {
    System.out.println("C");
} else if (nilai >= 40) {
    System.out.println("D");
} else {
    System.out.println("E");
}

// Nested if (if di dalam if)
if (umur >= 17) {
    if (punyaSIM) {
        System.out.println("Boleh menyetir");
    } else {
        System.out.println("Buat SIM dulu");
    }
}

// Ternary operator (if-else singkat)
String status = (nilai >= 70) ? "Lulus" : "Tidak Lulus";
```

**switch-case — contoh lengkap:**

```java
// Switch dengan tipe int
int hari = 3;
switch (hari) {
    case 1: System.out.println("Senin"); break;
    case 2: System.out.println("Selasa"); break;
    case 3: System.out.println("Rabu"); break;     // ← dieksekusi
    case 4: System.out.println("Kamis"); break;
    case 5: System.out.println("Jumat"); break;
    case 6: System.out.println("Sabtu"); break;
    case 7: System.out.println("Minggu"); break;
    default: System.out.println("Tidak valid");
}
// ⚠️ PENTING: tanpa "break", eksekusi akan JATUH ke case berikutnya (fall-through)!

// Switch dengan tipe String
String buah = "Apel";
switch (buah) {
    case "Apel":
        System.out.println("Harga: Rp 10.000");
        break;
    case "Jeruk":
        System.out.println("Harga: Rp 8.000");
        break;
    default:
        System.out.println("Buah tidak tersedia");
}
```

**Kapan pakai if-else vs switch:**

| Situasi | Pilihan |
|---------|---------|
| Kondisi range/perbandingan (`>`, `<`, `>=`) | **if-else** |
| Kondisi boolean kompleks (`&&`, `||`) | **if-else** |
| Membandingkan satu variabel dengan banyak **nilai tetap** | **switch** |
| Tipe data: int, char, String, enum | **switch** bisa |
| Tipe data: double, boolean | **switch** TIDAK bisa |

---

### Perulangan

Perulangan (loop) digunakan untuk **menjalankan blok kode berulang kali** sampai kondisi tertentu terpenuhi.

| Jenis | Kapan Dipakai | Pengecekan Kondisi | Minimal Eksekusi |
|-------|---------------|-------------------|------------------|
| **for** | Jumlah perulangan **sudah diketahui** | Sebelum eksekusi | 0 kali |
| **while** | Jumlah perulangan **belum diketahui** | Sebelum eksekusi | 0 kali |
| **do-while** | Minimal harus **dijalankan 1 kali** | Sesudah eksekusi | **1 kali** |

```java
// ========== FOR ==========
// Cetak angka 1-5
for (int i = 1; i <= 5; i++) {
    System.out.println(i);
}
// Output: 1 2 3 4 5

// For terbalik (countdown)
for (int i = 5; i >= 1; i--) {
    System.out.println(i);
}
// Output: 5 4 3 2 1

// For dengan langkah 2
for (int i = 0; i <= 10; i += 2) {
    System.out.println(i);
}
// Output: 0 2 4 6 8 10

// Nested for (perulangan bersarang) — cetak segitiga bintang
for (int i = 1; i <= 5; i++) {
    for (int j = 1; j <= i; j++) {
        System.out.print("*");
    }
    System.out.println();
}
// Output:
// *
// **
// ***
// ****
// *****

// For-each (iterasi array)
String[] buah = {"Apel", "Jeruk", "Mangga"};
for (String b : buah) {
    System.out.println(b);
}

// ========== WHILE ==========
// Cetak angka 1-5
int i = 1;
while (i <= 5) {
    System.out.println(i);
    i++;
}

// While untuk input user (jumlah perulangan tidak diketahui)
Scanner sc = new Scanner(System.in);
String input = "";
while (!input.equals("exit")) {
    System.out.print("Masukkan perintah: ");
    input = sc.nextLine();
    System.out.println("Anda ketik: " + input);
}

// ========== DO-WHILE ==========
// Minimal jalan 1 kali, baru cek kondisi
int j = 10;
do {
    System.out.println(j);   // Tetap cetak 10 meskipun kondisi false
    j++;
} while (j < 5);
// Output: 10  (badan loop dieksekusi 1 kali sebelum cek kondisi)
```

**break dan continue:**

```java
// break: keluar dari loop sepenuhnya
for (int i = 1; i <= 10; i++) {
    if (i == 5) break;       // berhenti di 5
    System.out.println(i);
}
// Output: 1 2 3 4

// continue: skip iterasi saat ini, lanjut ke iterasi berikutnya
for (int i = 1; i <= 10; i++) {
    if (i % 2 == 0) continue; // skip angka genap
    System.out.println(i);
}
// Output: 1 3 5 7 9
```

---

### Function / Method

**Function (Method)** adalah blok kode yang bisa **dipanggil berulang kali** untuk melakukan tugas tertentu. Tujuannya: **reusability** (tidak perlu menulis kode yang sama berulang kali).

```java
// ========== Function DENGAN return value ==========
// Mengembalikan nilai → tipe return BUKAN void
public static int tambah(int a, int b) {
    return a + b;
}

public static double hitungLuasLingkaran(double radius) {
    return Math.PI * radius * radius;
}

// ========== Function TANPA return value ==========
// Tidak mengembalikan nilai → tipe return = void
public static void cetakSalam(String nama) {
    System.out.println("Hello, " + nama + "!");
}

public static void cetakGaris(int panjang) {
    for (int i = 0; i < panjang; i++) {
        System.out.print("=");
    }
    System.out.println();
}

// ========== Pemanggilan ==========
int hasil = tambah(3, 5);                   // hasil = 8
double luas = hitungLuasLingkaran(7.0);     // luas = 153.93...
cetakSalam("Andi");                          // Output: Hello, Andi!
cetakGaris(20);                              // Output: ====================
```

**Istilah penting:**

| Istilah | Penjelasan | Contoh |
|---------|------------|--------|
| **Parameter** | Variabel di **definisi** function | `int a, int b` dalam `tambah(int a, int b)` |
| **Argument** | Nilai yang **dikirim** saat memanggil | `3, 5` dalam `tambah(3, 5)` |
| **Return value** | Nilai yang dikembalikan function | `return a + b;` → mengembalikan 8 |
| **void** | Function yang **tidak** mengembalikan nilai | `public static void cetakSalam(...)` |
| **Scope** | Wilayah di mana variabel bisa diakses | Variabel di dalam function hanya bisa diakses di function itu |

**Pass by Value vs Pass by Reference:**

```java
// Java menggunakan Pass by Value
public static void ubah(int x) {
    x = 100;  // hanya mengubah SALINAN, bukan variabel asli
}

int angka = 5;
ubah(angka);
System.out.println(angka);  // Output: 5 (TIDAK berubah!)

// Tapi untuk OBJECT, referensinya yang di-copy
public static void ubahNama(Mahasiswa m) {
    m.nama = "Budi";  // mengubah objek yang dirujuk → BERUBAH
}
```

---

### OOP (Object-Oriented Programming)

OOP adalah paradigma pemrograman yang mengorganisasi kode ke dalam **objek** yang memiliki **data (atribut/property)** dan **perilaku (method)**.

**Konsep dasar — Class dan Object:**

```java
// CLASS = cetakan/blueprint
// OBJECT = hasil cetakan (instance)

// Analogi: 
// Class "Mobil" = blueprint/rancangan mobil
// Object = mobil merah milik Andi, mobil biru milik Budi (instance nyata)

class Mobil {
    // Atribut (data/property)
    String merk;
    String warna;
    int tahun;
    int kecepatan = 0;

    // Constructor (dipanggil saat membuat object baru)
    Mobil(String merk, String warna, int tahun) {
        this.merk = merk;
        this.warna = warna;
        this.tahun = tahun;
    }

    // Method (perilaku)
    void gas() {
        kecepatan += 10;
        System.out.println(merk + " melaju " + kecepatan + " km/h");
    }

    void rem() {
        kecepatan = Math.max(0, kecepatan - 10);
        System.out.println(merk + " melambat " + kecepatan + " km/h");
    }
}

// Membuat object (instance)
Mobil mobilAndi = new Mobil("Toyota", "Merah", 2024);
Mobil mobilBudi = new Mobil("Honda", "Biru", 2023);

mobilAndi.gas();   // Toyota melaju 10 km/h
mobilAndi.gas();   // Toyota melaju 20 km/h
mobilAndi.rem();   // Toyota melambat 10 km/h
```

---

**4 Pilar OOP — dengan contoh lengkap:**

#### Pilar 1: Encapsulation (Penyembunyian Data)

Menyembunyikan data internal dan **hanya mengizinkan akses melalui method** (getter/setter).

**Analogi:** Kapsul obat → isi obat tersembunyi, kita cuma menelan kapsulnya.

```java
class RekeningBank {
    // Data disembunyikan (private)
    private String nomorRekening;
    private double saldo;
    private String pemilik;

    // Constructor
    public RekeningBank(String noRek, String pemilik, double saldoAwal) {
        this.nomorRekening = noRek;
        this.pemilik = pemilik;
        this.saldo = saldoAwal;
    }

    // Getter (membaca data)
    public double getSaldo() {
        return saldo;
    }

    public String getPemilik() {
        return pemilik;
    }

    // Setter dengan VALIDASI (mengubah data dengan aman)
    public void setor(double jumlah) {
        if (jumlah > 0) {
            saldo += jumlah;
            System.out.println("Setor " + jumlah + " berhasil. Saldo: " + saldo);
        } else {
            System.out.println("Jumlah harus positif!");
        }
    }

    public void tarik(double jumlah) {
        if (jumlah > 0 && jumlah <= saldo) {
            saldo -= jumlah;
            System.out.println("Tarik " + jumlah + " berhasil. Saldo: " + saldo);
        } else {
            System.out.println("Saldo tidak cukup atau jumlah tidak valid!");
        }
    }
}

// Penggunaan:
RekeningBank rek = new RekeningBank("123456", "Andi", 1000000);
rek.setor(500000);     // Setor 500000 berhasil. Saldo: 1500000
rek.tarik(200000);     // Tarik 200000 berhasil. Saldo: 1300000
rek.tarik(5000000);    // Saldo tidak cukup!

// rek.saldo = -999;   ← COMPILE ERROR! saldo adalah private
// Ini gunanya encapsulation: melindungi data dari akses tidak sah
```

**Access Modifier:**

| Modifier | Class sendiri | Package sama | Subclass | Semua class |
|----------|:----:|:----:|:----:|:----:|
| **private** | ✅ | ❌ | ❌ | ❌ |
| **default** (tanpa modifier) | ✅ | ✅ | ❌ | ❌ |
| **protected** | ✅ | ✅ | ✅ | ❌ |
| **public** | ✅ | ✅ | ✅ | ✅ |

---

#### Pilar 2: Inheritance (Pewarisan)

Kelas anak (subclass) **mewarisi** atribut dan method dari kelas induk (superclass).

**Analogi:** Anak mewarisi sifat orang tua (warna kulit, golongan darah), tapi bisa punya sifat tambahan sendiri.

```java
// Superclass (kelas induk)
class Hewan {
    String nama;
    int umur;

    Hewan(String nama, int umur) {
        this.nama = nama;
        this.umur = umur;
    }

    void makan() {
        System.out.println(nama + " sedang makan");
    }

    void tidur() {
        System.out.println(nama + " sedang tidur");
    }

    void info() {
        System.out.println("Nama: " + nama + ", Umur: " + umur + " tahun");
    }
}

// Subclass (kelas anak) — mewarisi semua dari Hewan
class Kucing extends Hewan {
    String warnaBulu;

    Kucing(String nama, int umur, String warnaBulu) {
        super(nama, umur);           // panggil constructor induk
        this.warnaBulu = warnaBulu;  // atribut tambahan
    }

    // Method tambahan (hanya dimiliki Kucing)
    void mendengkur() {
        System.out.println(nama + " mendengkur... prrrr");
    }
}

class Anjing extends Hewan {
    String ras;

    Anjing(String nama, int umur, String ras) {
        super(nama, umur);
        this.ras = ras;
    }

    void menggonggong() {
        System.out.println(nama + " menggonggong! Guk guk!");
    }
}

// Penggunaan:
Kucing k = new Kucing("Milo", 2, "Oranye");
k.makan();        // Milo sedang makan       ← WARISAN dari Hewan
k.tidur();        // Milo sedang tidur        ← WARISAN dari Hewan
k.mendengkur();   // Milo mendengkur... prrrr ← MILIK Kucing sendiri

Anjing a = new Anjing("Rex", 3, "Golden Retriever");
a.makan();          // Rex sedang makan
a.menggonggong();   // Rex menggonggong! Guk guk!
// a.mendengkur();  ← ERROR! Anjing tidak punya method mendengkur
```

**Keyword penting dalam Inheritance:**
- `extends` — untuk mewarisi class
- `super` — merujuk ke kelas induk (memanggil constructor/method induk)
- `this` — merujuk ke objek saat ini

---

#### Pilar 3: Polymorphism (Banyak Bentuk)

Satu method bisa memiliki **perilaku berbeda** tergantung konteks.

**Analogi:** Tombol "play" → di musik: putar lagu, di video: putar film, di game: mulai game. Sama-sama "play" tapi hasilnya beda.

**Tipe 1: Overriding (Runtime Polymorphism)**
Method di kelas anak **menimpa** method kelas induk yang namanya sama.

```java
class Hewan {
    void suara() {
        System.out.println("...");
    }
}

class Kucing extends Hewan {
    @Override
    void suara() {
        System.out.println("Meow!");    // menimpa method suara() dari Hewan
    }
}

class Anjing extends Hewan {
    @Override
    void suara() {
        System.out.println("Guk guk!"); // menimpa method suara() dari Hewan
    }
}

class Sapi extends Hewan {
    @Override
    void suara() {
        System.out.println("Mooo!");
    }
}

// Polymorphism beraksi — variabel tipe Hewan tapi isinya macam-macam
Hewan h1 = new Kucing();
Hewan h2 = new Anjing();
Hewan h3 = new Sapi();

h1.suara();  // Meow!     ← method yang dipanggil sesuai OBJEK ASLI, bukan tipe variabel
h2.suara();  // Guk guk!
h3.suara();  // Mooo!

// Contoh penggunaan dalam array/collection
Hewan[] kebunBinatang = { new Kucing(), new Anjing(), new Sapi() };
for (Hewan h : kebunBinatang) {
    h.suara();  // masing-masing memanggil suara() yang berbeda
}
```

**Tipe 2: Overloading (Compile-time Polymorphism)**
Method **nama sama** tapi **parameter berbeda** di dalam satu class.

```java
class Kalkulator {
    // Overloading: nama sama, parameter beda
    int tambah(int a, int b) {
        return a + b;
    }

    double tambah(double a, double b) {  // parameter beda tipe
        return a + b;
    }

    int tambah(int a, int b, int c) {    // parameter beda jumlah
        return a + b + c;
    }
}

Kalkulator calc = new Kalkulator();
calc.tambah(3, 5);          // memanggil tambah(int, int) → 8
calc.tambah(3.5, 2.1);      // memanggil tambah(double, double) → 5.6
calc.tambah(1, 2, 3);       // memanggil tambah(int, int, int) → 6
```

**Perbedaan Overloading vs Overriding:**

| Aspek | Overloading | Overriding |
|-------|-------------|------------|
| **Definisi** | Nama sama, **parameter beda** | Nama sama, **parameter sama**, di subclass |
| **Terjadi di** | **Satu class** yang sama | **Dua class** (parent-child) |
| **Polymorphism** | Compile-time (static) | Runtime (dynamic) |
| **Return type** | Boleh berbeda | Harus sama atau covariant |
| **Anotasi** | Tidak perlu | `@Override` |

---

#### Pilar 4: Abstraction (Abstraksi)

Menyembunyikan **detail implementasi** yang kompleks dan hanya menampilkan **fungsionalitas penting** ke pengguna.

**Analogi:** Mobil → kita cuma tahu setir, gas, rem. Kita tidak perlu tahu bagaimana mesin pembakaran internal bekerja.

**Abstract Class:**

```java
// Abstract class TIDAK bisa diinstansiasi langsung (tidak bisa new Bentuk())
abstract class Bentuk {
    String warna;

    Bentuk(String warna) {
        this.warna = warna;
    }

    // Method ABSTRACT: hanya deklarasi, TANPA body (implementasi)
    // Subclass WAJIB mengimplementasikan method ini
    abstract double hitungLuas();
    abstract double hitungKeliling();

    // Method BIASA: punya body (implementasi)
    void info() {
        System.out.println("Bentuk warna " + warna);
        System.out.println("Luas: " + hitungLuas());
        System.out.println("Keliling: " + hitungKeliling());
    }
}

class Lingkaran extends Bentuk {
    double radius;

    Lingkaran(String warna, double radius) {
        super(warna);
        this.radius = radius;
    }

    @Override
    double hitungLuas() {
        return Math.PI * radius * radius;    // WAJIB diimplementasikan
    }

    @Override
    double hitungKeliling() {
        return 2 * Math.PI * radius;         // WAJIB diimplementasikan
    }
}

class PersegiPanjang extends Bentuk {
    double panjang, lebar;

    PersegiPanjang(String warna, double panjang, double lebar) {
        super(warna);
        this.panjang = panjang;
        this.lebar = lebar;
    }

    @Override
    double hitungLuas() { return panjang * lebar; }

    @Override
    double hitungKeliling() { return 2 * (panjang + lebar); }
}

// Penggunaan:
// Bentuk b = new Bentuk("Merah");  ← ERROR! abstract class tidak bisa di-new
Bentuk b1 = new Lingkaran("Merah", 7);
Bentuk b2 = new PersegiPanjang("Biru", 5, 3);

b1.info();  // Luas: 153.93..., Keliling: 43.98...
b2.info();  // Luas: 15.0, Keliling: 16.0
```

**Interface:**

```java
// Interface = kontrak yang HARUS dipatuhi
interface Drawable {
    void draw();              // semua method abstract (secara default)
    void setColor(String c);
}

interface Resizable {
    void resize(double factor);
}

// Class bisa implements BANYAK interface (multiple implementation)
class Circle implements Drawable, Resizable {
    @Override
    public void draw() { System.out.println("Drawing circle"); }

    @Override
    public void setColor(String c) { System.out.println("Color: " + c); }

    @Override
    public void resize(double factor) { System.out.println("Resize x" + factor); }
}
```

**Perbedaan Abstract Class dan Interface:**

| Aspek | Abstract Class | Interface |
|-------|---------------|-----------|
| **Method** | Bisa punya method biasa & abstract | Semua method abstract (Java 8+: bisa default) |
| **Variabel** | Bisa punya variabel instance | Hanya konstanta (`public static final`) |
| **Constructor** | Punya constructor | Tidak punya constructor |
| **Inheritance** | Single inheritance (`extends` satu saja) | Multiple implementation (`implements` banyak) |
| **Keyword** | `extends` | `implements` |
| **Kapan pakai** | Ada atribut/method yang SAMA di semua subclass | Hanya mendefinisikan KONTRAK (apa yang harus bisa dilakukan) |

---

### Error Handling

Error Handling adalah mekanisme menangani kesalahan saat program berjalan agar program **tidak crash**.

**Jenis Error:**

| Jenis | Waktu Terdeteksi | Contoh |
|-------|-----------------|--------|
| **Syntax Error** | Compile-time | Kurang titik koma, typo |
| **Runtime Error** | Saat program jalan | Bagi dengan nol, array index di luar batas |
| **Logic Error** | Tidak terdeteksi otomatis | Program jalan tapi hasilnya SALAH |

**try-catch-finally — contoh lengkap:**

```java
// Contoh 1: Menangkap exception spesifik
try {
    int[] angka = {1, 2, 3};
    System.out.println(angka[5]);       // ArrayIndexOutOfBoundsException!
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("Error: Index di luar batas array!");
    System.out.println("Pesan: " + e.getMessage());
} finally {
    System.out.println("Blok finally SELALU dijalankan, error atau tidak.");
}
// Output:
// Error: Index di luar batas array!
// Pesan: Index 5 out of bounds for length 3
// Blok finally SELALU dijalankan, error atau tidak.

// Contoh 2: Multiple catch (menangkap beberapa jenis exception)
try {
    Scanner sc = new Scanner(System.in);
    System.out.print("Masukkan angka: ");
    int angka = sc.nextInt();              // bisa InputMismatchException
    int hasil = 100 / angka;               // bisa ArithmeticException
    System.out.println("Hasil: " + hasil);
} catch (InputMismatchException e) {
    System.out.println("Error: Input bukan angka!");
} catch (ArithmeticException e) {
    System.out.println("Error: Tidak bisa bagi dengan nol!");
} catch (Exception e) {
    System.out.println("Error umum: " + e.getMessage());
}

// Contoh 3: throw (melempar exception secara manual)
public static void setUmur(int umur) {
    if (umur < 0) {
        throw new IllegalArgumentException("Umur tidak boleh negatif!");
    }
    System.out.println("Umur: " + umur);
}

// Contoh 4: throws (mendeklarasikan bahwa method bisa throw exception)
public static void bacaFile(String path) throws IOException {
    FileReader fr = new FileReader(path);  // bisa throw IOException
    // ...
}
```

**Jenis Exception di Java:**

| Jenis | Penjelasan | Contoh | Harus Ditangani? |
|-------|------------|--------|-----------------|
| **Checked Exception** | Dipaksa oleh compiler | `IOException`, `SQLException`, `FileNotFoundException` | ✅ Ya (try-catch atau throws) |
| **Unchecked Exception** | Runtime error | `NullPointerException`, `ArithmeticException`, `ArrayIndexOutOfBounds` | ❌ Tidak wajib |
| **Error** | Masalah sistem yang serius | `OutOfMemoryError`, `StackOverflowError` | ❌ Tidak ditangani |

---

### Struktur Data Dasar

| Struktur Data | Prinsip | Operasi Utama | Contoh Penggunaan |
|---------------|---------|---------|-------------------|
| **Array** | Kumpulan data fixed-size, akses via index | Akses O(1), insert/delete O(n) | Daftar nilai mahasiswa |
| **Linked List** | Node terhubung via pointer | Insert/delete O(1) di head, akses O(n) | Playlist musik |
| **Stack** | **LIFO** (Last In, First Out) | `push()`, `pop()`, `peek()` | Undo/redo, call stack |
| **Queue** | **FIFO** (First In, First Out) | `enqueue()`, `dequeue()` | Antrian printer, BFS |
| **Tree** | Hierarki parent-child | Insert, search, delete | File system, DOM HTML |
| **Hash Table** | Key-value pair | Insert/search/delete avg O(1) | Dictionary, cache |

**Contoh implementasi Stack:**

```java
// Stack = LIFO (Last In, First Out)
// Analogi: tumpukan piring → piring terakhir ditaruh, pertama diambil

Stack<Integer> stack = new Stack<>();
stack.push(10);   // [10]
stack.push(20);   // [10, 20]
stack.push(30);   // [10, 20, 30]

stack.peek();     // 30 (lihat paling atas, tanpa menghapus)
stack.pop();      // 30 (ambil & hapus paling atas) → [10, 20]
stack.pop();      // 20 → [10]
```

**Contoh implementasi Queue:**

```java
// Queue = FIFO (First In, First Out)
// Analogi: antrian di bank → orang pertama datang, pertama dilayani

Queue<String> queue = new LinkedList<>();
queue.add("Andi");    // [Andi]
queue.add("Budi");    // [Andi, Budi]
queue.add("Citra");   // [Andi, Budi, Citra]

queue.peek();         // Andi (lihat depan, tanpa menghapus)
queue.poll();         // Andi (ambil & hapus paling depan) → [Budi, Citra]
queue.poll();         // Budi → [Citra]
```

---

### Konsep API & Recursion

**API (Application Programming Interface)** adalah sekumpulan aturan yang memungkinkan satu software berkomunikasi dengan software lain.

```
Client  →  HTTP Request  →  API Server  →  Database
Client  ←  HTTP Response ←  API Server  ←  Database
```

**Recursion — fungsi yang memanggil dirinya sendiri:**

```java
// Factorial: 5! = 5 × 4 × 3 × 2 × 1 = 120
int factorial(int n) {
    if (n <= 1) return 1;           // BASE CASE: kondisi berhenti
    return n * factorial(n - 1);    // RECURSIVE CASE: memanggil diri sendiri
}

// Penjelasan alur:
// factorial(5) = 5 * factorial(4)
//              = 5 * 4 * factorial(3)
//              = 5 * 4 * 3 * factorial(2)
//              = 5 * 4 * 3 * 2 * factorial(1)
//              = 5 * 4 * 3 * 2 * 1
//              = 120

// Fibonacci: 0, 1, 1, 2, 3, 5, 8, 13, ...
int fibonacci(int n) {
    if (n <= 0) return 0;           // base case
    if (n == 1) return 1;           // base case
    return fibonacci(n - 1) + fibonacci(n - 2);  // recursive case
}
```

**Rekursi harus punya:**
1. **Base case** — kondisi berhenti agar tidak infinite loop
2. **Recursive case** — pemanggilan diri sendiri dengan parameter yang berubah menuju base case

---

## Contoh Pertanyaan & Jawaban — Programming

<details>
<summary><b>Q: Apa perbedaan Array dan ArrayList?</b></summary>

| Aspek | Array | ArrayList |
|-------|-------|-----------|
| **Ukuran** | **Tetap** (fixed saat deklarasi) | **Dinamis** (otomatis bertambah/berkurang) |
| **Tipe data** | Primitif & objek | Hanya objek (wrapper class: Integer, bukan int) |
| **Performa** | Lebih cepat | Sedikit lebih lambat |
| **Method** | Tidak punya built-in method | `add()`, `remove()`, `get()`, `size()`, dll |

```java
// Array
int[] arr = new int[5];        // ukuran TETAP 5
arr[0] = 10;

// ArrayList
ArrayList<Integer> list = new ArrayList<>();
list.add(10);                   // ukuran otomatis bertambah
list.add(20);
list.remove(0);                 // bisa hapus
```
</details>

<details>
<summary><b>Q: Jelaskan konsep Constructor! Apa bedanya dengan method biasa?</b></summary>

**Constructor** adalah method khusus yang **dipanggil otomatis saat objek dibuat** (`new`). Fungsinya: menginisialisasi nilai awal objek.

| Aspek | Constructor | Method Biasa |
|-------|-------------|-------------|
| **Nama** | Sama dengan nama class | Bebas |
| **Return type** | Tidak ada | Harus ada (void, int, dll) |
| **Pemanggilan** | Otomatis saat `new` | Manual |
| **Tujuan** | Inisialisasi objek | Melakukan aksi |

```java
class Mahasiswa {
    String nama;
    int umur;

    // Constructor
    Mahasiswa(String nama, int umur) {
        this.nama = nama;  // this = objek ini, nama = parameter
        this.umur = umur;
    }

    // Method biasa
    void perkenalan() {
        System.out.println("Halo, saya " + nama + ", umur " + umur);
    }
}

Mahasiswa mhs = new Mahasiswa("Andi", 21);  // constructor dipanggil otomatis
mhs.perkenalan();                            // method dipanggil manual
```
</details>

<details>
<summary><b>Q: Apa itu keyword 'static'?</b></summary>

`static` artinya milik **CLASS**, bukan milik **OBJECT**. Bisa diakses tanpa membuat objek.

```java
class MathHelper {
    static final double PI = 3.14159;  // konstanta static

    static int tambah(int a, int b) {  // method static
        return a + b;
    }
}

// Akses tanpa new:
double pi = MathHelper.PI;
int hasil = MathHelper.tambah(3, 5);
```

| Aspek | Static | Non-Static (Instance) |
|-------|--------|----------------------|
| **Milik** | Class | Object |
| **Akses** | Tanpa membuat objek | Harus buat objek dulu |
| **Memori** | Satu untuk semua objek | Setiap objek punya sendiri |
</details>

---

# 3️⃣ Information Technology

## Daftar Materi

- Hardware & Software
- Sistem Operasi
- Jaringan Dasar & Internet
- IP Address
- Client-Server
- Cloud Computing
- Virtualisasi
- Keamanan Informasi
- Teknologi Informasi dalam Organisasi

---

## Ringkasan Teori

### Hardware & Software

| Aspek | Hardware | Software |
|-------|----------|----------|
| **Definisi** | Komponen **fisik** komputer yang bisa disentuh | **Program/instruksi** yang berjalan di atas hardware |
| **Contoh** | CPU, RAM, HDD/SSD, Monitor, Keyboard | Windows, Linux, Chrome, MySQL |
| **Kategori** | Input, Output, Processing, Storage | System Software, Application Software |
| **Kerusakan** | Fisik (panas, aus, patah) | Bug, error, virus, crash |

**Komponen utama hardware:**

| Komponen | Fungsi | Analogi |
|----------|--------|---------|
| **CPU (Processor)** | Otak komputer — memproses semua instruksi | Otak manusia |
| **RAM** | Memori sementara (volatile) — data hilang saat mati | Meja kerja (makin besar, makin banyak yang bisa dikerjakan) |
| **HDD/SSD** | Penyimpanan permanen (non-volatile) | Lemari arsip (data tetap ada walau komputer mati) |
| **Motherboard** | Papan utama — menghubungkan semua komponen | Tulang belakang |
| **GPU** | Memproses grafis dan visual | Asisten khusus gambar |
| **PSU** | Menyuplai daya listrik ke semua komponen | Jantung (pompa energi) |
| **NIC (Network Card)** | Menghubungkan komputer ke jaringan | Antena/port komunikasi |

**Perbedaan RAM dan ROM:**

| Aspek | RAM | ROM |
|-------|-----|-----|
| **Sifat** | Volatile (data hilang saat mati) | Non-volatile (data tetap tersimpan) |
| **Fungsi** | Menyimpan data yang **sedang** diproses | Menyimpan **firmware/BIOS** |
| **Kecepatan** | Cepat (nanosecond) | Lebih lambat |
| **Bisa ditulis?** | Bisa baca & tulis | Hanya baca (read-only) |
| **Contoh** | DDR4, DDR5 | BIOS chip, firmware |

**Perbedaan HDD dan SSD:**

| Aspek | HDD | SSD |
|-------|-----|-----|
| **Teknologi** | Piringan magnetik berputar | Flash memory (chip) |
| **Kecepatan** | 80-160 MB/s | 200-7000 MB/s |
| **Tahan guncangan** | Tidak (ada bagian bergerak) | Ya (tidak ada bagian bergerak) |
| **Harga** | Murah (per GB) | Mahal (per GB) |
| **Daya** | Lebih boros | Lebih hemat |
| **Umur** | Lebih lama (teori) | Terbatas (write cycle) |

**Jenis software:**

| Kategori | Penjelasan | Contoh |
|----------|------------|--------|
| **System Software** | Mengelola hardware & menyediakan platform | Windows, Linux, macOS |
| **Application Software** | Digunakan user untuk tugas tertentu | Chrome, Word, MySQL |
| **Utility Software** | Perawatan & optimasi sistem | Antivirus, disk cleaner |
| **Firmware** | Software yang tertanam di hardware | BIOS, firmware printer |

---

### Sistem Operasi

**Sistem Operasi (OS)** adalah software yang **mengelola hardware** dan **menyediakan layanan** untuk aplikasi.

**Analogi:** OS adalah **manajer restoran** — mengatur siapa yang memasak (CPU), di meja mana (RAM), menyimpan resep di lemari (storage), dan melayani tamu (user).

**5 fungsi utama OS:**

| Fungsi | Penjelasan | Contoh |
|--------|------------|--------|
| **Manajemen Proses** | Menjalankan & mengatur program | Multitasking: Chrome + Word berjalan bersamaan |
| **Manajemen Memori** | Mengalokasikan RAM untuk proses | Menentukan berapa RAM untuk Chrome, berapa untuk Word |
| **Manajemen File** | Mengatur penyimpanan file dalam folder | File system: NTFS (Windows), ext4 (Linux) |
| **Manajemen I/O** | Mengatur perangkat input/output | Driver keyboard, mouse, printer |
| **Keamanan** | Mengontrol akses pengguna | Login, permission, firewall |

**Perbedaan 32-bit dan 64-bit:**

| Aspek | 32-bit | 64-bit |
|-------|--------|--------|
| **Max RAM** | 4 GB | 16 Exabyte (praktis tak terbatas) |
| **Register** | 32-bit | 64-bit |
| **Performa** | Lebih lambat untuk data besar | Lebih cepat |
| **Kompatibilitas** | Hanya jalankan app 32-bit | Bisa jalankan 32-bit & 64-bit |
| **Contoh** | Windows 10 32-bit (jarang) | Windows 10/11 64-bit (umum) |

**Contoh Sistem Operasi:**

| OS | Tipe | Kelebihan |
|----|------|-----------|
| **Windows** | Desktop/Laptop | User-friendly, banyak software |
| **macOS** | Desktop/Laptop (Apple) | Stabil, cocok untuk desain |
| **Linux (Ubuntu, CentOS)** | Server & Desktop | Open source, gratis, aman |
| **Android** | Mobile | Open source, banyak app |
| **iOS** | Mobile (Apple) | Aman, performa konsisten |

---

### Jaringan Dasar & Internet

**Jaringan Komputer** adalah dua atau lebih komputer yang saling terhubung untuk **berbagi data dan sumber daya**.

| Jenis Jaringan | Jangkauan | Contoh |
|----------------|-----------|--------|
| **PAN** (Personal Area Network) | < 10 meter | Bluetooth headset ke HP |
| **LAN** (Local Area Network) | Satu gedung/kampus | Jaringan lab komputer |
| **MAN** (Metropolitan Area Network) | Satu kota | Jaringan antar cabang bank di satu kota |
| **WAN** (Wide Area Network) | Antar kota/negara | Internet |

**Internet** adalah jaringan global (WAN terbesar) yang menghubungkan miliaran perangkat di seluruh dunia menggunakan protokol **TCP/IP**.

**Cara kerja internet (sederhana):**
```
Kamu ketik google.com di browser
  → Browser tanya DNS: "google.com itu IP berapa?"
  → DNS jawab: "142.250.x.x"
  → Browser kirim HTTP Request ke IP tersebut via TCP/IP
  → Server Google kirim HTTP Response (halaman web)
  → Browser menampilkan halaman Google
```

---

### IP Address

**IP Address** adalah alamat unik yang diberikan kepada setiap perangkat dalam jaringan, agar perangkat bisa saling berkomunikasi.

**Analogi:** IP address = **alamat rumah** komputer. Tanpa alamat, surat (data) tidak bisa dikirim.

| Versi | Format | Contoh | Kapasitas |
|-------|--------|--------|-----------|
| **IPv4** | 32-bit, 4 oktet desimal (0-255) | `192.168.1.1` | ~4.3 miliar alamat |
| **IPv6** | 128-bit, 8 grup heksadesimal | `2001:0db8:85a3::8a2e:0370:7334` | 340 undecillion alamat |

**Kelas IP (IPv4):**

| Kelas | Range Oktet Pertama | Default Subnet Mask | Jumlah Host | Penggunaan |
|-------|---------------------|---------------------|-------------|------------|
| **A** | 1 – 126 | 255.0.0.0 (/8) | ~16 juta | Organisasi besar |
| **B** | 128 – 191 | 255.255.0.0 (/16) | ~65 ribu | Organisasi menengah |
| **C** | 192 – 223 | 255.255.255.0 (/24) | 254 | Jaringan kecil |
| **D** | 224 – 239 | - | - | Multicast |
| **E** | 240 – 255 | - | - | Eksperimen |

**IP Private vs Public:**

| Jenis | Penggunaan | Range | Contoh |
|-------|------------|-------|--------|
| **Private** | Jaringan internal (LAN) | 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 | 192.168.1.100 |
| **Public** | Bisa diakses dari internet | Di luar range private | 8.8.8.8 (Google DNS) |

> **Kenapa butuh IP Private?** Karena jumlah IPv4 terbatas (~4.3 miliar). Dengan NAT, banyak perangkat di LAN bisa sharing 1 IP Public.

---

### Client-Server

**Model Client-Server** adalah arsitektur di mana **client meminta** layanan dan **server menyediakan** layanan.

```
┌──────────┐     HTTP Request     ┌──────────┐     Query      ┌──────────┐
│  Client  │ ──────────────────→  │  Server  │ ────────────→  │ Database │
│ (Browser)│ ←──────────────────  │  (Web)   │ ←────────────  │ (MySQL)  │
└──────────┘     HTTP Response    └──────────┘     Result     └──────────┘
```

| Aspek | Client | Server |
|-------|--------|--------|
| **Peran** | Meminta layanan (request) | Menyediakan layanan (response) |
| **Contoh** | Browser, aplikasi mobile, Postman | Apache, Nginx, Node.js |
| **Jumlah** | Banyak client | Biasanya sedikit server |
| **Resource** | Ringan | Berat (RAM, CPU, storage besar) |

**Perbedaan arsitektur:**

| Model | Penjelasan | Contoh |
|-------|------------|--------|
| **Client-Server** | Client dan server terpisah | Website, email |
| **Peer-to-Peer (P2P)** | Setiap komputer bisa jadi client dan server | Torrent, blockchain |
| **3-Tier Architecture** | Client → Application Server → Database Server | Aplikasi enterprise |

---

### Cloud Computing

**Cloud Computing** adalah penyediaan layanan komputasi (server, storage, database, networking, software) melalui **internet**, sehingga pengguna **tidak perlu memiliki infrastruktur fisik** sendiri.

**Analogi:** Cloud = **PLN (listrik)** → kita tidak perlu punya pembangkit listrik sendiri, cukup langganan dan pakai sesuai kebutuhan, bayar sesuai pemakaian.

**Keuntungan Cloud:**
- **On-demand** — pakai kapan saja, berapa saja
- **Pay-as-you-go** — bayar sesuai pemakaian
- **Scalable** — bisa tambah/kurangi resource dengan cepat
- **No maintenance** — tidak perlu urus hardware sendiri

**3 Model Layanan:**

| Model | Apa yang User Kelola | Apa yang Provider Kelola | Analogi | Contoh |
|-------|---------------------|--------------------------|---------|--------|
| **IaaS** | OS, Middleware, App, Data | Server fisik, Storage, Network | Sewa tanah kosong → bangun rumah sendiri | AWS EC2, GCP Compute Engine |
| **PaaS** | App, Data | OS, Middleware, Server, Network | Sewa apartemen → tinggal isi furniture | Heroku, Google App Engine |
| **SaaS** | Hanya pakai | Semua | Hotel → tinggal tidur | Gmail, Google Docs, Zoom |

```
Apa yang kamu kelola:

                On-premise    IaaS      PaaS      SaaS
Application   │    ✅     │   ✅    │   ✅    │   ❌   │
Data          │    ✅     │   ✅    │   ✅    │   ❌   │
Runtime       │    ✅     │   ✅    │   ❌    │   ❌   │
Middleware    │    ✅     │   ✅    │   ❌    │   ❌   │
OS            │    ✅     │   ✅    │   ❌    │   ❌   │
Virtualization│    ✅     │   ❌    │   ❌    │   ❌   │
Server        │    ✅     │   ❌    │   ❌    │   ❌   │
Storage       │    ✅     │   ❌    │   ❌    │   ❌   │
Network       │    ✅     │   ❌    │   ❌    │   ❌   │

✅ = kamu kelola   ❌ = provider kelola
```

**4 Model Deployment:**

| Model | Penjelasan | Contoh |
|-------|------------|--------|
| **Public Cloud** | Dikelola pihak ketiga, dipakai banyak organisasi | AWS, GCP, Azure |
| **Private Cloud** | Dikelola sendiri, untuk satu organisasi | OpenStack di data center sendiri |
| **Hybrid Cloud** | Kombinasi public + private | Data sensitif di private, app di public |
| **Community Cloud** | Dipakai bersama oleh komunitas/industri tertentu | Cloud khusus bank-bank |

---

### Virtualisasi

**Virtualisasi** adalah teknologi yang membuat **versi virtual** dari hardware, OS, storage, atau jaringan sehingga **satu hardware fisik bisa menjalankan banyak sistem virtual**.

**Analogi:** Apartemen → satu gedung fisik, tapi ada banyak "rumah" terpisah di dalamnya.

```
Tanpa Virtualisasi:           Dengan Virtualisasi:
┌──────────────┐              ┌──────────────┐
│   1 Server   │              │   1 Server   │
│   1 OS       │              │  ┌────┬────┐ │
│   1 Aplikasi │              │  │ VM1│ VM2│ │
│              │              │  │Win │Linux│ │
│              │              │  │App1│App2│ │
│ (boros!)     │              │  └────┴────┘ │
└──────────────┘              │  Hypervisor  │
                              └──────────────┘
```

| Komponen | Penjelasan |
|----------|------------|
| **Hypervisor** | Software yang membuat & mengelola VM |
| **Virtual Machine (VM)** | Komputer virtual yang berjalan di atas hypervisor |
| **Host** | Mesin fisik yang menjalankan hypervisor |
| **Guest** | OS yang berjalan di dalam VM |

**Tipe Hypervisor:**

| Tipe | Penjelasan | Performa | Contoh |
|------|------------|----------|--------|
| **Type 1 (Bare Metal)** | Langsung di atas hardware (tanpa OS host) | Lebih cepat | VMware ESXi, Hyper-V, KVM |
| **Type 2 (Hosted)** | Di atas OS host | Lebih lambat | VirtualBox, VMware Workstation |

**Perbedaan Virtualisasi vs Cloud Computing:**

| Aspek | Virtualisasi | Cloud Computing |
|-------|-------------|-----------------|
| **Definisi** | Teknologi mempartisi hardware jadi banyak VM | Layanan IT via internet |
| **Fokus** | Memaksimalkan penggunaan hardware | Menyediakan layanan on-demand |
| **Lokasi** | On-premise (biasanya) | Remote (internet) |
| **Hubungan** | Virtualisasi adalah **teknologi dasar/fondasi** cloud computing |

**Perbedaan VM vs Container (Docker):**

| Aspek | Virtual Machine | Container |
|-------|----------------|-----------|
| **Isolasi** | Full OS per VM | Berbagi OS kernel |
| **Ukuran** | GB (besar) | MB (ringan) |
| **Boot time** | Menit | Detik |
| **Resource** | Boros | Hemat |
| **Contoh** | VirtualBox, VMware | Docker, Kubernetes |

---

### Keamanan Informasi — CIA Triad

**CIA Triad** adalah 3 prinsip dasar keamanan informasi:

| Prinsip | Penjelasan | Ancaman | Implementasi |
|---------|------------|---------|-------------|
| **Confidentiality** (Kerahasiaan) | Data hanya bisa diakses oleh **pihak berwenang** | Data bocor, dicuri | Enkripsi, password, access control, VPN |
| **Integrity** (Integritas) | Data **tidak diubah** tanpa izin | Data dimanipulasi | Hashing, checksum, digital signature |
| **Availability** (Ketersediaan) | Sistem **selalu tersedia** saat dibutuhkan | DDoS, server down | Backup, redundancy, UPS, CDN |

**Ancaman keamanan umum — penjelasan detail:**

| Ancaman | Penjelasan | Contoh Nyata | Pencegahan |
|---------|------------|-------------|------------|
| **Malware** | Software berbahaya | Virus, worm, trojan, ransomware | Antivirus, jangan download sembarangan |
| **Phishing** | Menipu user via email/web palsu | Email "bank" palsu minta password | Cek URL, jangan klik link mencurigakan |
| **DDoS** | Membanjiri server dengan traffic | Server website down | Firewall, CDN, rate limiting |
| **SQL Injection** | Menyisipkan SQL via input user | `' OR 1=1 --` di form login | Prepared statement, input validation |
| **Man-in-the-Middle** | Menyadap komunikasi | Sniffing di WiFi publik | HTTPS, VPN |
| **Brute Force** | Mencoba semua kombinasi password | Password cracker | Rate limiting, CAPTCHA, 2FA |
| **Social Engineering** | Memanipulasi manusia | Telepon mengaku IT support | Edukasi karyawan |

**Contoh SQL Injection:**
```
Form login biasa:
Username: admin
Password: 12345

SQL yang dijalankan:
SELECT * FROM users WHERE username = 'admin' AND password = '12345'
→ Login berhasil jika ada data yang cocok

SQL Injection:
Username: admin' OR '1'='1' --
Password: (apapun)

SQL yang dijalankan:
SELECT * FROM users WHERE username = 'admin' OR '1'='1' --' AND password = ''
→ Kondisi '1'='1' selalu TRUE → Login berhasil tanpa password!

Pencegahan: gunakan Prepared Statement
```

**Perbedaan Enkripsi Simetris dan Asimetris:**

| Aspek | Simetris | Asimetris |
|-------|----------|-----------|
| **Jumlah kunci** | 1 kunci (sama untuk encrypt & decrypt) | 2 kunci (public key & private key) |
| **Kecepatan** | Lebih cepat | Lebih lambat |
| **Masalah** | Bagaimana mengirim kunci dengan aman? | Lebih aman, tapi lebih lambat |
| **Contoh** | AES, DES, 3DES | RSA, ECC |
| **Penggunaan** | Enkripsi data besar, database | HTTPS/SSL, digital signature, email |

---

### Peran TI dalam Organisasi

| Peran | Penjelasan | Contoh Implementasi |
|-------|------------|-------------------|
| **Otomasi proses bisnis** | Mengganti proses manual dengan sistem digital | ERP (SAP), sistem absensi digital, e-procurement |
| **Pengambilan keputusan** | Menyediakan data dan analisis untuk keputusan | Business Intelligence, Dashboard, Data Mining |
| **Komunikasi** | Memudahkan komunikasi internal & eksternal | Email, Slack, Zoom, video conference |
| **Penyimpanan & pengelolaan data** | Menyimpan data secara aman dan terorganisir | Cloud storage, database, backup system |
| **Keunggulan kompetitif** | Memberi keunggulan dibanding kompetitor | E-commerce, mobile app, AI/ML |
| **Customer relationship** | Mengelola hubungan dengan pelanggan | CRM (Customer Relationship Management) |

---

## Contoh Pertanyaan & Jawaban — Information Technology

<details>
<summary><b>Q: Apa itu IoT (Internet of Things)?</b></summary>

**IoT** adalah konsep di mana objek fisik (things) terhubung ke internet dan bisa **mengumpulkan serta bertukar data** secara otomatis.

**Contoh:**
- Smart home: lampu otomatis, AC yang bisa dikontrol dari HP
- Wearable: smartwatch yang monitor detak jantung
- Industri: sensor suhu di pabrik yang otomatis lapor ke server
- Pertanian: sensor kelembaban tanah yang otomatis siram tanaman

**Komponen IoT:**
1. **Sensor/Actuator** — mengumpulkan data dari lingkungan
2. **Konektivitas** — WiFi, Bluetooth, Zigbee, 4G/5G
3. **Cloud** — menyimpan & memproses data
4. **Interface** — dashboard/app untuk memonitor
</details>

<details>
<summary><b>Q: Apa perbedaan Antivirus dan Firewall?</b></summary>

| Aspek | Antivirus | Firewall |
|-------|-----------|----------|
| **Fungsi** | Mendeteksi & menghapus **malware** | Memfilter **traffic jaringan** |
| **Layer** | Aplikasi (file, program) | Jaringan (packet, IP, port) |
| **Cara kerja** | Scan file, bandingkan dengan database virus | Aturan allow/deny berdasarkan IP, port, protokol |
| **Analogi** | Satpam yang periksa identitas orang di dalam gedung | Pagar & gerbang yang mengatur siapa boleh masuk/keluar |
</details>

---

# 4️⃣ Web Systems and Technologies

## Daftar Materi

- HTML, CSS, JavaScript
- Frontend vs Backend
- HTTP / HTTPS
- REST API
- Request / Response
- HTTP Method (GET, POST, PUT, DELETE)
- JSON
- Database pada Aplikasi Web
- Authentication & Authorization
- Session / Token
- Web Server

---

## Ringkasan Teori

### HTML, CSS, JavaScript

| Teknologi | Peran | Analogi |
|-----------|-------|---------|
| **HTML** | **Struktur** halaman web (konten) | Tulang/kerangka rumah |
| **CSS** | **Tampilan/styling** halaman web | Cat, wallpaper, dekorasi rumah |
| **JavaScript** | **Interaktivitas** dan logika | Sistem listrik, AC, elevator (bikin rumah "hidup") |

**HTML — contoh elemen penting:**

```html
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Halaman Web Saya</title>
</head>
<body>
    <!-- Heading -->
    <h1>Judul Utama</h1>
    <h2>Sub Judul</h2>
    <p>Ini adalah paragraf.</p>

    <!-- Link dan Gambar -->
    <a href="https://google.com">Klik di sini</a>
    <img src="foto.jpg" alt="Deskripsi gambar">

    <!-- List -->
    <ul>                          <!-- Unordered list (bullet) -->
        <li>Item 1</li>
        <li>Item 2</li>
    </ul>
    <ol>                          <!-- Ordered list (angka) -->
        <li>Langkah 1</li>
        <li>Langkah 2</li>
    </ol>

    <!-- Tabel -->
    <table border="1">
        <tr><th>Nama</th><th>NIM</th></tr>
        <tr><td>Andi</td><td>001</td></tr>
    </table>

    <!-- Form -->
    <form action="/submit" method="POST">
        <label for="nama">Nama:</label>
        <input type="text" id="nama" name="nama">
        <input type="password" name="password">
        <button type="submit">Kirim</button>
    </form>

    <!-- Semantic HTML5 -->
    <header>Header</header>
    <nav>Navigasi</nav>
    <main>Konten Utama</main>
    <section>Bagian</section>
    <article>Artikel</article>
    <footer>Footer</footer>
</body>
</html>
```

**CSS — contoh styling:**

```css
/* Selector dasar */
h1 { color: blue; }                /* element selector */
.container { width: 80%; }         /* class selector (.) */
#header { background: #333; }      /* id selector (#) */

/* Box Model: content → padding → border → margin */
.box {
    width: 200px;          /* lebar konten */
    padding: 10px;         /* jarak konten ke border */
    border: 1px solid #ccc; /* garis tepi */
    margin: 20px;          /* jarak ke elemen lain */
}

/* Flexbox (layout modern) */
.flex-container {
    display: flex;
    justify-content: center;   /* horizontal center */
    align-items: center;       /* vertical center */
    gap: 10px;                 /* jarak antar item */
}

/* Responsive (menyesuaikan ukuran layar) */
@media (max-width: 768px) {
    .container { width: 100%; }
}
```

**CSS Box Model:**
```
┌──────────────────────────────┐
│          MARGIN              │  ← Jarak ke elemen lain (transparan)
│  ┌──────────────────────┐   │
│  │       BORDER         │   │  ← Garis tepi
│  │  ┌──────────────┐   │   │
│  │  │   PADDING     │   │   │  ← Jarak konten ke border
│  │  │  ┌────────┐  │   │   │
│  │  │  │CONTENT │  │   │   │  ← Isi sebenarnya (teks, gambar)
│  │  │  └────────┘  │   │   │
│  │  └──────────────┘   │   │
│  └──────────────────────┘   │
└──────────────────────────────┘
```

**JavaScript — contoh interaktivitas:**

```javascript
// Variabel
let nama = "Andi";        // bisa diubah
const PI = 3.14;           // tidak bisa diubah
var umur = 21;             // cara lama (hindari)

// Function
function salam(nama) {
    return "Hello, " + nama + "!";
}

// Arrow function (ES6)
const tambah = (a, b) => a + b;

// DOM Manipulation (mengubah halaman web)
document.getElementById("judul").innerText = "Teks baru";
document.querySelector(".btn").addEventListener("click", function() {
    alert("Tombol diklik!");
});

// Array methods
let angka = [1, 2, 3, 4, 5];
let genap = angka.filter(n => n % 2 === 0);    // [2, 4]
let kaliDua = angka.map(n => n * 2);           // [2, 4, 6, 8, 10]
let total = angka.reduce((a, b) => a + b, 0);  // 15

// Async/Await (mengambil data dari API)
async function ambilData() {
    const response = await fetch("https://api.example.com/users");
    const data = await response.json();
    console.log(data);
}
```

---

### Frontend vs Backend

| Aspek | Frontend | Backend |
|-------|----------|---------|
| **Definisi** | Bagian yang **dilihat & diinteraksikan** user | Bagian yang **memproses logika & data** di server |
| **Berjalan di** | **Browser** (client-side) | **Server** (server-side) |
| **Teknologi** | HTML, CSS, JavaScript, React, Vue, Angular | PHP, Node.js, Python, Java, Go, Ruby |
| **Tugas** | Tampilan UI, form, animasi, validasi input | API, database, autentikasi, business logic |
| **Contoh pekerjaan** | Mendesain halaman login | Memproses username & password ke database |

**Full-stack** = menguasai frontend + backend.

**Contoh alur lengkap login:**
```
1. [Frontend] User isi form login (username + password)
2. [Frontend] JavaScript validasi: apakah field kosong?
3. [Frontend] Kirim POST request ke /api/login
4. [Backend]  Server terima request
5. [Backend]  Query database: SELECT * FROM users WHERE username = '...'
6. [Backend]  Bandingkan password (bcrypt hash)
7. [Backend]  Jika cocok → buat JWT token, kirim response 200
8. [Backend]  Jika tidak → kirim response 401 Unauthorized
9. [Frontend] Terima response, simpan token, redirect ke dashboard
```

---

### HTTP / HTTPS

**HTTP (HyperText Transfer Protocol)** adalah protokol komunikasi antara client (browser) dan server.

**HTTPS** = HTTP + **SSL/TLS** (enkripsi) → data terenkripsi saat dikirim.

| Aspek | HTTP | HTTPS |
|-------|------|-------|
| **Port** | 80 | 443 |
| **Keamanan** | Tidak terenkripsi (bisa disadap) | Terenkripsi (SSL/TLS) |
| **URL** | `http://` | `https://` |
| **Sertifikat** | Tidak perlu | Butuh SSL certificate |
| **Penggunaan** | Website non-sensitif | Login, pembayaran, data pribadi |

**Anatomi HTTP Request:**
```
GET /api/mahasiswa HTTP/1.1        ← Method + URL + Versi HTTP
Host: www.example.com              ← Domain tujuan
Authorization: Bearer eyJhbG...    ← Token autentikasi (opsional)
Content-Type: application/json     ← Format data yang dikirim
Accept: application/json           ← Format data yang diharapkan
                                   ← Baris kosong (pemisah header & body)
{ "nim": "001" }                   ← Body (untuk POST/PUT)
```

**HTTP Status Code:**

| Kode | Kategori | Arti | Contoh Penggunaan |
|------|----------|------|-------------------|
| **200** | ✅ Success | OK | Request berhasil |
| **201** | ✅ Success | Created | Data baru berhasil dibuat (setelah POST) |
| **204** | ✅ Success | No Content | Berhasil tapi tidak ada data yang dikembalikan |
| **301** | ↗️ Redirect | Moved Permanently | URL pindah permanen |
| **304** | ↗️ Redirect | Not Modified | Data belum berubah, pakai cache |
| **400** | ❌ Client Error | Bad Request | Request tidak valid (format salah) |
| **401** | ❌ Client Error | Unauthorized | Belum login / token expired |
| **403** | ❌ Client Error | Forbidden | Sudah login tapi tidak punya akses |
| **404** | ❌ Client Error | Not Found | URL/resource tidak ditemukan |
| **405** | ❌ Client Error | Method Not Allowed | Pakai GET tapi endpoint hanya terima POST |
| **500** | 💥 Server Error | Internal Server Error | Bug di server / server crash |
| **503** | 💥 Server Error | Service Unavailable | Server sedang down / maintenance |

---

### REST API & HTTP Method

**REST (Representational State Transfer)** adalah arsitektur untuk membangun API yang berkomunikasi melalui HTTP.

**6 Prinsip REST:**
1. **Stateless** — Server tidak menyimpan state client. Setiap request berdiri sendiri
2. **Client-Server** — Pemisahan frontend dan backend
3. **Uniform Interface** — Menggunakan HTTP method standar + URL yang konsisten
4. **Resource-based** — Setiap data diakses via URL (endpoint)
5. **Cacheable** — Response bisa di-cache untuk performa
6. **Layered System** — Bisa ada proxy/load balancer di antara client dan server

**HTTP Method (CRUD):**

| Method | Fungsi | CRUD | Idempotent? | Contoh |
|--------|--------|------|-------------|--------|
| **GET** | Mengambil data | **R**ead | ✅ Ya | `GET /api/users` |
| **POST** | Membuat data baru | **C**reate | ❌ Tidak | `POST /api/users` + body |
| **PUT** | Mengubah seluruh data | **U**pdate (full) | ✅ Ya | `PUT /api/users/1` + body |
| **PATCH** | Mengubah sebagian data | **U**pdate (partial) | ✅ Ya | `PATCH /api/users/1` + body |
| **DELETE** | Menghapus data | **D**elete | ✅ Ya | `DELETE /api/users/1` |

> **Idempotent** = dipanggil 1x atau 100x hasilnya SAMA. GET ambil data → hasil sama. DELETE hapus data → sudah terhapus, panggil lagi tetap sudah terhapus. POST buat data → tiap panggilan bikin data baru (TIDAK idempotent).

**Contoh endpoint REST API yang baik:**
```
GET    /api/mahasiswa           → Ambil semua mahasiswa
GET    /api/mahasiswa/1         → Ambil mahasiswa id=1
POST   /api/mahasiswa           → Tambah mahasiswa baru (data di body)
PUT    /api/mahasiswa/1         → Update semua field mahasiswa id=1
PATCH  /api/mahasiswa/1         → Update sebagian field mahasiswa id=1
DELETE /api/mahasiswa/1         → Hapus mahasiswa id=1

GET    /api/mahasiswa/1/nilai   → Ambil semua nilai mahasiswa id=1
POST   /api/mahasiswa/1/nilai   → Tambah nilai baru untuk mahasiswa id=1
```

---

### JSON

**JSON (JavaScript Object Notation)** adalah format pertukaran data yang **ringan**, **mudah dibaca** manusia, dan **mudah diproses** mesin.

```json
{
    "nim": "001",
    "nama": "Andi Wijaya",
    "prodi": "Teknik Informatika",
    "ipk": 3.75,
    "aktif": true,
    "alamat": null,
    "mata_kuliah": ["Database", "Programming", "Web"],
    "wali_dosen": {
        "nip": "D001",
        "nama": "Dr. Budi"
    }
}
```

| Tipe Data JSON | Contoh | Keterangan |
|----------------|--------|------------|
| **String** | `"nama": "Andi"` | Teks dalam tanda kutip ganda |
| **Number** | `"ipk": 3.75` | Integer atau desimal (tanpa kutip) |
| **Boolean** | `"aktif": true` | `true` atau `false` |
| **Array** | `"mk": ["DB", "Web"]` | Daftar berurutan dalam `[]` |
| **Object** | `"dosen": { ... }` | Pasangan key-value dalam `{}` |
| **Null** | `"alamat": null` | Tidak ada nilai |

**Perbedaan JSON vs XML:**

| Aspek | JSON | XML |
|-------|------|-----|
| **Ukuran** | Lebih kecil | Lebih besar (banyak tag) |
| **Keterbacaan** | Lebih mudah dibaca | Lebih verbose |
| **Parsing** | Cepat (native JS) | Lebih lambat |
| **Penggunaan** | REST API, modern web | SOAP, konfigurasi legacy |

---

### Authentication & Authorization

| Aspek | Authentication (AuthN) | Authorization (AuthZ) |
|-------|----------------------|---------------------|
| **Pertanyaan** | "**Siapa** kamu?" | "Apa yang **boleh** kamu lakukan?" |
| **Proses** | Verifikasi **identitas** | Verifikasi **hak akses** |
| **Urutan** | Dilakukan **pertama** | Dilakukan **setelah** autentikasi |
| **Contoh** | Login username + password | Admin boleh hapus, user biasa tidak |
| **Gagal** | 401 Unauthorized | 403 Forbidden |

**Contoh alur:**
```
1. User login (Authentication) → "Saya Andi, password 12345"
2. Server verifikasi → "OK, kamu memang Andi" ✅
3. Andi mau hapus data (Authorization) → "Apakah Andi boleh hapus?"
4. Cek role: Andi = "user biasa" → "TIDAK boleh hapus!" ❌ (403 Forbidden)
5. Admin mau hapus data → Cek role: Admin = "admin" → "BOLEH!" ✅
```

---

### Session vs Token (JWT)

| Aspek | Session | Token (JWT) |
|-------|---------|-------------|
| **Disimpan di** | **Server** (session store / Redis) | **Client** (localStorage / cookie) |
| **Stateful/Stateless** | **Stateful** — server harus ingat session | **Stateless** — token self-contained |
| **Skalabilitas** | Kurang scalable (server harus sync) | Sangat scalable |
| **Cara kerja** | Server buat session ID → kirim via cookie → client kirim cookie setiap request | Server buat token → client simpan → client kirim token di header Authorization |
| **Logout** | Hapus session di server | Hapus token di client (atau blacklist) |

**Alur Session-based:**
```
1. User login → Server buat session, simpan di server (session_id: "abc123")
2. Server kirim cookie: Set-Cookie: session_id=abc123
3. Setiap request berikutnya, browser otomatis kirim cookie
4. Server cek: "ada session abc123 ga di memory?" → ada → OK
5. Logout: server hapus session abc123
```

**Alur Token-based (JWT):**
```
1. User login → Server buat JWT token (berisi user_id, role, expiry)
2. Server kirim token ke client
3. Client simpan di localStorage/cookie
4. Setiap request: header "Authorization: Bearer eyJhbG..."
5. Server TIDAK perlu menyimpan apa-apa — tinggal verifikasi signature token
```

**Struktur JWT (3 bagian dipisahkan titik):**
```
eyJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoxfQ.abc123signature
│                    │ │                   │ │              │
└──── HEADER ────────┘ └──── PAYLOAD ──────┘ └── SIGNATURE ─┘

Header:    { "alg": "HS256", "typ": "JWT" }
Payload:   { "user_id": 1, "role": "admin", "exp": 1234567890 }
Signature: HMACSHA256(base64(header) + "." + base64(payload), "secret_key")
```

---

### Web Server & Database pada Web

**Web Server** adalah software yang menerima HTTP request dan mengirim HTTP response.

| Web Server | Keterangan | Market Share |
|------------|------------|-------------|
| **Apache** | Open source, modular, .htaccess | Paling populer (legacy) |
| **Nginx** | Ringan, cepat, reverse proxy, load balancing | Populer untuk high traffic |
| **IIS** | Microsoft, Windows Server | Enterprise Microsoft |

**Database pada Aplikasi Web:**

| Tipe | Penjelasan | Contoh | Cocok Untuk |
|------|------------|--------|------------|
| **SQL (Relasional)** | Data dalam tabel berhubungan, schema fixed | MySQL, PostgreSQL, SQLite | Transaksi, data terstruktur |
| **NoSQL (Non-Relasional)** | Data fleksibel, schema-less | MongoDB, Redis, Firebase | Big data, real-time, data tidak terstruktur |

**Arsitektur MVC (Model-View-Controller):**

| Komponen | Tugas | Contoh |
|----------|-------|--------|
| **Model** | Mengelola data & business logic | Class Mahasiswa, query database |
| **View** | Menampilkan data ke user (UI) | File HTML, template EJS/Blade |
| **Controller** | Menerima input, mengatur alur logika | Route handler |

```
User klik tombol "Lihat Nilai"
  → Controller menerima request GET /nilai
  → Controller minta data ke Model
  → Model query database → return data
  → Controller kirim data ke View
  → View render HTML dengan data → tampilkan ke User
```

---

## Contoh Pertanyaan & Jawaban — Web Systems

<details>
<summary><b>Q: Apa perbedaan GET dan POST?</b></summary>

| Aspek | GET | POST |
|-------|-----|------|
| **Fungsi** | **Mengambil** data | **Mengirim/membuat** data |
| **Data dikirim via** | URL (query string): `?nama=Andi&umur=21` | Body request (tidak terlihat di URL) |
| **Keamanan** | Kurang aman (data terlihat di URL & history browser) | Lebih aman (data di body) |
| **Idempotent** | Ya (panggil berkali-kali hasilnya sama) | Tidak (tiap panggilan bisa buat data baru) |
| **Caching** | Bisa di-cache browser | Tidak di-cache |
| **Panjang data** | Terbatas (~2048 karakter URL) | Tidak terbatas |
| **Bookmark** | Bisa di-bookmark | Tidak bisa |
</details>

<details>
<summary><b>Q: Apa perbedaan Cookie, Local Storage, dan Session Storage?</b></summary>

| Aspek | Cookie | Local Storage | Session Storage |
|-------|--------|---------------|-----------------|
| **Kapasitas** | ~4 KB | ~5-10 MB | ~5-10 MB |
| **Expiry** | Bisa diatur (tanggal tertentu) | Tidak expired (permanen) | Hilang saat **tab** ditutup |
| **Dikirim ke server** | ✅ Ya (setiap HTTP request) | ❌ Tidak | ❌ Tidak |
| **Akses** | Client + Server | Client only (JavaScript) | Client only (JavaScript) |
| **Penggunaan** | Session ID, tracking, preferences | User settings, cache data | Data sementara saat browsing |
</details>

<details>
<summary><b>Q: Apa itu CORS dan kenapa penting?</b></summary>

**CORS (Cross-Origin Resource Sharing)** adalah mekanisme keamanan browser yang mengontrol akses resource dari **domain yang berbeda**.

**Masalah:** Browser menerapkan **Same-Origin Policy** — secara default, JavaScript di `siteA.com` **tidak boleh** request ke `siteB.com`.

**Solusi:** Server bisa mengizinkan akses dari domain lain via header CORS:

```
Access-Control-Allow-Origin: https://frontend.com
Access-Control-Allow-Methods: GET, POST, PUT, DELETE
Access-Control-Allow-Headers: Content-Type, Authorization
```

**Kenapa penting?** Karena di arsitektur modern, frontend dan backend sering di domain berbeda (frontend: `app.com`, backend API: `api.com`).
</details>

---

# 5️⃣ Enterprise Networking

## Daftar Materi

- OSI Model (7 Layer) & TCP/IP
- LAN / WAN
- IP Address & Subnetting
- Router & Switch
- VLAN
- DHCP, DNS, NAT
- TCP vs UDP
- Routing
- Firewall & Network Security
- Topologi Jaringan

---

## Ringkasan Teori

### OSI Model (7 Layer)

**OSI (Open Systems Interconnection)** adalah model referensi 7 layer yang menjelaskan bagaimana data dikirim melalui jaringan.

| No | Layer | Fungsi | Protokol/Contoh | Perangkat | Data Unit |
|----|-------|--------|-----------------|-----------|-----------|
| 7 | **Application** | Interface untuk user/aplikasi | HTTP, FTP, SMTP, DNS, SSH | - | Data |
| 6 | **Presentation** | Format, enkripsi, kompresi data | SSL/TLS, JPEG, ASCII, MPEG | - | Data |
| 5 | **Session** | Membuka/memelihara/menutup sesi komunikasi | NetBIOS, RPC, PPTP | - | Data |
| 4 | **Transport** | Pengiriman end-to-end, error recovery, flow control | **TCP**, **UDP** | - | **Segment** |
| 3 | **Network** | Routing & logical addressing (IP) | **IP**, ICMP, ARP | **Router** | **Packet** |
| 2 | **Data Link** | Frame, MAC addressing, error detection | Ethernet, PPP, ARP | **Switch**, Bridge | **Frame** |
| 1 | **Physical** | Transmisi bit mentah (sinyal fisik) | Kabel UTP, Fiber Optik, WiFi | Hub, Repeater | **Bits** |

> 💡 **Tips hafal dari atas:** "**A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing"
> 💡 **Tips hafal dari bawah:** "**P**lease **D**o **N**ot **T**hrow **S**ausage **P**izza **A**way"

**Proses Enkapsulasi (kirim data):**
```
Layer 7-5: [DATA]                                    ← Aplikasi membuat data
Layer 4:   [TCP Header | DATA]                       ← Tambah port → Segment
Layer 3:   [IP Header | TCP Header | DATA]           ← Tambah IP → Packet
Layer 2:   [MAC Header | IP | TCP | DATA | Trailer]  ← Tambah MAC → Frame
Layer 1:   10110100101...                             ← Ubah ke sinyal → Bits
```

**Proses De-enkapsulasi (terima data) = kebalikannya:**
```
Layer 1: Terima bits → ubah ke frame
Layer 2: Buka MAC header → kirim ke layer 3
Layer 3: Buka IP header → kirim ke layer 4
Layer 4: Buka TCP header → kirim ke layer 5-7
Layer 7: Terima data asli
```

---

### TCP/IP Model

| Layer TCP/IP | Setara OSI | Protokol |
|-------------|------------|----------|
| **Application** | Layer 7 + 6 + 5 | HTTP, FTP, DNS, SMTP, SSH |
| **Transport** | Layer 4 | TCP, UDP |
| **Internet** | Layer 3 | IP, ICMP, ARP |
| **Network Access** | Layer 2 + 1 | Ethernet, Wi-Fi, PPP |

---

### LAN vs WAN

| Aspek | LAN | WAN |
|-------|-----|-----|
| **Jangkauan** | Lokal (satu gedung/kampus) | Luas (antar kota/negara) |
| **Kecepatan** | Tinggi (100 Mbps – 10 Gbps) | Relatif lebih rendah |
| **Kepemilikan** | Pribadi/organisasi | ISP/pihak ketiga |
| **Biaya** | Rendah | Tinggi |
| **Latency** | Rendah | Tinggi |
| **Contoh** | Jaringan kantor, lab komputer | Internet, MPLS, VPN site-to-site |

---

### IP Address & Subnetting

**Subnetting** adalah teknik membagi satu jaringan besar menjadi beberapa sub-jaringan (subnet) yang lebih kecil.

**Kenapa subnetting?**
- **Efisiensi IP** — tidak membuang alamat IP
- **Keamanan** — memisahkan traffic antar departemen
- **Performa** — mengurangi broadcast domain

**Langkah menghitung Subnet — contoh `192.168.1.0/26`:**

```
Step 1: Ubah prefix ke subnet mask
  /26 → 26 bit pertama = 1, sisanya 0
  11111111.11111111.11111111.11000000
  = 255.255.255.192

Step 2: Hitung block size
  Block size = 256 - 192 = 64
  Artinya: setiap subnet berukuran 64 alamat

Step 3: Hitung jumlah subnet
  Bit subnet = 26 - 24 = 2 bit dipinjam
  Jumlah subnet = 2^2 = 4 subnet

Step 4: Hitung host per subnet
  Bit host = 32 - 26 = 6 bit
  Jumlah host = 2^6 - 2 = 62 host per subnet
  (-2 karena: 1 untuk Network Address, 1 untuk Broadcast)

Step 5: Buat daftar subnet
  Subnet 1: 192.168.1.0   – 192.168.1.63
            Network: .0    | Host: .1-.62     | Broadcast: .63
  Subnet 2: 192.168.1.64  – 192.168.1.127
            Network: .64   | Host: .65-.126   | Broadcast: .127
  Subnet 3: 192.168.1.128 – 192.168.1.191
            Network: .128  | Host: .129-.190  | Broadcast: .191
  Subnet 4: 192.168.1.192 – 192.168.1.255
            Network: .192  | Host: .193-.254  | Broadcast: .255
```

**Contoh soal lain — `10.0.0.0/28`:**
```
/28 → Subnet Mask: 255.255.255.240
Block size: 256 - 240 = 16
Jumlah subnet: 2^4 = 16 (dari /24)
Host per subnet: 2^4 - 2 = 14

Subnet 1:  10.0.0.0   – 10.0.0.15   (Host: .1-.14)
Subnet 2:  10.0.0.16  – 10.0.0.31   (Host: .17-.30)
Subnet 3:  10.0.0.32  – 10.0.0.47   (Host: .33-.46)
...
```

**Tabel subnet umum:**

| CIDR | Subnet Mask | Block Size | Jumlah Host |
|------|-------------|------------|-------------|
| /24 | 255.255.255.0 | 256 | 254 |
| /25 | 255.255.255.128 | 128 | 126 |
| /26 | 255.255.255.192 | 64 | 62 |
| /27 | 255.255.255.224 | 32 | 30 |
| /28 | 255.255.255.240 | 16 | 14 |
| /29 | 255.255.255.248 | 8 | 6 |
| /30 | 255.255.255.252 | 4 | 2 |

> **Rumus cepat:** Jumlah host = `2^(32 - prefix) - 2`

---

### Router & Switch

| Aspek | Router | Switch |
|-------|--------|--------|
| **Layer OSI** | Layer 3 (Network) | Layer 2 (Data Link) |
| **Berdasarkan** | **IP Address** | **MAC Address** |
| **Fungsi** | Menghubungkan **jaringan BERBEDA** | Menghubungkan **perangkat dalam SATU jaringan** |
| **Broadcast domain** | Memisahkan broadcast domain | Satu broadcast domain |
| **Tabel** | Routing table | MAC address table |
| **Contoh** | Menghubungkan LAN ke internet | Menghubungkan PC, printer di satu LAN |

**Perbedaan Hub vs Switch vs Router:**

| Aspek | Hub | Switch | Router |
|-------|-----|--------|--------|
| **Layer** | 1 (Physical) | 2 (Data Link) | 3 (Network) |
| **Kirim data ke** | Semua port (broadcast) | Port tujuan saja (unicast) | Jaringan tujuan |
| **Collision domain** | 1 (semua port sharing) | Per port (terpisah) | Per interface |
| **Cerdas?** | Tidak | Ya (MAC table) | Ya (routing table) |

---

### VLAN (Virtual LAN)

**VLAN** membagi satu switch fisik menjadi beberapa **jaringan logis** yang terpisah.

```
Tanpa VLAN (semua bisa komunikasi):     Dengan VLAN (terpisah):
┌─────────────────────┐                ┌─────────────────────────┐
│      Switch         │                │        Switch           │
│  PC1──PC2──PC3──PC4 │                │  VLAN 10    VLAN 20     │
│  (semua 1 jaringan) │                │  PC1, PC2   PC3, PC4    │
└─────────────────────┘                │  (IT Dept)  (HR Dept)   │
                                       └─────────────────────────┘
                                       PC1 ↔ PC2 ✅ (VLAN sama)
                                       PC1 ↔ PC3 ❌ (VLAN beda, butuh router)
```

**Manfaat VLAN:**
- **Keamanan** — Traffic IT dan HR terpisah
- **Efisiensi** — Broadcast hanya di dalam VLAN
- **Fleksibilitas** — PC bisa dipindah VLAN tanpa pindah kabel
- **Manajemen** — Mudah mengelola jaringan besar

**Tipe port VLAN:**

| Tipe Port | Fungsi | Membawa VLAN |
|-----------|--------|-------------|
| **Access Port** | Terhubung ke end device (PC, printer) | 1 VLAN saja |
| **Trunk Port** | Menghubungkan antar switch | Banyak VLAN (dengan tagging 802.1Q) |

**Inter-VLAN Routing:** Agar VLAN berbeda bisa berkomunikasi, butuh **Router** atau **Layer 3 Switch**.

---

### DHCP, DNS, NAT

#### DHCP (Dynamic Host Configuration Protocol)

Memberikan IP address **secara otomatis** ke perangkat dalam jaringan.

**Analogi:** Resepsionis hotel → otomatis memberikan nomor kamar saat tamu check-in.

**Proses DHCP — DORA:**
```
1. [D]iscover → Client broadcast: "Ada DHCP server tidak?"
2. [O]ffer    → Server: "Ada! Saya tawarkan IP 192.168.1.10"
3. [R]equest  → Client: "OK, saya mau IP 192.168.1.10"
4. [A]ck      → Server: "Setuju! IP 192.168.1.10 milik kamu selama 24 jam"
```

**Yang diberikan DHCP:** IP address, subnet mask, default gateway, DNS server.

#### DNS (Domain Name System)

Menerjemahkan **nama domain** (google.com) menjadi **IP address** (142.250.x.x).

**Analogi:** Buku kontak telepon → kita cari nama "Andi" → dapat nomor 08123456.

**Hierarki DNS:**
```
User ketik "www.google.com"
  → 1. Browser cek cache lokal
  → 2. OS cek file hosts
  → 3. DNS Resolver (ISP/Google DNS 8.8.8.8)
  → 4. Root DNS Server ("." — tahu semua TLD)
  → 5. TLD DNS Server (".com" — tahu domain .com)
  → 6. Authoritative DNS ("google.com" — tahu IP Google)
  → Return IP: 142.250.190.78
  → Browser kirim request ke IP tersebut
```

**Tipe DNS Record:**

| Record | Fungsi | Contoh |
|--------|--------|--------|
| **A** | Domain → IPv4 | google.com → 142.250.190.78 |
| **AAAA** | Domain → IPv6 | google.com → 2404:6800::200e |
| **CNAME** | Alias domain | www.google.com → google.com |
| **MX** | Mail server | google.com → mail.google.com |
| **NS** | Name server | google.com → ns1.google.com |

#### NAT (Network Address Translation)

Mengubah **IP private** menjadi **IP public** sehingga perangkat di LAN bisa akses internet.

**Analogi:** Resepsionis kantor → semua karyawan menelepon keluar lewat 1 nomor kantor. Dari luar, yang terlihat hanya nomor kantor (IP public), bukan nomor pribadi (IP private).

```
┌──────────────────────┐                    ┌──────────────┐
│     LAN (Private)    │     NAT Router     │   Internet   │
│                      │                    │              │
│  PC1: 192.168.1.10 ──┤                    │              │
│  PC2: 192.168.1.11 ──┤── 203.0.113.5 ───→│  google.com  │
│  PC3: 192.168.1.12 ──┤   (IP Public)      │              │
│                      │                    │              │
└──────────────────────┘                    └──────────────┘
Dari luar, semua PC terlihat sebagai 203.0.113.5
```

**Tipe NAT:**

| Tipe | Penjelasan | Contoh |
|------|------------|--------|
| **Static NAT** | 1 IP private → 1 IP public (tetap) | Web server internal yang perlu diakses dari luar |
| **Dynamic NAT** | IP private → IP public dari pool (berubah-ubah) | Pool 5 IP public untuk 20 PC |
| **PAT / NAT Overload** | Banyak IP private → **1 IP public** (beda port) | Yang paling umum di rumah/kantor |

---

### TCP vs UDP

| Aspek | TCP | UDP |
|-------|-----|-----|
| **Nama** | Transmission Control Protocol | User Datagram Protocol |
| **Connection** | Connection-oriented (3-way handshake) | Connectionless |
| **Reliabilitas** | Reliable (ada ACK, retransmission) | Unreliable (kirim & lupakan) |
| **Urutan data** | Terjamin urut (sequence number) | Tidak dijamin urut |
| **Kecepatan** | Lebih lambat (overhead besar) | Lebih cepat (overhead kecil) |
| **Header size** | 20 byte | 8 byte |
| **Error checking** | Ya + recovery | Ya tapi tanpa recovery |
| **Flow control** | Ada (window size) | Tidak ada |
| **Contoh** | HTTP/HTTPS, FTP, Email (SMTP), SSH | DNS, Video streaming, VoIP, Gaming online |

**Kenapa pilih UDP untuk streaming/gaming?** Karena lebih penting **cepat** daripada **akurat**. Kalau 1 frame video hilang, skip saja. Kalau pakai TCP, harus kirim ulang → lag!

**TCP 3-Way Handshake:**
```
Client          Server
  │── SYN ──────→ │    1. Client: "Halo, saya mau koneksi" (SYN)
  │←── SYN-ACK ──│    2. Server: "OK, saya juga siap" (SYN-ACK)
  │── ACK ──────→ │    3. Client: "Baik, koneksi terbuka!" (ACK)
  │               │
  │←─── DATA ───→│    4. Kirim-terima data...
  │               │
  │── FIN ──────→ │    5. Client: "Saya mau putus"
  │←── ACK ──────│    6. Server: "OK"
  │←── FIN ──────│    7. Server: "Saya juga selesai"
  │── ACK ──────→ │    8. Client: "Bye!" → Koneksi ditutup
```

---

### Routing

**Routing** adalah proses menentukan **jalur terbaik** untuk mengirim data dari sumber ke tujuan melalui jaringan.

| Tipe Routing | Penjelasan | Kelebihan | Kekurangan |
|-------------|------------|-----------|------------|
| **Static Routing** | Route dikonfigurasi **manual** oleh admin | Aman, hemat resource | Tidak adaptif, ribet untuk jaringan besar |
| **Dynamic Routing** | Route ditentukan **otomatis** oleh protokol | Adaptif, scalable | Butuh resource, lebih kompleks |
| **Default Route** | Route "catch-all" untuk tujuan yang tidak dikenal | Simpel | Tidak efisien untuk jaringan besar |

**Protokol routing dinamis:**

| Protokol | Tipe | Metrik | Cocok Untuk |
|----------|------|--------|------------|
| **RIP** (v1, v2) | Distance Vector | Hop count (max 15) | Jaringan kecil |
| **OSPF** | Link State | Cost (bandwidth) | Jaringan menengah-besar |
| **EIGRP** | Hybrid | Bandwidth + delay | Jaringan Cisco |
| **BGP** | Path Vector | AS path, policy | Antar ISP / internet |

---

### Firewall & Network Security

**Firewall** adalah sistem keamanan yang **memfilter traffic jaringan** berdasarkan aturan yang ditentukan.

**Analogi:** Satpam di gerbang → memeriksa siapa yang boleh masuk/keluar berdasarkan daftar aturan.

| Tipe Firewall | Layer | Penjelasan |
|---------------|-------|------------|
| **Packet Filter** | 3-4 | Filter berdasarkan IP, port, protokol |
| **Stateful Inspection** | 3-4 | Melacak state koneksi (lebih cerdas) |
| **Application Layer (WAF)** | 7 | Filter berdasarkan konten aplikasi |
| **Next-Generation (NGFW)** | 3-7 | Kombinasi semua + deep packet inspection |

**Konsep keamanan jaringan:**

| Konsep | Penjelasan | Analogi |
|--------|------------|---------|
| **IDS** (Intrusion Detection System) | **Mendeteksi** serangan (pasif — hanya alert) | CCTV yang merekam |
| **IPS** (Intrusion Prevention System) | **Mendeteksi + memblokir** serangan (aktif) | Satpam yang langsung tangkap |
| **VPN** (Virtual Private Network) | Koneksi **terenkripsi** melalui internet publik | Terowongan rahasia |
| **ACL** (Access Control List) | Daftar aturan **allow/deny** traffic | Daftar tamu undangan |
| **DMZ** (Demilitarized Zone) | Zona jaringan antara LAN & internet untuk server publik | Halaman depan rumah |

---

### Topologi Jaringan

| Topologi | Bentuk | Kelebihan | Kekurangan |
|----------|--------|-----------|------------|
| **Star** ⭐ | Semua terhubung ke 1 pusat (switch/hub) | Mudah kelola, 1 node mati tidak pengaruh | Pusat mati → semua mati |
| **Bus** | 1 kabel utama, semua terhubung | Murah, sederhana | Kabel putus → semua mati |
| **Ring** 🔄 | Membentuk lingkaran | Data teratur, token passing | 1 node mati → putus |
| **Mesh** 🕸️ | Setiap node terhubung ke semua | Sangat reliable, redundant | Mahal, banyak kabel |
| **Tree** 🌳 | Hierarki (star + bus) | Scalable, terorganisir | Backbone mati → cabang mati |
| **Hybrid** | Kombinasi beberapa topologi | Fleksibel | Kompleks |

> **Yang paling umum dipakai:** **Star** (di kantor/kampus) karena mudah dikelola dan troubleshoot.

---

## Contoh Pertanyaan & Jawaban — Enterprise Networking

<details>
<summary><b>Q: Jelaskan proses pengiriman data dari Layer 7 ke Layer 1 (Enkapsulasi)!</b></summary>

**Enkapsulasi** adalah proses penambahan header di setiap layer saat data dikirim dari atas ke bawah.

| Layer | Nama Data | Header yang Ditambahkan |
|-------|-----------|------------------------|
| Application (7-5) | **Data** | Data dari aplikasi |
| Transport (4) | **Segment** | Port number sumber & tujuan (TCP/UDP header) |
| Network (3) | **Packet** | IP address sumber & tujuan (IP header) |
| Data Link (2) | **Frame** | MAC address sumber & tujuan (Ethernet header + trailer FCS) |
| Physical (1) | **Bits** | Diubah menjadi sinyal listrik / cahaya / gelombang radio |

Proses sebaliknya (penerima) disebut **De-enkapsulasi** — membuka header dari Layer 1 ke Layer 7.
</details>

<details>
<summary><b>Q: Bagaimana cara menghitung Subnetting? Contoh: 192.168.10.0/27</b></summary>

```
Step 1: /27 → Subnet Mask = 255.255.255.224
        Binary: 11111111.11111111.11111111.111 00000
                                             ↑  ↑
                                     3 bit subnet  5 bit host

Step 2: Block size = 256 - 224 = 32

Step 3: Jumlah subnet = 2^3 = 8

Step 4: Jumlah host = 2^5 - 2 = 30

Step 5: Daftar subnet:
  Subnet 1: 192.168.10.0   – 192.168.10.31  (Host: .1 – .30)
  Subnet 2: 192.168.10.32  – 192.168.10.63  (Host: .33 – .62)
  Subnet 3: 192.168.10.64  – 192.168.10.95  (Host: .65 – .94)
  Subnet 4: 192.168.10.96  – 192.168.10.127 (Host: .97 – .126)
  Subnet 5: 192.168.10.128 – 192.168.10.159 (Host: .129 – .158)
  Subnet 6: 192.168.10.160 – 192.168.10.191 (Host: .161 – .190)
  Subnet 7: 192.168.10.192 – 192.168.10.223 (Host: .193 – .222)
  Subnet 8: 192.168.10.224 – 192.168.10.255 (Host: .225 – .254)
```
</details>

<details>
<summary><b>Q: Apa itu ARP dan bagaimana cara kerjanya?</b></summary>

**ARP (Address Resolution Protocol)** menerjemahkan **IP address** (Layer 3) menjadi **MAC address** (Layer 2).

**Kenapa perlu?** Karena di dalam LAN, switch mengirim data berdasarkan **MAC address**, bukan IP address.

```
1. PC-A ingin kirim data ke PC-B (IP: 192.168.1.2)
2. PC-A cek ARP cache → "MAC 192.168.1.2 ada ga?" → tidak ada
3. PC-A broadcast ARP Request ke semua perangkat: 
   "Hey, siapa pemilik IP 192.168.1.2? Kasih tahu MAC address kamu!"
4. PC-B menjawab (unicast) ARP Reply: 
   "Itu saya! MAC address saya: AA:BB:CC:DD:EE:FF"
5. PC-A simpan di ARP cache (agar tidak perlu broadcast lagi)
6. PC-A kirim data ke MAC AA:BB:CC:DD:EE:FF melalui switch
```
</details>

<details>
<summary><b>Q: Apa perbedaan Unicast, Broadcast, dan Multicast?</b></summary>

| Tipe | Penjelasan | Jumlah Penerima | Contoh |
|------|------------|-----------------|--------|
| **Unicast** | Satu pengirim → **satu** penerima | 1 | Browsing web, kirim email |
| **Broadcast** | Satu pengirim → **semua** perangkat di jaringan | Semua | ARP request, DHCP discover |
| **Multicast** | Satu pengirim → **sekelompok** penerima tertentu | Beberapa | Video conference, IPTV, OSPF |
</details>

---

# 📋 Checklist Belajar

## Prioritas Tinggi ⭐⭐⭐
- [ ] **Database** — Normalisasi (1NF, 2NF, 3NF), ERD, SQL (SELECT, JOIN, GROUP BY, HAVING), ACID
- [ ] **Programming** — OOP (4 pilar + contoh kode), Percabangan, Perulangan
- [ ] **Web** — REST API, HTTP Method (GET/POST/PUT/DELETE), HTTP Status Code, Authentication

## Prioritas Sedang ⭐⭐
- [ ] **Networking** — OSI 7 Layer, TCP vs UDP, Subnetting (cara hitung!), 3-Way Handshake
- [ ] **IT** — Cloud Computing (IaaS/PaaS/SaaS), CIA Triad, Virtualisasi vs Container

## Prioritas Normal ⭐
- [ ] **Database** — Index, Subquery, View, Transaction
- [ ] **Programming** — Error Handling (try-catch), Recursion, Stack vs Queue, Static keyword
- [ ] **Web** — Session vs Token (JWT), CORS, MVC, Cookie vs LocalStorage, JSON vs XML
- [ ] **Networking** — VLAN, DHCP (DORA), DNS, NAT, Routing (Static vs Dynamic), Firewall
- [ ] **IT** — Sistem Operasi, Enkripsi (Simetris vs Asimetris), IoT

---

# 🔥 Tips Belajar

1. ✅ Baca ringkasan teori setiap mata kuliah **minimal 2 kali**
2. ✅ Coba jawab contoh pertanyaan **tanpa melihat jawaban** dulu
3. ✅ Tulis ulang jawaban dengan **bahasa sendiri** (bukan hafalan)
4. ✅ Fokus ke materi **prioritas tinggi** terlebih dahulu
5. ✅ Pahami **konsep dan analogi**, jangan cuma hafal definisi
6. ✅ Latih **menjelaskan ke orang lain** atau di depan cermin
7. ✅ Untuk Database: coba **tulis SQL langsung** di MySQL/online editor
8. ✅ Untuk Programming: coba **ketik dan jalankan** contoh kode
9. ✅ Untuk Networking: hafal **OSI Layer** dan **cara hitung subnetting**
10. ✅ Tidur yang cukup malam sebelumnya! 😴
