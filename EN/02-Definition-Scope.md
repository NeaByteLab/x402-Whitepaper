# x402 - Definition And Scope

## Definition (Operational)

- **x402** makes an HTTP request “paid” via a simple pattern:
  - requests without payment credentials return **HTTP `402`** with payment requirements (challenge)
  - the same request retried with valid credentials is **granted access**

## Usage Scope

- Best suited for:
  - **APIs and data** (per request or per query)
  - **content paywalls** (per article or per access)
  - **compute and inference** (per unit)
  - resources that require **instant access** and have **small, frequent pricing**

## Non-Goal (Avoid Wrong Expectations)

- x402 is not a full replacement for:
  - enterprise contracts (PO, invoice, SLA)
  - monthly billing with complex reconciliation
- The whitepaper is a _design and spec overview_ rather than a complete threat model. Implementations must close details that might be implicit:
  - what must be signed and bound
  - expiry and nonce storage policy
  - idempotency and auditability

## Decision Lens (Practical)

- Choose x402 when transactions are **small and frequent**, **instant access** is needed, and onboarding must be minimal.
- Choose billing when transactions are **large and infrequent**, enterprise workflows exist, and PO/invoice/SLA is required.

## References (Official)

- Whitepaper scope and motivation: [x402 whitepaper PDF](https://www.x402.org/x402-whitepaper.pdf)
- Stripe framing, no API keys and on-demand: [Stripe machine payments](https://docs.stripe.com/payments/machine)
