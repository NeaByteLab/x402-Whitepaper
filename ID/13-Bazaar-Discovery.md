# x402 - Bazaar Discovery

## Tujuan

Bazaar adalah discovery layer untuk ekosistem x402, bentuknya katalog machine-readable untuk:

- endpoint HTTP berbayar
- MCP tools berbayar

Tujuannya adalah mengurangi friction discovery, sehingga agent dapat mencari layanan, memilih berdasarkan harga dan schema, lalu membayar dan mengeksekusi tanpa integrasi manual.

## Model Arsitektur

- Listing biasanya disediakan oleh facilitator yang mendukung Bazaar melalui endpoint seperti `/discovery/resources`
- Service menjadi discoverable jika route mengaktifkan extension Bazaar dan menyertakan metadata yang cukup, terutama schema input dan output

## Data Yang Penting Untuk Agent

Listing yang kuat perlu memuat:

- **resource** (URL)
- **type** (`http` atau `mcp`)
- **accepts[]** (scheme, asset, maxAmountRequired, network, payTo, mimeType)
- **outputSchema** dan input schema untuk membantu agent menyusun request yang valid

## Risiko Dan Guardrails

- Listing adalah jalur supply chain untuk agent, sehingga butuh policy di sisi agent:
  - allowlist facilitator URL
  - max spend per call dan per periode
  - validation untuk network dan asset yang diterima
- Metadata yang ambigu meningkatkan error rate agent, sehingga schema dan deskripsi parameter perlu ringkas dan jelas

## Referensi (Official)

- Bazaar documentation: [x402 Bazaar](https://docs.x402.org/extensions/bazaar)
