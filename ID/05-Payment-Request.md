# x402 - Payment Request (402 Challenge)

## Tujuan

Challenge `402` adalah kontrak yang menyatakan:

- resource apa yang diminta
- berapa harga dan dalam asset apa
- ke mana pembayaran diarahkan
- pada network apa
- batas waktu dan anti-replay (expiry/nonce/paymentId)

## Wire Format (x402 V2)

Pada x402 V2, payment requirements dikirim lewat header:

- **`PAYMENT-REQUIRED`**: Base64-encoded JSON `PaymentRequired`

Payload ini berisi detail acceptance (mis. skema pembayaran, harga, network, payTo) dan dapat mengumumkan extension yang didukung (mis. idempotency).

## Field Penting (Kontrak Data)

Implementasi V2 modern berpusat pada `accepts[]` di dalam `PaymentRequired`.

- Untuk daftar field canonical V2, lihat: [20 - Canonical Fields](./20-Canonical-Fields.md)
- Catatan penting: beberapa dokumen historis menampilkan nama alias seperti `paymentAddress` atau `assetAddress`. Secara praktik, V2 akan mempublikasikan konsep yang sama lewat `accepts[].payTo` dan `accepts[].asset`.

Jika server memakai `price` (mis. `'$0.01'`), facilitator atau mekanisme yang dipakai akan memetakan `price` ke `amount` token yang dibutuhkan. Client sebaiknya berpegang pada `accepts[]` yang diterima di `PAYMENT-REQUIRED`, bukan menebak sendiri asset atau amount.

## Makna Security (Yang Wajib Dikunci)

- **Binding**: signature di sisi client harus mengikat field yang menentukan nilai ekonomi dan tujuan pembayaran:
  - resource
  - amount (max + actual)
  - recipient (paymentAddress/payTo)
  - asset (assetAddress/asset)
  - network
  - expiresAt, nonce, paymentId
- **Expiry** bukan dekorasi: tanpa expiry yang pendek, challenge menjadi target replay
- **Resource binding** harus eksplisit: jika `resource` tidak terikat, akses bisa “dipindahkan” lintas endpoint

## Implementasi (Stripe Framing)

- Stripe x402 menyebut server mengembalikan 402 + payment details, lalu Stripe mengelola deposit address dan capture `PaymentIntent` setelah settlement. Referensi: [Stripe x402 payments](https://docs.stripe.com/payments/machine/x402)

## Referensi (Official)

- Payment request format (spec): [x402 whitepaper PDF](https://www.x402.org/x402-whitepaper.pdf)
- HTTP 402 + header `PAYMENT-REQUIRED`: [x402 HTTP 402](https://docs.x402.org/core-concepts/http-402)
