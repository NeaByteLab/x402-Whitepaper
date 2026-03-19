# x402 - Threat Model (Lab Checklist)

Dokumen ini fokus pada kelemahan implementasi yang realistis untuk diuji di lab.

## 1) Integrity (Binding) Attacks

- **Tampering field**: ubah `resource`, `paymentAddress/payTo`, `asset/assetAddress`, `network`
  - Expected: signature verification gagal → akses ditolak
- **Price mismatch**:
  - skenario: challenge max \(0.10\), client membayar \(0.01\) tapi server tetap fulfill
  - Expected: ditolak (amount harus konsisten dan diverifikasi)

## 2) Replay & Freshness

- **Replay credential**: authorization yang sama dipakai dua kali
  - Expected: kedua ditolak (nonce/paymentId store)
- **Expiry bypass**: gunakan credential setelah `expiresAt`
  - Expected: ditolak
- **Nonce reuse**: kirim ulang dengan nonce sama tapi payload “mirip”
  - Expected: ditolak

## 3) Idempotency & Retry Reality

- **Timeout retry**:
  - skenario: client timeout lalu retry 2–3x
  - Expected: fulfill hanya sekali (idempotency key)
- **Partial failure**:
  - skenario: settlement sukses, response gagal, lalu client retry
  - Expected: tidak terjadi double charge/double fulfill

Catatan implementasi:

- x402 menyediakan extension **payment-identifier** untuk idempotency berbasis caching response per payment ID. Referensi: [x402 Payment-Identifier](https://docs.x402.org/extensions/payment-identifier)

## 4) Settlement-Specific Failure Modes

- **Duplicate settlement race (contoh nyata di Solana)**:
  - risiko: submit tx yang sama berkali-kali sebelum confirm → server bisa fulfill berkali-kali “bayar sekali”
  - mitigasi: settlement cache/duplicate detection
  - Referensi: [x402 Facilitator](https://docs.x402.org/core-concepts/facilitator)

## 5) DoS Economics

- **402 handshake mahal**: challenge generation memukul DB/crypto terlalu sering
  - Expected: rate limit, caching, cheap failure path

## 6) Agentic-Specific Risks (Bila Client Adalah LLM Agent)

- **Prompt injection purchasing**: agent terpengaruh konten eksternal untuk mengakses endpoint berbayar tanpa intent
  - Mitigasi: policy engine (allowlist host, max spend, rate limit, approval threshold)

## Referensi (Official)

- Whitepaper spec fields yang memitigasi replay (nonce/expiresAt/paymentId): [x402 whitepaper PDF](https://www.x402.org/x402-whitepaper.pdf)
- Facilitator dan mitigasi duplicate settlement (Solana): [x402 Facilitator](https://docs.x402.org/core-concepts/facilitator)
- Stripe x402 (lifecycle dan komponen ops): [Stripe x402 payments](https://docs.stripe.com/payments/machine/x402)
- Header V2 & wire format: [x402 HTTP 402](https://docs.x402.org/core-concepts/http-402)
- Signed offers & receipts (audit proof): [x402 Signed Offers & Receipts](https://docs.x402.org/extensions/offer-receipt)
