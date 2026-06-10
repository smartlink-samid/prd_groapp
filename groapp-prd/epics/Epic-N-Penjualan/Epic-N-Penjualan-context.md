# PRD - [Nama Epic]

## Business Process Diagram
```mermaid
flowchart TD

A([Customer ingin membeli barang/jasa])
A --> B[Pemilik Usaha / Admin Operasional / Kasir melayani customer]

B --> C[Konfirmasi detail transaksi<br/>barang/jasa, qty, harga, pajak,<br/>tanggal transaksi, customer]

C --> D{Customer perlu dicatat spesifik?}
D -->|Ya| E[Pilih / catat customer spesifik]
D -->|Tidak| F[Gunakan Pelanggan Umum]

E --> G[Tentukan rencana pembayaran]
F --> G

G --> H{Rencana pembayaran?}

H -->|Bayar langsung/lunas| I[Set metode pembayaran<br/>cash, transfer, QRIS, bank, dll]

H -->|Bayar nanti/tagihan| J[Set jatuh tempo pembayaran]

H -->|Bayar sebagian / DP| K[Set nominal pembayaran awal<br/>dan jatuh tempo sisa tagihan]

H -->|Cicilan / termin sederhana| L[Set sebagai tagihan bertahap<br/>dicatat lewat pembayaran sebagian]

I --> M[Buat Draft Transaksi Penjualan]
J --> M
K --> M
L --> M

M --> N{Customer batal sebelum invoice final?}
N -->|Ya| O([Draft dibatalkan<br/>tanpa dampak akuntansi])
N -->|Tidak| P[Terbitkan Invoice / Transaksi Penjualan]

P --> Q[Invoice menjadi Final]
Q --> R[Jurnal penjualan terbentuk<br/>Piutang / Pendapatan / Pajak]

R --> S{Sesuai rencana pembayaran}

S -->|Bayar langsung/lunas| T[Catat pembayaran penuh]
T --> U([Status: Lunas])

S -->|Bayar nanti/tagihan| V([Status: Belum Dibayar])
V --> W[Masuk pemantauan Piutang Usaha]

S -->|Bayar sebagian / DP| X[Catat pembayaran sebagian]
X --> Y([Status: Dibayar Sebagian])
Y --> W

S -->|Cicilan / termin sederhana| Z([Status: Belum Lunas])
Z --> W

W --> AA[Pemilik Usaha / Admin Operasional / Admin Keuangan<br/>memantau piutang]

AA --> AB{Melewati jatuh tempo?}
AB -->|Ya| AC([Status: Overdue / Jatuh Tempo])
AB -->|Tidak| AD([Tetap Belum Lunas])

AC --> AE[User follow-up manual ke customer]
AD --> AE

AE --> AF{Customer melakukan pembayaran?}
AF -->|Tidak| AG([Invoice tetap menjadi piutang terbuka])
AF -->|Ya| AH[Catat pembayaran]

AH --> AI{Total tagihan sudah lunas?}
AI -->|Ya| U
AI -->|Tidak| Y

R --> AJ{Invoice final perlu dibatalkan?}
AJ -->|Ya| AK[Batalkan invoice melalui<br/>proses koreksi / reversal]
AK --> AL[Jika perlu, buat invoice baru]
AL --> M

AJ -->|Tidak| W
```




## Problem Statement Epic

Problem utama epic Penjualan adalah UMKM membutuhkan cara yang sederhana untuk mengelola penjualan tunai maupun tagihan, tetapi proses bisnis penjualan sering melibatkan banyak kondisi: transaksi bisa langsung lunas, belum dibayar, dibayar sebagian, dibayar lewat termin sederhana, atau melewati jatuh tempo. Jika proses dari pembuatan invoice, pencatatan pembayaran, pemantauan piutang, sampai pembentukan jurnal akuntansi tidak terhubung dengan baik, user akan kesulitan mengetahui status transaksi secara akurat. Dampaknya, cashflow bisa terganggu karena piutang tidak tertagih tepat waktu, dan laporan keuangan bisa tidak akurat karena pendapatan, piutang, kas/bank, dan pajak tidak tercatat secara konsisten.

## Goal Epic

Epic Penjualan bertujuan membantu UMKM mengelola siklus penjualan secara sederhana, terstruktur, dan akurat secara akuntansi, mulai dari pencatatan transaksi penjualan, penerbitan invoice, pencatatan pembayaran, hingga pemantauan piutang usaha. Dengan adanya epic ini, user diharapkan dapat mengetahui status setiap transaksi penjualan secara jelas, menjaga piutang tetap terpantau, serta memastikan pencatatan pendapatan, piutang, kas/bank, dan pajak terbentuk secara konsisten tanpa harus memahami proses jurnal secara manual.

| No | Tujuan                                                            | Penjelasan                                                                                                                                                                              |
| -: | ----------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|  1 | Memudahkan user mencatat transaksi penjualan tunai maupun tagihan | User dapat mencatat penjualan barang/jasa non-inventory dengan alur yang mudah dipahami oleh UMKM kecil                                                                                 |
|  2 | Membuat status transaksi penjualan lebih jelas dan terkontrol     | Setiap transaksi dapat diketahui statusnya, seperti draft, final, belum dibayar, dibayar sebagian, lunas, jatuh tempo, atau dibatalkan                                                  |
|  3 | Membantu user memantau piutang usaha                              | Invoice yang belum lunas dapat dipantau berdasarkan customer, sisa tagihan, dan jatuh tempo                                                                                             |
|  4 | Menghubungkan pembayaran customer dengan invoice yang benar       | Pembayaran penuh maupun sebagian dapat mengurangi tagihan dan memperbarui status invoice secara konsisten                                                                               |
|  5 | Menjaga pencatatan akuntansi penjualan tetap akurat               | Sistem membantu membentuk pencatatan pendapatan, piutang, kas/bank, dan pajak berdasarkan aturan transaksi yang benar                                                                   |
|  6 | Mengurangi risiko kesalahan koreksi transaksi final               | Invoice yang sudah final tidak diubah bebas, melainkan dikoreksi melalui pembatalan atau reversal agar audit trail tetap terjaga                                                        |
|  7 | Menyediakan fondasi MVP yang sederhana namun scalable             | MVP fokus pada kebutuhan UMKM kecil, tetapi tetap membuka ruang pengembangan ke kebutuhan lebih kompleks seperti inventory, termin lanjutan, reminder otomatis, dan enterprise workflow |


## Indikator Kebehasilan Epic

| Kategori                           | Target                                                                                                                                               | Deskripsi                                                                                                                                                                                                                                 | Cara Pengukuran                                                                                                                                                                                                                                                                                                                         | Waktu Pengukuran                        | Penanggung Jawab |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | ---------------- |
| Keberhasilan Alur Penggunaan       | Minimal 95% skenario valid berhasil dibuat, disimpan sebagai draft, dan diterbitkan menjadi invoice final.                                           | Mengukur apakah user dapat membuat transaksi penjualan barang/jasa non-inventory dengan alur yang jelas, mulai dari memilih customer, mengisi item, qty, harga, pajak, tanggal transaksi, rencana pembayaran, hingga menerbitkan invoice. | QA menjalankan minimal 30 skenario valid end-to-end, mencakup transaksi tunai, transaksi tagihan, penggunaan customer spesifik, penggunaan Pelanggan Umum, item barang, item jasa, pajak aktif/nonaktif, dan invoice diterbitkan menjadi final.                                                                                         | Setelah release H+7 / saat beta testing | QA               |
| Keberhasilan Status Transaksi      | Minimal 100% skenario status utama menghasilkan status invoice yang benar.                                                                           | Mengukur apakah sistem mampu menampilkan status transaksi penjualan secara konsisten sesuai kondisi aktual pembayaran dan jatuh tempo.                                                                                                    | QA menjalankan minimal 30 skenario status, mencakup Draft, Final/Belum Dibayar, Dibayar Sebagian, Lunas, Overdue/Jatuh Tempo, dan Dibatalkan. QA memastikan perubahan status terjadi sesuai aksi transaksi dan pembayaran.                                                                                                              | Setelah release H+7 / saat beta testing | QA               |
| Keberhasilan Pengelolaan Piutang   | Minimal 100% invoice belum lunas tampil di daftar piutang dengan customer, sisa tagihan, dan jatuh tempo yang benar.                                 | Mengukur apakah invoice yang belum lunas dapat dipantau sebagai piutang usaha sehingga user dapat mengetahui tagihan mana yang perlu ditindaklanjuti.                                                                                     | QA menjalankan minimal 25 skenario piutang, mencakup invoice belum dibayar, invoice dibayar sebagian, invoice overdue, invoice lunas yang tidak lagi menjadi piutang terbuka, dan invoice dibatalkan yang tidak menambah piutang aktif.                                                                                                 | Setelah release H+7 / saat beta testing | QA               |
| Keberhasilan Pencatatan Pembayaran | Minimal 100% skenario pembayaran valid berhasil terhubung ke invoice yang benar dan memperbarui sisa tagihan serta status invoice.                   | Mengukur apakah pembayaran customer, baik penuh maupun sebagian, dapat dicatat pada invoice yang benar dan mengurangi outstanding amount secara konsisten.                                                                                | QA menjalankan minimal 30 skenario pembayaran, mencakup pembayaran lunas, pembayaran sebagian, pembayaran bertahap, pembayaran setelah jatuh tempo, pembayaran untuk invoice tagihan, dan pembayaran transaksi tunai. QA memastikan status dan sisa tagihan berubah benar.                                                              | Setelah release H+7 / saat beta testing | QA               |
| Validasi Aturan Akuntansi          | Minimal 100% skenario jurnal utama terbentuk sesuai aturan akuntansi yang disepakati.                                                                | Mengukur apakah invoice final, pembayaran, pajak, piutang, kas/bank, dan pembatalan/reversal menghasilkan pencatatan akuntansi yang konsisten.                                                                                            | BE bersama QA menjalankan minimal 30 skenario validasi jurnal, mencakup invoice final membentuk jurnal piutang-pendapatan-pajak, pembayaran penuh membentuk jurnal kas/bank-piutang, pembayaran sebagian mengurangi piutang, invoice draft tidak membentuk jurnal, dan pembatalan invoice final membentuk koreksi/reversal sesuai rule. | Setelah release H+7 / saat beta testing | BE               |
| Keberhasilan Validasi Data         | Minimal 100% skenario invalid tertolak, dan minimal 95% skenario valid berhasil disimpan.                                                            | Mengukur apakah sistem mampu mencegah transaksi penjualan yang tidak valid tanpa menghambat input transaksi yang benar.                                                                                                                   | QA menjalankan minimal 30 skenario validasi, mencakup customer kosong pada transaksi tagihan, item kosong, qty kosong/nol, harga kosong/invalid, tanggal transaksi kosong, jatuh tempo kosong untuk tagihan, nominal pembayaran melebihi tagihan, dan pembatalan invoice final tanpa alasan/kondisi valid.                              | Setelah release H+7 / saat beta testing | QA               |
| Kualitas Pengalaman Pengguna       | Minimal 80% user beta berhasil menyelesaikan tugas utama penjualan tanpa bantuan moderator.                                                          | Mengukur apakah user UMKM memahami cara membuat transaksi penjualan, memilih rencana pembayaran, membaca status invoice, mencatat pembayaran, dan melihat piutang tanpa bantuan tim internal.                                             | UI/UX melakukan usability testing dengan minimal 10 user/skenario, mencakup membuat invoice tunai, membuat invoice tagihan, mencatat pembayaran sebagian, melihat invoice overdue, membuka daftar piutang, dan memahami status invoice.                                                                                                 | Setelah release H+7 / saat beta testing | UI/UX            |
| Tingkat Penggunaan Fitur           | Minimal 70% tenant/user beta melakukan minimal 1 transaksi penjualan valid sampai invoice final.                                                     | Mengukur apakah fitur Penjualan benar-benar digunakan oleh user beta untuk mencatat transaksi penjualan, bukan hanya membuka halaman.                                                                                                     | Product menganalisis aktivitas tenant/user beta selama periode awal release, mencakup pembuatan draft transaksi, penerbitan invoice final, pencatatan pembayaran, pembukaan daftar penjualan, dan pembukaan daftar piutang.                                                                                                             | Setelah release H+14                    | PO               |
| Dampak terhadap Bisnis             | Minimal 70% invoice belum lunas pada tenant/user beta memiliki status piutang yang dapat dipantau dan ditindaklanjuti.                               | Mengukur apakah epic Penjualan membantu user menjaga kontrol terhadap piutang dan cashflow melalui status invoice, sisa tagihan, dan jatuh tempo.                                                                                         | Product menganalisis sample transaksi beta/release awal, mencakup jumlah invoice belum lunas, invoice overdue, invoice dibayar sebagian, invoice lunas, dan apakah data piutang tersedia untuk ditindaklanjuti oleh user.                                                                                                               | Setelah release H+30                    | PO               |
| Dampak terhadap Akurasi Laporan    | Minimal 95% sample transaksi penjualan menghasilkan data laporan penjualan, piutang, dan pembayaran yang konsisten dengan data invoice serta jurnal. | Mengukur apakah proses penjualan menghasilkan data yang dapat dipercaya untuk laporan penjualan, piutang usaha, kas/bank, pajak, dan jurnal akuntansi.                                                                                    | PO bersama BE dan QA melakukan audit sample minimal 30 transaksi setelah release, membandingkan invoice, pembayaran, outstanding amount, jurnal, dan data laporan terkait.                                                                                                                                                              | Setelah release H+30                    | PO               |



## Peta Flow Awal

```mermaid
flowchart TD
    A[User membuka daftar penjualan] --> B[User memilih aksi buat transaksi penjualan baru]
    B --> C[Sistem menampilkan form transaksi penjualan]
    C --> D[User mengisi data transaksi penjualan:<br/>customer, tanggal transaksi,<br/>item barang/jasa, qty, harga,<br/>pajak jika berlaku, rencana pembayaran]
    D --> E[Sistem memvalidasi data transaksi penjualan]
    E --> F{Apakah data transaksi valid?}

    F -->|Tidak| G[Sistem menampilkan pesan error pada field yang tidak valid]
    G --> D

    F -->|Ya| H[Sistem menyimpan transaksi sebagai draft]
    H --> I[User meninjau draft transaksi penjualan]
    I --> J{Apa aksi user terhadap draft?}

    J -->|Ubah draft| D
    J -->|Batalkan draft| AK[Sistem membatalkan draft transaksi penjualan<br/>tanpa dampak akuntansi]
    AK --> AL([End: Flow draft penjualan dibatalkan])

    J -->|Terbitkan invoice final| K[Sistem mengecek rencana pembayaran transaksi]
    K --> L{Apa rencana pembayaran transaksi?}

    L -->|Cash / lunas langsung| M[Sistem menerbitkan invoice final]
    M --> N[Sistem mencatat pembayaran penuh]
    N --> O[Sistem mengubah status invoice menjadi Lunas]
    O --> AA[Sistem menampilkan invoice final di daftar penjualan]

    L -->|Tagihan / bayar nanti| P[Sistem menerbitkan invoice final]
    P --> Q[Sistem mencatat invoice sebagai piutang usaha]
    Q --> R[Sistem mengubah status invoice menjadi Belum Dibayar]
    R --> AA

    L -->|DP / bayar sebagian| S[Sistem menerbitkan invoice final]
    S --> T[Sistem mencatat pembayaran awal / DP]
    T --> U[Sistem mencatat sisa tagihan sebagai piutang usaha]
    U --> V[Sistem mengubah status invoice menjadi Dibayar Sebagian]
    V --> AA

    L -->|Cicilan / termin sederhana| W[Sistem menerbitkan invoice final]
    W --> X[Sistem mencatat invoice sebagai tagihan termin sederhana]
    X --> Y[Sistem mencatat sisa tagihan sebagai piutang usaha]
    Y --> Z[Sistem mengubah status invoice menjadi Belum Lunas]
    Z --> AA

    AA --> AB[User membuka detail invoice final]
    AB --> AC[Sistem menampilkan detail invoice final]
    AC --> AD[User memilih aksi terhadap invoice]
    AD --> AE{Aksi apa yang dipilih user?}

    AE -->|Unduh invoice| AF[Sistem menyiapkan file invoice untuk diunduh]
    AF --> AG[User mengunduh invoice]
    AG --> AJ([End: Flow pembuatan transaksi penjualan dan invoice selesai])

    AE -->|Bagikan invoice| AH[Sistem menyiapkan opsi berbagi invoice]
    AH --> AI[User membagikan invoice ke customer]
    AI --> AJ

    AE -->|Tidak ada aksi lanjutan| AJ
```

## Role Access Matrix

| Aksi / Fitur                                  | Pemilik Usaha | Administrator Perusahaan | Admin Keuangan | Kasir | Catatan                                                                                         |
| --------------------------------------------- | ------------: | -----------------------: | -------------: | ----: | ----------------------------------------------------------------------------------------------- |
| Melihat daftar transaksi penjualan            |          true |                     true |           true |  true |                                                                                                 |
| Melihat detail transaksi penjualan            |          true |                     true |           true |  true |                                                                                                 |
| Membuat draft transaksi penjualan             |          true |                     true |           true |  true | Kasir hanya boleh membuat transaksi cash/lunas langsung                                         |
| Mengubah draft transaksi penjualan            |          true |                     true |           true | false |                                                                                                 |
| Membatalkan draft transaksi penjualan         |          true |                     true |           true | false |                                                                                                 |
| Menerbitkan invoice final                     |          true |                     true |           true |  true | Kasir hanya boleh menerbitkan invoice cash/lunas langsung                                       |
| Mencatat pembayaran invoice                   |          true |                     true |           true |  true | Kasir hanya boleh mencatat pembayaran untuk transaksi cash/lunas langsung saat transaksi dibuat |
| Melihat daftar piutang usaha                  |          true |                     true |           true | false |                                                                                                 |
| Melihat detail piutang per customer / invoice |          true |                     true |           true | false |                                                                                                 |
| Menandai / memproses invoice overdue          |          true |                     true |           true | false |                                                                                                 |
| Mengunduh invoice                             |          true |                     true |           true |  true | Kasir hanya untuk invoice cash/lunas langsung                                                   |
| Membagikan invoice                            |          true |                     true |           true |  true | Kasir hanya untuk invoice cash/lunas langsung                                                   |


## Scope

### In

* ## Scope

### In Scope

* **Pencatatan Transaksi Penjualan**  
  Sistem mendukung pencatatan transaksi penjualan barang/jasa sebagai dasar pembuatan invoice dan pencatatan keuangan penjualan.

* **Pembuatan Draft Penjualan**  
  Sistem mendukung pembuatan transaksi penjualan sebagai draft sebelum diterbitkan menjadi invoice final, termasuk pengisian data customer, item, qty, harga, pajak jika berlaku, tanggal transaksi, dan rencana pembayaran.

* **Penerbitan Invoice Final**  
  Sistem mendukung penerbitan invoice final dari draft penjualan yang sudah valid, dengan status dan dampak keuangan yang terbentuk sesuai kondisi transaksi.

* **Pencatatan Pembayaran Penjualan**  
  Sistem mendukung pencatatan pembayaran penuh maupun sebagian yang terhubung ke invoice penjualan, sehingga status dan sisa tagihan dapat diperbarui secara konsisten.

* **Pengelolaan Status Invoice**  
  Sistem mendukung perubahan status invoice berdasarkan kondisi pembayaran dan jatuh tempo, seperti lunas, belum dibayar, dibayar sebagian, belum lunas, atau jatuh tempo.

* **Pemantauan Piutang Usaha**  
  Sistem mendukung pemantauan piutang usaha berdasarkan invoice yang belum lunas, termasuk informasi customer, sisa tagihan, dan jatuh tempo.

* **Pembatalan Draft Penjualan**  
  Sistem mendukung pembatalan draft penjualan sebelum invoice final diterbitkan, tanpa membentuk dampak akuntansi.

* **Koreksi / Reversal Invoice Final**  
  Sistem mendukung koreksi atau reversal untuk invoice final yang perlu dibatalkan, agar perubahan atas transaksi final tetap memiliki jejak dan dampak akuntansi yang terkontrol.

### Planned

### Future Capability

| Capability | Deskripsi Fungsional | Kapan Dikerjakan |
|---|---|---|
| Reminder Tagihan Otomatis | Sistem dapat membantu user mengirim atau menjadwalkan pengingat tagihan kepada customer berdasarkan invoice yang belum lunas dan tanggal jatuh tempo. | Setelah pemantauan piutang dasar stabil dan pola penagihan user sudah tervalidasi. |
| Tren Penjualan | Sistem dapat menampilkan ringkasan tren penjualan berdasarkan periode, customer, status invoice, atau performa transaksi untuk membantu user memahami perkembangan penjualan. | Setelah data transaksi penjualan cukup konsisten dan kebutuhan analitik sudah lebih jelas. |
| Skema Termin Lanjutan | Sistem dapat mendukung pengaturan termin pembayaran yang lebih kompleks, seperti beberapa tanggal jatuh tempo, persentase per termin, atau rule pembayaran bertahap yang lebih fleksibel. | Setelah kebutuhan termin sederhana terbukti digunakan dan ditemukan kebutuhan variasi pembayaran yang lebih kompleks. |
| Integrasi Payment Gateway | Sistem dapat menerima pembayaran online dan mencocokkannya otomatis dengan invoice penjualan. | Setelah pencatatan pembayaran manual stabil dan kebutuhan pembayaran online menjadi prioritas. |
| Bagikan Invoice Otomatis ke Customer | Sistem dapat mengirim invoice secara otomatis melalui channel tertentu, misalnya email atau WhatsApp, setelah invoice final diterbitkan. | Setelah format invoice dan flow penerbitan invoice final sudah stabil. |

### Out of Scope

### Out of Scope

| Item | Catatan |
|---|---|
| Manajemen Inventory / Stok Barang | Epic Penjualan tidak mengelola stok, pergerakan barang, stok masuk/keluar, atau penyesuaian persediaan. |
| Pembelian / Pengadaan | Epic Penjualan tidak mencakup proses pembelian barang/jasa dari supplier, purchase order, atau pencatatan hutang usaha. |
| Retur Pembelian | Retur pembelian merupakan bagian dari proses pengadaan/pembelian, bukan penjualan. |
| Payroll / Penggajian | Epic Penjualan tidak mencakup perhitungan gaji, komisi, bonus, atau pembayaran karyawan. |
| Manajemen Kas dan Bank Umum | Epic Penjualan hanya mencatat pembayaran yang terkait invoice penjualan, bukan pengelolaan kas/bank secara umum seperti transfer antar akun atau rekonsiliasi bank. |
| CRM Lanjutan | Epic Penjualan tidak mencakup pengelolaan pipeline sales, follow-up prospek, segmentasi customer, atau aktivitas customer relationship management lanjutan. |
| E-commerce / Marketplace Integration | Epic Penjualan tidak mencakup sinkronisasi order dari marketplace, toko online, atau channel penjualan eksternal. |
| Point of Sale Lengkap | Epic Penjualan tidak mencakup kemampuan POS lengkap seperti shift kasir, cash drawer, barcode scanner, receipt printer, atau closing kasir. |
| Laporan Keuangan Lengkap | Epic Penjualan hanya membentuk data dan jurnal terkait penjualan, bukan laporan keuangan lengkap seperti neraca, laba rugi, atau arus kas. |

## Aturan Bisnis Epic

* [Aturan bisnis epic 1]
* [Aturan bisnis epic 2]

## Diagram Konsep

```mermaid
flowchart TD
    A[Entitas/Konsep A] --> B[Entitas/Konsep B]
```

## Supporting Information

[Supporting information epic]

