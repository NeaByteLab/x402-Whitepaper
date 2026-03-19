# x402 - Stripe Interoperability

## Tujuan

Dokumen ini menjelaskan bagaimana x402 dipakai di konteks Stripe machine payments, serta implikasi arsitekturalnya.

## Model Stripe (Ringkas)

- Server mengembalikan `402` dengan payment details
- Client membayar, lalu retry request dengan authorization
- Stripe mengelola deposit addresses dan melakukan capture `PaymentIntent` saat dana settle onchain

## Implikasi Arsitektur

- Ada state tambahan berupa `PaymentIntent` dan lifecycle capture, ini relevan untuk observability dan reconciliation
- Deposit address unik mengurangi onchain visibility volume, ini adalah pertimbangan privacy untuk merchant

## Batasan Operasional

- Fitur machine payments perlu enablement untuk akun
- Ada batas ketersediaan bisnis per wilayah untuk menerima stablecoin via Stripe

## Referensi (Official)

- Stripe machine payments: [Stripe Machine Payments](https://docs.stripe.com/payments/machine)
- Stripe x402: [Stripe x402 Payments](https://docs.stripe.com/payments/machine/x402)
- Stripe MPP: [Stripe MPP Payments](https://docs.stripe.com/payments/machine/mpp)
- Sample repo: [stripe-samples/machine-payments README](https://raw.githubusercontent.com/stripe-samples/machine-payments/main/README.md)
