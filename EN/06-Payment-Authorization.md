# x402 - Payment Authorization

## Purpose

An authorization credential proves that:

- the client approved payment for a specific challenge
- the payment amount does not exceed `maxAmountRequired`
- the signature comes from the relevant wallet

## Authorization Contents (Whitepaper)

The whitepaper describes a signed message containing:

- all fields from the payment request
- the **actual amount** being paid, which must be \( \le \) `maxAmountRequired`
- a timestamp
- a cryptographic signature from the paying wallet

The whitepaper mentions **EIP-712** signatures.

## EIP-712 Meaning (Practical)

- Reduces signing ambiguity compared to arbitrary string signing
- Helps wallet UIs display what is being approved clearly (domain, amount, asset, resource)

## Wallet As Identity (Implication)

x402 documentation positions a wallet as:

- the payment mechanism
- a unique buyer/seller identity within the protocol

## Failure Modes To Test

- Signature validates, but binding is incomplete, tampering might pass
- Authorization is reused, replay succeeds without nonce storage
- Timestamp or expiry is ignored, credentials remain valid too long

## References (Official)

- Payment authorization and EIP-712: [x402 whitepaper PDF](https://www.x402.org/x402-whitepaper.pdf)
- Stripe x402 (402 → pay → retry + authorization): [Stripe x402 payments](https://docs.stripe.com/payments/machine/x402)
- Wallet role: [x402 Wallet](https://docs.x402.org/core-concepts/wallet)
