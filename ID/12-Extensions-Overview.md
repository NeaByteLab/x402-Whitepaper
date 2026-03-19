# x402 - Extensions Overview

## Tujuan

Extensions adalah layer komposabel di atas core x402. Extension dapat memperkaya:

- response `402` (mis. offer signing, discovery metadata)
- response settlement `200` (mis. receipt signing, idempotency identifiers)
  tanpa mengubah business logic utama.

## Extension Points (Resource Server)

Resource server extension bisa hook di tiga titik:

- **Enrich Declaration** - saat route registration, dapat mempersempit deklarasi extension sesuai transport
- **Enrich 402 Response** - menambah data ke `PaymentRequired` saat server mengembalikan `402`
- **Enrich Settlement Response** - menambah data ke `PAYMENT-RESPONSE` setelah pembayaran sukses

## Extension Points (Facilitator)

Facilitator extension digunakan oleh mekanisme verify dan settle, contoh paling umum adalah gas sponsoring. Dengan ini, facilitator bisa menjalankan settlement tanpa memaksa payer memiliki native gas token.

## Extension Yang Praktis Untuk Dokumentasi Ini

- **Payment Identifier** - idempotency berbasis payment ID dan caching response untuk retry aman
- **Signed Offers And Receipts** - artefak kriptografis untuk auditability, dispute, dan reputasi
- **Bazaar** - discovery layer untuk endpoint HTTP dan MCP tools
- **Sign-In-With-X** - wallet authentication untuk akses ulang konten yang sudah dibayar

## Safe Defaults (Developer And Security Friendly)

- Deklarasikan extension per-route, dan register extension di server secara eksplisit
- Hindari asumsi extension selalu ada, karena deklarasi yang tidak diregister bisa di-ignore
- Gunakan idempotency dan receipt signing sebagai baseline untuk endpoint berbayar yang dipakai agent

## Referensi (Official)

- Extensions overview: [x402 Extensions Overview](https://docs.x402.org/extensions/overview)
- Payment identifier: [x402 Payment-Identifier](https://docs.x402.org/extensions/payment-identifier)
- Signed offers and receipts: [x402 Signed Offers And Receipts](https://docs.x402.org/extensions/offer-receipt)
- Bazaar: [x402 Bazaar](https://docs.x402.org/extensions/bazaar)
- Sign-in-with-x: [x402 Sign-In-With-X](https://docs.x402.org/extensions/sign-in-with-x)
