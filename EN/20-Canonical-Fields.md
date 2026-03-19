# x402 - Canonical Fields And Raw HTTP Examples

## Purpose

This document standardizes x402 V2 field terminology and provides raw HTTP examples so non-SDK implementations stay safe and unambiguous.

## Canonical Field Dictionary (x402 V2)

The same concept appears across documents under different names. For V2, use this mapping as the reference.

| Concept     | Canonical (V2)                                   | Common Aliases                             | Practical Notes                                                       |
| ----------- | ------------------------------------------------ | ------------------------------------------ | --------------------------------------------------------------------- |
| Network     | `accepts[].network`                              | `network`                                  | CAIP-2 format, example `eip155:84532`                                 |
| Scheme      | `accepts[].scheme`                               | -                                          | Example `exact`                                                       |
| Price       | `accepts[].price`                                | `maxAmountRequired`                        | `price` is often a fiat string, facilitator maps it to token `amount` |
| Amount      | `accepts[].amount`                               | `maxAmountRequired`                        | Smallest token unit, USDC uses 6 decimals                             |
| Asset       | `accepts[].asset`                                | `assetAddress`, `assetType`                | Typically a token contract address                                    |
| Recipient   | `accepts[].payTo`                                | `paymentAddress`, `paymentAddress/payTo`   | Value transfer destination                                            |
| Resource    | `resource.url`                                   | `resource`, `url`                          | Must be bound to payment to prevent cross-endpoint replay             |
| Expiry      | `accepts[].maxTimeoutSeconds` and/or `expiresAt` | `expiresAt`                                | Expiry models vary, but credentials must have a freshness window      |
| Anti-Replay | `nonce` and/or `paymentId`                       | `nonce`, `paymentId`, `payment identifier` | Add idempotency for real-world retries                                |

## Header Wire Format (x402 V2)

- `PAYMENT-REQUIRED` - server to client on `402`, carries the offer and `accepts[]`
- `PAYMENT-SIGNATURE` - client to server on retry, carries the payment credential
- `PAYMENT-RESPONSE` - server to client on `200`, carries the settlement receipt

All three headers are Base64-encoded JSON.

## Raw HTTP Example (402 Challenge → Paid Retry)

This example is derived from a simple lab flow and simplified to focus on wire format.

### 1) Request Without Payment

```text
GET /paid HTTP/1.1
Host: api.example.local
```

### 2) Server Responds With 402 + PAYMENT-REQUIRED

```text
HTTP/1.1 402 Payment Required
PAYMENT-REQUIRED: <base64-json>
Content-Type: application/json

{}
```

Example decoded `PAYMENT-REQUIRED` JSON:

```json
{
  "x402Version": 2,
  "resource": {
    "url": "http://localhost:4022/paid",
    "description": "Real x402 paid endpoint (Base Sepolia)",
    "mimeType": "application/json"
  },
  "accepts": [
    {
      "scheme": "exact",
      "network": "eip155:84532",
      "amount": "10000",
      "asset": "0x036CbD...CF7e",
      "payTo": "0xA7c3...1c71",
      "maxTimeoutSeconds": 300,
      "extra": {
        "name": "USDC",
        "version": "2"
      }
    }
  ]
}
```

### 3) Client Retries With PAYMENT-SIGNATURE

```text
GET /paid HTTP/1.1
Host: api.example.local
PAYMENT-SIGNATURE: <base64-json>
```

### 4) Server Responds With 200 + PAYMENT-RESPONSE (Receipt)

```text
HTTP/1.1 200 OK
PAYMENT-RESPONSE: <base64-json>
Content-Type: application/json

{"data":"paid-content-real","receipt":{...}}
```

Example decoded `PAYMENT-RESPONSE` JSON:

```json
{
  "success": true,
  "transaction": "0xb31c...ab8e",
  "network": "eip155:84532",
  "payer": "0xA7c3...1c71"
}
```

## Implementation Notes (Common Pitfalls)

- **`PAYMENT-SIGNATURE` does not appear in responses**: it is a request header (client → server)
- **The receipt lives in `PAYMENT-RESPONSE`**: servers can log it and optionally propagate it into the response body
- **Retries are the default**: add idempotency for endpoints with side effects

## References (Official)

- x402 whitepaper: [x402 whitepaper PDF](https://www.x402.org/x402-whitepaper.pdf)
- HTTP 402 and V2 headers: [x402 HTTP 402](https://docs.x402.org/core-concepts/http-402)
- Quickstart for buyers: [Quickstart For Buyers](https://docs.x402.org/getting-started/quickstart-for-buyers)
