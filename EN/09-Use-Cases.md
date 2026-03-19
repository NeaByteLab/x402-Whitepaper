# x402 - Use Cases

## Use Cases Mentioned In The Whitepaper

- **Agents accessing APIs on demand** (per request)
- **Pay-per-use model inference** (per inference or unit)
- **Agents paying for cloud compute and storage** (per GPU-minute, per GB)
- **Context retrieval** (for example, premium news per article)
- **Micropayments for human content access** (per article or episode)

Reference: [x402 whitepaper PDF](https://www.x402.org/x402-whitepaper.pdf)

## Mapping Use Case To Technical Requirements

- **Per request (API and data)**:
  - strict `resource` binding
  - idempotency is mandatory, network retries are default behavior
- **Per unit (compute or inference)**:
  - the unit definition must be explicit (one request equals one charge)
  - metering must be auditable
- **Per time (streaming or compute seconds)**:
  - channels and batching become more relevant
  - metering abuse risk increases

## Stripe Perspective (Machine Payments)

- Stripe highlights pay-per-use models as low as 0.01 USDC, suitable for API requests and paywalled data or content. Reference: [Stripe machine payments](https://docs.stripe.com/payments/machine)
