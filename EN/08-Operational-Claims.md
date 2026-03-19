# x402 - Operational Claims

## Common Claims (And What They Mean)

- **No chargebacks** - on-chain settlement is typically final, the long reversal window of card payments does not apply
- **No PCI for developers** - if only crypto/on-chain payments are accepted, card data is not processed by the merchant

## Practical Limits (Still Requires Hardening)

- No chargebacks does not mean no fraud or abuse:
  - replay
  - tampering
  - duplicate settlement
  - metering and fulfillment abuse
  - DoS economics
- The primary risk remains **key custody**, leaked keys usually mean irreversible loss

## Stripe Operational Framing

Stripe emphasizes operational continuity similar to standard Stripe payments:

- settlement into Stripe balance and multi-currency payouts
- refunds via API or Dashboard
- unique deposit addresses to reduce on-chain visibility of processing volume

Reference: [Stripe machine payments](https://docs.stripe.com/payments/machine)

## Signed Offers And Receipts (Auditability)

x402 provides an extension that signs:

- **offers** on `402` responses (commitment to payment terms)
- **receipts** on `200` responses (proof of delivery)

These artifacts support audit trails, dispute resolution, and reputation (verifiable proof-of-interaction).

Reference: [x402 Signed Offers & Receipts](https://docs.x402.org/extensions/offer-receipt)

## References (Official)

- Whitepaper motivation and friction claims: [x402 whitepaper PDF](https://www.x402.org/x402-whitepaper.pdf)
