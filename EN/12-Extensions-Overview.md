# x402 - Extensions Overview

## Purpose

Extensions are a composable layer on top of core x402. Extensions can enrich:

- `402` responses (for example, offer signing and discovery metadata)
- settlement responses (for example, receipt signing and idempotency identifiers)

without changing the primary business logic.

## Extension Points (Resource Server)

Resource server extensions can hook into three points:

- **Enrich Declaration** - at route registration time, can narrow declarations based on transport context
- **Enrich 402 Response** - adds data to `PaymentRequired` when returning `402`
- **Enrich Settlement Response** - adds data to `PAYMENT-RESPONSE` after successful payment

## Extension Points (Facilitator)

Facilitator extensions are used by verify and settle mechanisms, the most common example is gas sponsoring. This enables settlement without requiring the payer to hold native gas tokens.

## Extensions Worth Treating As Baseline

- **Payment Identifier** - idempotency via payment IDs and response caching for safe retries
- **Signed Offers And Receipts** - cryptographic artifacts for auditability, disputes, and reputation
- **Bazaar** - discovery layer for HTTP endpoints and MCP tools
- **Sign-In-With-X** - wallet authentication for accessing previously purchased content without repaying

## Safe Defaults (Developer And Security Friendly)

- Declare extensions per route and register extensions explicitly on the server
- Do not assume extensions exist, undeclared or unregistered extensions can be ignored
- Treat idempotency and receipt signing as defaults for paid endpoints used by agents

## References (Official)

- Extensions overview: [x402 Extensions Overview](https://docs.x402.org/extensions/overview)
- Payment identifier: [x402 Payment-Identifier](https://docs.x402.org/extensions/payment-identifier)
- Signed offers and receipts: [x402 Signed Offers & Receipts](https://docs.x402.org/extensions/offer-receipt)
- Bazaar: [x402 Bazaar](https://docs.x402.org/extensions/bazaar)
- Sign-in-with-x: [x402 Sign-In-With-X](https://docs.x402.org/extensions/sign-in-with-x)
