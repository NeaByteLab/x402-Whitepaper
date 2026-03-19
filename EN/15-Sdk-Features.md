# x402 - SDK Features

## Purpose

This document maps feature support across SDKs so implementation choices do not break later:

- which language is suitable for server and client
- which networks and mechanisms are supported
- which extensions are actually available per SDK

## Key Summary

- Core components (server, client, facilitator) exist in TypeScript, Go, and Python
- Network support differs, Stellar and Aptos are not uniformly supported
- Extension support is not uniform, offer-receipt and sign-in-with-x are not available everywhere

## Developer Experience Implications

- If a design depends on a specific extension, verify SDK support early
- Multi-network designs require compatible runtime and mechanisms
- HTTP framework integration should be checked per SDK

## References (Official)

- SDK feature parity: [x402 SDK Features](https://docs.x402.org/sdk-features)
