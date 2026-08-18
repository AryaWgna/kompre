# 📘 Panduan Belajar Ujian Komprehensif — Teknik Informatika

Panduan lengkap untuk mempersiapkan ujian komprehensif yang mencakup **5 mata kuliah**. Setiap bagian berisi ringkasan teori, penjelasan konsep, dan contoh pertanyaan beserta jawabannya.

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

### Primary Key & Foreign Key

| Jenis Key | Definisi | Aturan |
|-----------|----------|--------|
| **Primary Key (PK)** | Kolom yang secara unik mengidentifikasi setiap baris | Harus **unik** dan **tidak boleh NULL** |
| **Foreign Key (FK)** | Kolom yang mereferensikan Primary Key di tabel lain | Menjaga **integritas referensial** |
| **Candidate Key** | Kolom yang berpotensi menjadi Primary Key | Bisa ada lebih dari satu |
| **Composite Key** | Primary Key yang terdiri dari **lebih dari satu kolom** | Contoh: (nim, kode_mk) |

```sql
CREATE TABLE mahasiswa (
    nim VARCHAR(10) PRIMARY KEY,
    nama VARCHAR(100),
    prodi VARCHAR(50)
);

CREATE TABLE nilai (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nim VARCHAR(10),
    mata_kuliah VARCHAR(50),
    nilai CHAR(1),
    FOREIGN KEY (nim) REFERENCES mahasiswa(nim)
);
```

### ERD (Entity Relationship Diagram)

ERD adalah diagram yang menggambarkan **hubungan antar entitas** dalam database.

**Komponen ERD:**

| Komponen | Simbol | Penjelasan |
|----------|--------|------------|
| **Entitas** | Persegi panjang | Objek yang datanya disimpan (Mahasiswa, Dosen) |
| **Atribut** | Elips | Properti dari entitas (nim, nama, alamat) |
| **Relasi** | Belah ketupat | Hubungan antar entitas (mengambil, mengajar) |
| **Kardinalitas** | Garis + notasi | 1:1, 1:N, M:N |

**Jenis Kardinalitas:**

| Kardinalitas | Contoh |
|-------------|--------|
| **One-to-One (1:1)** | 1 mahasiswa punya 1 KTM |
| **One-to-Many (1:N)** | 1 dosen membimbing banyak mahasiswa |
| **Many-to-Many (M:N)** | Banyak mahasiswa mengambil banyak mata kuliah |

### Normalisasi

Normalisasi adalah proses **mengorganisasi data** dalam database untuk mengurangi **redundansi** dan meningkatkan **integritas data**.

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

---

#### ✅ Tahap 1: Normalisasi ke 1NF

> **Aturan 1NF:** Setiap kolom hanya berisi **satu nilai (atomik)**. Tidak boleh ada multi-value.

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

---

### SQL (Structured Query Language)

**DDL (Data Definition Language):**

```sql
-- Membuat tabel
CREATE TABLE mahasiswa (
    nim VARCHAR(10) PRIMARY KEY,
    nama VARCHAR(100) NOT NULL,
    prodi VARCHAR(50) DEFAULT 'TI'
);

-- Mengubah struktur tabel
ALTER TABLE mahasiswa ADD COLUMN email VARCHAR(100);

-- Menghapus tabel
DROP TABLE mahasiswa;
```

**DML (Data Manipulation Language):**

```sql
-- INSERT: Menambah data
INSERT INTO mahasiswa (nim, nama, prodi) VALUES ('001', 'Andi', 'TI');

-- SELECT: Membaca data
SELECT * FROM mahasiswa;
SELECT nama, prodi FROM mahasiswa WHERE prodi = 'TI';

-- UPDATE: Mengubah data
UPDATE mahasiswa SET nama = 'Budi' WHERE nim = '001';

-- DELETE: Menghapus data
DELETE FROM mahasiswa WHERE nim = '001';
```

**Klausa penting dalam SELECT:**

| Klausa | Fungsi | Contoh |
|--------|--------|--------|
| `WHERE` | Filter baris | `WHERE prodi = 'TI'` |
| `ORDER BY` | Mengurutkan hasil | `ORDER BY nama ASC` |
| `GROUP BY` | Mengelompokkan data | `GROUP BY prodi` |
| `HAVING` | Filter setelah GROUP BY | `HAVING COUNT(*) > 5` |
| `LIMIT` | Membatasi jumlah hasil | `LIMIT 10` |
| `DISTINCT` | Menghilangkan duplikat | `SELECT DISTINCT prodi` |

**Aggregate Function:**

| Fungsi | Kegunaan |
|--------|----------|
| `COUNT()` | Menghitung jumlah baris |
| `SUM()` | Menjumlahkan nilai |
| `AVG()` | Menghitung rata-rata |
| `MAX()` | Nilai tertinggi |
| `MIN()` | Nilai terendah |

### JOIN

JOIN digunakan untuk **menggabungkan data dari dua atau lebih tabel** berdasarkan kolom yang berhubungan.

| Tipe JOIN | Penjelasan |
|-----------|------------|
| **INNER JOIN** | Hanya data yang **cocok di kedua tabel** |
| **LEFT JOIN** | **Semua data tabel kiri** + data cocok dari kanan (NULL jika tidak cocok) |
| **RIGHT JOIN** | **Semua data tabel kanan** + data cocok dari kiri (NULL jika tidak cocok) |
| **FULL OUTER JOIN** | **Semua data dari kedua tabel** (NULL di sisi yang tidak cocok) |
| **CROSS JOIN** | Semua kombinasi baris dari kedua tabel (kartesian) |

```sql
-- INNER JOIN
SELECT m.nama, n.mata_kuliah, n.nilai
FROM mahasiswa m
INNER JOIN nilai n ON m.nim = n.nim;

-- LEFT JOIN (semua mahasiswa ditampilkan, meskipun belum ada nilai)
SELECT m.nama, n.mata_kuliah, n.nilai
FROM mahasiswa m
LEFT JOIN nilai n ON m.nim = n.nim;
```

### Index

Index adalah **struktur data** (biasanya B-Tree) yang mempercepat pencarian data pada tabel.

| Aspek | Tanpa Index | Dengan Index |
|-------|-------------|-------------|
| **SELECT/WHERE** | Full table scan — lambat | Langsung ke lokasi — cepat |
| **INSERT/UPDATE/DELETE** | Normal | Lebih lambat (index harus diupdate) |
| **Storage** | Normal | Membutuhkan ruang tambahan |

```sql
-- Membuat index
CREATE INDEX idx_nama ON mahasiswa(nama);

-- Melihat index
SHOW INDEX FROM mahasiswa;

-- Menghapus index
DROP INDEX idx_nama ON mahasiswa;
```

> **Kapan pakai index?** Pada kolom yang sering dipakai di `WHERE`, `JOIN`, atau `ORDER BY`.

### Transaction & ACID

Transaction adalah serangkaian operasi database yang dieksekusi sebagai **satu kesatuan yang utuh**.

| Properti ACID | Penjelasan |
|---------------|------------|
| **Atomicity** | Semua operasi berhasil, atau semua dibatalkan |
| **Consistency** | Database tetap valid sebelum dan sesudah transaksi |
| **Isolation** | Setiap transaksi berjalan independen |
| **Durability** | Data yang sudah di-commit tersimpan permanen |

```sql
START TRANSACTION;

UPDATE rekening SET saldo = saldo - 500000 WHERE id = 1;
UPDATE rekening SET saldo = saldo + 500000 WHERE id = 2;

-- Jika semua berhasil:
COMMIT;

-- Jika ada error:
ROLLBACK;
```

---

## Contoh Pertanyaan & Jawaban — Database

<details>
<summary><b>Q: Apa perbedaan DDL, DML, dan DCL?</b></summary>

| Kategori | Singkatan | Fungsi | Contoh |
|----------|-----------|--------|--------|
| **DDL** | Data Definition Language | Mendefinisikan struktur database | `CREATE`, `ALTER`, `DROP` |
| **DML** | Data Manipulation Language | Memanipulasi data | `SELECT`, `INSERT`, `UPDATE`, `DELETE` |
| **DCL** | Data Control Language | Mengatur hak akses | `GRANT`, `REVOKE` |
| **TCL** | Transaction Control Language | Mengatur transaksi | `COMMIT`, `ROLLBACK`, `SAVEPOINT` |
</details>

<details>
<summary><b>Q: Jelaskan perbedaan WHERE dan HAVING!</b></summary>

| Aspek | WHERE | HAVING |
|-------|-------|--------|
| **Waktu filter** | **Sebelum** GROUP BY | **Sesudah** GROUP BY |
| **Digunakan dengan** | Kolom biasa | Aggregate function |
| **Contoh** | `WHERE prodi = 'TI'` | `HAVING COUNT(*) > 5` |

```sql
-- WHERE: filter sebelum grouping
SELECT prodi, COUNT(*) as jumlah
FROM mahasiswa
WHERE angkatan = 2022
GROUP BY prodi;

-- HAVING: filter setelah grouping
SELECT prodi, COUNT(*) as jumlah
FROM mahasiswa
GROUP BY prodi
HAVING COUNT(*) > 10;
```
</details>

<details>
<summary><b>Q: Apa perbedaan DELETE, TRUNCATE, dan DROP?</b></summary>

| Perintah | Fungsi | Bisa WHERE? | Bisa ROLLBACK? | Kecepatan |
|----------|--------|-------------|----------------|-----------|
| **DELETE** | Menghapus baris data | Ya | Ya | Lambat |
| **TRUNCATE** | Menghapus semua data, struktur tetap | Tidak | Tidak | Cepat |
| **DROP** | Menghapus tabel beserta strukturnya | Tidak | Tidak | Cepat |
</details>

<details>
<summary><b>Q: Apa itu View dalam database?</b></summary>

**View** adalah tabel virtual yang dibuat dari query `SELECT`. View tidak menyimpan data, hanya menyimpan query.

```sql
CREATE VIEW v_mahasiswa_ti AS
SELECT nim, nama FROM mahasiswa WHERE prodi = 'TI';

-- Menggunakan view
SELECT * FROM v_mahasiswa_ti;
```

**Manfaat:** Menyederhanakan query kompleks, membatasi akses data, dan konsistensi.
</details>

<details>
<summary><b>Q: Apa perbedaan Primary Key dan Unique Key?</b></summary>

| Aspek | Primary Key | Unique Key |
|-------|-------------|------------|
| **Jumlah per tabel** | Hanya **1** | Bisa **banyak** |
| **NULL** | **Tidak boleh** NULL | **Boleh** NULL (sekali) |
| **Fungsi** | Identitas utama baris | Menjamin keunikan kolom |
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
1. **Finite** — Memiliki akhir (berhenti)
2. **Definite** — Setiap langkah jelas dan tidak ambigu
3. **Input** — Memiliki nol atau lebih input
4. **Output** — Menghasilkan minimal satu output
5. **Effective** — Setiap langkah bisa dikerjakan

**Cara merepresentasikan algoritma:**

| Metode | Penjelasan |
|--------|------------|
| **Pseudocode** | Ditulis menyerupai bahasa pemrograman tapi dalam bahasa manusia |
| **Flowchart** | Menggunakan simbol-simbol diagram alir |
| **Kode program** | Langsung ditulis dalam bahasa pemrograman |

### Variabel & Tipe Data

**Variabel** adalah tempat penyimpanan data di memori yang memiliki nama dan tipe.

| Tipe Data | Contoh Nilai | Ukuran |
|-----------|-------------|--------|
| **int** | 1, 42, -7 | 4 byte |
| **float/double** | 3.14, -0.5 | 4/8 byte |
| **char** | 'A', 'z' | 1-2 byte |
| **String** | "Hello World" | Variabel |
| **boolean** | true, false | 1 bit |

```java
int umur = 21;
double ipk = 3.75;
String nama = "Andi";
boolean lulus = true;
```

### Percabangan

**if-else:**
```java
if (nilai >= 80) {
    System.out.println("A");
} else if (nilai >= 60) {
    System.out.println("B");
} else {
    System.out.println("C");
}
```

**switch-case:**
```java
switch (hari) {
    case 1: System.out.println("Senin"); break;
    case 2: System.out.println("Selasa"); break;
    case 3: System.out.println("Rabu"); break;
    default: System.out.println("Tidak valid");
}
```

### Perulangan

| Jenis | Kapan Dipakai | Pengecekan Kondisi |
|-------|---------------|-------------------|
| **for** | Jumlah perulangan sudah diketahui | Sebelum eksekusi |
| **while** | Jumlah perulangan belum diketahui | Sebelum eksekusi |
| **do-while** | Minimal harus dijalankan 1 kali | Sesudah eksekusi |

```java
// for
for (int i = 0; i < 5; i++) {
    System.out.println(i);
}

// while
int i = 0;
while (i < 5) {
    System.out.println(i);
    i++;
}

// do-while (minimal jalan 1 kali)
int j = 0;
do {
    System.out.println(j);
    j++;
} while (j < 5);
```

### Function / Method

**Function** adalah blok kode yang bisa dipanggil berulang kali untuk melakukan tugas tertentu.

```java
// Function dengan return value
public static int tambah(int a, int b) {
    return a + b;
}

// Function tanpa return value (void)
public static void spiHello(String nama) {
    System.out.println("Hello, " + nama);
}

// Pemanggilan
int hasil = tambah(3, 5);  // hasil = 8
spiHello("Andi");           // Output: Hello, Andi
```

**Istilah penting:**

| Istilah | Penjelasan |
|---------|------------|
| **Parameter** | Variabel yang diterima oleh function (di definisi) |
| **Argument** | Nilai yang dikirim saat memanggil function |
| **Return value** | Nilai yang dikembalikan function |
| **void** | Function yang tidak mengembalikan nilai |

### OOP (Object-Oriented Programming)

**4 Pilar OOP:**

| Pilar | Penjelasan | Analogi |
|-------|------------|---------|
| **Encapsulation** | Menyembunyikan data internal, akses melalui method | Kapsul obat: isi tersembunyi, akses lewat menelan |
| **Inheritance** | Kelas anak mewarisi sifat kelas induk | Anak mewarisi sifat orang tua |
| **Polymorphism** | Satu method, perilaku berbeda tergantung konteks | Tombol "play": di musik → putar lagu, di video → putar film |
| **Abstraction** | Menyembunyikan detail kompleks, hanya tampilkan yang penting | Mobil: kita cuma tahu setir & gas, mesin tersembunyi |

```java
// === ENCAPSULATION ===
class Mahasiswa {
    private String nama;       // disembunyikan (private)
    private double ipk;

    // Akses melalui getter & setter
    public String getNama() { return nama; }
    public void setNama(String nama) { this.nama = nama; }

    public double getIpk() { return ipk; }
    public void setIpk(double ipk) {
        if (ipk >= 0 && ipk <= 4.0) {
            this.ipk = ipk;
        }
    }
}
```

```java
// === INHERITANCE ===
class Hewan {
    String nama;
    void makan() {
        System.out.println(nama + " sedang makan");
    }
}

class Kucing extends Hewan {   // Kucing mewarisi Hewan
    void mendengkur() {
        System.out.println(nama + " mendengkur...");
    }
}
```

```java
// === POLYMORPHISM ===

// Overriding (runtime polymorphism)
class Hewan {
    void suara() { System.out.println("..."); }
}
class Kucing extends Hewan {
    @Override
    void suara() { System.out.println("Meow!"); }
}
class Anjing extends Hewan {
    @Override
    void suara() { System.out.println("Guk!"); }
}

// Overloading (compile-time polymorphism)
class Kalkulator {
    int tambah(int a, int b) { return a + b; }
    double tambah(double a, double b) { return a + b; }
}
```

```java
// === ABSTRACTION ===
abstract class Bentuk {
    abstract double hitungLuas();   // method abstrak (tanpa body)

    void info() {                  // method biasa (punya body)
        System.out.println("Ini adalah bentuk geometri");
    }
}

class Lingkaran extends Bentuk {
    double radius;
    Lingkaran(double r) { this.radius = r; }

    @Override
    double hitungLuas() {
        return Math.PI * radius * radius;
    }
}
```

**Perbedaan Abstract Class dan Interface:**

| Aspek | Abstract Class | Interface |
|-------|---------------|-----------|
| **Method** | Bisa punya method biasa & abstract | Semua method abstract (Java 8+: bisa default) |
| **Variabel** | Bisa punya variabel instance | Hanya konstanta (`public static final`) |
| **Inheritance** | Single inheritance | Multiple implementation |
| **Constructor** | Punya constructor | Tidak punya constructor |
| **Keyword** | `extends` | `implements` |

### Error Handling

Error Handling adalah mekanisme menangani kesalahan saat program berjalan agar **tidak crash**.

| Blok | Fungsi |
|------|--------|
| **try** | Blok kode yang mungkin menghasilkan error |
| **catch** | Menangkap dan menangani error spesifik |
| **finally** | Selalu dijalankan, berhasil maupun gagal |
| **throw** | Melempar exception secara manual |
| **throws** | Mendeklarasikan bahwa method bisa melempar exception |

```java
try {
    int[] arr = {1, 2, 3};
    System.out.println(arr[5]);       // ArrayIndexOutOfBoundsException
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("Error: Index di luar batas!");
} catch (Exception e) {
    System.out.println("Error umum: " + e.getMessage());
} finally {
    System.out.println("Selesai.");
}
```

**Jenis Exception:**

| Jenis | Penjelasan | Contoh |
|-------|------------|--------|
| **Checked Exception** | Harus ditangani (compile-time) | `IOException`, `SQLException` |
| **Unchecked Exception** | Tidak wajib ditangani (runtime) | `NullPointerException`, `ArithmeticException` |

### Struktur Data Dasar

| Struktur Data | Prinsip | Operasi | Contoh Penggunaan |
|---------------|---------|---------|-------------------|
| **Array** | Kumpulan data dengan ukuran tetap, akses via index | Akses O(1), insert/delete O(n) | Daftar nilai mahasiswa |
| **Linked List** | Node yang terhubung via pointer | Insert/delete di head O(1), akses O(n) | Playlist musik |
| **Stack** | **LIFO** (Last In, First Out) | `push()`, `pop()`, `peek()` | Undo/redo, call stack |
| **Queue** | **FIFO** (First In, First Out) | `enqueue()`, `dequeue()` | Antrian printer, BFS |
| **Tree** | Struktur hierarki (parent-child) | Insert, search, delete | File system, DOM |
| **Hash Table** | Key-value pair dengan hash function | Insert/search/delete rata-rata O(1) | Dictionary, cache |

### Konsep API

**API (Application Programming Interface)** adalah sekumpulan aturan yang memungkinkan satu software berkomunikasi dengan software lain.

```
Client  →  HTTP Request  →  API Server  →  Database
Client  ←  HTTP Response ←  API Server  ←  Database
```

---

## Contoh Pertanyaan & Jawaban — Programming

<details>
<summary><b>Q: Apa perbedaan Overloading dan Overriding?</b></summary>

| Aspek | Overloading | Overriding |
|-------|-------------|------------|
| **Definisi** | Method **sama namanya** tapi **beda parameter** | Method di kelas anak **menimpa** method kelas induk |
| **Terjadi di** | **Satu class** yang sama | **Dua class** (parent-child) |
| **Polimorfisme** | Compile-time | Runtime |
| **Return type** | Boleh berbeda | Harus sama |
</details>

<details>
<summary><b>Q: Apa perbedaan Array dan ArrayList?</b></summary>

| Aspek | Array | ArrayList |
|-------|-------|-----------|
| **Ukuran** | **Tetap** (fixed) | **Dinamis** (bisa bertambah/berkurang) |
| **Tipe data** | Primitif & objek | Hanya objek (wrapper class) |
| **Performa** | Lebih cepat | Sedikit lebih lambat |
| **Method** | Tidak punya built-in method | `add()`, `remove()`, `size()`, dll. |
</details>

<details>
<summary><b>Q: Apa itu Recursion? Berikan contoh!</b></summary>

**Recursion** adalah teknik di mana sebuah function **memanggil dirinya sendiri** sampai mencapai kondisi berhenti (base case).

```java
// Factorial: 5! = 5 x 4 x 3 x 2 x 1 = 120
int factorial(int n) {
    if (n <= 1) return 1;           // base case
    return n * factorial(n - 1);    // recursive case
}
```

**Harus punya:**
1. **Base case** — kondisi berhenti agar tidak infinite loop
2. **Recursive case** — pemanggilan diri sendiri dengan perubahan parameter
</details>

<details>
<summary><b>Q: Jelaskan konsep Constructor!</b></summary>

**Constructor** adalah method khusus yang **dipanggil otomatis saat objek dibuat**. Fungsinya untuk menginisialisasi nilai awal objek.

```java
class Mahasiswa {
    String nama;
    int umur;

    // Constructor
    Mahasiswa(String nama, int umur) {
        this.nama = nama;
        this.umur = umur;
    }
}

// Penggunaan
Mahasiswa mhs = new Mahasiswa("Andi", 21);
```

**Aturan:**
- Nama sama dengan nama class
- Tidak punya return type
- Dipanggil otomatis saat `new`
</details>

---

# 3️⃣ Information Technology

## Daftar Materi

- Hardware & Software
- Sistem Operasi
- Jaringan Dasar
- Internet
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
| **Definisi** | Komponen fisik komputer yang bisa disentuh | Program/instruksi yang berjalan di atas hardware |
| **Contoh** | CPU, RAM, HDD/SSD, Monitor, Keyboard | Windows, Linux, Chrome, MySQL |
| **Kategori** | Input, Output, Processing, Storage | System Software, Application Software |

**Komponen utama hardware:**

| Komponen | Fungsi |
|----------|--------|
| **CPU (Processor)** | Otak komputer — memproses instruksi |
| **RAM** | Memori sementara (volatile) — menyimpan data yang sedang diproses |
| **HDD/SSD** | Penyimpanan permanen (non-volatile) |
| **Motherboard** | Papan utama yang menghubungkan semua komponen |
| **GPU** | Memproses grafis dan visual |
| **PSU** | Menyuplai daya listrik |

### Sistem Operasi

**Sistem Operasi (OS)** adalah software yang mengelola hardware dan menyediakan layanan untuk aplikasi.

**Fungsi utama OS:**
1. **Manajemen Proses** — Menjalankan dan mengatur proses/program
2. **Manajemen Memori** — Mengalokasikan RAM untuk proses
3. **Manajemen File** — Mengatur penyimpanan file
4. **Manajemen I/O** — Mengatur perangkat input/output
5. **Keamanan** — Mengontrol akses pengguna

| Contoh OS | Tipe |
|-----------|------|
| Windows | Desktop/Laptop |
| macOS | Desktop/Laptop (Apple) |
| Linux (Ubuntu, CentOS) | Server & Desktop |
| Android | Mobile |
| iOS | Mobile (Apple) |

### Jaringan Dasar & Internet

**Jaringan Komputer** adalah dua atau lebih komputer yang saling terhubung untuk berbagi data dan sumber daya.

| Jenis Jaringan | Jangkauan |
|----------------|-----------|
| **PAN** (Personal Area Network) | Pribadi (< 10m) — Bluetooth |
| **LAN** (Local Area Network) | Lokal (satu gedung/kampus) |
| **MAN** (Metropolitan Area Network) | Satu kota |
| **WAN** (Wide Area Network) | Antar kota/negara — Internet |

**Internet** adalah jaringan global (WAN terbesar) yang menghubungkan miliaran perangkat menggunakan protokol TCP/IP.

### IP Address

**IP Address** adalah alamat unik yang diberikan kepada setiap perangkat dalam jaringan.

| Versi | Format | Contoh |
|-------|--------|--------|
| **IPv4** | 32-bit, 4 oktet desimal | `192.168.1.1` |
| **IPv6** | 128-bit, 8 grup heksadesimal | `2001:0db8:85a3::8a2e:0370:7334` |

| Kelas | Range | Default Subnet Mask | Penggunaan |
|-------|-------|---------------------|------------|
| **A** | 1.0.0.0 – 126.255.255.255 | 255.0.0.0 | Jaringan besar |
| **B** | 128.0.0.0 – 191.255.255.255 | 255.255.0.0 | Jaringan menengah |
| **C** | 192.0.0.0 – 223.255.255.255 | 255.255.255.0 | Jaringan kecil |

**IP Private vs Public:**

| Jenis | Penggunaan | Contoh |
|-------|------------|--------|
| **Private** | Jaringan lokal (LAN) | 192.168.x.x, 10.x.x.x |
| **Public** | Bisa diakses dari internet | IP dari ISP |

### Client-Server

**Model Client-Server** adalah arsitektur di mana **client** meminta layanan dan **server** menyediakan layanan.

```
┌──────────┐     Request     ┌──────────┐
│  Client  │ ──────────────→ │  Server  │
│ (Browser)│ ←────────────── │  (Web)   │
└──────────┘     Response    └──────────┘
```

| Komponen | Peran | Contoh |
|----------|-------|--------|
| **Client** | Meminta layanan | Browser, aplikasi mobile |
| **Server** | Menyediakan layanan | Web server, database server |

### Cloud Computing

**Cloud Computing** adalah penyediaan layanan komputasi melalui **internet** secara on-demand.

**3 Model Layanan:**

| Model | Pengelolaan User | Pengelolaan Provider | Contoh |
|-------|-----------------|---------------------|--------|
| **IaaS** | OS, Middleware, App, Data | Server, Storage, Network | AWS EC2, GCP Compute |
| **PaaS** | App, Data | OS, Middleware, Server | Heroku, Google App Engine |
| **SaaS** | Hanya pakai | Semua | Gmail, Google Docs, Zoom |

**4 Model Deployment:**

| Model | Penjelasan |
|-------|------------|
| **Public Cloud** | Dikelola pihak ketiga, dipakai banyak organisasi |
| **Private Cloud** | Dikelola sendiri, untuk satu organisasi |
| **Hybrid Cloud** | Kombinasi public + private |
| **Community Cloud** | Dipakai bersama oleh komunitas/industri tertentu |

### Virtualisasi

**Virtualisasi** adalah teknologi yang membuat **versi virtual** dari hardware, OS, storage, atau jaringan sehingga **satu hardware fisik bisa menjalankan banyak sistem virtual**.

| Komponen | Penjelasan |
|----------|------------|
| **Hypervisor** | Software yang mengelola virtual machine |
| **Virtual Machine (VM)** | Komputer virtual yang berjalan di atas hypervisor |
| **Host** | Mesin fisik yang menjalankan hypervisor |
| **Guest** | OS yang berjalan di dalam VM |

**Tipe Hypervisor:**

| Tipe | Penjelasan | Contoh |
|------|------------|--------|
| **Type 1 (Bare Metal)** | Langsung di atas hardware | VMware ESXi, Hyper-V |
| **Type 2 (Hosted)** | Di atas OS | VirtualBox, VMware Workstation |

### Keamanan Informasi — CIA Triad

| Prinsip | Penjelasan | Implementasi |
|---------|------------|-------------|
| **Confidentiality** | Data hanya diakses pihak berwenang | Enkripsi, password, access control |
| **Integrity** | Data tidak diubah tanpa izin | Hashing, checksum, digital signature |
| **Availability** | Sistem selalu tersedia saat dibutuhkan | Backup, redundancy, UPS |

**Ancaman keamanan umum:**

| Ancaman | Penjelasan |
|---------|------------|
| **Malware** | Software berbahaya (virus, worm, trojan, ransomware) |
| **Phishing** | Menipu user untuk memberikan data sensitif |
| **DDoS** | Membanjiri server dengan traffic agar down |
| **SQL Injection** | Menyisipkan SQL berbahaya melalui input user |
| **Man-in-the-Middle** | Menyadap komunikasi antara dua pihak |

### Peran TI dalam Organisasi

| Peran | Contoh |
|-------|--------|
| **Otomasi proses** | ERP, sistem absensi digital |
| **Pengambilan keputusan** | Business Intelligence, Dashboard |
| **Komunikasi** | Email, video conference |
| **Penyimpanan data** | Cloud storage, database |
| **Keunggulan kompetitif** | E-commerce, mobile app |

---

## Contoh Pertanyaan & Jawaban — Information Technology

<details>
<summary><b>Q: Apa perbedaan RAM dan ROM?</b></summary>

| Aspek | RAM | ROM |
|-------|-----|-----|
| **Sifat** | Volatile (hilang saat mati) | Non-volatile (tetap tersimpan) |
| **Fungsi** | Menyimpan data yang sedang diproses | Menyimpan firmware/BIOS |
| **Kecepatan** | Cepat | Lebih lambat |
| **Bisa ditulis?** | Bisa baca & tulis | Hanya baca (read-only) |
</details>

<details>
<summary><b>Q: Apa perbedaan 32-bit dan 64-bit?</b></summary>

| Aspek | 32-bit | 64-bit |
|-------|--------|--------|
| **Max RAM** | 4 GB | 16 Exabyte (praktis tak terbatas) |
| **Register** | 32-bit | 64-bit |
| **Performa** | Lebih lambat untuk data besar | Lebih cepat |
| **Kompatibilitas** | Hanya jalankan app 32-bit | Bisa jalankan app 32-bit & 64-bit |
</details>

<details>
<summary><b>Q: Apa perbedaan Enkripsi Simetris dan Asimetris?</b></summary>

| Aspek | Simetris | Asimetris |
|-------|----------|-----------|
| **Kunci** | Satu kunci (sama untuk enkripsi & dekripsi) | Dua kunci (public & private) |
| **Kecepatan** | Lebih cepat | Lebih lambat |
| **Keamanan** | Kunci harus dibagi dengan aman | Public key bisa disebar bebas |
| **Contoh** | AES, DES | RSA, ECC |
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
| **HTML** | Struktur/kerangka halaman web | Tulang/kerangka tubuh |
| **CSS** | Tampilan/styling halaman web | Kulit, rambut, pakaian |
| **JavaScript** | Interaktivitas dan logika | Otot dan otak (gerakan & kecerdasan) |

```html
<!-- HTML: Struktur -->
<!DOCTYPE html>
<html>
<head>
    <title>Contoh Web</title>
    <style>
        /* CSS: Styling */
        body { font-family: Arial; background: #f0f0f0; }
        h1 { color: #333; }
    </style>
</head>
<body>
    <h1 id="judul">Hello World</h1>
    <button onclick="ubahTeks()">Klik Saya</button>

    <script>
        // JavaScript: Interaktivitas
        function ubahTeks() {
            document.getElementById("judul").innerText = "Teks berubah!";
        }
    </script>
</body>
</html>
```

### Frontend vs Backend

| Aspek | Frontend | Backend |
|-------|----------|---------|
| **Definisi** | Bagian yang dilihat dan diinteraksikan user | Bagian yang memproses logika & data di server |
| **Berjalan di** | **Browser** (client-side) | **Server** (server-side) |
| **Teknologi** | HTML, CSS, JavaScript, React, Vue | PHP, Node.js, Python, Java, Go |
| **Tugas** | Tampilan UI, form, animasi | API, database, autentikasi, business logic |

**Full-stack** = menguasai frontend + backend.

### HTTP / HTTPS

**HTTP (HyperText Transfer Protocol)** adalah protokol komunikasi antara client (browser) dan server.

**HTTPS** = HTTP + **SSL/TLS** (enkripsi) → data terenkripsi saat dikirim.

| Aspek | HTTP | HTTPS |
|-------|------|-------|
| **Port** | 80 | 443 |
| **Keamanan** | Tidak terenkripsi | Terenkripsi (SSL/TLS) |
| **URL** | `http://` | `https://` |

**HTTP Status Code:**

| Kode | Kategori | Contoh |
|------|----------|--------|
| **1xx** | Informational | 100 Continue |
| **2xx** | Sukses | **200 OK**, 201 Created |
| **3xx** | Redirect | 301 Moved Permanently, **304 Not Modified** |
| **4xx** | Client Error | **400 Bad Request**, **401 Unauthorized**, **403 Forbidden**, **404 Not Found** |
| **5xx** | Server Error | **500 Internal Server Error**, 503 Service Unavailable |

### REST API

**REST (Representational State Transfer)** adalah arsitektur untuk membangun API yang berkomunikasi melalui HTTP.

**Prinsip REST:**
1. **Stateless** — Server tidak menyimpan state client
2. **Resource-based** — Setiap data diakses via URI
3. **Uniform Interface** — Menggunakan HTTP method standar
4. **Client-Server** — Pemisahan client dan server

### HTTP Method

| Method | Fungsi | CRUD | Contoh |
|--------|--------|------|--------|
| **GET** | Mengambil data | Read | `GET /api/users` |
| **POST** | Membuat data baru | Create | `POST /api/users` |
| **PUT** | Mengubah seluruh data | Update | `PUT /api/users/1` |
| **PATCH** | Mengubah sebagian data | Update (partial) | `PATCH /api/users/1` |
| **DELETE** | Menghapus data | Delete | `DELETE /api/users/1` |

```
GET /api/mahasiswa         → Ambil semua data mahasiswa
GET /api/mahasiswa/1       → Ambil mahasiswa dengan id=1
POST /api/mahasiswa        → Tambah mahasiswa baru (data di body)
PUT /api/mahasiswa/1       → Update semua field mahasiswa id=1
DELETE /api/mahasiswa/1    → Hapus mahasiswa id=1
```

### JSON

**JSON (JavaScript Object Notation)** adalah format pertukaran data yang ringan dan mudah dibaca.

```json
{
    "nim": "12522010",
    "nama": "Arya",
    "prodi": "Teknik Informatika",
    "ipk": 3.75,
    "aktif": true,
    "mata_kuliah": ["Database", "Programming", "Web"],
    "alamat": {
        "kota": "Jakarta",
        "provinsi": "DKI Jakarta"
    }
}
```

| Tipe Data JSON | Contoh |
|----------------|--------|
| String | `"nama": "Arya"` |
| Number | `"ipk": 3.75` |
| Boolean | `"aktif": true` |
| Array | `"mk": ["DB", "Web"]` |
| Object | `"alamat": { ... }` |
| Null | `"foto": null` |

### Authentication & Authorization

| Aspek | Authentication (AuthN) | Authorization (AuthZ) |
|-------|----------------------|---------------------|
| **Pertanyaan** | "**Siapa** kamu?" | "Apa yang **boleh** kamu lakukan?" |
| **Proses** | Verifikasi **identitas** | Verifikasi **hak akses** |
| **Contoh** | Login username + password | Admin bisa hapus, user biasa tidak |
| **Urutan** | Dilakukan **pertama** | Dilakukan **setelah** autentikasi |

### Session vs Token (JWT)

| Aspek | Session | Token (JWT) |
|-------|---------|-------------|
| **Disimpan di** | **Server** (session store) | **Client** (localStorage / cookie) |
| **Stateful/Stateless** | **Stateful** — server harus ingat | **Stateless** — self-contained |
| **Skalabilitas** | Kurang scalable (server harus sync) | Lebih scalable |
| **Cara kerja** | Server simpan session ID → kirim via cookie | Server buat token → client simpan & kirim di header |

**Struktur JWT:**
```
header.payload.signature

Header:    { "alg": "HS256", "typ": "JWT" }
Payload:   { "user_id": 1, "role": "admin", "exp": 1234567890 }
Signature: HMACSHA256(base64(header) + "." + base64(payload), secret)
```

### Web Server

**Web Server** adalah software yang menerima HTTP request dan mengirim HTTP response (file HTML, gambar, API response, dll).

| Web Server | Keterangan |
|------------|------------|
| **Apache** | Open source, paling banyak dipakai, modular |
| **Nginx** | Ringan, cepat, bagus untuk reverse proxy & load balancing |
| **IIS** | Microsoft, terintegrasi dengan Windows Server |

### Database pada Aplikasi Web

| Tipe | Penjelasan | Contoh |
|------|------------|--------|
| **SQL (Relasional)** | Data dalam tabel berhubungan | MySQL, PostgreSQL |
| **NoSQL** | Data fleksibel (document, key-value, dll) | MongoDB, Redis, Firebase |

---

## Contoh Pertanyaan & Jawaban — Web Systems

<details>
<summary><b>Q: Apa perbedaan GET dan POST?</b></summary>

| Aspek | GET | POST |
|-------|-----|------|
| **Fungsi** | Mengambil data | Mengirim/membuat data |
| **Data dikirim via** | URL (query string) | Body request |
| **Keamanan** | Kurang aman (terlihat di URL) | Lebih aman (di body) |
| **Idempotent** | Ya (hasil sama walau dipanggil berkali-kali) | Tidak |
| **Caching** | Bisa di-cache | Tidak di-cache |
| **Panjang data** | Terbatas (~2048 karakter) | Tidak terbatas |
</details>

<details>
<summary><b>Q: Apa itu CORS?</b></summary>

**CORS (Cross-Origin Resource Sharing)** adalah mekanisme keamanan browser yang mengontrol akses resource dari **domain yang berbeda**.

Secara default, browser memblokir request ke domain lain (Same-Origin Policy). CORS mengizinkan server menentukan domain mana yang boleh mengakses resource-nya melalui header:

```
Access-Control-Allow-Origin: https://example.com
Access-Control-Allow-Methods: GET, POST, PUT
Access-Control-Allow-Headers: Content-Type, Authorization
```
</details>

<details>
<summary><b>Q: Apa perbedaan Cookie, Local Storage, dan Session Storage?</b></summary>

| Aspek | Cookie | Local Storage | Session Storage |
|-------|--------|---------------|-----------------|
| **Kapasitas** | ~4 KB | ~5 MB | ~5 MB |
| **Expired** | Bisa diatur | Tidak expired | Hilang saat tab ditutup |
| **Dikirim ke server** | Ya (setiap request) | Tidak | Tidak |
| **Akses** | Client + Server | Client only | Client only |
</details>

<details>
<summary><b>Q: Jelaskan arsitektur MVC!</b></summary>

**MVC (Model-View-Controller)** adalah pola arsitektur yang memisahkan aplikasi menjadi 3 komponen:

| Komponen | Tugas | Contoh |
|----------|-------|--------|
| **Model** | Mengelola data dan business logic | Class `Mahasiswa`, query database |
| **View** | Menampilkan data ke user (UI) | File HTML, template |
| **Controller** | Menerima input user, mengatur alur | Route handler, servlet |

```
User → Controller → Model → Database
                  → View  → User
```
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

| No | Layer | Fungsi | Protokol/Contoh | Perangkat |
|----|-------|--------|-----------------|-----------|
| 7 | **Application** | Interface untuk user | HTTP, FTP, SMTP, DNS | - |
| 6 | **Presentation** | Format, enkripsi, kompresi data | SSL/TLS, JPEG, ASCII | - |
| 5 | **Session** | Membuka/memelihara/menutup sesi | NetBIOS, RPC | - |
| 4 | **Transport** | Pengiriman end-to-end, error recovery | TCP, UDP | - |
| 3 | **Network** | Routing & logical addressing | IP, ICMP, ARP | **Router** |
| 2 | **Data Link** | Frame, MAC addressing, error detection | Ethernet, PPP | **Switch**, Bridge |
| 1 | **Physical** | Transmisi bit mentah (sinyal fisik) | Kabel UTP, Fiber Optik | Hub, Repeater |

> 💡 **Tips hafal dari bawah:** "**P**lease **D**o **N**ot **T**hrow **S**ausage **P**izza **A**way"

### TCP/IP Model

| Layer TCP/IP | Setara OSI | Protokol |
|-------------|------------|----------|
| **Application** | Application + Presentation + Session | HTTP, FTP, DNS, SMTP |
| **Transport** | Transport | TCP, UDP |
| **Internet** | Network | IP, ICMP, ARP |
| **Network Access** | Data Link + Physical | Ethernet, Wi-Fi |

### LAN vs WAN

| Aspek | LAN | WAN |
|-------|-----|-----|
| **Jangkauan** | Lokal (satu gedung/kampus) | Luas (antar kota/negara) |
| **Kecepatan** | Tinggi (100 Mbps – 10 Gbps) | Relatif lebih rendah |
| **Kepemilikan** | Pribadi/organisasi | ISP/pihak ketiga |
| **Biaya** | Rendah | Tinggi |
| **Contoh** | Jaringan kantor, lab komputer | Internet, jaringan antar cabang |

### IP Address & Subnetting

**Subnetting** adalah teknik membagi satu jaringan besar menjadi beberapa sub-jaringan (subnet) yang lebih kecil.

**Cara menghitung subnet:**

```
IP: 192.168.1.0/26

Subnet Mask: /26 → 255.255.255.192
Jumlah host per subnet: 2^(32-26) - 2 = 62 host

Subnet 1: 192.168.1.0   – 192.168.1.63   (Network: .0, Broadcast: .63)
Subnet 2: 192.168.1.64  – 192.168.1.127  (Network: .64, Broadcast: .127)
Subnet 3: 192.168.1.128 – 192.168.1.191  (Network: .128, Broadcast: .191)
Subnet 4: 192.168.1.192 – 192.168.1.255  (Network: .192, Broadcast: .255)
```

**Tabel subnet umum:**

| CIDR | Subnet Mask | Jumlah Subnet (dari /24) | Jumlah Host |
|------|-------------|--------------------------|-------------|
| /24 | 255.255.255.0 | 1 | 254 |
| /25 | 255.255.255.128 | 2 | 126 |
| /26 | 255.255.255.192 | 4 | 62 |
| /27 | 255.255.255.224 | 8 | 30 |
| /28 | 255.255.255.240 | 16 | 14 |
| /29 | 255.255.255.248 | 32 | 6 |
| /30 | 255.255.255.252 | 64 | 2 |

> **Rumus jumlah host:** `2^(32 - prefix) - 2` (dikurangi network address dan broadcast)

### Router & Switch

| Aspek | Router | Switch |
|-------|--------|--------|
| **Layer OSI** | Layer 3 (Network) | Layer 2 (Data Link) |
| **Berdasarkan** | **IP Address** | **MAC Address** |
| **Fungsi** | Menghubungkan **jaringan berbeda** | Menghubungkan **perangkat dalam satu jaringan** |
| **Broadcast domain** | Memisahkan broadcast domain | Satu broadcast domain |
| **Tabel** | Routing table | MAC address table |

**Perbedaan Switch vs Hub:**

| Aspek | Switch | Hub |
|-------|--------|-----|
| **Pengiriman data** | Hanya ke port tujuan (unicast) | Ke semua port (broadcast) |
| **Collision domain** | Setiap port = 1 collision domain | Semua port = 1 collision domain |
| **Kecepatan** | Full-duplex | Half-duplex |
| **Cerdas?** | Ya (MAC table) | Tidak |

### VLAN (Virtual LAN)

**VLAN** membagi satu switch fisik menjadi beberapa **jaringan logis** yang terpisah.

```
Tanpa VLAN:
[PC1]──┐
[PC2]──┤── Switch ── Semua bisa komunikasi
[PC3]──┘

Dengan VLAN:
[PC1]──┐ VLAN 10 (IT)
[PC2]──┤── Switch ── Terpisah!
[PC3]──┘ VLAN 20 (HR)
```

**Manfaat VLAN:**
- **Keamanan** — Memisahkan traffic antar departemen
- **Efisiensi** — Mengurangi broadcast domain
- **Fleksibilitas** — Pengelompokan tanpa perlu switch tambahan
- **Manajemen** — Mudah mengelola jaringan besar

**Tipe port VLAN:**

| Tipe Port | Fungsi |
|-----------|--------|
| **Access Port** | Terhubung ke end device, milik 1 VLAN |
| **Trunk Port** | Menghubungkan antar switch, membawa banyak VLAN |

### DHCP, DNS, NAT

| Layanan | Fungsi | Analogi |
|---------|--------|---------|
| **DHCP** | Memberikan IP address **otomatis** ke perangkat | Resepsionis hotel memberikan nomor kamar |
| **DNS** | Menerjemahkan **nama domain** → **IP address** | Buku telepon (nama → nomor) |
| **NAT** | Mengubah **IP private** → **IP public** | Resepsionis yang mewakili semua karyawan ke dunia luar |

**Cara kerja DHCP (DORA):**

```
1. Discover  → Client broadcast mencari DHCP server
2. Offer     → Server menawarkan IP address
3. Request   → Client meminta IP yang ditawarkan
4. Ack       → Server mengonfirmasi pemberian IP
```

**Cara kerja DNS:**

```
User ketik "google.com"
  → Browser cek cache lokal
  → DNS Resolver (ISP)
  → Root DNS Server
  → TLD DNS Server (.com)
  → Authoritative DNS Server (google.com)
  → Return IP: 142.250.x.x
```

**Tipe NAT:**

| Tipe | Penjelasan |
|------|------------|
| **Static NAT** | 1 IP private → 1 IP public (tetap) |
| **Dynamic NAT** | IP private → IP public dari pool (berubah-ubah) |
| **PAT (Port Address Translation)** | Banyak IP private → 1 IP public (menggunakan port berbeda) |

### TCP vs UDP

| Aspek | TCP | UDP |
|-------|-----|-----|
| **Connection** | Connection-oriented (3-way handshake) | Connectionless |
| **Reliabilitas** | Reliable (ada ACK, retransmission) | Unreliable |
| **Urutan data** | Terjamin urut | Tidak dijamin |
| **Kecepatan** | Lebih lambat | Lebih cepat |
| **Header size** | 20 byte | 8 byte |
| **Contoh** | HTTP, FTP, Email (SMTP), SSH | DNS, Video streaming, VoIP, Gaming |

**TCP 3-Way Handshake:**

```
Client          Server
  │── SYN ──────→ │    1. Client minta koneksi
  │←── SYN-ACK ──│    2. Server setuju & minta konfirmasi
  │── ACK ──────→ │    3. Client konfirmasi → koneksi terbuka
```

### Routing

**Routing** adalah proses menentukan jalur terbaik untuk mengirim data dari sumber ke tujuan.

| Tipe Routing | Penjelasan | Kapan Dipakai |
|-------------|------------|---------------|
| **Static Routing** | Route dikonfigurasi manual oleh admin | Jaringan kecil, sederhana |
| **Dynamic Routing** | Route ditentukan otomatis oleh protokol | Jaringan besar, berubah-ubah |

**Protokol routing dinamis:**

| Protokol | Tipe | Algoritma |
|----------|------|-----------|
| **RIP** | Distance Vector | Hop count (max 15) |
| **OSPF** | Link State | Dijkstra (cost/bandwidth) |
| **BGP** | Path Vector | Antar AS (Autonomous System) |

### Firewall & Network Security

**Firewall** adalah sistem keamanan yang **memfilter traffic** jaringan berdasarkan aturan yang ditentukan.

| Tipe Firewall | Penjelasan |
|---------------|------------|
| **Packet Filter** | Filter berdasarkan IP, port, protokol |
| **Stateful Inspection** | Melacak state koneksi |
| **Application Layer** | Filter berdasarkan aplikasi/konten (Layer 7) |
| **Next-Generation (NGFW)** | Kombinasi semua + IDS/IPS |

**Konsep keamanan jaringan:**

| Konsep | Penjelasan |
|--------|------------|
| **IDS** (Intrusion Detection System) | Mendeteksi serangan (pasif — hanya alert) |
| **IPS** (Intrusion Prevention System) | Mendeteksi + memblokir serangan (aktif) |
| **VPN** (Virtual Private Network) | Membuat koneksi aman/terenkripsi melalui internet publik |
| **ACL** (Access Control List) | Daftar aturan izin/tolak traffic di router |

### Topologi Jaringan

| Topologi | Bentuk | Kelebihan | Kekurangan |
|----------|--------|-----------|------------|
| **Star** | Semua terhubung ke satu pusat (switch/hub) | Mudah dikelola, 1 node mati tidak pengaruh | Pusat mati → semua mati |
| **Bus** | Satu kabel utama, semua terhubung ke kabel | Murah, sederhana | Kabel putus → semua mati |
| **Ring** | Membentuk lingkaran | Data mengalir searah, teratur | 1 node mati → jaringan putus |
| **Mesh** | Setiap node terhubung ke semua node lain | Sangat reliable, redundant | Mahal, kompleks |
| **Tree** | Kombinasi star + bus (hierarki) | Scalable | Backbone mati → cabang mati |

---

## Contoh Pertanyaan & Jawaban — Enterprise Networking

<details>
<summary><b>Q: Jelaskan proses pengiriman data dari Layer 7 ke Layer 1 (Enkapsulasi)!</b></summary>

**Enkapsulasi** adalah proses penambahan header di setiap layer saat data dikirim dari atas ke bawah.

| Layer | Nama Data | Header yang Ditambahkan |
|-------|-----------|------------------------|
| Application (7) | **Data** | — |
| Transport (4) | **Segment** | Port number (TCP/UDP header) |
| Network (3) | **Packet** | IP address (IP header) |
| Data Link (2) | **Frame** | MAC address (Ethernet header + trailer) |
| Physical (1) | **Bits** | Diubah jadi sinyal listrik/cahaya |

Proses sebaliknya (penerima) disebut **De-enkapsulasi** — membuka header dari Layer 1 ke Layer 7.
</details>

<details>
<summary><b>Q: Bagaimana cara menghitung Subnetting? Contoh: 192.168.10.0/27</b></summary>

```
IP: 192.168.10.0/27
Subnet Mask: /27 → 11111111.11111111.11111111.11100000 → 255.255.255.224

Jumlah subnet: 2^3 = 8 (3 bit dipinjam dari host portion)
Jumlah host per subnet: 2^5 - 2 = 30

Block size: 256 - 224 = 32

Subnet 1: 192.168.10.0   – 192.168.10.31  (Host: .1 – .30)
Subnet 2: 192.168.10.32  – 192.168.10.63  (Host: .33 – .62)
Subnet 3: 192.168.10.64  – 192.168.10.95  (Host: .65 – .94)
...
Subnet 8: 192.168.10.224 – 192.168.10.255 (Host: .225 – .254)
```
</details>

<details>
<summary><b>Q: Apa itu ARP dan bagaimana cara kerjanya?</b></summary>

**ARP (Address Resolution Protocol)** menerjemahkan **IP address** menjadi **MAC address**.

```
1. PC-A ingin kirim data ke 192.168.1.2
2. PC-A cek ARP cache → tidak ada
3. PC-A broadcast ARP Request: "Siapa pemilik 192.168.1.2?"
4. PC-B (pemilik 192.168.1.2) menjawab: "MAC saya AA:BB:CC:DD:EE:FF"
5. PC-A simpan di ARP cache → kirim data menggunakan MAC address tersebut
```
</details>

<details>
<summary><b>Q: Apa perbedaan Unicast, Broadcast, dan Multicast?</b></summary>

| Tipe | Penjelasan | Contoh |
|------|------------|--------|
| **Unicast** | Satu pengirim → **satu** penerima | Browsing web, kirim email |
| **Broadcast** | Satu pengirim → **semua** perangkat di jaringan | ARP request, DHCP discover |
| **Multicast** | Satu pengirim → **sekelompok** penerima tertentu | Video conference, IPTV |
</details>

<details>
<summary><b>Q: Jelaskan perbedaan Inter-VLAN Routing vs Intra-VLAN!</b></summary>

| Aspek | Intra-VLAN | Inter-VLAN |
|-------|-----------|------------|
| **Definisi** | Komunikasi **dalam** VLAN yang sama | Komunikasi **antar** VLAN berbeda |
| **Perangkat** | Switch saja cukup | Butuh **Router** atau **Layer 3 Switch** |
| **Contoh** | PC1 (VLAN 10) → PC2 (VLAN 10) | PC1 (VLAN 10) → PC3 (VLAN 20) |
</details>

---

# 📋 Checklist Belajar

## Prioritas Tinggi ⭐⭐⭐
- [ ] Database — Normalisasi (1NF, 2NF, 3NF), ERD, SQL, JOIN
- [ ] Programming — OOP (4 pilar), Struktur Data
- [ ] Web — REST API, HTTP Method, Authentication

## Prioritas Sedang ⭐⭐
- [ ] Networking — OSI 7 Layer, TCP vs UDP, Subnetting
- [ ] IT — Cloud Computing, CIA Triad, Virtualisasi

## Prioritas Normal ⭐
- [ ] Database — Index, Transaction, ACID
- [ ] Programming — Error Handling, Recursion
- [ ] Web — Session vs Token, CORS, MVC
- [ ] Networking — VLAN, DHCP/DNS/NAT, Routing Protocol
- [ ] IT — Sistem Operasi, Enkripsi

---

# 🔥 Tips Belajar

1. ✅ Baca ringkasan teori setiap mata kuliah **minimal 2 kali**
2. ✅ Coba jawab contoh pertanyaan **tanpa melihat jawaban**
3. ✅ Tulis ulang jawaban dengan **bahasa sendiri**
4. ✅ Fokus ke materi **prioritas tinggi** terlebih dahulu
5. ✅ Pahami **konsep dan analogi**, bukan hafalan
6. ✅ Latih **menjelaskan** ke orang lain atau di depan cermin
