# x402 - Stripe Interoperability

## Purpose

This document explains how x402 is used in the context of Stripe machine payments and the architectural implications.

## Stripe Model (Condensed)

- The server returns `402` with payment details
- The client pays and retries with authorization
- Stripe manages deposit addresses and captures `PaymentIntent` when funds settle on-chain

## Architectural Implications

- Additional state exists in `PaymentIntent` objects and capture lifecycle, this matters for observability and reconciliation
- Unique deposit addresses reduce on-chain visibility of processing volume, this is a privacy consideration for merchants

## Operational Constraints

- Machine payments features require enablement on the Stripe account
- Stablecoin acceptance via Stripe has regional or business constraints

## References (Official)

- Stripe machine payments: [Stripe Machine Payments](https://docs.stripe.com/payments/machine)
- Stripe x402: [Stripe x402 Payments](https://docs.stripe.com/payments/machine/x402)
- Stripe MPP: [Stripe MPP Payments](https://docs.stripe.com/payments/machine/mpp)
- Sample repo: [stripe-samples/machine-payments README](https://raw.githubusercontent.com/stripe-samples/machine-payments/main/README.md)
