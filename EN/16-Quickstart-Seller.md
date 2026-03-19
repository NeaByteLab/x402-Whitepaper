# x402 - Quickstart (Seller)

## Purpose

This document summarizes the minimum steps to make a paid HTTP endpoint with x402, focused on a safe test-first workflow.

## Prerequisites

- An existing API or server
- Node.js, Go, or Python
- A wallet to receive funds (EVM or Solana)
- A facilitator for verify and settle, for testing `https://x402.org/facilitator` can be used

## Core Steps (Condensed)

- Install middleware packages for the chosen framework
- Configure protected routes with `accepts[]` entries, including `scheme`, `price`, `network`, and `payTo`
- Register schemes for the networks in use (EVM and or SVM)
- Run the server and validate two phases
  - requests without payment headers return `402`
  - retried requests with valid payment payloads return `200`

## 5-Minute Smoke Test (No Internal SDK Assumptions)

The goal of this smoke test is to validate wire format, not perfect wallet integration.

- Ensure the first request returns `402` and includes `PAYMENT-REQUIRED`
- Decode `PAYMENT-REQUIRED` and confirm `accepts[]` contains `scheme`, `network`, `asset`, and `payTo`
- Ensure the paid retry returns `200` and includes `PAYMENT-RESPONSE`

Raw HTTP examples and canonical V2 field mapping: [20 - Canonical Fields](./20-Canonical-Fields.md)

## Safe Defaults (Security Friendly)

- Start on testnets, move to mainnet after the flow and idempotency are stable
- Enable idempotency for network retries and timeouts
- Log verify and settle events for auditability
- Apply rate limiting on endpoints that can be expensive to serve

## References (Official)

- Seller quickstart (Express, Next.js, Hono, Go, Python): [x402 Quickstart For Sellers](https://docs.x402.org/getting-started/quickstart-for-sellers)
- Lifecycle hooks (logging, access control, recovery): [x402 Lifecycle Hooks](https://docs.x402.org/advanced-concepts/lifecycle-hooks)
- Facilitator (verify, settle, duplicate settlement): [x402 Facilitator](https://docs.x402.org/core-concepts/facilitator)
