# Extracted Tables from draft-irtf-cfrg-pairing-friendly-curves-12

This directory contains extracted tables from the IETF draft specification on Pairing-Friendly Curves.

## Files

### 1. Adoption Status Table (Main)
- **table_adoption.xml** (410 lines) - Raw XML extraction with full formatting
- **table_adoption.md** (49 lines) - Markdown-formatted table for readability

**Content**: Comprehensive adoption status of pairing-friendly curves across standards, cryptographic libraries, and applications. Shows security level classifications (128-bit, 192-bit, 256-bit) for various curve types including BN, BLS, BLS24, BLS48, and others.

**Coverage**: 
- Standards: ISO/IEC, TCG, FIDO/W3C
- Libraries: mcl, RELIC, AMCL, Kyushu Univ., MIRACL, Adjoint, bls12377js, Intel IPP, TEPLA
- Applications: Zcash, Ethereum, Chia Network, DFINITY, Algorand

### 2. Legacy 100-bit Security Level Table
- **table_100bit.xml** (105 lines) - Raw XML extraction with full formatting
- **table_100bit.md** (28 lines) - Markdown-formatted table for readability

**Content**: Adoption status of pairing-friendly curves with 100-bit security level (legacy/deprecated). Shows which standards, libraries, and applications still support these lower security parameters like BN256I, BN254N, BN254CX, etc.

**Note**: These curves are below the 128-bit security threshold and are included for reference to help implementers understand which legacy curves are still in use.

## Security Context

- **Arnd 128**: Around 128-bit security (±5 bits)
- **~**: Between security levels
- **Arnd 192**: Around 192-bit security (±5 bits)
- **Arnd 256**: Around 256-bit security (±5 bits)

## Source

Extracted from: draft-irtf-cfrg-pairing-friendly-curves-12.xml
Generated: 2026-03-17

## Usage

The Markdown files (.md) are suitable for:
- Quick reference and documentation
- Integration into notes and technical documentation
- Human-readable comparisons

The XML files (.xml) are suitable for:
- Programmatic processing
- Round-trip conversion to other formats
- Preservation of RFC XML formatting
