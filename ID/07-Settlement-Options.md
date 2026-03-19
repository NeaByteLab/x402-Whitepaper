# x402 - Settlement Options

## Tujuan

Settlement adalah proses “pembayaran benar-benar terjadi” (atau diverifikasi terjadi) sebelum resource diberikan.

## Opsi Settlement (Menurut Whitepaper)

- **On-chain settlement**: transaksi langsung di blockchain
- **Layer-2 settlement**: optimistic/ZK untuk biaya lebih rendah
- **Payment channels**: untuk micropayments frekuensi tinggi pada pasangan yang trusted
- **Batched settlements**: agregasi banyak micropayments dalam satu transaksi

## Pertanyaan Arsitektur Yang Wajib Dijawab

- **Finality policy**: kapan dianggap “paid”?
  - observed tx? confirmed? N confirmations?
- **Timeout policy**: batas waktu menunggu settlement sebelum request dianggap gagal
- **Consistency**: apa yang terjadi jika settlement sukses tapi response ke client gagal (client retry)?

## Stripe-Backed Settlement (Implikasi)

- Stripe x402: Stripe mengelola deposit addresses dan melakukan capture `PaymentIntent` saat dana settle on-chain. Ini menambah komponen stateful (PaymentIntent + settlement detection). Referensi: [Stripe x402 payments](https://docs.stripe.com/payments/machine/x402)

## Network & Token Support (V2)

Pada x402 V2, network identifier menggunakan **CAIP-2** (format `namespace:reference`) untuk menghindari ambiguitas lintas-chain.

Catatan penting untuk EVM:

- Token yang digunakan perlu mendukung **EIP-3009** (`transferWithAuthorization`) agar pembayaran bisa diotorisasi via signature dan dapat disponsori gas-nya oleh facilitator.

## Referensi (Official)

- Transaction settlement options: [x402 whitepaper PDF](https://www.x402.org/x402-whitepaper.pdf)
- Network & token support (CAIP-2, EIP-3009): [x402 Networks & Token Support](https://docs.x402.org/core-concepts/network-and-token-support)
