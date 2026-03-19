# x402 - Quickstart (Seller)

## Tujuan

Dokumen ini merangkum langkah minimum untuk membuat endpoint HTTP berbayar dengan x402, fokus pada pola yang aman untuk testing terlebih dulu.

## Prasyarat

- Server/API yang sudah ada
- Node.js atau Go atau Python
- Wallet untuk menerima pembayaran (EVM atau Solana)
- Facilitator untuk verify dan settle, untuk testing dapat memakai `https://x402.org/facilitator`

## Langkah Inti (Ringkas)

- Install paket middleware sesuai framework
- Daftarkan routes yang diproteksi, isi `accepts[]` dengan `scheme`, `price`, `network`, dan `payTo`
- Register scheme untuk network yang dipakai (EVM dan atau SVM)
- Jalankan server dan uji 2 fase
  - request tanpa payment header menghasilkan `402`
  - request retry dengan payment payload menghasilkan `200`

## 5-Minute Smoke Test (Tanpa SDK Internal)

Tujuan smoke test ini adalah memvalidasi wire format, bukan “integrasi wallet” yang sempurna.

- Pastikan request pertama menghasilkan `402` dan header `PAYMENT-REQUIRED` ada
- Decode `PAYMENT-REQUIRED` untuk memastikan `accepts[]` berisi `scheme`, `network`, `asset`, dan `payTo`
- Pastikan request paid-retry menghasilkan `200` dan header `PAYMENT-RESPONSE` ada

Contoh HTTP mentah dan mapping field canonical V2: [20 - Canonical Fields](./20-Canonical-Fields.md)

## Safe Defaults (Security Friendly)

- Mulai dari testnet dulu, lalu pindah mainnet setelah flow dan idempotency stabil
- Aktifkan idempotency untuk retry network dan timeout
- Catat event verify dan settle untuk auditability
- Terapkan rate limit pada endpoint yang berpotensi mahal

## Referensi (Official)

- Seller quickstart (kode Express/Next.js/Hono/Go/Python): [x402 Quickstart For Sellers](https://docs.x402.org/getting-started/quickstart-for-sellers)
- Lifecycle hooks (logging, access control, recovery): [x402 Lifecycle Hooks](https://docs.x402.org/advanced-concepts/lifecycle-hooks)
- Facilitator (verify, settle, duplicate settlement): [x402 Facilitator](https://docs.x402.org/core-concepts/facilitator)
