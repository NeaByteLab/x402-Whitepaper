# x402 - Payment Authorization

## Purpose

An authorization credential proves that:

- the client approved payment for a specific challenge
- the payment amount does not exceed `maxAmountRequired`
- the signature comes from the relevant wallet

In x402 V2 wire format, this credential is carried in the **`PAYMENT-SIGNATURE`** header (client → server).

## Authorization Contents (Whitepaper)

The whitepaper describes a signed message containing:

- all fields from the payment request
- the **actual amount** being paid, which must be \( \le \) `maxAmountRequired`
- a timestamp
- a cryptographic signature from the paying wallet

The whitepaper mentions **EIP-712** signatures.

## Canonical V2 Binding (What Must Be Bound)

For modern V2 implementations, the binding source of truth is `PAYMENT-REQUIRED`, specifically `resource` and `accepts[]`. The canonical V2 terminology mapping is in: [20 - Canonical Fields](./20-Canonical-Fields.md)

At minimum, the signed credential must bind:

- **resource**: `resource.url` (at least the relevant path + query)
- **network**: `accepts[].network` (CAIP-2)
- **asset**: `accepts[].asset`
- **recipient**: `accepts[].payTo`
- **amount**: the paid amount and its maximum (if using a “max + actual” model)
- **freshness**: timestamp plus an expiry window (or `expiresAt`)
- **anti-replay**: `nonce` and/or `paymentId` (or the payment identifier extension)

If any of these bindings are missing, tampering and replay across endpoints or recipients becomes easier.

## EIP-712 Meaning (Practical)

- Reduces signing ambiguity compared to arbitrary string signing
- Helps wallet UIs display what is being approved clearly (domain, amount, asset, resource)

## Server Verification Checklist (Practical)

Use this checklist when processing retry requests carrying `PAYMENT-SIGNATURE`.

- **Decode** the `PAYMENT-SIGNATURE` payload and extract:
  - payer identity (address/signing key)
  - bound fields (resource, network, asset, payTo, amount, expiry, nonce/paymentId)
  - signature/proof required by the mechanism
- **MUST** check resource binding:
  - the payload resource must match the currently requested resource
- **MUST** check destination binding:
  - `payTo` must match the offered value exactly
- **MUST** check economic binding:
  - asset must match
  - amount must satisfy the selected rule (exact, or \( \le \) max)
- **MUST** check freshness:
  - not expired
  - timestamp is within an acceptable window
- **MUST** prevent replay:
  - reject previously used nonce/paymentId (or use the idempotency extension)
- **SHOULD** persist the receipt:
  - store `PAYMENT-RESPONSE` for audit trails and disputes

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
