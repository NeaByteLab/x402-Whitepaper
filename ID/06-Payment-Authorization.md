# x402 - Payment Authorization

## Tujuan

Authorization adalah kredensial yang membuktikan:

- klien menyetujui pembayaran untuk challenge tertentu
- nilai pembayaran tidak melebihi batas (`maxAmountRequired`)
- signature berasal dari wallet yang relevan

Dalam wire format x402 V2, kredensial ini dikirim sebagai header **`PAYMENT-SIGNATURE`** (client → server).

## Isi Authorization (Menurut Whitepaper)

Whitepaper menyebut pesan yang ditandatangani memuat:

- seluruh field payment request
- **actual amount** yang dibayar (harus \( \le \) `maxAmountRequired`)
- timestamp
- signature kriptografis dari wallet pembayar

Whitepaper menyebut signature mengikuti **EIP-712**.

## Canonical V2 Binding (Yang Harus Terikat)

Untuk implementasi V2 modern, sumber field yang harus dibinding adalah `PAYMENT-REQUIRED` khususnya `resource` dan `accepts[]`. Mapping istilah canonical V2 ada di: [20 - Canonical Fields](./20-Canonical-Fields.md)

Minimal, kredensial yang ditandatangani harus mengikat:

- **resource**: `resource.url` (minimal path + query yang relevan)
- **network**: `accepts[].network` (CAIP-2)
- **asset**: `accepts[].asset`
- **recipient**: `accepts[].payTo`
- **amount**: amount yang dibayar dan batas maksimumnya (jika modelnya “max + actual”)
- **freshness**: timestamp + expiry window (atau `expiresAt`)
- **anti-replay**: `nonce` dan atau `paymentId` (atau payment identifier extension)

Jika salah satu binding di atas tidak ada, maka “tamper lalu replay” lintas endpoint atau lintas recipient menjadi lebih mudah.

## Makna EIP-712 (Praktis)

- Mengurangi ambiguity dalam signing (dibanding sign string bebas)
- Membantu wallet UI menampilkan detail “yang disetujui” dengan jelas (domain, amount, asset, resource)

## Checklist Verifikasi Server (Praktis)

Gunakan checklist ini saat memproses request retry yang membawa `PAYMENT-SIGNATURE`.

- **Decode** payload `PAYMENT-SIGNATURE` dan ekstrak:
  - siapa payer-nya (address/signing key)
  - field binding (resource, network, asset, payTo, amount, expiry, nonce/paymentId)
  - signature / proof yang dibutuhkan mekanisme
- **MUST** cek resource binding:
  - resource di payload harus cocok dengan resource yang diminta sekarang
- **MUST** cek destination binding:
  - `payTo` harus sama persis dengan yang di-offer
- **MUST** cek economic binding:
  - asset harus sama
  - amount harus sesuai aturan yang dipakai (exact atau \( \le \) max)
- **MUST** cek freshness:
  - belum expired
  - timestamp masuk window yang wajar
- **MUST** cegah replay:
  - tolak nonce/paymentId yang sudah pernah dipakai (atau gunakan idempotency extension)
- **SHOULD** catat receipt:
  - simpan `PAYMENT-RESPONSE` (receipt) untuk audit trail dan dispute

## Wallet As Identity (Implikasi)

Dokumentasi x402 memposisikan wallet sebagai:

- mekanisme pembayaran
- identitas unik buyer/seller (address digunakan sebagai identifier dalam protokol)

## Failure Modes Yang Wajib Diuji

- Signature valid, tapi field yang terikat tidak lengkap → **tampering** mungkin lolos
- Authorization dipakai ulang → **replay** (tanpa nonce store)
- Timestamp/expiry diabaikan → credential hidup terlalu lama

## Referensi (Official)

- Payment Authorization + EIP-712: [x402 whitepaper PDF](https://www.x402.org/x402-whitepaper.pdf)
- Stripe x402 (402 → pay → retry + authorization): [Stripe x402 payments](https://docs.stripe.com/payments/machine/x402)
- Wallet role: [x402 Wallet](https://docs.x402.org/core-concepts/wallet)
