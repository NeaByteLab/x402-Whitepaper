# x402 - Quickstart (Buyer)

## Tujuan

Dokumen ini merangkum pola minimum untuk client yang dapat membayar endpoint x402 secara otomatis, termasuk cara membaca `402` dan melakukan retry dengan payment payload.

## Prasyarat

- Target endpoint x402 yang dapat diakses
- Runtime Node.js atau Go atau Python
- Wallet signer dengan saldo token yang sesuai (EVM atau Solana)

## Langkah Inti (Ringkas)

- Install paket client dan mekanisme (EVM atau SVM)
- Buat wallet signer
- Register scheme ke `x402Client` untuk network yang diperlukan
- Wrap HTTP client, mis. `fetch`, `axios`, `net/http`, atau `httpx`
- Panggil endpoint seperti biasa, wrapper akan
  - menerima `402`
  - membaca `PAYMENT-REQUIRED`
  - membentuk `PAYMENT-SIGNATURE`
  - retry request

## Safe Defaults (User And Security Friendly)

- Batasi spending per request dan per periode, gunakan hook `onBeforePaymentCreation`
- Allowlist host dan facilitator URL
- Simpan `PAYMENT-RESPONSE` untuk audit trail
- Gunakan idempotency payment identifier untuk retry yang aman

## Referensi (Official)

- Buyer quickstart (Fetch/Axios/Go/Python): [x402 Quickstart For Buyers](https://docs.x402.org/getting-started/quickstart-for-buyers)
- Lifecycle hooks (spending limits, fallback): [x402 Lifecycle Hooks](https://docs.x402.org/advanced-concepts/lifecycle-hooks)
- HTTP 402 headers (V2 wire format): [x402 HTTP 402](https://docs.x402.org/core-concepts/http-402)
- Payment identifier (idempotency): [x402 Payment-Identifier](https://docs.x402.org/extensions/payment-identifier)
