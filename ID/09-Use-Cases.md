# x402 - Use Cases

## Use Cases Yang Disebut Di Whitepaper

- **Agents accessing APIs on demand** (per request)
- **Pay-per-use model inference** (per inference/unit)
- **Agents paying for cloud compute & storage** (per GPU-minute / per GB)
- **Context retrieval** (mis. premium news per artikel)
- **Micropayments untuk human content access** (per artikel/episode)

Referensi: [x402 whitepaper PDF](https://www.x402.org/x402-whitepaper.pdf)

## Mapping Use Case → Requirement Teknis

- **Per request (API/data)**:
  - binding `resource` harus ketat
  - idempotency wajib (retry network adalah default)
- **Per unit (compute/inference)**:
  - definisi unit harus jelas (1 request = 1 charge)
  - metering harus auditable
- **Per time (streaming/compute seconds)**:
  - settlement channels/batching lebih relevan
  - risiko metering abuse meningkat

## Stripe Perspective (Machine Payments)

- Stripe menyoroti model pay-per-use serendah 0.01 USDC, cocok untuk API request atau paywall data/konten. Referensi: [Stripe machine payments](https://docs.stripe.com/payments/machine)
