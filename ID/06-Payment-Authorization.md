# x402 - Payment Authorization

## Tujuan

Authorization adalah kredensial yang membuktikan:

- klien menyetujui pembayaran untuk challenge tertentu
- nilai pembayaran tidak melebihi batas (`maxAmountRequired`)
- signature berasal dari wallet yang relevan

## Isi Authorization (Menurut Whitepaper)

Whitepaper menyebut pesan yang ditandatangani memuat:

- seluruh field payment request
- **actual amount** yang dibayar (harus \( \le \) `maxAmountRequired`)
- timestamp
- signature kriptografis dari wallet pembayar

Whitepaper menyebut signature mengikuti **EIP-712**.

## Makna EIP-712 (Praktis)

- Mengurangi ambiguity dalam signing (dibanding sign string bebas)
- Membantu wallet UI menampilkan detail “yang disetujui” dengan jelas (domain, amount, asset, resource)

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
