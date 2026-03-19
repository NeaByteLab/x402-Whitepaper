# x402 - Quickstart (Buyer)

## Purpose

This document summarizes the minimum pattern for a client that can pay x402-protected endpoints automatically, including reading `402` and retrying with a payment payload.

## Prerequisites

- A reachable x402 endpoint
- Node.js, Go, or Python runtime
- A wallet signer with the required token balance (EVM or Solana)

## Core Steps (Condensed)

- Install client packages and the required mechanism packages (EVM or SVM)
- Create a wallet signer
- Register schemes on `x402Client` for the required networks
- Wrap the HTTP client, for example `fetch`, `axios`, `net/http`, or `httpx`
- Call the endpoint normally, the wrapper will
  - receive `402`
  - read `PAYMENT-REQUIRED`
  - create `PAYMENT-SIGNATURE`
  - retry the request

## Safe Defaults (User And Security Friendly)

- Enforce per-request and periodic spend limits via `onBeforePaymentCreation`
- Allowlist hosts and facilitator URLs
- Persist `PAYMENT-RESPONSE` for audit trails
- Use payment identifier idempotency for safe retries

## References (Official)

- Buyer quickstart (Fetch, Axios, Go, Python): [x402 Quickstart For Buyers](https://docs.x402.org/getting-started/quickstart-for-buyers)
- Lifecycle hooks (spend limits, fallback): [x402 Lifecycle Hooks](https://docs.x402.org/advanced-concepts/lifecycle-hooks)
- HTTP 402 headers (V2 wire format): [x402 HTTP 402](https://docs.x402.org/core-concepts/http-402)
- Payment identifier (idempotency): [x402 Payment-Identifier](https://docs.x402.org/extensions/payment-identifier)
