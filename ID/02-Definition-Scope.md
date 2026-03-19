# x402 - Definition & Scope

## Definisi (Operasional)

- **x402**: mekanisme untuk menjadikan request HTTP “berbayar” melalui pola:
  - request tanpa kredensial pembayaran → **HTTP `402`** + detail pembayaran (challenge)
  - request ulang dengan kredensial valid → **akses diberikan**

## Scope Penggunaan

- Cocok untuk:
  - **API/data** (per request / per query)
  - **paywall konten** (per artikel / per akses)
  - **compute/inference** (per unit)
  - resource yang butuh **akses instan** dan **harga kecil namun sering**

## Non-Goal (Agar Tidak Salah Ekspektasi)

- x402 bukan pengganti total untuk:
  - kontrak enterprise (PO/invoice/SLA)
  - billing bulanan dengan rekonsiliasi kompleks
- Whitepaper bersifat _design + spec overview_ dan bukan threat model formal yang lengkap, jadi implementasi perlu menutup detail yang tidak eksplisit:
  - binding yang wajib ditandatangani
  - kebijakan expiry/nonce storage
  - idempotency dan auditability

## Decision Lens (Praktis)

- Pilih x402 jika: **kecil & sering**, perlu **instant access**, onboarding harus minimal
- Pilih billing jika: transaksi **besar & jarang**, hubungan enterprise, butuh PO/invoice/SLA

## Referensi (Official)

- Whitepaper scope & motivasi: [x402 whitepaper PDF](https://www.x402.org/x402-whitepaper.pdf)
- Stripe framing “no API key, on-demand”: [Stripe machine payments](https://docs.stripe.com/payments/machine)
