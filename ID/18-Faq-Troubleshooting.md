# x402 - FAQ And Troubleshooting

## Tujuan

Dokumen ini merangkum jawaban praktis untuk pertanyaan umum dan failure mode yang paling sering muncul saat integrasi.

## Konsep Dasar

- x402 adalah protokol open-source, bukan produk vendor tunggal
- Flow utama: request, `402`, sign payload, retry, verify, settle, response

## Troubleshooting Paling Umum

### Tetap `402` Walau Sudah Kirim `PAYMENT-SIGNATURE`

Penyebab yang sering:

- saldo token tidak cukup atau address terkena policy checks
- amount tidak match, scheme `exact` menuntut equality ketat
- signature invalid, mis. chain id atau payload field tidak sesuai

### Testnet Berhasil, Mainnet Gagal

Penyebab yang sering:

- gas fee mainnet lebih tinggi, wallet perlu native token untuk gas bila tidak disponsori
- token mainnet belum ada, atau network identifier salah

## Refund Model (Konsekuensi)

- Scheme `exact` adalah push payment dan umumnya irreversible
- Refund biasanya dilakukan via business logic, yaitu seller mengirim transfer baru kembali ke buyer

## Facilitator Trust Boundary

- Payment payload ditandatangani buyer dan settle onchain
- Facilitator tidak memegang dana dan tidak bisa mengubah transaksi tanpa invalidating signature

## Referensi (Official)

- FAQ: [x402 FAQ](https://docs.x402.org/faq)
- Whitepaper: [x402 Whitepaper PDF](https://www.x402.org/x402-whitepaper.pdf)
