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

[Isi goal epic]

## Indikator Kebehasilan Epic

| Kategori | Nama Indikator | Deskripsi | Cara Pengukuran | Waktu Pengukuran | Target | Penanggung Jawab |
| -------- | -------------- | --------- | --------------- | ---------------- | ------ | ---------------- |
|          |                |           |                 |                  |        |                  |

## Peta Flow Awal

```mermaid
flowchart TD
    A[Start] --> B[Flow utama]
    B --> C[End]
```

## Role Access Matrix

| Fitur        | Role 1 | Role 2 | Role 3 | Role 4 |
| ------------ | ------ | ------ | ------ | ------ |
| [Nama Fitur] | ✓      | ✓      | -      | -      |

## Scope

### In

* [Scope in]

### Planned

* [Planned scope / planned features]

### Out of Scope

* [Out of scope]

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

