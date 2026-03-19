# x402 - Bazaar Discovery

## Purpose

Bazaar is the discovery layer for the x402 ecosystem. It provides a machine-readable catalog for:

- paid HTTP endpoints
- paid MCP tools

The goal is to reduce discovery friction so an agent can find services, select based on price and schema, pay, and execute without manual integration.

## Architecture Model

- A facilitator that supports Bazaar typically exposes an endpoint such as `/discovery/resources`
- A service becomes discoverable when the Bazaar extension is enabled on a route and sufficient metadata is provided, especially input and output schemas

## Data That Matters For Agents

A strong listing includes:

- **resource** (URL)
- **type** (`http` or `mcp`)
- **accepts[]** (scheme, asset, maxAmountRequired, network, payTo, mimeType)
- **schemas** for inputs and outputs so requests can be constructed correctly

## Risks And Guardrails

- Listings act as a supply chain for agents, agent-side policy is required:
  - allowlist facilitator URLs
  - max spend per call and per period
  - validate network and asset constraints
- Ambiguous metadata increases agent error rate, schema and parameter descriptions must be concise and explicit

## References (Official)

- Bazaar documentation: [x402 Bazaar](https://docs.x402.org/extensions/bazaar)
