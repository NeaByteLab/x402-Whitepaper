# x402 - Breakdown Agenda

Agenda ini menyusun urutan bedah agar cepat turun ke implementasi lab.

## A) Lock Spec (Hard Requirements)

- Field wajib payment request (challenge) dan semantics:
  - binding resource, recipient, asset, network, amount
  - expiry (`expiresAt`) harus pendek dan dipatuhi
  - nonce/paymentId wajib disimpan untuk anti-replay
- Authorization rules:
  - signature scheme (EIP-712 bila relevan)
  - actual amount \( \le \) maxAmountRequired

Referensi: [x402 whitepaper PDF](https://www.x402.org/x402-whitepaper.pdf)

## B) Build Mock (Tanpa Chain Dulu)

- Implement server yang:
  - mengeluarkan 402 challenge deterministik
  - memverifikasi credential (signature + binding + expiry + nonce)
  - mengunci idempotency (fulfill sekali)
- Implement client yang:
  - parse 402 challenge
  - sign credential
  - retry request

## C) Add Settlement Simulation (Baru Kemudian Chain)

- Simulasi status settlement:
  - observed (belum final)
  - confirmed/final
- Kebijakan finality/timeout dibuat eksplisit

## D) Threat Tests (Wajib Lulus)

- Tampering
- Replay
- Expiry
- Idempotency retry
- Duplicate settlement (network-specific)
- DoS economics

Referensi failure mode Solana: [x402 Facilitator](https://docs.x402.org/core-concepts/facilitator)

## E) Hardening Checklist

- Nonce/paymentId store + TTL
- Idempotency key per request fulfillment
- Rate limit per IP / per client identity
- Logging + audit trail yang bisa direkonsiliasi
- Policy untuk agent (allowlist, caps, approval threshold)

## Stripe-Backed Path (Opsional)

Jika targetnya integrasi Stripe:

- pahami deposit address dan `PaymentIntent` capture setelah settlement
- pahami requirement enablement dan boundary “US business acceptance”

Referensi: [Stripe machine payments](https://docs.stripe.com/payments/machine) dan [Stripe x402 payments](https://docs.stripe.com/payments/machine/x402)
