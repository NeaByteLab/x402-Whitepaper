# x402 - Breakdown Agenda

This agenda structures the review order so it quickly becomes a lab implementation.

## A) Lock Spec (Hard Requirements)

- Required payment request (challenge) fields and semantics:
  - binding for resource, recipient, asset, network, amount
  - expiry (`expiresAt`) must be short and enforced
  - nonce and paymentId must be stored to prevent replay
- Authorization rules:
  - signature scheme (EIP-712 where applicable)
  - actual amount \( \le \) maxAmountRequired

Reference: [x402 whitepaper PDF](https://www.x402.org/x402-whitepaper.pdf)

## B) Build Mock (No Chain First)

- Build a server that:
  - returns deterministic 402 challenges
  - verifies credentials (signature, binding, expiry, nonce)
  - enforces idempotency, fulfill once
- Build a client that:
  - parses 402 challenges
  - signs credentials
  - retries requests

## C) Add Settlement Simulation (Then Chain)

- Simulate settlement status:
  - observed (not final)
  - confirmed or final
- Make finality and timeout policies explicit

## D) Threat Tests (Must Pass)

- tampering
- replay
- expiry
- idempotency retry
- duplicate settlement (network-specific)
- DoS economics

Solana failure mode reference: [x402 Facilitator](https://docs.x402.org/core-concepts/facilitator)

## E) Hardening Checklist

- nonce and paymentId store with TTL
- idempotency key per fulfillment
- rate limiting per IP or per client identity
- logs and an auditable trail that can be reconciled
- agent policy (allowlists, caps, approval thresholds)

## Stripe-Backed Path (Optional)

If Stripe integration is the target:

- understand deposit address handling and `PaymentIntent` capture after settlement
- understand enablement requirements and US business boundaries

References: [Stripe machine payments](https://docs.stripe.com/payments/machine) and [Stripe x402 payments](https://docs.stripe.com/payments/machine/x402)
