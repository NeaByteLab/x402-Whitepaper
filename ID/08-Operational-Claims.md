# x402 - Operational Claims

## Klaim Umum (Dan Maksudnya)

- **No chargebacks**: settlement onchain umumnya final, model “reversal months later” seperti kartu tidak berlaku
- **No PCI (untuk developer)**: bila hanya menerima crypto/onchain, data kartu tidak diproses oleh merchant

## Batas Realitas (Yang Tetap Harus Di-Hardening)

- “No chargebacks” ≠ “no fraud / no abuse”
  - replay
  - tampering
  - duplicate settlement
  - metering/fulfillment abuse
  - DoS economics
- Risiko terbesar biasanya **key custody**: kebocoran key = loss irreversible

## Stripe Framing (Ops)

Stripe menekankan integrasi operasional yang mirip pembayaran Stripe biasa:

- settlement ke Stripe balance dan payout multi-currency
- refunds via API/Dashboard
- deposit address unik untuk mengurangi on-chain visibility volume

Referensi: [Stripe machine payments](https://docs.stripe.com/payments/machine)

## Signed Offers & Receipts (Auditability)

x402 menyediakan extension untuk menandatangani:

- **Offer** pada setiap response `402` (komitmen term pembayaran)
- **Receipt** pada setiap response `200` (bukti delivery)

Artifact ini berguna untuk audit trail, dispute resolution, dan reputasi (verifiable proof-of-interaction).

Referensi: [x402 Signed Offers & Receipts](https://docs.x402.org/extensions/offer-receipt)

## Referensi (Official)

- x402 whitepaper (motivasi dan klaim friction reduction): [x402 whitepaper PDF](https://www.x402.org/x402-whitepaper.pdf)
