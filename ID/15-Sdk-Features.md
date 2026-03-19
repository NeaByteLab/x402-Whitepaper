# x402 - SDK Features

## Tujuan

Dokumen ini memetakan dukungan fitur antar SDK agar keputusan implementasi tidak salah, terutama untuk:

- pilihan bahasa untuk server dan client
- pilihan network dan mekanisme pembayaran
- pilihan extension yang benar-benar tersedia

## Ringkasan Yang Paling Penting

- Core (server, client, facilitator) tersedia di TypeScript, Go, dan Python
- Network support berbeda, khususnya untuk Stellar dan Aptos yang belum merata
- Extension support tidak seragam, contoh offer-receipt dan sign-in-with-x tidak tersedia di semua SDK

## Implikasi Untuk Developer Experience

- Jika target fitur butuh extension tertentu, SDK support harus dicek lebih dulu
- Jika target multi-network, pilihan runtime dan mekanisme harus sesuai
- Jika target HTTP framework spesifik, integrasi SDK perlu dipastikan tersedia

## Referensi (Official)

- SDK feature parity: [x402 SDK Features](https://docs.x402.org/sdk-features)
