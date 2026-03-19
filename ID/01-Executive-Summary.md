# x402 - Executive Summary

## Inti

- **x402** adalah pola pembayaran **pay-per-use** untuk resource HTTP: endpoint dapat mengembalikan **HTTP `402 Payment Required`** sampai klien melampirkan kredensial/otorisasi pembayaran.
- Tujuan desain: membuat **akses instan** ke API/data/digital service tanpa onboarding berat (akun, API key, subscription) dan mendukung micropayments.

## Makna Arsitektural (Level Sistem)

- **HTTP 402** diperlakukan sebagai _payment negotiation surface_, bukan error biasa
- Sistem yang lengkap biasanya punya komponen:
  - **Client/Agent**: requester yang bisa membayar dan retry otomatis
  - **Resource server**: pemilik resource yang memverifikasi kredensial pembayaran sebelum memberi akses
  - **Wallet/Signer**: komponen signing pembayaran (dan custody key adalah risiko utama)
  - **Settlement/verifier**: blockchain node atau **facilitator** yang membantu verify/settle
  - **(Stripe-backed)**: deposit address + `PaymentIntent` capture setelah settlement

## Risiko Utama (Menentukan Benar/Salah Implementasi)

- **Binding & freshness**: field penting pada challenge harus terikat ke signature + ada expiry/nonce untuk mencegah replay
- **Idempotency**: retry jaringan tidak boleh menghasilkan _double fulfillment_ atau _double charge_
- **Ops reality**: settlement tidak selalu sinkron, jadi perlu kebijakan finality/timeout

## Referensi (Official)

- x402 whitepaper (konsep + spec + contoh): [x402 whitepaper PDF](https://www.x402.org/x402-whitepaper.pdf)
- Stripe machine payments overview: [Stripe machine payments](https://docs.stripe.com/payments/machine)
- Stripe x402 (deposit address + `PaymentIntent` capture): [Stripe x402 payments](https://docs.stripe.com/payments/machine/x402)
- x402 facilitator (flow + failure mode Solana duplicate settlement): [x402 Facilitator](https://docs.x402.org/core-concepts/facilitator)
