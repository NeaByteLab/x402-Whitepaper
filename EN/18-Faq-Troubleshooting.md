# x402 - FAQ And Troubleshooting

## Purpose

This document summarizes practical answers for common questions and the most common integration failure modes.

## Basics

- x402 is an open-source protocol, not a single-vendor product
- Core flow: request, `402`, sign payload, retry, verify, settle, response

## Common Troubleshooting

### Still Receiving `402` After Sending `PAYMENT-SIGNATURE`

Typical causes:

- insufficient token balance, or address flagged by policy checks
- amount mismatch, the `exact` scheme requires strict equality
- invalid signature, for example wrong chain ID or payload fields

### Works On Testnet, Fails On Mainnet

Typical causes:

- higher mainnet gas fees, the wallet might need native gas tokens when not sponsored
- missing mainnet tokens or incorrect network identifier

## Refund Model (Implication)

- The `exact` scheme is a push payment and is typically irreversible
- Refunds are usually handled via business logic, the seller sends a new transfer back to the buyer

## Facilitator Trust Boundary

- Payment payloads are signed by the buyer and settle on-chain
- Facilitators do not custody funds and cannot modify transactions without breaking signature checks

## References (Official)

- FAQ: [x402 FAQ](https://docs.x402.org/faq)
- Whitepaper: [x402 Whitepaper PDF](https://www.x402.org/x402-whitepaper.pdf)
