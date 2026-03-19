# x402 - Executive Summary

## Core Idea

- **x402** is a **pay-per-use** payment pattern for HTTP resources. An endpoint can return **HTTP `402 Payment Required`** until the client attaches valid payment credentials.
- The design goal is **instant access** to APIs, data, and digital services without heavy onboarding (accounts, API keys, subscriptions), while supporting micropayments.

## Architectural Meaning (System Level)

- **HTTP 402** is treated as a _payment negotiation surface_, not a standard error.
- A complete system typically includes:
  - **Client/Agent** - requests resources and retries automatically after payment
  - **Resource Server** - owns the resource and verifies payment credentials before granting access
  - **Wallet/Signer** - signs payment authorizations, key custody is the primary risk
  - **Settlement/Verifier** - a blockchain node or a **facilitator** that verifies and settles
  - **(Stripe-backed)** - deposit addresses plus `PaymentIntent` capture after settlement

## Primary Risks (Determines Correctness)

- **Binding and freshness** - critical challenge fields must be bound to the signature, with expiry and nonce to prevent replay
- **Idempotency** - network retries must not lead to _double fulfillment_ or _double charge_
- **Operational reality** - settlement is not always synchronous, a clear finality and timeout policy is required

## References (Official)

- x402 whitepaper (concept, spec, examples): [x402 whitepaper PDF](https://www.x402.org/x402-whitepaper.pdf)
- Stripe machine payments overview: [Stripe machine payments](https://docs.stripe.com/payments/machine)
- Stripe x402 (deposit addresses and `PaymentIntent` capture): [Stripe x402 payments](https://docs.stripe.com/payments/machine/x402)
- x402 facilitator (flow and Solana duplicate settlement failure mode): [x402 Facilitator](https://docs.x402.org/core-concepts/facilitator)
