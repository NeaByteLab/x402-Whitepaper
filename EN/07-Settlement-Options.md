# x402 - Settlement Options

## Purpose

Settlement is the process where payment truly happens, or is verified as having happened, before a resource is fulfilled.

## Settlement Options (Whitepaper)

- **On-chain settlement** - direct blockchain transactions
- **Layer-2 settlement** - optimistic or ZK rollups for lower fees
- **Payment channels** - high-frequency micropayments between trusted parties
- **Batched settlements** - aggregate many micropayments into one transaction

## Architectural Questions That Must Be Answered

- **Finality policy** - when is payment considered paid?
  - observed tx, confirmed, N confirmations
- **Timeout policy** - how long to wait before treating the request as failed
- **Consistency** - what happens if settlement succeeds but the client never receives the response and retries

## Stripe-Backed Settlement (Implication)

- Stripe x402 manages deposit addresses and captures `PaymentIntent` when funds settle on-chain. This adds state and lifecycle considerations (PaymentIntent and settlement detection). Reference: [Stripe x402 payments](https://docs.stripe.com/payments/machine/x402)

## Network And Token Support (V2)

In x402 V2, network identifiers use **CAIP-2** (`namespace:reference`) to avoid cross-chain ambiguity.

Important note for EVM:

- Tokens should support **EIP-3009** (`transferWithAuthorization`) so payments can be authorized via signatures and gas can be sponsored by facilitators.

## References (Official)

- Settlement options: [x402 whitepaper PDF](https://www.x402.org/x402-whitepaper.pdf)
- Networks and tokens (CAIP-2, EIP-3009): [x402 Networks & Token Support](https://docs.x402.org/core-concepts/network-and-token-support)
