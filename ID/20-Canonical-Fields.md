# x402 - Canonical Fields And Raw HTTP Examples

## Tujuan

Dokumen ini menyamakan istilah field x402 V2 dan memberi contoh HTTP mentah agar implementasi tanpa SDK tetap aman dan tidak ambigu.

## Canonical Field Dictionary (x402 V2)

Konsep di bawah ini disebut dalam beberapa dokumen dengan nama yang berbeda. Untuk V2, gunakan mapping ini sebagai acuan.

| Konsep      | Canonical (V2)                                     | Alias Yang Sering Muncul                   | Catatan Praktis                                                                 |
| ----------- | -------------------------------------------------- | ------------------------------------------ | ------------------------------------------------------------------------------- |
| Network     | `accepts[].network`                                | `network`                                  | Format CAIP-2, contoh `eip155:84532`                                            |
| Skema       | `accepts[].scheme`                                 | -                                          | Contoh `exact`                                                                  |
| Harga       | `accepts[].price`                                  | `maxAmountRequired`                        | `price` biasanya berbentuk fiat string, facilitator memetakan ke `amount` token |
| Amount      | `accepts[].amount`                                 | `maxAmountRequired`                        | Nilai token terkecil, contoh USDC 6 decimals                                    |
| Asset       | `accepts[].asset`                                  | `assetAddress`, `assetType`                | Biasanya kontrak token, contoh USDC on-chain                                    |
| Penerima    | `accepts[].payTo`                                  | `paymentAddress`, `paymentAddress/payTo`   | Ini tujuan value transfer                                                       |
| Resource    | `resource.url`                                     | `resource`, `url`                          | Harus dibinding ke pembayaran untuk cegah replay lintas endpoint                |
| Expiry      | `accepts[].maxTimeoutSeconds` dan atau `expiresAt` | `expiresAt`                                | Model expiry bisa beda, prinsipnya credential harus punya freshness window      |
| Anti-Replay | `nonce` dan atau `paymentId`                       | `nonce`, `paymentId`, `payment identifier` | Disarankan tambah idempotency untuk retry reality                               |

## Header Wire Format (x402 V2)

- `PAYMENT-REQUIRED` - server ke client saat `402`, berisi offer dan `accepts[]`
- `PAYMENT-SIGNATURE` - client ke server saat retry, berisi credential pembayaran
- `PAYMENT-RESPONSE` - server ke client saat `200`, berisi resi settlement

Semua header di atas berupa Base64-encoded JSON.

## Raw HTTP Example (402 Challenge → Paid Retry)

Contoh ini diambil dari flow lab sederhana, bentuknya disederhanakan agar fokus ke wire format.

### 1) Request Tanpa Pembayaran

```text
GET /paid HTTP/1.1
Host: api.example.local
```

### 2) Server Menjawab 402 + PAYMENT-REQUIRED

```text
HTTP/1.1 402 Payment Required
PAYMENT-REQUIRED: <base64-json>
Content-Type: application/json

{}
```

Contoh JSON hasil decode `PAYMENT-REQUIRED`:

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

### 3) Client Retry Dengan PAYMENT-SIGNATURE

```text
GET /paid HTTP/1.1
Host: api.example.local
PAYMENT-SIGNATURE: <base64-json>
```

### 4) Server Menjawab 200 + PAYMENT-RESPONSE (Receipt)

```text
HTTP/1.1 200 OK
PAYMENT-RESPONSE: <base64-json>
Content-Type: application/json

{"data":"paid-content-real","receipt":{...}}
```

Contoh JSON hasil decode `PAYMENT-RESPONSE`:

```json
{
  "success": true,
  "transaction": "0xb31c...ab8e",
  "network": "eip155:84532",
  "payer": "0xA7c3...1c71"
}
```

## Implementation Notes (Yang Sering Kejadian)

- **`PAYMENT-SIGNATURE` tidak muncul di response**: itu header request dari client ke server
- **Receipt ada di `PAYMENT-RESPONSE`**: server bisa log dan juga propagate ke body bila dibutuhkan
- **Retry reality itu default**: idempotency perlu untuk endpoint yang punya side effect

## Referensi (Official)

- x402 whitepaper: [x402 whitepaper PDF](https://www.x402.org/x402-whitepaper.pdf)
- HTTP 402 dan header V2: [x402 HTTP 402](https://docs.x402.org/core-concepts/http-402)
- Quickstart buyer: [Quickstart For Buyers](https://docs.x402.org/getting-started/quickstart-for-buyers)
