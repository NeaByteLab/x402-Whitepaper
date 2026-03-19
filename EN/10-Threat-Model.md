# x402 - Threat Model (Lab Checklist)

This document focuses on realistic implementation weaknesses to test in a lab.

## 1) Integrity (Binding) Attacks

- **Field tampering**: modify `resource`, `paymentAddress/payTo`, `asset/assetAddress`, `network`
  - Expected: signature verification fails and access is denied
- **Price mismatch**:
  - scenario: the challenge max is \(0.10\), the client pays \(0.01\) and the server still fulfills
  - Expected: denied, amount must be consistent and verified

## 2) Replay And Freshness

- **Replay credential**: the same authorization is used twice
  - Expected: the second attempt is rejected via nonce or paymentId storage
- **Expiry bypass**: use credentials after `expiresAt`
  - Expected: rejected
- **Nonce reuse**: resend with the same nonce and similar payload
  - Expected: rejected

## 3) Idempotency And Retry Reality

- **Timeout retry**:
  - scenario: the client times out and retries 2 to 3 times
  - Expected: fulfill only once via idempotency
- **Partial failure**:
  - scenario: settlement succeeds, response fails, client retries
  - Expected: no double charge and no double fulfillment

Implementation note:

- x402 provides the **payment-identifier** extension for idempotency via response caching per payment ID. Reference: [x402 Payment-Identifier](https://docs.x402.org/extensions/payment-identifier)

## 4) Settlement-Specific Failure Modes

- **Duplicate settlement race (Solana real-world example)**:
  - risk: submit the same transaction multiple times before confirmation, the server fulfills multiple resources while only one payment settles
  - mitigation: settlement cache or duplicate detection
  - Reference: [x402 Facilitator](https://docs.x402.org/core-concepts/facilitator)

## 5) DoS Economics

- **Expensive 402 handshake**: challenge generation triggers heavy DB or crypto work
  - Expected: rate limiting, caching, cheap failure path

## 6) Agentic-Specific Risks (LLM Clients)

- **Prompt injection purchasing**: an agent is influenced by external content to call paid endpoints without intent
  - Mitigation: policy engine, host allowlists, max spend, rate limits, approval thresholds

## References (Official)

- Whitepaper spec fields that mitigate replay (nonce, expiresAt, paymentId): [x402 whitepaper PDF](https://www.x402.org/x402-whitepaper.pdf)
- Facilitator and duplicate settlement mitigation (Solana): [x402 Facilitator](https://docs.x402.org/core-concepts/facilitator)
- Stripe x402 lifecycle and ops components: [Stripe x402 payments](https://docs.stripe.com/payments/machine/x402)
- V2 headers and wire format: [x402 HTTP 402](https://docs.x402.org/core-concepts/http-402)
- Signed offers and receipts (audit proof): [x402 Signed Offers & Receipts](https://docs.x402.org/extensions/offer-receipt)
