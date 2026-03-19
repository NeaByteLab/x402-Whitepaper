# x402 - Payment Request (402 Challenge)

## Purpose

A `402` challenge acts as a contract that defines:

- which resource is being requested
- the price and the asset used
- the payment destination
- the target network
- expiry and anti-replay data (expiry, nonce, paymentId)

## Wire Format (x402 V2)

In x402 V2, payment requirements are carried via:

- **`PAYMENT-REQUIRED`** - Base64-encoded JSON `PaymentRequired`

This payload includes acceptance details (scheme, price, network, payTo) and can declare supported extensions (for example, idempotency).

## Key Fields (Data Contract)

The whitepaper describes a payment request payload with fields such as:

- **maxAmountRequired**
- **assetType**
- **assetAddress**
- **paymentAddress**
- **network**
- **expiresAt**
- **nonce**
- **paymentId**

Practical 402 examples also include:

- **resource**
- **description**
- **payTo**
- **asset**

## Security Meaning (Must Be Locked Down)

- **Binding** - the client signature must bind the fields that define economic value and destination:
  - resource
  - amount (max and actual)
  - recipient (paymentAddress or payTo)
  - asset (assetAddress or asset)
  - network
  - expiresAt, nonce, paymentId
- **Expiry** is not decorative, a short expiry reduces replay window.
- **Resource binding** must be explicit, if `resource` is not bound, access can be shifted across endpoints.

## Stripe Implementation Framing

- Stripe x402 describes the server returning 402 with payment details, then Stripe manages deposit addresses and captures `PaymentIntent` after settlement. Reference: [Stripe x402 payments](https://docs.stripe.com/payments/machine/x402)

## References (Official)

- Payment request format: [x402 whitepaper PDF](https://www.x402.org/x402-whitepaper.pdf)
- HTTP 402 and `PAYMENT-REQUIRED`: [x402 HTTP 402](https://docs.x402.org/core-concepts/http-402)
