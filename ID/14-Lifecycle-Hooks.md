# x402 - Lifecycle Hooks

## Tujuan

Lifecycle hooks memberi kontrol untuk mengintersep event penting pada payment flow, ini berguna untuk:

- observability, audit log, dan analytics
- custom access control
- recovery dari kegagalan transient
- policy enforcement, mis. spending limit pada client

## Server Hooks (Resource Server)

Hook yang umum digunakan:

- **On Before Verify** - validasi awal, dapat abort dengan reason
- **On After Verify** - logging dan metering setelah verifikasi
- **On Verify Failure** - recovery path bila verifikasi gagal
- **On Before Settle** - policy gate sebelum settlement
- **On After Settle** - record payment dan update entitlement setelah settlement sukses
- **On Settle Failure** - recovery path bila settlement gagal

## Client Hooks

Hook yang relevan untuk security dan UX:

- **On Before Payment Creation** - enforce spending limits, allowlist, atau approval threshold
- **On After Payment Creation** - logging atau receipt capture
- **On Payment Creation Failure** - fallback strategy
- HTTP hook **On Payment Required** - mencoba header alternatif sebelum bayar, mis. API key auth

## Facilitator Hooks

Hook facilitator berguna untuk:

- compliance checks
- metrics lintas service
- populasi bazaar catalog setelah verifikasi

## Safe Defaults

- Default behavior harus fail-closed untuk route berbayar, yaitu tidak ada grant access tanpa verifikasi dan settlement
- Recovery yang mengubah hasil perlu audit log, terutama jika `recovered: true` dipakai
- Client spending limit adalah baseline untuk agent, bukan fitur opsional

## Referensi (Official)

- Lifecycle hooks: [x402 Lifecycle Hooks](https://docs.x402.org/advanced-concepts/lifecycle-hooks)
