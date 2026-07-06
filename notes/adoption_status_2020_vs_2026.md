# Adoption Status: 2020 vs 2026 Comparison

> Prepared for Issue #72. Baseline ("2020") reconstructed from the draft's
> pre-2026 §Adoption Status content (last substantively updated around the
> [2020-03-27 lepidum.co.jp post](https://lepidum.co.jp/blog/2020-03-27/ietf-draft-pfc/),
> now dead). Current ("2026") reflects the content merged via PR #83
> (2026-07-05), based on ISO/IEC 15946-5:2022 and a fresh library/application survey.

## 1. Continued Adoption

Curves and libraries present in both the 2020 and 2026 surveys, confirming the "widely used" criterion has held over time.

| Category | Item | 2020 | 2026 |
|---|---|---|---|
| Standard | ISO/IEC 15946-5 | 2nd edition (2017) | **3rd edition (2022)** — BN462 (Annex D.2.3) and BLS48-581 (Annex D.3.5) now confirmed as *exact* numerical-example matches |
| Standard | TCG / FIDO / W3C (BN256I, BN512I, BN638) | listed | unchanged |
| Library | mcl | BN254N, BN_SNARK1, BN382M, BN462, BLS12-381 | unchanged (still the reference implementation for BN462) |
| Library | RELIC | 6 BN + 6 BLS curves incl. BN446, BLS48-575 | unchanged |
| Library | AMCL / MIRACL | separate listings | **merged**: "MIRACL Core" now covers both, plus BLS48-581 (not in 2020 AMCL) |
| Library | PBC, TEPLA, Intel-IPP, Adjoint, bls12377js | listed | unchanged |
| Application | Zcash (BLS12-381) | "experimental implementation" | production (wording updated, no functional change) |
| Application | Chia (via RELIC) | listed | unchanged |
| Application | DFINITY (via mcl, BN462) | listed | unchanged — **still the only identified BN462 application deployment** |
| Application | Algorand | listed | unchanged |

## 2. New Adoption (since 2020)

| Category | Item | Notes |
|---|---|---|
| Library | **blst** | Supranational; commissioned by the Ethereum Foundation; now the primary BLS12-381 library for Ethereum consensus clients and Filecoin |
| Library | **gnark-crypto** | Consensys; BLS12-381, BN254, BLS12-377, BLS24-315, BW6-761 |
| Library | **noble-curves** | Paul Miller; JS/TS, BLS12-381 — widely used in Ethereum JS tooling |
| Library | **arkworks** | Rust ecosystem for zkSNARKs; BLS12-381, BN254 |
| Library | **constantine** | Nim; BLS12-381, BN254, BLS12-377, BW6-761 |
| Library | **CIRCL** | Cloudflare; BLS12-381 |
| Library | **zkcrypto** | Rust; BLS12-381 |
| Library | **herumi/bls** *(this survey)* | ETH2-mode wrapper over mcl (`blsSetETHmode`), used by some Ethereum consensus client implementations alongside blst |
| Application | **Filecoin** | BLS12-381 via blst |
| Application | **EIP-2537** | On-chain BLS12-381 precompile contract, enabling pairing operations directly in the EVM |

## 3. Migration Trends

- **Ethereum's BLS12-381 story consolidated.** In 2020, the draft cited a single third-party Go implementation ("pureGo-bls" by Meyer) as *the* Ethereum 2.0 reference. By 2026, this has been superseded by an ecosystem of audited, production-grade libraries (blst as the Ethereum Foundation–commissioned primary, plus gnark-crypto, noble-curves, arkworks, constantine used across different client teams and tooling), and BLS12-381 pairing operations are now available on-chain via EIP-2537. This reflects BLS12-381's transition from "adopted but thinly implemented" to "the de facto standard curve with a mature, multi-implementation library ecosystem."
- **BN462 adoption has not grown.** Despite being one of the draft's two recommended 128-bit curves since its earliest versions, no new production deployment beyond DFINITY was identified in this round of research (2026), matching the 2020 baseline. Its library support did grow marginally (MIRACL Core consolidation), but application-level adoption is flat. This is a relevant data point for the "widely used" criterion discussion in #76.
- **No BN254-family migration to BLS12-381 was identified as a *retirement* story within this draft's scope** — the draft has never recommended BN254 (it recommends BN462), so this specific migration (well documented elsewhere, e.g., Ethereum's own move away from alt_bn128/BN254 for new applications) is adjacent context rather than a direct before/after change in this survey.

## Outstanding items for further research (optional, not blocking)

- Confirm whether other Ethereum consensus clients beyond those already known use herumi/bls specifically (vs. blst) — low priority, doesn't change the summary conclusion.
- No further BN462 deployment evidence was found; if any team is aware of undocumented BN462 usage, it would be a valuable addition ahead of RGLC.
