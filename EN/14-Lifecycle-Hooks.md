# x402 - Lifecycle Hooks

## Purpose

Lifecycle hooks provide control points to intercept critical payment events. Common uses:

- observability, audit logs, and analytics
- custom access control
- recovery from transient failures
- policy enforcement, for example spending limits on the client

## Server Hooks (Resource Server)

Common hooks:

- **On Before Verify** - early validation and policy gates, can abort with a reason
- **On After Verify** - logging and metering after verification
- **On Verify Failure** - recovery path for verification failures
- **On Before Settle** - policy gate before settlement
- **On After Settle** - record payment and update entitlements after successful settlement
- **On Settle Failure** - recovery path for settlement failures

## Client Hooks

Hooks that matter for security and UX:

- **On Before Payment Creation** - enforce spend limits, allowlists, and approval thresholds
- **On After Payment Creation** - logging and receipt capture
- **On Payment Creation Failure** - fallback strategies
- HTTP hook **On Payment Required** - attempt alternate headers before paying, for example API key auth

## Facilitator Hooks

Facilitator hooks are useful for:

- compliance checks
- cross-service metrics
- bazaar catalog population after verification

## Safe Defaults

- Default behavior should fail closed for paid routes, no access is granted without verify and settle
- Recovery that changes results should be auditable, especially when a recovered result is returned
- Client spending limits should be treated as baseline for agents, not an optional add-on

## References (Official)

- Lifecycle hooks: [x402 Lifecycle Hooks](https://docs.x402.org/advanced-concepts/lifecycle-hooks)
