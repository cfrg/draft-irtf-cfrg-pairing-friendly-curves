# Adoption Status of Pairing-Friendly Curves
> Updated: 2026-04-21 (PR #80 / Issue #80)

## Survey Methodology

All major pairing-based cryptography libraries were surveyed. Libraries were identified by:
1. Known major implementations (blst, mcl, RELIC, MIRACL Core, AMCL, gnark-crypto, noble-curves, arkworks, constantine, CIRCL, zkcrypto, PBC)
2. GitHub searches using "optimal ate pairing", "miller loop", "pairing-friendly curves", "bilinear pairing elliptic curve"
   - Bluetooth and Wi-Fi "pairing" results were filtered by checking for bilinearity, G1/G2/GT, and Miller loop keywords

For each library, the supported curves were identified from documentation and source code. Curves in the draft (BLS12-381, BN462, BLS48-581) are marked in **bold**.

Security levels follow [BD18], [GMT19], [MAF19], [FK18].

---

## International Standards

| Standard | Edition | Curves relevant to this draft |
|---|---|---|
| ISO/IEC 15946-5 | 2022 (3rd ed.) | **BN462** (Annex D.2.3; u = 2^114 + 2^101 − 2^14 − 1, exact match); **BLS48-581** (Annex D.3.5; u = −2^32 − 2^30 − 2^10 + 2^7 − 1, exact match); BN446 (Annex D.2.2; pre-exTNFS legacy; same edition introduces post-exTNFS guideline \|p\| ≥ 461 bits for BN/BLS12) |
| ISO/IEC 15946-5 | 2017 (2nd ed.) | BN384, BN512I (pre-exTNFS; superseded by 2022 edition) |
| TCG TPM 2.0 | — | BN512I, BN638 |
| FIDO Alliance | — | BN512I, BN638 |
| W3C WebAuthn | — | BN512I, BN638 |

---

## Cryptographic Libraries

| Library | Maintainer | Status | Curves ≥ 128-bit (pairing-friendly) |
|---|---|---|---|
| **blst** | Supranational | Active | **BLS12-381** |
| **mcl** | Herumi (Shigeo Mitsunari) | Active | **BLS12-381**, **BN462**, BN254N, BN382M, BN_SNARK1 |
| **Kyushu Univ. BLS48** | Kyushu University | Archived (reference impl) | **BLS48-581** |
| **gnark-crypto** | Consensys | Active | **BLS12-381**, BN254, BLS12-377, BLS24-315, BW6-761, BW6-633 |
| **noble-curves** | Paul Miller | Active | **BLS12-381**, BN254 |
| **arkworks** | arkworks-rs contributors | Active | **BLS12-381**, BN254, BLS12-377, BLS24-315, BW6-761, MNT4-298, MNT6-298 |
| **constantine** | Mamy Ratsimbazafy | Active | **BLS12-381**, BN254, BLS12-377, BW6-761, BLS24-317 |
| **CIRCL** | Cloudflare | Active | **BLS12-381** |
| **zkcrypto** | zkcrypto contributors | Active | **BLS12-381** |
| **RELIC** | Diego Aranha et al. | Active | **BLS12-381**, BLS12-446, BLS12-455, BLS12-638, BLS24-477, **BLS48-575**, BN382R, BN446, BN638, CP8-544, K54-569, KSS18-508, OT8-511 |
| **MIRACL Core** | MIRACL Ltd. | Active | **BLS12-381**, **BLS48-581**, **BN462**, BLS12-383, BLS12-461, BLS24-479, BLS48-556, BN254N, BN254CX, BN256I, BN512I |
| **Adjoint** | Adjoint Inc. | Stale | **BLS12-381**, **BN462**, BN254N, BN254B, BN254S1, BN254S2, BN_SNARK1 |
| **PBC** | Stanford University | Legacy | BN (generic), MNT, Freeman, supersingular |
| **TEPLA** | University of Tsukuba | Legacy | BN254N, BN254B |
| **Intel-IPP** | Intel | Active | BN256I |
| **bls12377js** | Celo Foundation | Stale | BLS12-377 |

---

## Applications

| Application | Organization | Curve |
|---|---|---|
| Ethereum (consensus / EIP-2537) | Ethereum Foundation | **BLS12-381** |
| Zcash (Sapling/Orchard) | Zcash Foundation | **BLS12-381** |
| Chia Network | Chia Network Inc. | **BLS12-381** |
| DFINITY / Internet Computer | DFINITY Foundation | **BLS12-381**, BN382M, **BN462** |
| Algorand | Algorand Foundation | **BLS12-381** |
| Filecoin | Protocol Labs | **BLS12-381** (via blst) |

---

## Summary: Draft Curves vs. Survey

| Curve | ISO/IEC 15946-5:2022 | # Libraries | # Applications |
|---|---|---|---|
| **BLS12-381** | BLS12 examples added (2022 ed.) | 12+ (blst, mcl, RELIC, MIRACL Core, gnark-crypto, noble-curves, arkworks, constantine, CIRCL, zkcrypto, Adjoint, …) | 6+ (Ethereum, Zcash, Chia, DFINITY, Algorand, Filecoin) |
| **BN462** | Annex D.2.3 (exact u-value match) | 3 (mcl, MIRACL Core, Adjoint) | 1 (DFINITY) |
| **BLS48-581** | Annex D.3.5 (exact u-value match) | 2 (Kyushu Univ., MIRACL Core) | — |
| BN446 (proposed) | Annex D.2.2 (pre-exTNFS legacy; same ed. sets \|p\|≥461 guideline) | 1 (RELIC) | — |
| BLS48-575 (proposed) | Not listed | 1 (RELIC) | — |
| BLS24-509 (proposed) | Not listed (ISO 2022 has BLS24 at 559-bit p, not 509) | 0 | — |
