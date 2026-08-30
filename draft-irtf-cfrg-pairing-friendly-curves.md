---
title: "Pairing-Friendly Curves"
docname: draft-irtf-cfrg-pairing-friendly-curves-latest
category: info
ipr: trust200902
area: IRTF
workgroup: CFRG
keyword: Internet-Draft
submissiontype: IRTF

stand_alone: yes
pi: [toc, sortrefs, symrefs]

author:
  -
    ins: Y. Sakemi
    name: Yumi Sakemi
    role: editor
    org: GMO CONNECT Inc.
    email: sakemi-yumi@gmo-connect.jp
  -
    ins: S. Kanno
    name: Satoru Kanno
    org: GMO CONNECT Inc.
    email: kanno@gmo-connect.jp
  -
    ins: R. Wahby
    name: Riad S. Wahby
    org: Stanford University
    email: rsw@cs.stanford.edu

--- abstract

Pairing-based cryptography, a subfield of elliptic curve cryptography, has received attention due to its flexible and practical functionality. Pairings are special maps defined using elliptic curves and it can be applied to construct several cryptographic protocols such as identity-based encryption, attribute-based encryption, and so on. At CRYPTO 2016, Kim and Barbulescu proposed an efficient number field sieve algorithm named exTNFS for the discrete logarithm problem in a finite field. Several types of pairing-friendly curves such as Barreto-Naehrig curves are affected by the attack. In particular, a Barreto-Naehrig curve with a 254-bit characteristic was adopted by a lot of cryptographic libraries as a parameter of 128-bit security, however, it ensures no more than the 100-bit security level due to the effect of the attack. In this memo, we list the security levels of certain pairing-friendly curves, and motivate our choices of curves. First, we summarize the adoption status of pairing-friendly curves in standards, libraries and applications, and classify them in the 128-bit, 192-bit, and 256-bit security levels. Then, from the viewpoints of "security" and "widely used", we select the recommended pairing-friendly curves considering exTNFS.

--- middle

# Introduction  {#introduction}

## Pairing-based Cryptography  {#pairing-based-cryptography}

Elliptic curve cryptography is an important area in currently deployed cryptography. The cryptographic algorithms based on elliptic curve cryptography, such as the Elliptic Curve Digital Signature Algorithm (ECDSA), are widely used in many applications.

Pairing-based cryptography, a subfield of elliptic curve cryptography, has attracted much attention due to its flexible and practical functionality. Pairings are special maps defined using elliptic curves. Pairings are fundamental in the construction of several cryptographic algorithms and protocols such as identity-based encryption (IBE), attribute-based encryption (ABE), authenticated key exchange (AKE), short signatures, and so on. Several applications of pairing-based cryptography are currently in practical use.

As the importance of pairings grows, elliptic curves where pairings are efficiently computable are studied and the special curves called pairing-friendly curves are proposed.

## Applications of Pairing-based Cryptography  {#applications-of-pairing-based-cryptography}

Several applications using pairing-based cryptography have already been standardized and deployed. We list here some examples of applications available in the real world.

IETF published RFCs for pairing-based cryptography such as Identity-Based Cryptography {{RFC5091}}, Sakai-Kasahara Key Encryption (SAKKE) {{RFC6508}}, and Identity-Based Authenticated Key Exchange (IBAKE) {{RFC6539}}. SAKKE is applied to Multimedia Internet KEYing (MIKEY) {{RFC6509}} and used in 3GPP {{SAKKE}}.

Pairing-based key agreement protocols are standardized in ISO/IEC {{ISOIEC11770-3}}. In {{ISOIEC11770-3}}, a key agreement scheme by Joux {{Joux00}}, identity-based key agreement schemes by Smart-Chen-Cheng {{CCS07}} and Fujioka-Suzuki-Ustaoglu {{FSU10}} are specified.

MIRACL implements M-Pin, a multi-factor authentication protocol {{M-Pin}}. The M-Pin protocol includes a type of zero-knowledge proof, where pairings are used for its construction.

The Trusted Computing Group (TCG) specified the Elliptic Curve Direct Anonymous Attestation (ECDAA) in the specification of a Trusted Platform Module (TPM) {{TPM}}. ECDAA is a protocol for proving the attestation held by a TPM to a verifier without revealing the attestation held by that TPM. Pairings are used in the construction of ECDAA. FIDO Alliance {{FIDO}} and W3C {{W3C}} also published an ECDAA algorithm similar to TCG.

Intel introduced Intel Enhanced Privacy ID (EPID) that enables remote attestation of a hardware device while preserving the privacy of the device as part of the functionality of Intel Software Guard Extensions (SGX) {{EPID}}. They extended TPM ECDAA to realize such functionality. A pairing-based EPID was proposed {{BL10}} and distributed along with Intel SGX applications.

Zcash implemented their own zero-knowledge proof algorithm named Zero-Knowledge Succinct Non-Interactive Argument of Knowledge (zk-SNARKs) {{Zcash}}. zk-SNARKs are used for protecting the privacy of transactions of Zcash. They use pairings to construct zk-SNARKs.

Cloudflare introduced Geo Key Manager {{Cloudflare}} to restrict distribution of customers' private keys to a subset of their data centers. To achieve this functionality, ABE is used, and pairings take a role as a building block. In addition, Cloudflare published a new cryptographic library, the Cloudflare Interoperable, Reusable Cryptographic Library (CIRCL) {{CIRCL}} in 2019. They plan to include securely implemented subroutines for pairing computations on certain secure pairing-friendly curves in CIRCL.

Currently, Boneh-Lynn-Shacham (BLS) signature schemes are being standardized {{I-D.irtf-cfrg-bls-signature}} and utilized in several blockchain projects such as Ethereum {{Ethereum}}, Algorand {{Algorand}}, Chia Network {{Chia}}, and DFINITY {{DFINITY}}. The aggregation functionality of BLS signatures is effective for their applications of decentralization and scalability.

## Motivation and Contribution  {#goal}

At CRYPTO 2016, Kim and Barbulescu proposed an efficient number field sieve (NFS) algorithm for the discrete logarithm problem in a finite field GF(p^k) {{KB16}}. The attack improves the polynomial selection that is the first step in the number field sieve algorithm for discrete logarithms in GF(p^k). The idea is applicable when the embedding degree k is a composite that satisfies k = i * j with gcd(i, j) = 1 and i, j > 1. The basic idea is based on the equality GF(p^k) = (GF(p^i)^j) and one of the improvement for reducing the amount of cost for solving the discrete logarithm problem is using sub-field calculation. Several types of pairing-friendly curves such as Barreto-Naehrig curves (BN curves){{BN05}} and Barreto-Lynn-Scott curves (BLS curves){{BLS02}} are affected by the attack, since a pairing-friendly curve suitable for cryptographic applications requires that the discrete logarithm problem is sufficiently difficult. Please refer to {{KB16}} for detailed ideas and calculation algorithms of the attack. In particular, BN254, which is a BN curve with a 254-bit characteristic effective for pairing calculations, was adopted by a lot of cryptographic libraries as a parameter of the 128-bit security level, however, BN254 ensures no more than the 100-bit security level due to the effect of the attack, where the security levels described in this memo correspond to the security strength of NIST recommendation {{NIST}}.

To resolve this effect immediately, several research groups and implementers re-evaluated the security of pairing-friendly curves and they respectively proposed various curves that are secure against the attack {{BD18}} {{BLS12_381}}.

In this memo, we list the security levels of certain pairing-friendly curves, and motivate our choices of curves. First, we summarize the adoption status of pairing-friendly curves in international standards, libraries and applications, and classify them in the 128-bit, 192-bit, and 256-bit security levels. Then, from the viewpoints of "security" and "widely used", pairing-friendly curves corresponding to each security level are selected in accordance with the security evaluation by Barbulescu and Duquesne {{BD18}}.

As a result, we recommend the BLS curve with 381-bit characteristic of embedding degree 12 and the BN curve with the 462-bit characteristic for the 128-bit security level, and the BLS curves of embedding degree 48 with the 581-bit characteristic for the 256-bit security level. This memo shows their specific test vectors.

## Requirements Terminology  {#requirements-terminology}

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in BCP 14 {{RFC2119}} {{RFC8174}} when, and only when, they appear in all capitals, as shown here.


# Preliminaries  {#preliminaries}

## Elliptic Curves  {#elliptic-curve}

Let p be a prime number at least 5, and let q be a power of p. Let GF(q) be a finite field. The curve defined by the following equation E is called an elliptic curve:

~~~~~~~~~~
E : y^2 = x^3 + a * x + b,
~~~~~~~~~~

and a and b in GF(q) satisfy the discriminant inequality 4 * a^3 + 27 * b^2 != 0 mod q. This is called the Weierstrass normal form of an elliptic curve.

A solution (x,y) to the equation E can be thought of as a point on the corresponding curve. For a natural number k, we define the set of (GF(q^k))-rational points of E, denoted by E(GF(q^k)), to be the set of all solutions (x,y) in GF(q^k), together with a 'point at infinity' O_E, which is defined to lie on every vertical line passing through the curve E.

The set E(GF(q^k)) forms a group under a group law that can be defined geometrically as follows. For P and Q in E(GF(q^k)) define P + Q to be the reflection around the x-axis of the unique third point R of intersection of the straight line passing through P and Q with the curve E. If the straight line is tangent to E, we say that it passes through that point twice. The identity of this group is the point at infinity O_E. We also define scalar multiplication [K]P for a positive integer K as the point P added to itself (K-1) times. Here, [0]P becomes the point at infinity O_E and the relation [-K]P = -([K]P) is satisfied.

## Pairings  {#pairing}

A pairing is a bilinear map defined on two subgroups of rational points of an elliptic curve. Examples include the Weil pairing, the Tate pairing, the optimal Ate pairing {{Ver09}}, and so on. The optimal Ate pairing is considered to be the most efficient to compute and is the one that is most commonly used for practical implementation.

Let E be an elliptic curve defined over a prime field GF(p), and let r be a large prime dividing the order of E(GF(p)). Let k be the minimum integer for which r is a divisor of p^k - 1; this is called the embedding degree of E over GF(p). Let pi be the p-power Frobenius endomorphism, which maps a point (x, y) of E to (x^p, y^p), and let E(GF(p^k))[r] be the r-torsion subgroup of E(GF(p^k)). The pairing is defined on the two subgroups

~~~~~~~~~~
G_1 = E(GF(p^k))[r] intersect ker(pi - [1]) = E(GF(p))[r],
G_2 = E(GF(p^k))[r] intersect ker(pi - [p]),
~~~~~~~~~~

where ker(pi - [a]) denotes the set of points T with pi(T) = [a]T. For the curves considered in this document, both G_1 and G_2 have order r. G_2 is identified by the eigenvalue p, and not merely as an order r subgroup of E(GF(p^k)): that weaker description is satisfied by G_1 as well, and the pairing computed in {{comp_pairing}} is degenerate when both of its arguments are taken from G_1.

Let d be a divisor of k and E' be an elliptic curve defined over GF(p^(k/d)). If an isomorphism from E' to E(GF(p^k)) exists, then E' is called the twist of E. It can sometimes be convenient for efficiency to do the computations of G_2 in the twist E', and so consider G_2 to instead be a subgroup of E'. Let G_T be an order r subgroup of the multiplicative group (GF(p^k))^*; this exists by definition of k.

A pairing is defined as a bilinear map e: (G_1, G_2) -> G_T satisfying the following properties:

1. Bilinearity: for any S in G_1, T in G_2, and integers K and L, e([K]S, [L]T) = e(S, T)^{K * L}.
2. Non-degeneracy: for any T in G_2, e(S, T) = 1 if and only if S = O_E. Similarly, for any S in G_1, e(S, T) = 1 if and only if T = O_E.

In applications, it is also necessary that for any S in G_1 and T in G_2, this bilinear map is efficiently computable.

We define some of the terminology used in this memo as follows:

GF(p):
:   a finite field with characteristic p.

GF(p^k):
:   an extension field of degree k.

(GF(p))*:
:   the multiplicative group of GF(p).

(GF(p^k))*:
:   the multiplicative group of GF(p^k).

b:
:   the coefficient of the curve equation E: y^2 = x^3 + b.

O_E:
:   the point at infinity over an elliptic curve E.

E(GF(p^k)):
:   the group of GF(p^k)-rational points of E.

\#E(GF(p^k)):
:   the number of GF(p^k)-rational points of E.

r:
:   the order of G_1 and G_2.

E(GF(p^k))[r]:
:   the r-torsion subgroup of E(GF(p^k)), that is, the set of points T of E(GF(p^k)) with [r]T = O_E.

pi:
:   the p-power Frobenius endomorphism, which maps a point (x, y) of E to (x^p, y^p).

BP:
:   a point in G_1. (The 'base point' of a cyclic subgroup of G_1)

h:
:   the cofactor h = #E(GF(p)) / r, where gcd(h, r)=1.

## Barreto-Naehrig Curves  {#BNdef}

A BN curve {{BN05}} is a family of pairing-friendly curves proposed in 2005. A pairing over BN curves constructs optimal Ate pairings.

A BN curve is defined by elliptic curves E and E' parameterized by a well-chosen integer t. E is defined over GF(p), where p is a prime number and at least 5, and E(GF(p)) has a subgroup of prime order r. The characteristic p and the order r are parameterized by

~~~~~~~~~~
p = 36 * t^4 + 36 * t^3 + 24 * t^2 + 6 * t + 1
r = 36 * t^4 + 36 * t^3 + 18 * t^2 + 6 * t + 1
~~~~~~~~~~

for an integer t.

The elliptic curve E has an equation of the form E: y^2 = x^3 + b, where b is a primitive element of the multiplicative group (GF(p))^* of order (p - 1).

In the case of BN curves, we can use twists of the degree 6. If nu is an element that is neither a square nor a cube in an extension field GF(p^2), the twist E' of E is defined over an extension field GF(p^2) by the equation E': y^2 = x^3 + b' with b' = b / nu or b' = b * nu. BN curves are called D-type if b' = b / nu, and M-type if b' = b * nu. The embedding degree k is 12.

A pairing e is defined by taking G_1 as a subgroup of E(GF(p)) of order r, G_2 as a subgroup of E'(GF(p^2)), and G_T as an order r subgroup of the multiplicative group (GF(p^12))^*.

## Barreto-Lynn-Scott Curves  {#BLSdef}

A BLS curve {{BLS02}} is another family of pairing-friendly curves proposed in 2002. Similar to BN curves, a pairing over BLS curves constructs optimal Ate pairings.

A BLS curve is defined by elliptic curves E and E' parameterized by a well-chosen integer t. E is defined over a finite field GF(p) by an equation of the form E: y^2 = x^3 + b, and its twist E': y^2 = x^3 + b', is defined in the same way as BN curves. In contrast to BN curves, E(GF(p)) does not have a prime order. Instead, its order is divisible by a large parameterized prime r and denoted by h * r with cofactor h. The pairing is defined on the r-torsion points. In the same way as BN curves, BLS curves can be categorized as D-type and M-type.

BLS curves vary in accordance with different embedding degrees. In this memo, we deal with the BLS12 and BLS48 families with embedding degrees 12 and 48 with respect to r, respectively.

In BLS curves, parameters p and r are given by the following equations:

~~~~~~~~~~
BLS12:
    p = (t - 1)^2 * (t^4 - t^2 + 1) / 3 + t
    r = t^4 - t^2 + 1
BLS48:
    p = (t - 1)^2 * (t^16 - t^8 + 1) / 3 + t
    r = t^16 - t^8 + 1
~~~~~~~~~~

for a well chosen integer t where t must be 1 (mod 3).

A pairing e is defined by taking G_1 as a subgroup of E(GF(p)) of order r, G_2 as an order r subgroup of E'(GF(p^2)) for BLS12 and of E'(GF(p^8)) for BLS48, and G_T as an order r subgroup of the multiplicative group (GF(p^12))^* for BLS12 and of the multiplicative group (GF(p^48))^* for BLS48.


# Security of Pairing-Friendly Curves  {#security_pfc}

## Evaluating the Security of Pairing-Friendly Curves  {#evaluating-the-security-of-pairing-friendly-curves}

The security of pairing-friendly curves is evaluated by the hardness of the following discrete logarithm problems:

- The elliptic curve discrete logarithm problem (ECDLP) in G_1 and G_2
- The finite field discrete logarithm problem (FFDLP) in G_T

There are other hard problems over pairing-friendly curves used for proving the security of pairing-based cryptography. Such problems include the computational bilinear Diffie-Hellman (CBDH) problem, the bilinear Diffie-Hellman (BDH) problem, the decision bilinear Diffie-Hellman (DBDH) problem, the gap DBDH problem, etc. {{ECRYPT}}. Almost all of these variants are reduced to the hardness of discrete logarithm problems described above and are believed to be easier than the discrete logarithm problems.

Although it would be sufficient to attack any of these problems to attack pairing-based crytography, the only known attacks thus far attack the discrete logarithm problem directly, so we focus on the discrete logarithm in this memo.

The security levels of pairing-friendly curves are estimated by the computational cost of the most efficient algorithm for solving the above discrete logarithm problems. The best-known algorithms for solving the discrete logarithm problems are based on Pollard's rho algorithm {{Pollard78}} and Index Calculus {{HR83}}. To make index calculus algorithms more efficient, number field sieve (NFS) algorithms are utilized.

## Impact of Recent Attacks  {#impact}

In 2016, Kim and Barbulescu proposed a new variant of the NFS algorithms, the extended tower number field sieve (exTNFS), which drastically reduces the complexity of solving FFDLP {{KB16}}. The exTNFS improves the polynomial selection that is the first step in the number field sieve algorithm for discrete logarithms in GF(p^k). The idea is applicable when the embedding degree k is a composite that can be written as k = i * j with gcd(i, j) = 1 and i, j > 1. Any k divisible by 6 meets this condition, taking i to be the largest power of 2 dividing k and j to be k / i; {{KB16}} accordingly gives k = 6 as the first case it treats. BN curves and BLS curves, whose embedding degree is divisible by 6, are therefore affected by the exTNFS. The basic idea of the exTNFS is based on the equality GF(p^k) = (GF(p^i)^j) and one of the improvement for reducing the amount of cost for solving FFDLP is using sub-field calculation. Please refer to {{KB16}} for detailed ideas and calculation algorithms of exTNFS. Due to exTNFS, the security levels of certain pairing-friendly curves asymptotically dropped down. For instance, Barbulescu and Duquesne estimated that the security of the BN curves, which had been believed to provide 128-bit security (BN256, for example) was reduced to approximately 100 bits {{BD18}}. Here, the security levels described in this memo correspond to the security strength of NIST recommendation {{NIST}}.

There has since been research into the minimum bit length of the parameters of pairing-friendly curves for each security level when applying exTNFS as an attacking method for FFDLP. For 128-bit security, Barbulescu and Duquesne estimated the minimum bit length of p of BN curves and BLS12 curves after exTNFS as 461 bits {{BD18}}. For 256-bit security, Kiyomura et al. estimated the minimum bit length of p^k of BLS48 curves as 27,410 bits, which indicated 572 bits of p {{KIK17}}.


# Selection of Pairing-Friendly Curves  {#secure_params}

In this section, we introduce some of the known secure pairing-friendly curves that consider the impact of exTNFS.

First, we show the adoption status of pairing-friendly curves in standards, libraries and applications, and classify them in accordance with the 128-bit, 192-bit, and 256-bit security levels. Then, from the viewpoints of "security" and "widely used", pairing-friendly curves corresponding to each security level are selected and their parameters are indicated.

In our selection policy, it is important that selected curves are shown in peer-reviewed papers for security and that they are widely used in cryptographic libraries. In addition, "efficiency" is one of the important aspects but greatly dependant on implementations, so we choose to prioritize "security" and "widely used" over "efficiency" in consideration of future interconnections and interoperability over the internet.

Within this policy, when "widely used" does not by itself distinguish between candidate curves at a given security level, we prefer the curve that provides security margin above the nominal security level: a margin advantage is a security consideration, and by the priority above it outranks an efficiency advantage of a lower-margin alternative. BLS12_381 is the one exception to this preference for margin: although its security level is estimated at approximately 126 bits {{GMT19}}, slightly below the nominal 128-bit target, it is retained because its adoption in production deployments (see {{impl}}) decisively satisfies the "widely used" criterion. Where no candidate at a given security level satisfies "widely used" -- as is currently the case at the 256-bit level -- this exception does not apply, and the preference for margin governs without qualification.

As a result, we recommend the BLS curve with 381-bit characteristic of embedding degree 12 and the BN curve with the 462-bit characteristic for the 128-bit security level, and the BLS curves of embedding degree 48 with the 581-bit characteristic for the 256-bit security level. On the other hand, we do not show the parameters for 192-bit security here because there are no curves that match our selection policy.

## Adoption Status of Pairing-friendly Curves  {#impl}

We show the pairing-friendly curves that have been selected by existing standards, cryptographic libraries, and applications. A comprehensive curve-by-curve comparison, including proposed alternatives that were not selected, is maintained at [https://cfrg.github.io/draft-irtf-cfrg-pairing-friendly-curves/adoption-status.html](https://cfrg.github.io/draft-irtf-cfrg-pairing-friendly-curves/adoption-status.html).

The adoption status of pairing-friendly curves is surveyed in standards, libraries and applications. In this survey, "Arnd" is an abbreviation for "Around". The curves categorized as 'Arnd 128-bit', 'Arnd 192-bit' and 'Arnd 256-bit' for each label show that their security levels are within the range of plus/minus 5 bits for each security level. Other labels shown with '~' mean that the security level of the categorized curve is outside the range of each security level. Specifically, the security level of the categorized curves is more than the previous column and is less than the next column. The details are described as the following subsections. A BN curve with a XXX-bit characteristic p is denoted as BNXXX and a BLS curve of embedding degree k with a XXX-bit p is denoted as BLSk_XXX.

This section omits parameters with security levels below the "Arnd 128-bit" range due to space limitations and viewpoints of secure usage of parameters. On the other hand, indicating which standards, libraries, and applications use these lower security level parameters would be useful information for implementers, therefore {{adoption_status_100bit_security}} shows these parameters.

The security level for each curve is evaluated in accordance with {{BD18}}, {{GMT19}}, {{MAF19}} and {{FK18}}. Note that the Freeman curves {{Freeman06}} and MNT curves {{MNT01}} are not included in this survey because {{BD18}} does not show the security levels of these curves.

### International Standards  {#standardization}

ISO/IEC 15946 series specifies public-key cryptographic techniques based on elliptic curves. The third edition of ISO/IEC 15946-5 {{ISOIEC15946-5}} (published 2022) reorganized the numerical examples and extended coverage to include BLS12, BLS24, and BLS48 curves. The BN462 parameter in this document matches the numerical example in Annex D.2.3 of {{ISOIEC15946-5}} exactly (t = 2^114 + 2^101 - 2^14 - 1, written u there), and the BLS48_581 parameter matches Annex D.3.5 exactly (t = -2^32 - 2^30 - 2^10 + 2^7 - 1). The same edition introduces a post-exTNFS security guideline that the characteristic p of BN and BLS12 curves should be at least 461 bits for the 128-bit security level. As described below, BN curves with 256-bit p and 512-bit p from earlier editions of ISO/IEC 15946-5 are referenced by other standards and libraries; these curves are denoted as BN256I and BN512I, where the suffix 'I' is given from the initials of the standard name ISO.

TCG adopts the BN256I and a BN curve with 638-bit p specified by their own{{TPM}}. FIDO Alliance {{FIDO}} and W3C {{W3C}} adopt BN256I, BN512I, the BN638 by TCG, and the BN curve with 256-bit p proposed by Devegili et al.{{DSD07}} (named BN256D). The suffix 'D' of BN256D is given from the initials of the first author's name of the paper which proposed the parameter.

### Cryptographic Libraries  {#cryptographic_libraries}

There are a lot of cryptographic libraries that support pairing calculations.

blst {{blst}} is a high-performance pairing library maintained by Supranational. It supports BLS12_381 and is used in production by Ethereum consensus clients, Filecoin, and other applications.

Several additional actively maintained libraries support BLS12_381. gnark-crypto {{gnark-crypto}}, developed by Consensys, supports BLS12_381, BN254, BLS12_377, BLS24_315, and BW6_761. noble-curves {{noble-curves}} is a JavaScript/TypeScript library by Paul Miller supporting BLS12_381. The arkworks ecosystem {{arkworks}} provides Rust crates for pairing-friendly curves used in zero-knowledge proof systems, including BLS12_381 and BN254. constantine {{constantine}} is a cryptographic library written in Nim that supports BLS12_381, BN254, BLS12_377, and BW6_761. CIRCL {{CIRCL}} is the Cloudflare Interoperable, Reusable Cryptographic Library and includes support for BLS12_381. zkcrypto {{zkcrypto}} is a collection of Rust crates for zero-knowledge cryptography supporting BLS12_381.

PBC is a library for pairing-based cryptography published by Stanford University that supports BN curves, MNT curves, Freeman curves, and supersingular curves {{PBC}}. Users can generate pairing parameters by using PBC and use pairing operations with the generated parameters.

mcl{{mcl}} is a library for pairing-based cryptography that supports four BN curves and BLS12_381 {{GMT19}}. These BN curves include BN254 proposed by Nogami et al. {{NASKM08}} (named BN254N), BN_SNARK1 suitable for SNARK applications{{libsnark}}, BN382M, and BN462. The suffix 'N' of BN254N and the suffix 'M' of BN382M are respectively given from the initials of the first author's name of the proposed paper and the library's name mcl. Kyushu University published a library that supports the BLS48_581 {{BLS48}}. The University of Tsukuba Elliptic Curve and Pairing Library (TEPLA) {{TEPLA}} supports two BN curves, BN254N and BN254 proposed by Beuchat et al. {{BGMORT10}} (named BN254B). The suffix 'B' of BN254B is given from the initials of the first author's name of the proposed paper. Intel published a cryptographic library named Intel Integrated Performance Primitives (Intel-IPP) {{Intel-IPP}} and the library supports BN256I.

RELIC {{RELIC}} uses various types of pairing-friendly curves including six BN curves (BN158, BN254R, BN256R, BN382R, BN446, and BN638), where BN254R, BN256R, and BN382R are RELIC specific parameters that are different from BN254N, BN254B, BN256I, BN256D, and BN382M. The suffix 'R' of BN382R is given from the initials of the library's name RELIC. In addition, RELIC supports six BLS curves (BLS12_381, BLS12_446, BLS12_455, BLS12_638, BLS24_477, and BLS48_575 {{MAF19}}), Cocks-Pinch curves of embedding degree 8 with 544-bit p{{GMT19}}, pairing-friendly curves constructed by Scott et al. {{SG19}} based on Kachisa-Scott-Schaefer curves with embedding degree 54 with 569-bit p (named K54_569){{MAF19}}, a KSS curve {{KSS08}} of embedding degree 18 with 508-bit p (named KSS18_508) {{AFKMR12}}, Optimal TNFS-secure curve {{FM19}} of embedding degree 8 with 511-bit p (OT8_511), and a supersingular curve {{S86}} with 1536-bit p (SS_1536).

MIRACL Core {{MIRACL}} (the successor to the Apache Milagro Crypto Library (AMCL) {{AMCL}}) supports five BLS curves (BLS12_381, BLS12_461, BLS24_479, BLS48_556, and BLS48_581) and five BN curves (BN254N, BN254CX proposed by CertiVox, BN256I, BN512I, and BN462).

Adjoint published a library that supports the BLS12_381 and six BN curves (BN_SNARK1, BN254B, BN254N, BN254S1, BN254S2, and BN462) {{AdjointLib}}, where BN254S1 and BN254S2 are BN curves adopted by an old version of AMCL {{AMCLv2}}. The suffix 'S' of BN254S1 and BN254S2 are given from the initials of developper's name because he proposed these parameters.

The Celo foundation published the bls12377js library {{bls12377js}}. The supported curve is the BLS12_377 curve which is shown in {{BCGMMW20}}.

### Applications  {#applications}

Zcash uses a BN curve (named BN128) in their library libsnark {{libsnark}}. In response to the exTNFS attacks, they proposed new parameters using BLS12_381 {{BLS12_381}} {{GMT19}} and published its implementation {{zkcrypto}}.

Ethereum adopted BLS12_381 for its consensus layer. The BLS12_381 precompile is also specified as an Ethereum precompile contract in EIP-2537 {{EIP2537}}, enabling on-chain pairing operations. Filecoin {{Filecoin}} uses BLS12_381 via the blst library {{blst}}. Chia Network published their implementation {{Chia}} by integrating the RELIC toolkit {{RELIC}}. DFINITY uses mcl, and Algorand published an implementation which supports BLS12_381.

## For 128-bit Security  {#for-128-bits-of-security}

The survey in {{impl}} shows a lot of cases of adopting BN and BLS curves. Among them, BLS12_381 and BN462 match our selection policy. Especially, the one that best matches the policy is BLS12_381 from the viewpoint of "widely used" and "efficiency", so we introduce the parameters of BLS12_381 in this memo.

On the other hand, from the viewpoint of the future use, the parameter of BN462 is also introduced. As shown in recent security evaluations for BLS12_381{{BD18}} {{GMT19}}, its security level close to 128-bit but it is less than 128-bit. If the attack is improved even a little, BLS12_381 will not be suitable for the curve of the 128-bit security level. As curves of 128-bit security level are currently the most widely used, we recommend both BLS12_381 and BN462 in this memo in order to have a more efficient and a more prudent option respectively.

### BLS Curves for the 128-bit Security Level (BLS12_381)  {#parameter-BLS12_381}

In this part, we introduce the parameters of the Barreto-Lynn-Scott curve of embedding degree 12 with 381-bit p that is adopted by a lot of applications such as Zcash {{Zcash}}, Ethereum {{Ethereum}}, and so on.

The BLS12_381 curve is shown in {{BLS12_381}} and it is defined by the parameter

~~~~~~~~~~
t = -2^63 - 2^62 - 2^60 - 2^57 - 2^48 - 2^16
~~~~~~~~~~

where the size of p becomes 381-bit length.

{: #tower_bls12_381}
For the finite field GF(p), the towers of extension field GF(p^2), GF(p^6) and GF(p^12) are defined by indeterminates u, v, and w as follows:

~~~~~~~~~~
GF(p^2) = GF(p)[u] / (u^2 + 1)
GF(p^6) = GF(p^2)[v] / (v^3 - u - 1)
GF(p^12) = GF(p^6)[w] / (w^2 - v).
~~~~~~~~~~

Defined by t, the elliptic curve E and its twist E' are represented by E: y^2 = x^3 + 4 and E': y^2 = x^3 + 4(u + 1). BLS12_381 is categorized as M-type.

The untwist isomorphism psi: E'(GF(p^2)) -> E(GF(p^12)) is given by

~~~~~~~~~~
psi(x', y') = (x' / w^2, y' / w^3)
~~~~~~~~~~

where w^2 = v in GF(p^6) and v^3 = u + 1, per the tower defined in {{tower_bls12_381}}.

We have to note that the security level of this pairing is expected to be 126 rather than 128 bits {{GMT19}}.

Parameters of BLS12_381 are given as follows.

- G_1 is the largest prime-order subgroup of E(GF(p)) - BP = (x,y) : a 'base point', i.e., a generator of G_1
- G_2 is an r-order subgroup of E'(GF(p^2)) - BP' = (x',y') : a 'base point', i.e., a generator of G_2 - x' = x'_0 + x'_1 * u (x'_0, x'_1 in GF(p)) - y' = y'_0 + y'_1 * u (y'_0, y'_1 in GF(p)) - h' : the cofactor #E'(GF(p^2))/r

p:
:   0x1a0111ea397fe69a4b1ba7b6434bacd764774b84f38512bf6730d2a0f6b0f6241eabfffeb153ffffb9feffffffffaaab

r:
:   0x73eda753299d7d483339d80809a1d80553bda402fffe5bfeffffffff00000001

x:
:   0x17f1d3a73197d7942695638c4fa9ac0fc3688c4f9774b905a14e3a3f171bac586c55e83ff97a1aeffb3af00adb22c6bb

y:
:   0x08b3f481e3aaa0f1a09e30ed741d8ae4fcf5e095d5d00af600db18cb2c04b3edd03cc744a2888ae40caa232946c5e7e1

h:
:   0x396c8c005555e1568c00aaab0000aaab

b:
:   4

x'_0:
:   0x024aa2b2f08f0a91260805272dc51051c6e47ad4fa403b02b4510b647ae3d1770bac0326a805bbefd48056c8c121bdb8

x'_1:
:   0x13e02b6052719f607dacd3a088274f65596bd0d09920b61ab5da61bbdc7f5049334cf11213945d57e5ac7d055d042b7e

y'_0:
:   0x0ce5d527727d6e118cc9cdc6da2e351aadfd9baa8cbdd3a76d429a695160d12c923ac9cc3baca289e193548608b82801

y'_1:
:   0x0606c4a02ea734cc32acd2b02bc28b99cb3e287e85a763af267492ab572e99ab3f370d275cec1da1aaa9075ff05f79be

h':
:   0x5d543a95414e7f1091d50792876a202cd91de4547085abaa68a205b2e5a7ddfa628f1cb4d9e82ef21537e293a6691ae1616ec6e786f0c70cf1c38e31c7238e5

b':
:   4 * (u + 1)

As mentioned above, BLS12_381 is adopted in a lot of applications. Since it is expected that BLS12_381 will continue to be widely used more and more in the future, {{point-serialization}} defines a normative point serialization format for it (with test vectors in {{point-serialization-test-vectors}}). This serialization format is also adopted in {{I-D.irtf-cfrg-bls-signature}} {{zkcrypto}}.

In addition, many pairing-based cryptographic applications use a hashing to an elliptic curve procedure that outputs a rational point on an elliptic curve from an arbitrary input. {{RFC9380}} specifies ciphersuites for hashing to an elliptic curve, including BLS12_381, and is valuable information for implementers.

### BN Curves for the 128-bit Security Level (BN462)  {#bn-curves}

A BN curve with the 128-bit security level is shown in {{BD18}}, which we call BN462. BN462 is defined by the parameter

~~~~~~~~~~
t = 2^114 + 2^101 - 2^14 - 1
~~~~~~~~~~

for the definition in {{BNdef}}.

{: #tower_bn462}
For the finite field GF(p), the towers of extension field GF(p^2), GF(p^6) and GF(p^12) are defined by indeterminates u, v, and w as follows:

~~~~~~~~~~
GF(p^2) = GF(p)[u] / (u^2 + 1)
GF(p^6) = GF(p^2)[v] / (v^3 - u - 2)
GF(p^12) = GF(p^6)[w] / (w^2 - v).
~~~~~~~~~~

Defined by t, the elliptic curve E and its twist E' are represented by E: y^2 = x^3 + 5 and E': y^2 = x^3 - u + 2, respectively. The size of p becomes 462-bit length. BN462 is categorized as D-type.

The untwist isomorphism psi: E'(GF(p^2)) -> E(GF(p^12)) is given by

~~~~~~~~~~
psi(x', y') = (x' * w^2, y' * w^3)
~~~~~~~~~~

where w^2 = v in GF(p^6) and v^3 = u + 2, per the tower defined in {{tower_bn462}}.

We have to note that BN462 is significantly slower than BLS12_381, but has 134-bit security level {{GMT19}}, so may be more resistant to future small improvements to the exTNFS attack.

We note also that CP8_544 is about 20% faster that BN462 {{GMT19}}, has 131-bit security level, and that due to its construction will not be affected by future small improvements to the exTNFS attack. However, as this curve is not widely used (it is only implemented in one library), we instead chose BN462 for our 'safe' option.

We give the following parameters for BN462.

- G_1 is the largest prime-order subgroup of E(GF(p)) - BP = (x,y) : a 'base point', i.e., a generator of G_1
- G_2 is an r-order subgroup of E'(GF(p^2)) - BP' = (x',y') : a 'base point', i.e., a generator of G_2 - x' = x'_0 + x'_1 * u (x'_0, x'_1 in GF(p)) - y' = y'_0 + y'_1 * u (y'_0, y'_1 in GF(p)) - h' : the cofactor #E'(GF(p^2))/r

p:
:   0x240480360120023ffffffffff6ff0cf6b7d9bfca0000000000d812908f41c8020ffffffffff6ff66fc6ff687f640000000002401b00840138013

r:
:   0x240480360120023ffffffffff6ff0cf6b7d9bfca0000000000d812908ee1c201f7fffffffff6ff66fc7bf717f7c0000000002401b007e010800d

x:
:   0x21a6d67ef250191fadba34a0a30160b9ac9264b6f95f63b3edbec3cf4b2e689db1bbb4e69a416a0b1e79239c0372e5cd70113c98d91f36b6980d

y:
:   0x0118ea0460f7f7abb82b33676a7432a490eeda842cccfa7d788c659650426e6af77df11b8ae40eb80f475432c66600622ecaa8a5734d36fb03de

h:
:   1

b:
:   5

x'_0:
:   0x0257ccc85b58dda0dfb38e3a8cbdc5482e0337e7c1cd96ed61c913820408208f9ad2699bad92e0032ae1f0aa6a8b48807695468e3d934ae1e4df

x'_1:
:   0x1d2e4343e8599102af8edca849566ba3c98e2a354730cbed9176884058b18134dd86bae555b783718f50af8b59bf7e850e9b73108ba6aa8cd283

y'_0:
:   0x0a0650439da22c1979517427a20809eca035634706e23c3fa7a6bb42fe810f1399a1f41c9ddae32e03695a140e7b11d7c3376e5b68df0db7154e

y'_1:
:   0x073ef0cbd438cbe0172c8ae37306324d44d5e6b0c69ac57b393f1ab370fd725cc647692444a04ef87387aa68d53743493b9eba14cc552ca2a93a

h':
:   0x240480360120023ffffffffff6ff0cf6b7d9bfca0000000000d812908fa1ce0227fffffffff6ff66fc63f5f7f4c0000000002401b008a0168019

b':
:   -u + 2

## For 256-bit Security  {#for-256-bits-of-security}

As shown in the survey in {{impl}}, there are three candidates of pairing-friendly curves for 256-bit security. According to our selection policy, we select BLS48_581, as it is the most widely adopted by cryptographic libraries.

The selected BLS48 curve is shown in {{KIK17}} and it is defined by the parameter

~~~~~~~~~~
t = -1 + 2^7 - 2^10 - 2^30 - 2^32.
~~~~~~~~~~

In this case, the size of p becomes 581-bit.

{: #tower_bls48_581}
For the finite field GF(p), the towers of extension field GF(p^2), GF(p^4), GF(p^8), GF(p^24) and GF(p^48) are defined by indeterminates u, v, w, z, and s as follows:

~~~~~~~~~~
GF(p^2) = GF(p)[u] / (u^2 + 1)
GF(p^4) = GF(p^2)[v] / (v^2 + u + 1)
GF(p^8) = GF(p^4)[w] / (w^2 + v)
GF(p^24) = GF(p^8)[z] / (z^3 + w)
GF(p^48)= GF(p^24)[s] / (s^2 + z).
~~~~~~~~~~

The elliptic curve E and its twist E' are represented by E: y^2 = x^3 + 1 and E': y^2 = x^3 - 1 / w. BLS48_581 is categorized as D-type.

The untwist isomorphism psi: E'(GF(p^8)) -> E(GF(p^48)) is given by

~~~~~~~~~~
psi(x', y') = (x' * xi^2, y' * xi^3)
~~~~~~~~~~

where xi = u * s in GF(p^48) satisfies xi^6 = -w (the twist coefficient), per the tower defined in {{tower_bls48_581}}. Concretely: u in GF(p^2) satisfies u^2 = -1, so u^6 = -1; s in GF(p^48) satisfies s^2 = -z and z^3 = -w, so s^6 = w; hence xi^6 = u^6 * s^6 = (-1) * w = -w.

We then give the parameters for BLS48_581 as follows.

- G_1 is the largest prime-order subgroup of E(GF(p)) - BP = (x,y) : a 'base point', i.e., a generator of G_1
- G_2 is an r-order subgroup of E'(GF(p^8)) - BP' = (x',y') : a 'base point', i.e., a generator of G_2 - x' = x'_0 + x'_1 * u + x'_2 * v + x'_3 * u * v + x'_4 * w + x'_5 * u * w + x'_6 * v * w + x'_7 * u * v * w (x'_0, ..., x'_7 in GF(p)) - y' = y'_0 + y'_1 * u + y'_2 * v + y'_3 * u * v + y'_4 * w + y'_5 * u * w + y'_6 * v * w + y'_7 * u * v * w (y'_0, ..., y'_7 in GF(p)) - h' : the cofactor #E'(GF(p^8))/r

p:
:   0x1280f73ff3476f313824e31d47012a0056e84f8d122131bb3be6c0f1f3975444a48ae43af6e082acd9cd30394f4736daf68367a5513170ee0a578fdf721a4a48ac3edc154e6565912b

r:
:   0x2386f8a925e2885e233a9ccc1615c0d6c635387a3f0b3cbe003fad6bc972c2e6e741969d34c4c92016a85c7cd0562303c4ccbe599467c24da118a5fe6fcd671c01

x:
:   0x02af59b7ac340f2baf2b73df1e93f860de3f257e0e86868cf61abdbaedffb9f7544550546a9df6f9645847665d859236ebdbc57db368b11786cb74da5d3a1e6d8c3bce8732315af640

y:
:   0x0cefda44f6531f91f86b3a2d1fb398a488a553c9efeb8a52e991279dd41b720ef7bb7beffb98aee53e80f678584c3ef22f487f77c2876d1b2e35f37aef7b926b576dbb5de3e2587a70

x'_0:
:   0x05d615d9a7871e4a38237fa45a2775debabbefc70344dbccb7de64db3a2ef156c46ff79baad1a8c42281a63ca0612f400503004d80491f510317b79766322154dec34fd0b4ace8bfab

x'_1:
:   0x07c4973ece2258512069b0e86abc07e8b22bb6d980e1623e9526f6da12307f4e1c3943a00abfedf16214a76affa62504f0c3c7630d979630ffd75556a01afa143f1669b36676b47c57

x'_2:
:   0x01fccc70198f1334e1b2ea1853ad83bc73a8a6ca9ae237ca7a6d6957ccbab5ab6860161c1dbd19242ffae766f0d2a6d55f028cbdfbb879d5fea8ef4cded6b3f0b46488156ca55a3e6a

x'_3:
:   0x0be2218c25ceb6185c78d8012954d4bfe8f5985ac62f3e5821b7b92a393f8be0cc218a95f63e1c776e6ec143b1b279b9468c31c5257c200ca52310b8cb4e80bc3f09a7033cbb7feafe

x'_4:
:   0x038b91c600b35913a3c598e4caa9dd63007c675d0b1642b5675ff0e7c5805386699981f9e48199d5ac10b2ef492ae589274fad55fc1889aa80c65b5f746c9d4cbb739c3a1c53f8cce5

x'_5:
:   0x0c96c7797eb0738603f1311e4ecda088f7b8f35dcef0977a3d1a58677bb037418181df63835d28997eb57b40b9c0b15dd7595a9f177612f097fc7960910fce3370f2004d914a3c093a

x'_6:
:   0x0b9b7951c6061ee3f0197a498908aee660dea41b39d13852b6db908ba2c0b7a449cef11f293b13ced0fd0caa5efcf3432aad1cbe4324c22d63334b5b0e205c3354e41607e60750e057

x'_7:
:   0x0827d5c22fb2bdec5282624c4f4aaa2b1e5d7a9defaf47b5211cf741719728a7f9f8cfca93f29cff364a7190b7e2b0d4585479bd6aebf9fc44e56af2fc9e97c3f84e19da00fbc6ae34

y'_0:
:   0x00eb53356c375b5dfa497216452f3024b918b4238059a577e6f3b39ebfc435faab0906235afa27748d90f7336d8ae5163c1599abf77eea6d659045012ab12c0ff323edd3fe4d2d7971

y'_1:
:   0x0284dc75979e0ff144da6531815fcadc2b75a422ba325e6fba01d72964732fcbf3afb096b243b1f192c5c3d1892ab24e1dd212fa097d760e2e588b423525ffc7b111471db936cd5665

y'_2:
:   0x0b36a201dd008523e421efb70367669ef2c2fc5030216d5b119d3a480d370514475f7d5c99d0e90411515536ca3295e5e2f0c1d35d51a652269cbc7c46fc3b8fde68332a526a2a8474

y'_3:
:   0x0aec25a4621edc0688223fbbd478762b1c2cded3360dcee23dd8b0e710e122d2742c89b224333fa40dced2817742770ba10d67bda503ee5e578fb3d8b8a1e5337316213da92841589d

y'_4:
:   0x0d209d5a223a9c46916503fa5a88325a2554dc541b43dd93b5a959805f1129857ed85c77fa238cdce8a1e2ca4e512b64f59f430135945d137b08857fdddfcf7a43f47831f982e50137

y'_5:
:   0x07d0d03745736b7a513d339d5ad537b90421ad66eb16722b589d82e2055ab7504fa83420e8c270841f6824f47c180d139e3aafc198caa72b679da59ed8226cf3a594eedc58cf90bee4

y'_6:
:   0x0896767811be65ea25c2d05dfdd17af8a006f364fc0841b064155f14e4c819a6df98f425ae3a2864f22c1fab8c74b2618b5bb40fa639f53dccc9e884017d9aa62b3d41faeafeb23986

y'_7:
:   0x035e2524ff89029d393a5c07e84f981b5e068f1406be8e50c87549b6ef8eca9a9533a3f8e69c31e97e1ad0333ec719205417300d8c4ab33f748e5ac66e84069c55d667ffcb732718b6

h:
:   0x85555841aaaec4ac

b:
:   1

h':
:   0x170e915cb0a6b7406b8d94042317f811d6bc3fc6e211ada42e58ccfcb3ac076a7e4499d700a0c23dc4b0c078f92def8c87b7fe63e1eea270db353a4ef4d38b5998ad8f0d042ea24c8f02be1c0c83992fe5d7725227bb27123a949e0876c0a8ce0a67326db0e955dcb791b867f31d6bfa62fbdd5f44a00504df04e186fae033f1eb43c1b1a08b6e086eff03c8fee9ebdd1e191a8a4b0466c90b389987de5637d5dd13dab33196bd2e5afa6cd19cf0fc3fc7db7ece1f3fac742626b1b02fcee04043b2ea96492f6afa51739597c54bb78aa6b0b99319fef9d09f768831018ee6564c68d054c62f2e0b4549426fec24ab26957a669dba2a2b6945ce40c9aec6afdeda16c79e15546cd7771fa544d5364236690ea06832679562a68731420ae52d0d35a90b8d10b688e31b6aee45f45b7a5083c71732105852decc888f64839a4de33b99521f0984a418d20fc7b0609530e454f0696fa2a8075ac01cc8ae3869e8d0fe1f3788ffac4c01aa2720e431da333c83d9663bfb1fb7a1a7b90528482c6be7892299030bb51a51dc7e91e9156874416bf4c26f1ea7ec578058563960ef92bbbb8632d3a1b695f954af10e9a78e40acffc13b06540aae9da5287fc4429485d44e6289d8c0d6a3eb2ece35012452751839fb48bc14b515478e2ff412d930ac20307561f3a5c998e6bcbfebd97effc6433033a2361bfcdc4fc74ad379a16c6dea49c209b1

b':
:   -1 / w

{{point-serialization}} defines a normative point serialization format for BLS48_581 (with test vectors in {{point-serialization-test-vectors}}), extending the format defined by {{ZCashRep}} for BLS12_381 as specified in {{I-D.ietf-cose-bls-key-representations}}.

# Serialization and Validation  {#point-serialization}

This section defines normative serialization and deserialization procedures for BLS12_381, and also extends them to BLS48_581. It also states what makes a deserialized value valid, and which of the remaining decisions belong to the calling protocol. What is encoded here are the objects that protocols transmit: points on E and on E', and scalars. Elements of GF(p) and of GF(p^m) appear as coordinates of those points rather than as objects with encodings of their own. The point format is based on the one originally defined by {{ZCashRep}} for BLS12_381 and is, in turn, based on the representation shown in {{SEC1}} with a small tweak to apply to GF(p^m). It is already relied upon, directly or indirectly, by {{I-D.irtf-cfrg-bbs-signatures}} and {{I-D.ietf-cose-bls-key-representations}}; the latter extends it to BLS48_581, and the extension is adopted here. Applicability to BN462 is discussed in {{bn462-applicability}}.

Not all of what follows originates with this document. Where an existing format is restated, it is restated normatively, because other specifications already cite this document for it.

| What | Where it comes from | How this document treats it |
|---|---|---|
| Scalar encoding | Existing practice: I2OSP and OS2IP {{RFC8017}}, together with a comparison against the group order | Specified here, for all three curves |
| BLS12_381 point encoding | {{ZCashRep}}, which adapts the representation of {{SEC1}} to GF(p^m) | Restated here as the format in use |
| BLS48_581 point encoding | The same format, extended to GF(p^8) in {{I-D.ietf-cose-bls-key-representations}} | That extension is adopted here |
| BN462 point encoding | -- | Not specified here; see {{bn462-applicability}} |
| Group membership and identity handling | This document | Specified here |

At a high level, the point serialization format is defined as follows:

- Serialized points include three metadata bits that indicate whether a point is compressed or not, whether a point is the point at infinity or not, and (for compressed points) the sign of the point's y-coordinate.
- For a curve with characteristic p represented in n = ceil(len(p) / 8) bytes, points on E are serialized into n bytes (compressed) or 2n bytes (uncompressed). Points on E', represented over GF(p^m) for the m given in {{point-serialization-params}}, are serialized into m\*n bytes (compressed) or 2\*m\*n bytes (uncompressed).
- The serialization of a point at infinity comprises a string of zero bytes, except that the metadata bits may be nonzero.
- The serialization of a compressed point other than the point at infinity comprises a serialized x-coordinate.
- The serialization of an uncompressed point other than the point at infinity comprises a serialized x-coordinate followed by a serialized y-coordinate.

## Parameters and Notation  {#point-serialization-params}

| Curve | n (bytes) | E' field | m | Compressed (E / E') | Uncompressed (E / E') |
|---|---|---|---|---|---|
| BLS12_381 | 48 | GF(p^2) | 2 | 48 / 96 bytes | 96 / 192 bytes |
| BLS48_581 | 73 | GF(p^8) | 8 | 73 / 584 bytes | 146 / 1168 bytes |

Below, we give detailed serialization and deserialization procedures, applicable to both curves using the parameters above. The following notation is used in the rest of this section:

- Elements of GF(p^m) are represented as a vector of m coefficients in GF(p), (y_0, ..., y_{m-1}), using the basis and coefficient ordering already defined for each curve in {{secure_params}}.
- For a byte string str, str[0] is defined as the first byte of str.
- The function sign_GF_p(y) returns one bit representing the sign of an element of GF(p). This function is defined as follows:

~~~~~~~~~~
sign_GF_p(y) := { 1 if y > (p - 1) / 2, else
                { 0 otherwise.
~~~~~~~~~~

- The function sign_GF_p^m(y), for an element y = (y_0, ..., y_{m-1}) of GF(p^m), returns one bit computed as follows: let i be the largest index in {0, ..., m-1} such that y_i is nonzero, or i = 0 if all coefficients are zero; return sign_GF_p(y_i). For BLS12_381 (m=2), this specializes to: sign_GF_p^2(y') = sign_GF_p(y'_0) if y'_1 equals 0, else sign_GF_p(y'_1). For BLS48_581 (m=8), this is the same function specified as sign_GF_p^8 in {{I-D.ietf-cose-bls-key-representations}}, evaluated over the coefficient ordering (y'_0, ..., y'_7) given in {{secure_params}}.
- The function OS2FE(str, n), for a byte string str of m\*n bytes, returns an element of GF(p^m) or INVALID. It inverts the coordinate serialization of step 3 of {{point-serialization-procedure}}: divide str into m consecutive blocks of n bytes each, and for each i in {0, ..., m-1} let y_i = OS2IP(str_i), where str_i is the block starting at offset (m - 1 - i) \* n, so that the blocks give the coefficients in decreasing index order. If any y_i is greater than or equal to p, OS2FE returns INVALID; otherwise it returns y = (y_0, ..., y_{m-1}).

{{point-deserialization-procedure}} requires deciding whether a field element y2 is a square and, where it is, computing a square root. This document does not specify how. The following pointers are provided for implementers and are not requirements.

For y2 in GF(p), all curves in this document have p = 3 (mod 4). Appendix I.1 of {{RFC9380}} therefore applies directly: y2 is a square exactly when y2^((p-1)/2) is 0 or 1, and a square root is given by y2^((p+1)/4).

For y2 in GF(p^m), that shortcut does not apply, since p^2 and p^8 are both 1 (mod 4). Appendix I.4 of {{RFC9380}} gives a constant-time Tonelli-Shanks procedure for a general field, and Appendix I.5 gives an is_square test for GF(p^2).

## Scalar Serialization  {#scalar-serialization}

This section defines a serialization format for elements of the scalar field GF(r), where r is the order of G_1 and G_2 as given for each curve in {{secure_params}}. Unlike point serialization, this format applies to all three curves in this document (BLS12_381, BN462, and BLS48_581), since no metadata bits are required.

For a curve with scalar field order r represented in n_s = ceil(len(r) / 8) bytes:

| Curve | n_s (bytes) |
|---|---|
| BLS12_381 | 32 |
| BN462 | 58 |
| BLS48_581 | 65 |

Serialization: a scalar kappa in the range [0, r - 1] is serialized as I2OSP(kappa, n_s).

Deserialization: given a byte string s_string of length n_s, let kappa = OS2IP(s_string). If kappa >= r, return INVALID. Otherwise, return kappa.

Rejecting values greater than or equal to r gives every scalar exactly one encoding, which matters wherever an encoded scalar is hashed or compared as bytes. The byte order is the big-endian order of I2OSP and OS2IP {{RFC8017}}, which is the order used by {{I-D.irtf-cfrg-bbs-signatures}} and {{I-D.ietf-cose-bls-key-representations}}. Other specifications for prime-order groups encode scalars in little-endian order, {{RFC8032}} among them, so an implementation working with both needs to convert.

This document does not define a distinct encoding for the zero scalar. Whether a protocol accepts it is a protocol-level decision, discussed in {{zero-scalar}}.

## Point Serialization  {#point-serialization-procedure}

This section defines point_to_octets_G1 and point_to_octets_G2. point_to_octets_G1 takes an element of G_1, which is a point on E, and point_to_octets_G2 takes an element of G_2, which is a point on E'; each returns a byte string. Both are given by the single procedure below, stated for a point P = (x, y) and the parameters n and m of {{point-serialization-params}}. This procedure uses the I2OSP function defined in {{RFC8017}}.

1. Compute the metadata bits C_bit, I_bit, and S_bit, as follows:
   - C_bit is 1 if point compression should be used, otherwise it is 0.
   - I_bit is 1 if P is the point at infinity, otherwise it is 0.
   - S_bit is 0 if P is the point at infinity or if point compression is not used. Otherwise (i.e., when point compression is used and P is not the point at infinity), if P is a point on E, S_bit = sign_GF_p(y), else if P is a point on E', S_bit = sign_GF_p^m(y).
2. Let m_byte = (C_bit * 2^7) + (I_bit * 2^6) + (S_bit * 2^5).
3. Let x_string be the serialization of x, which is defined as follows:
   - If P is the point at infinity on E, let x_string = I2OSP(0, n).
   - If P is a point on E other than the point at infinity, then x is an element of GF(p), i.e., an integer in the inclusive range [0, p - 1]. In this case, let x_string = I2OSP(x, n).
   - If P is the point at infinity on E', let x_string = I2OSP(0, m\*n).
   - If P is a point on E' other than the point at infinity, then x can be represented as (x_0, ..., x_{m-1}) where each x_i is an element of GF(p). In this case, let x_string = I2OSP(x_{m-1}, n) concatenated with I2OSP(x_{m-2}, n), ..., concatenated with I2OSP(x_0, n) (i.e., coefficients in decreasing index order). Notice that in all of the above cases, the 3 most significant bits of x_string[0] are guaranteed to be 0.
4. If point compression is used, let y_string be the empty string. Otherwise (i.e., when point compression is not used), let y_string be the serialization of y, which is defined in Step 3.
5. Let s_string be the concatenation of x_string and y_string.
6. Set s_string[0] = x_string[0] OR m_byte, where OR is computed bitwise. After this operation, the most significant bit of s_string[0] equals C_bit, the next bit equals I_bit, and the next equals S_bit. (This is true because the three most significant bits of x_string[0] are guaranteed to be zero, as discussed above.)
7. Return s_string.

## Point Deserialization  {#point-deserialization-procedure}

This section defines octets_to_point_G1 and octets_to_point_G2. Each takes a byte string and returns an element of G_1 or of G_2 respectively, or INVALID. A string that deserializes to a point of E or E' that is not in the group is INVALID, so a value returned by these procedures is a group element and a caller does not have to obtain one by a further step.

This document does not define a deserialization procedure that stops at E or E'. G_1 and G_2 are the groups on which the pairing of {{pairing}} is defined, and they are what the specifications citing this document exchange. Section 5.3 of {{I-D.irtf-cfrg-bls-signature}} requires the subgroup check of conforming implementations for the same reason, and Section 6.2 of {{I-D.irtf-cfrg-bbs-signatures}} describes the combined operation as the one that libraries provide.

The procedure below is stated once, for a string s_string and the parameters n and m of {{point-serialization-params}}. It uses the OS2IP function defined in {{RFC8017}} and the OS2FE function defined in {{point-serialization-params}}. octets_to_point_G1 is the case in which step 2 determines the curve E, and octets_to_point_G2 the case in which it determines E'.

Every GF(p) coefficient recovered by this procedure MUST be an integer in the inclusive range [0, p - 1]. A byte string that encodes a coordinate, or a coefficient of a coordinate, as a value greater than or equal to p is not a canonical encoding of a field element; implementations MUST return INVALID in that case. This applies to coordinates on E, recovered with OS2IP, as well as to the coefficients of coordinates on E', recovered with OS2FE.

1. Let m_byte = s_string[0] AND 0xE0, where AND is computed bitwise. In other words, the three most significant bits of m_byte equal the three most significant bits of s_string[0], and the remaining bits are 0. If m_byte equals any of 0x20, 0x60, or 0xE0, return INVALID. Otherwise:
   - Let C_bit equal the most significant bit of m_byte,
   - Let I_bit equal the second most significant bit of m_byte, and
   - Let S_bit equal the third most significant bit of m_byte.
2. If C_bit is 1:
   - If s_string has length n bytes, the output point is on the curve E.
   - If s_string has length m\*n bytes, the output point is on the curve E'.
   - If s_string has any other length, return INVALID.

   If C_bit is 0:
   - If s_string has length 2n bytes, the output point is on E.
   - If s_string has length 2\*m\*n bytes, the output point is on E'.
   - If s_string has any other length, return INVALID.
3. Let s_string[0] = s_string[0] AND 0x1F, where AND is computed bitwise. In other words, set the three most significant bits of s_string[0] to 0.
4. If I_bit is 1:
   - If s_string is not the all zeros string, return INVALID.
   - Otherwise (i.e., if s_string is the all zeros string), return the identity element of the group determined in step 2.

   Otherwise, I_bit must be 0. Continue.

5. If C_bit is 0:
   - Let x_string be the first half of s_string.
   - Let y_string be the last half of s_string.

   - If the curve that was determined in step 2 is E:

      - Let x = OS2IP(x_string).
      - Let y = OS2IP(y_string).
      - If the point P = (x, y) is not a valid point on E, return INVALID.

   - Otherwise, (i.e., when the curve that was determined in step 2 is E'):

      - Let x = OS2FE(x_string, n).
      - Let y = OS2FE(y_string, n).
      - If the point P = (x, y) is not a valid point on E', return INVALID.

   Let P = (x, y) and continue at step 8.

   Otherwise, C_bit must be 1. Continue.

6. Let x_string be s_string.

    - If the curve determined in step 2 is E:
      - Let x = OS2IP(x_string).
      - Let y2 = the right-hand side of the curve equation for E (given in {{secure_params}} for the curve in question), evaluated at x, in GF(p).
      - If y2 is not square in GF(p), return INVALID.
      - Otherwise, let y = sqrt(y2) in GF(p) and let Y_bit = sign_GF_p(y).

    - Otherwise, (i.e., when the curve that was determined in step 2 is E'):
      - Let x = OS2FE(x_string, n).
      - Let y2 = the right-hand side of the curve equation for E' (given in {{secure_params}} for the curve in question), evaluated at x, in GF(p^m).
      - If y2 is not square in GF(p^m), return INVALID.
      - Otherwise, let y = sqrt(y2) in GF(p^m) and let Y_bit = sign_GF_p^m(y).

7. If S_bit equals Y_bit, let P = (x, y). Otherwise, let P = (x, -y).

8. If the curve determined in step 2 is E and subgroup_check_G1(P) returns FALSE, return INVALID. If the curve determined in step 2 is E' and subgroup_check_G2(P) returns FALSE, return INVALID. Otherwise, return P.

Note that OS2FE outputs field elements in the towered representation of the curve in question:

- For BLS12_381, OS2FE(x_string, n) = (x'_0, x'_1) = x'_0 + x'_1 * u
- For BLS48_581, OS2FE(x_string, n) = (x'_0, ..., x'_7) = x'_0 + x'_1 * u + x'_2 * v + x'_3 * u * v + x'_4 * w + x'_5 * u * w + x'_6 * v * w + x'_7 * u * v * w

### Subgroup Membership  {#subgroup-check}

subgroup_check_G1(P) takes a point P on E and returns TRUE if P is an element of G_1 and FALSE otherwise. subgroup_check_G2(Q) does the same for a point Q on E' and the group G_2. Both are used by {{point-deserialization-procedure}}, and both are specified here as operations in their own right, because a point can also arise from point addition or scalar multiplication rather than from a byte string, and a caller may need to check such a point.

For every curve in this document, r^2 does not divide the order of E(GF(p)) or the order of E'(GF(p^m)); this can be seen from the values of r, h and h' given in {{secure_params}}. Consequently a point of order dividing r is an element of G_1, or of G_2, respectively, and both checks can be carried out as follows:

- subgroup_check_G1(P) returns TRUE if [r]P is the point at infinity on E, and FALSE otherwise.
- subgroup_check_G2(Q) returns TRUE if [r]Q is the point at infinity on E', and FALSE otherwise.

Faster tests are known for particular curves. Any method that decides the same predicate may be used.

These checks apply to all three curves in this document. Note that for BN462 the cofactor h of E(GF(p)) is 1, so every point of E(GF(p)) is an element of G_1 and subgroup_check_G1 always returns TRUE; the cofactor h' of E'(GF(p^2)) is not 1, so subgroup_check_G2 remains meaningful.

## Applicability to BN462  {#bn462-applicability}

This document does not specify a point encoding for BN462. Scalar serialization, defined in {{scalar-serialization}}, and the subgroup checks of {{subgroup-check}} are unaffected and apply to BN462 as well.

The coordinate encoding of {{point-serialization-procedure}} -- coordinates as fixed-length big-endian integers, with the coefficients of an element of GF(p^m) in decreasing index order -- carries over to BN462 unchanged. What does not fit is the placement of the metadata bits. BN462 has a 462-bit characteristic p, so its canonical GF(p) representation occupies n = ceil(462 / 8) = 58 bytes, that is 464 bits. This leaves 2 unused bits in the leading byte of a serialized coordinate, one short of the three (C_bit, I_bit, S_bit) that the scheme above places there. {{bn462-serialization-notes}} describes two ways of accommodating that: carrying the metadata in a byte of its own, following the general pattern of {{SEC1}}, which leaves the rest of the format unchanged; or restricting the format to uncompressed points, which need only C_bit and I_bit and therefore fit in the two bits available.

This document specifies neither. The format above is specified here because it is already widely used in applications, and because specifications depend on it: it originates with {{ZCashRep}} and is relied upon by {{I-D.irtf-cfrg-bbs-signatures}} and {{I-D.ietf-cose-bls-key-representations}}. For BN462 neither consideration holds: no specification examined requires a BN462 point encoding, and the implementations that do emit BN462 points have not converged on one of the variants above. Choosing among them would therefore be encoding design rather than the recording of established practice, and encoding design is outside the purpose of this document, which is the specification of curve parameters.

Implementations that nevertheless need to exchange BN462 points may find {{bn462-serialization-notes}} useful. It records, informatively, the encodings that existing implementations use and the alternatives that have been considered. It defines no format.

## Requirements on Calling Protocols  {#calling-protocol-requirements}

The procedures above leave three decisions to the protocol that uses them. A specification that adopts this format SHOULD state its answer to each, rather than relying on an implicit default.

### Accepted Point Form  {#accepted-point-form}

Both a compressed and an uncompressed form are defined. The compressed form is RECOMMENDED for values that are transmitted: it is the form {{ZCashRep}} places on the wire, and the form used by {{I-D.irtf-cfrg-bbs-signatures}}, {{I-D.ietf-cose-bls-key-representations}} and {{I-D.irtf-cfrg-bls-signature}}. The uncompressed form is retained because it remains in use for stored values, such as verification keys that are validated once, where recovering y from x on every use is not worth the saved space.

A protocol SHOULD state which forms it accepts. Accepting both means that one point has two encodings, which matters wherever an encoded point is hashed or compared as a byte string.

### Identity Element  {#identity-point-handling}

{{point-serialization-procedure}} and {{point-deserialization-procedure}} define a byte representation for the identity element of G_1 and of G_2, via the I_bit, and deserialization returns it as it does any other group element. Whether a calling protocol should accept it depends on that protocol's own semantics and threat model, not on the wire format: some protocols (e.g., certain zero-knowledge proof constructions) legitimately reference the identity element as part of a public statement, while for others it cannot arise in normal operation.

This document therefore defines two deserialization behaviors, and protocols using this document's serialization format SHOULD explicitly state which one they require, rather than relying on an implicit default:

- **Reject identity (RECOMMENDED default)**: after running {{point-deserialization-procedure}}, if the resulting point is the identity element, treat the overall result as INVALID. This is the appropriate choice for protocols where the identity element is not an expected input in normal operation.
- **Allow identity**: use the result of {{point-deserialization-procedure}} as-is, including when it is the identity element. This is appropriate for protocols with a specific, documented need to represent the identity element.

Section 5.2 of {{I-D.irtf-cfrg-bls-signature}} is an example of the first choice, and gives a protocol-level reason for it: the secret key corresponding to an identity public key is zero, and under such a key the identity element is a valid signature on every message.

### Zero Scalar  {#zero-scalar}

{{scalar-serialization}} accepts the zero scalar, and a protocol using it SHOULD state whether it does the same.

This is a separate decision from the one above. Section 3.1 of {{RFC9591}} rejects the identity element when deserializing a group element while accepting zero scalars, so the two choices are made independently in at least one existing specification.

# Security Considerations  {#security-considerations}

The recommended pairing-friendly curves are selected by considering the exTNFS proposed by Kim et al. in 2016 {{KB16}} and they are categorized in each security level in accordance with {{BD18}}. Implementers who will newly develop pairing-based cryptography applications SHOULD use the recommended parameters. As of 2026, as far as we've investigated the top cryptographic conferences, there are no fatal attacks that significantly reduce the security of pairing-friendly curves beyond what is already reflected in the security estimates cited in this memo ({{BD18}}, {{GMT19}}, {{KIK17}}). Continued refinements to the number field sieve and its tower variants (e.g., record discrete-logarithm computations and complexity analyses) have been published since 2020, but these are improvements to known algorithm families already accounted for by the post-exTNFS estimates used here, not new attack types that change the qualitative security picture.

BLS curves of embedding degree 12 typically require a characteristic p of 461 bits or larger to achieve the 128-bit security level {{BD18}}. Note that the security level of BLS12_381, which is adopted by a lot of libraries and applications, is slightly below 128 bits because a 381-bit characteristic is used {{BD18}} {{GMT19}}.

BN254 is used in most of the existing implementations as shown in {{impl}} and {{adoption_status_100bit_security}}, however, BN curves that were estimated as the 128-bit security level before exTNFS including BN254 ensure no more than the 100-bit security level by the effect of exTNFS.

In addition, implementors should be aware of the following points when they implement pairing-based cryptographic applications using recommended curves. Regarding the use case and applications of pairing-based cryptographic applications, please refer {{applications-of-pairing-based-cryptography}}.

In applications such as key agreement protocols, users exchange the elements in G_1 and G_2 as public keys. Such an element has to be so-called subgroup secure {{BCM15}}, that is, it has to have the correct order r. A point obtained from a byte string through {{point-deserialization-procedure}} has already been checked, since those procedures return an element of G_1 or of G_2 or nothing at all. A point obtained in any other way, for instance as the result of point addition or scalar multiplication, has not been, and implementors should apply {{subgroup-check}} to it.

The pairing-based protocols, such as the BLS signatures, use a scalar multiplication in G_1, G_2 and an exponentiation in G_T with the secret key. In order to prevent the leakage of secret key due to side channel attacks, implementors should apply countermeasure techniques such as montgomery ladder {{Montgomery}} {{CF06}} when they implement modules of a scalar multiplication and an exponentiation. Please refer {{Montgomery}} and {{CF06}} for the detailed algorithms of montgomery ladder.

A coordinate that is read from a byte string has to be checked against the order of the field it is claimed to lie in; a coefficient outside that range gives one value several encodings, which can lead to vulnerabilities such as signature forgery {{IEEE1363}}. {{point-deserialization-procedure}} makes this requirement normative for the procedures defined in this document.

Protocol designers using the serialization format in {{point-serialization}} should be deliberate about which of the two identity-element behaviors described in {{identity-point-handling}} their protocol requires, rather than assuming a default. Treating the identity element as an unremarkable, always-valid deserialization result -- when the calling protocol does not actually expect it -- can introduce timing side channels from identity-checking branches. Protocol specifications SHOULD state explicitly whether they require the identity-rejecting or identity-allowing behavior, consistent with their own security assumptions, and SHOULD do the same for the zero scalar ({{zero-scalar}}).

Recommended parameters are affected by the Cheon's attack which is a solving algorithm for the strong DH problem {{Cheon06}}. The mathematical problem that provides the security of the strong DH problem is called ECDLP with Auxiliary Inputs (ECDLPwAI). In ECDLPwAI, given rational points P, [K]P, [K^i]P, for i=1,...,n, then we find a secret K. Since the complexity of ECDLPwAI is given as O(sqrt((r-1)/n + sqrt(n)) where n divides r-1 by using Cheon's algorithm whereas the complexity of ECDLP is given as O(sqrt(r)), the complexity of ECDLPwAI with the ideal value n becomes dramatically smaller than that of ECDLP. Please refer {{Cheon06}} for the details of Cheon's algorithm. Therefore, implementers should be careful when they design cryptographic protocols based on the strong DH problem. For example, in the case of Short Signatures, they can prevent the Cheon's attack by carefully setting the maximum number of queries which corresponds to the parameter n.


# IANA Considerations  {#iana-considerations}

This document has no actions for IANA.


# Acknowledgements  {#acknowledgements}

The authors would like to appreciate a lot of authors including Akihiro Kato for their significant contribution to early versions of this memo. The authors would also like to acknowledge Kim Taechan, Hoeteck Wee, Sergey Gorbunov, Michael Scott, Chloe Martindale as an Expert Reviewer, Watson Ladd, Armando Faz, Rene Struik, and Diego F. Aranha for their valuable comments.


--- back

{::nomarkdown}
<references xmlns:xi="http://www.w3.org/2001/XInclude">
      <name>References</name>
      <references>
        <name>Normative References</name>
        <xi:include href="https://xml2rfc.tools.ietf.org/public/rfc/bibxml/reference.RFC.2119.xml" />
        <xi:include href="https://xml2rfc.tools.ietf.org/public/rfc/bibxml/reference.RFC.8174.xml" />
        <xi:include href="https://xml2rfc.tools.ietf.org/public/rfc/bibxml/reference.RFC.8017.xml" />
        <reference anchor="Ver09">
          <front>
            <title>Optimal Pairings</title>
            <seriesInfo name="DOI" value="10.1109/tit.2009.2034881" />
            <seriesInfo name="IEEE Transactions on Information Theory" value="Vol. 56, pp. 455-461" />
            <author initials="F." surname="Vercauteren" fullname="Frederik Vercauteren">
              <organization />
            </author>
            <date year="2010" month="January" />
          </front>
        </reference>
        <reference anchor="BN05">
          <front>
            <title>Pairing-Friendly Elliptic Curves of Prime Order</title>
            <seriesInfo name="DOI" value="10.1007/11693383_22" />
            <seriesInfo name="Selected Areas in Cryptography" value="pp. 319-331" />
            <author initials="P." surname="Barreto" fullname="Paulo S. L. M. Barreto">
              <organization />
            </author>
            <author initials="M." surname="Naehrig" fullname="Michael Naehrig">
              <organization />
            </author>
            <date year="2006" />
          </front>
        </reference>
        <reference anchor="BLS02">
          <front>
            <title>Constructing Elliptic Curves with Prescribed Embedding Degrees</title>
            <seriesInfo name="DOI" value="10.1007/3-540-36413-7_19" />
            <seriesInfo name="Security in Communication Networks" value="pp. 257-267" />
            <author initials="P." surname="Barreto" fullname="Paulo S. L. M. Barreto">
              <organization />
            </author>
            <author initials="B." surname="Lynn" fullname="Ben Lynn">
              <organization />
            </author>
            <author initials="M." surname="Scott" fullname="Michael Scott">
              <organization />
            </author>
            <date year="2003" />
          </front>
        </reference>
        <reference anchor="KB16">
          <front>
            <title>Extended Tower Number Field Sieve: A New Complexity for the Medium Prime Case</title>
            <seriesInfo name="DOI" value="10.1007/978-3-662-53018-4_20" />
            <seriesInfo name="Advances in Cryptology - CRYPTO 2016" value="pp. 543-571" />
            <author initials="T." surname="Kim" fullname="Taechan Kim">
              <organization />
            </author>
            <author initials="R." surname="Barbulescu" fullname="Razvan Barbulescu">
              <organization />
            </author>
            <date year="2016" />
          </front>
        </reference>
        <reference anchor="BD18">
          <front>
            <title>Updating Key Size Estimations for Pairings</title>
            <seriesInfo name="DOI" value="10.1007/s00145-018-9280-5" />
            <seriesInfo name="Journal of" value="Cryptology" />
            <author initials="R." surname="Barbulescu" fullname="Razvan Barbulescu">
              <organization />
            </author>
            <author initials="S." surname="Duquesne" fullname="Sylvain Duquesne">
              <organization />
            </author>
            <date year="2018" month="January" />
          </front>
        </reference>
        <reference anchor="KIK17">
          <front>
            <title>Secure and Efficient Pairing at 256-Bit Security Level</title>
            <seriesInfo name="DOI" value="10.1007/978-3-319-61204-1_4" />
            <seriesInfo name="Applied Cryptography and Network Security" value="pp. 59-79" />
            <author initials="Y." surname="Kiyomura" fullname="Yutaro Kiyomura">
              <organization />
            </author>
            <author initials="A." surname="Inoue" fullname="Akiko Inoue">
              <organization />
            </author>
            <author initials="Y." surname="Kawahara" fullname="Yuto Kawahara">
              <organization />
            </author>
            <author initials="M." surname="Yasuda" fullname="Masaya Yasuda">
              <organization />
            </author>
            <author initials="T." surname="Takagi" fullname="Tsuyoshi Takagi">
              <organization />
            </author>
            <author initials="T." surname="Kobayashi" fullname="Tetsutaro Kobayashi">
              <organization />
            </author>
            <date year="2017" />
          </front>
        </reference>

        <reference anchor="GMT19">
          <front>
            <title>Cocks–Pinch curves of embedding degrees five to eight and optimal ate pairing computation</title>
            <seriesInfo name="DOI" value="10.1007/s10623-020-00727-w" />
            <seriesInfo name="International Journal of Designs, Codes and Cryptography" value="vol. 88, pp. 1047-1081" />
            <author initials="A." surname="Guillevic">
              <organization />
            </author>
            <author initials="S. " surname="Masson" fullname="Simon Masson">
              <organization />
            </author>
            <author initials="E." surname="Thome" fullname="Emmanuel Thome">
              <organization />
            </author>
            <date year="2019" />
          </front>
        </reference>

        <reference anchor="NIST">
          <front>
            <title>NIST special publication 800-57 part 1 (revised) : Recommendation for key management, part 1: General (revised)</title>
            <seriesInfo name="National Institute of Standards and Technology" value="(NIST)" />
            <author initials="E." surname="Barker">
              <organization />
            </author>
            <date year="2020" />
          </front>
        </reference>

      </references>



      <references>
        <name>Informative References</name>
        <xi:include href="https://xml2rfc.tools.ietf.org/public/rfc/bibxml/reference.RFC.5091.xml" />
        <xi:include href="https://xml2rfc.tools.ietf.org/public/rfc/bibxml/reference.RFC.6508.xml" />
        <xi:include href="https://xml2rfc.tools.ietf.org/public/rfc/bibxml/reference.RFC.6539.xml" />
        <xi:include href="https://xml2rfc.tools.ietf.org/public/rfc/bibxml/reference.RFC.6509.xml" />

        <reference anchor="CF06">
          <front>
            <title>Handbook of Elliptic and Hyperelliptic Curve Cryptography</title>
            <seriesInfo name="DOI" value="10.1201/9780367801625" />
            <seriesInfo name="Chapman and Hall" value="CRC" />
            <author initials="H." surname="Cohen" fullname="Henri Cohen">
              <organization />
            </author>
            <author initials="G." surname="Frey" fullname="Gerhard Frey">
              <organization />
            </author>
            <date year="2006" />
          </front>
        </reference>

        <reference anchor="BCGMMW20">
          <front>
            <title>ZEXE: Enabling Decentralized Private Computation</title>
            <seriesInfo name="DOI" value="10.1109/SP40000.2020.00050" />
            <seriesInfo name="IEEE Symposium on Security and Privacy" value="2020" />
            <author initials="S." surname="Bowe" fullname="Sean Bowe">
              <organization />
            </author>
            <author initials="A." surname="Chiesa" fullname="Alessandro Chiesa">
              <organization />
            </author>
            <author initials="M." surname="Green" fullname="Matthew Green">
              <organization />
            </author>
            <author initials="I." surname="Miers" fullname="Ian Miers">
              <organization />
            </author>
            <author initials="P." surname="Mishra" fullname="Pratyush Mishra">
              <organization />
            </author>
            <author initials="H." surname="Wu" fullname="Howard Wu">
              <organization />
            </author>
            <date year="2020" />
          </front>
        </reference>

        <reference anchor="bls12377js" target="https://github.com/celo-org/bls12377js">
          <front>
            <title>bls12377js</title>
            <author>
              <organization>The Celo Foundation</organization>
            </author>
            <date year="2019" />
          </front>
        </reference>

        <reference anchor="SAKKE">
          <front>
            <title>Security of the mission critical service (Release 15)</title>
            <seriesInfo name="3GPP TS" value="33.180 15.3.0" />
            <author>
              <organization>3GPP</organization>
            </author>
            <date year="2018" />
          </front>
        </reference>
        <reference anchor="ISOIEC11770-3">
          <front>
            <title>ISO/IEC 11770-3:2015</title>
            <seriesInfo name="ISO/IEC" value="Information technology -- Security techniques -- Key management -- Part 3: Mechanisms using asymmetric techniques" />
            <author>
              <organization>ISO/IEC</organization>
            </author>
            <date year="2015" />
          </front>
        </reference>
        <reference anchor="Joux00">
          <front>
            <title>A One Round Protocol for Tripartite Diffie-Hellman</title>
            <seriesInfo name="DOI" value="10.1007/10722028_23" />
            <seriesInfo name="Lecture Notes in Computer Science" value="pp. 385-393" />
            <author initials="A." surname="Joux" fullname="Antoine Joux">
              <organization />
            </author>
            <date year="2000" />
          </front>
        </reference>
        <reference anchor="CCS07">
          <front>
            <title>Identity-based key agreement protocols from pairings</title>
            <seriesInfo name="DOI" value="10.1007/s10207-006-0011-9" />
            <seriesInfo name="International Journal of Information Security" value="Vol. 6, pp. 213-241" />
            <author initials="L." surname="Chen" fullname="L. Chen">
              <organization />
            </author>
            <author initials="Z." surname="Cheng" fullname="Z. Cheng">
              <organization />
            </author>
            <author initials="N." surname="Smart" fullname="N. P. Smart">
              <organization />
            </author>
            <date year="2007" month="January" />
          </front>
        </reference>
        <reference anchor="FSU10">
          <front>
            <title>Ephemeral Key Leakage Resilient and Efficient ID-AKEs That Can Share Identities, Private and Master Keys</title>
            <seriesInfo name="DOI" value="10.1007/978-3-642-17455-1_12" />
            <seriesInfo name="Lecture Notes in Computer Science" value="pp. 187-205" />
            <author initials="A." surname="Fujioka" fullname="Atsushi Fujioka">
              <organization />
            </author>
            <author initials="K." surname="Suzuki" fullname="Koutarou Suzuki">
              <organization />
            </author>
            <author initials="B." surname="Ustaoglu" fullname="Berkant Ustaoglu">
              <organization />
            </author>
            <date year="2010" />
          </front>
        </reference>
        <reference anchor="M-Pin" target="https://www.miracl.com/miracl-labs/m-pin-a-multi-factor-zero-knowledge-authentication-protocol">
          <front>
            <title>M-Pin: A Multi-Factor Zero Knowledge Authentication Protocol</title>
            <author initials="M." surname="Scott">
              <organization />
            </author>
            <date year="2019" month="July" />
          </front>
        </reference>
        <reference anchor="TPM" target="https://trustedcomputinggroup.org/resource/tpm-library-specification/">
          <front>
            <title>Trusted Platform Module Library Specification, Family \"2.0\", Level 00, Revision 01.38</title>
            <author>
              <organization>Trusted Computing Group (TCG)</organization>
            </author>
            <date />
          </front>
        </reference>
        <reference anchor="FIDO" target="https://fidoalliance.org/specs/fido-v2.0-rd-20180702/fido-ecdaa-algorithm-v2.0-rd-20180702.html">
          <front>
            <title>FIDO ECDAA Algorithm - FIDO Alliance Review Draft 02</title>
            <author initials="R." surname="Lindemann">
              <organization />
            </author>
            <date />
          </front>
        </reference>
        <reference anchor="W3C" target="https://www.w3.org/TR/webauthn/">
          <front>
            <title>Web Authentication: An API for accessing Public Key Credentials Level 1 - W3C Recommendation</title>
            <author initials="E." surname="Lundberg">
              <organization />
            </author>
            <date />
          </front>
        </reference>
        <reference anchor="EPID" target="https://software.intel.com/en-us/download/intel-sgx-intel-epid-provisioning-and-attestation-services">
          <front>
            <title>Intel (R) SGX: Intel (R) EPID Provisioning and Attestation Services</title>
            <author>
              <organization>Intel Corporation</organization>
            </author>
            <date />
          </front>
        </reference>
        <reference anchor="BL10">
          <front>
            <title>Enhanced Privacy ID from Bilinear Pairing for Hardware Authentication and Attestation</title>
            <seriesInfo name="DOI" value="10.1109/socialcom.2010.118" />
            <seriesInfo name="2010 IEEE Second International Conference on Social" value="Computing" />
            <author initials="E." surname="Brickell" fullname="Ernie Brickell">
              <organization />
            </author>
            <author initials="J." surname="Li" fullname="Jiangtao Li">
              <organization />
            </author>
            <date year="2010" month="August" />
          </front>
        </reference>

        <reference anchor="Zcash" target="https://z.cash/technology/zksnarks.html">
          <front>
            <title>What are zk-SNARKs?</title>
            <author initials="R." surname="Lindemann">
              <organization />
            </author>
            <date />
          </front>
        </reference>

        <reference anchor="ZCashRep" target="https://zips.z.cash/protocol/protocol.pdf#blspairing">
          <front>
            <title>Zcash Protocol Specification</title>
            <author initials="D-E." surname="Hopwood">
              <organization />
            </author>
            <author initials="S." surname="Bowe">
              <organization />
            </author>
            <author initials="T." surname="Hornby">
              <organization />
            </author>
            <author initials="N." surname="Wilcox">
              <organization />
            </author>
            <date year="2026" />
          </front>
          <seriesInfo name="Section" value="5.4.9.2" />
        </reference>

        <reference anchor="Cloudflare" target="https://blog.cloudflare.com/geo-key-manager-how-it-works/">
          <front>
            <title>Geo Key Manager: How It Works</title>
            <author initials="N." surname="Sullivan">
              <organization />
            </author>
            <date />
          </front>
        </reference>

        <xi:include href="https://xml2rfc.tools.ietf.org/public/rfc/bibxml3/reference.I-D.draft-irtf-cfrg-bls-signature-07.xml" />
        <xi:include href="https://xml2rfc.tools.ietf.org/public/rfc/bibxml/reference.RFC.8032.xml" />
        <xi:include href="https://xml2rfc.tools.ietf.org/public/rfc/bibxml/reference.RFC.9591.xml" />
        <xi:include href="https://xml2rfc.tools.ietf.org/public/rfc/bibxml/reference.RFC.9380.xml" />
        <xi:include href="https://xml2rfc.tools.ietf.org/public/rfc/bibxml3/reference.I-D.draft-irtf-cfrg-bbs-signatures-10.xml" />
        <xi:include href="https://xml2rfc.tools.ietf.org/public/rfc/bibxml3/reference.I-D.draft-ietf-cose-bls-key-representations-08.xml" />

        <reference anchor="Ethereum" target="https://medium.com/prysmatic-labs/ethereum-2-0-development-update-17-prysmatic-labs-ed5bcf82ec00">
          <front>
            <title>Ethereum 2.0 Development Update #17 - Prysmatic Labs</title>
            <author initials="R." surname="Jordan">
              <organization />
            </author>
            <date />
          </front>
        </reference>
        <reference anchor="Algorand" target="https://medium.com/algorand/digital-signatures-for-blockchains-5820e15fbe95">
          <front>
            <title>Efficient and Secure Digital Signatures for Proof-of-Stake Blockchains</title>
            <author initials="S." surname="Gorbunov">
              <organization />
            </author>
            <date />
          </front>
        </reference>
        <reference anchor="Chia" target="https://github.com/Chia-Network/bls-signatures">
          <front>
            <title>BLS signatures in C++, using the relic toolkit</title>
            <author>
              <organization>Chia Network</organization>
            </author>
            <date />
          </front>
        </reference>
        <reference anchor="DFINITY" target="https://dfinity.org/pdf-viewer/library/dfinity-consensus.pdf">
          <front>
            <title>DFINITY Technology Overview Series Consensus System Rev. 1</title>
            <author initials="D." surname="Williams">
              <organization />
            </author>
            <date>n.d.</date>
          </front>
        </reference>

<reference anchor="IEEE1363">
  <front>
    <title>IEEE Standard Specifications for Public-Key Cryptography</title>
    <author>
      <organization />
    </author>
    <date year="2000" />
  </front>
  <seriesInfo name="IEEE" value="standard" />
  <seriesInfo name="DOI" value="10.1109/IEEESTD.2000.92292" />
</reference>

<reference anchor="SEC1" target="https://www.secg.org/sec1-v2.pdf">
  <front>
    <title>SEC 1: Elliptic Curve Cryptography</title>
    <author>
      <organization>Standards for Efficient Cryptography Group (SECG)</organization>
    </author>
    <date year="2009" />
  </front>
</reference>

        <reference anchor="Cheon06">
          <front>
            <title>Security Analysis of the Strong Diffie-Hellman Problem</title>
            <seriesInfo name="DOI" value="10.1007/11761679_1" />
            <seriesInfo name="EUROCRYPT 2006" value="pp. 1-11" />
            <author initials="J. H. " surname="Cheon" fullname="Jung Hee Cheon">
              <organization />
            </author>
            <date year="2006" />
          </front>
        </reference>


<reference anchor="ECRYPT">
          <front>
            <title>Final Report on Main Computational Assumptions in Cryptography</title>
            <author>
              <organization>ECRYPT</organization>
            </author>
            <date />
          </front>
        </reference>
        <reference anchor="Pollard78">
          <front>
            <title>Monte Carlo methods for index computation $({\rm mod}\ p)$</title>
            <seriesInfo name="DOI" value="10.1090/s0025-5718-1978-0491431-9" />
            <seriesInfo name="Mathematics of Computation" value="Vol. 32, pp. 918-918" />
            <author initials="J." surname="Pollard" fullname="J. M. Pollard">
              <organization />
            </author>
            <date year="1978" month="September" />
          </front>
        </reference>
        <reference anchor="HR83">
          <front>
            <title>Fast Computation of Discrete Logarithms in GF (q)</title>
            <seriesInfo name="DOI" value="10.1007/978-1-4757-0602-4_1" />
            <seriesInfo name="Advances in Cryptology" value="pp. 3-13" />
            <author initials="M." surname="Hellman" fullname="Martin E. Hellman">
              <organization />
            </author>
            <author initials="J." surname="Reyneri" fullname="Justin M. Reyneri">
              <organization />
            </author>
            <date year="1983" />
          </front>
        </reference>

        <reference anchor="mcl" target="https://github.com/herumi/mcl">
          <front>
            <title>mcl - A portable and fast pairing-based cryptography library</title>
            <author initials="S." surname="Mitsunari">
              <organization />
            </author>
            <date year="2016" />
          </front>
        </reference>
        <reference anchor="BLS12_381" target="https://electriccoin.co/blog/new-snark-curve/">
          <front>
            <title>BLS12-381: New zk-SNARK Elliptic Curve Construction</title>
            <author initials="S." surname="Bowe">
              <organization />
            </author>
            <date />
          </front>
        </reference>
        <reference anchor="ISOIEC15946-5">
          <front>
            <title>ISO/IEC 15946-5:2022</title>
            <seriesInfo name="ISO/IEC" value="Information technology -- Security techniques -- Cryptographic techniques based on elliptic curves -- Part 5: Elliptic curve generation" />
            <author>
              <organization>ISO/IEC</organization>
            </author>
            <date year="2022" />
          </front>
        </reference>

        <reference anchor="MIRACL" target="https://github.com/miracl/core">
          <front>
            <title>The MIRACL Core Cryptographic Library</title>
            <author>
              <organization>MIRACL Ltd.</organization>
            </author>
            <date year="2019" />
          </front>
        </reference>

        <reference anchor="libsnark" target="https://github.com/zcash/libsnark">
          <front>
            <title>libsnark: a C++ library for zkSNARK proofs</title>
            <author>
              <organization>SCIPR Lab</organization>
            </author>
            <date year="2012" />
          </front>
        </reference>
        <reference anchor="zkcrypto" target="https://github.com/zkcrypto/pairing">
          <front>
            <title>zkcrypto - Pairing-friendly elliptic curve library</title>
            <author>
              <organization>zkcrypto</organization>
            </author>
            <date year="2017" />
          </front>
        </reference>
        <reference anchor="CIRCL" target="https://github.com/cloudflare/circl">
          <front>
            <title>CIRCL: Cloudflare Interoperable, Reusable Cryptographic Library</title>
            <author>
              <organization>Cloudflare</organization>
            </author>
            <date year="2019" />
          </front>
        </reference>
        <reference anchor="PBC" target="https://crypto.stanford.edu/pbc/">
          <front>
            <title>PBC Library - The Pairing-Based Cryptography Library</title>
            <author initials="B." surname="Lynn">
              <organization />
            </author>
            <date year="2006" />
          </front>
        </reference>
        <reference anchor="RELIC" target="https://github.com/relic-toolkit/relic">
          <front>
            <title>RELIC is an Efficient LIbrary for Cryptography</title>
            <author initials="C.P.L." surname="Gouvea">
              <organization />
            </author>
            <date year="2013" />
          </front>
        </reference>
        <reference anchor="TEPLA" target="http://www.cipher.risk.tsukuba.ac.jp/tepla/index_e.html">
          <front>
            <title>TEPLA: University of Tsukuba Elliptic Curve and Pairing Library</title>
            <author>
              <organization>University of Tsukuba</organization>
            </author>
            <date year="2013" />
          </front>
        </reference>

        <reference anchor="AMCL" target="https://github.com/apache/incubator-milagro-crypto">
          <front>
            <title>The Apache Milagro Cryptographic Library (AMCL)</title>
            <author>
              <organization>The Apache Software Foundation</organization>
            </author>
            <date year="2016" />
          </front>
        </reference>

        <reference anchor="Intel-IPP" target="https://software.intel.com/en-us/ipp-crypto-reference-arithmetic-of-the-group-of-elliptic-curve-points">
          <front>
            <title>Developer Reference for Intel Integrated Performance Primitives Cryptography 2019</title>
            <author>
              <organization>Intel Corporation</organization>
            </author>
            <date year="2018" />
          </front>
        </reference>
        <reference anchor="BLS48" target="https://github.com/mk-math-kyushu/bls48">
          <front>
            <title>bls48 - C++ library for Optimal Ate Pairing on BLS48</title>
            <author>
              <organization>Kyushu University</organization>
            </author>
            <date year="2017" />
          </front>
        </reference>

        <reference anchor="NASKM08">
          <front>
            <title>Integer Variable X-Based Ate Pairing</title>
            <seriesInfo name="DOI" value="10.1007/978-3-540-85538-5_13" />
            <seriesInfo name="Pairing 2008" value="pp. 178-191" />
            <author initials="Y." surname="Nogami" fullname="Yasuyuki Nogami">
              <organization />
            </author>
            <author initials="M." surname="Akane" fullname="Masataka Akane">
              <organization />
            </author>
            <author initials="Y." surname="Sakemi" fullname="Yumi Sakemi">
              <organization />
            </author>
            <author initials="H." surname="Kato" fullname="Hidehiro Kato">
              <organization />
            </author>
            <author initials="Y." surname="Morikawa" fullname="Yoshitaka Morikawa">
              <organization />
            </author>
            <date year="2008" />
          </front>
        </reference>

        <reference anchor="DSD07">
          <front>
            <title>Implementing Cryptographic Pairings over Barreto-Naehrig Curves</title>
            <seriesInfo name="DOI" value="10.1007/978-3-540-73489-5_10" />
            <seriesInfo name="Pairing 2007" value="pp. 197-207" />
            <author initials="A. J." surname="Devegili" fullname="Augusto Jun Devegili">
              <organization />
            </author>
            <author initials="M." surname="Scott" fullname="Michael Scott">
              <organization />
            </author>
            <author initials="R." surname="Dahab" fullname="Ricard Dahab">
              <organization />
            </author>
            <date year="2007" />
          </front>
        </reference>

        <reference anchor="BGMORT10">
          <front>
            <title>High-Speed Software Implementation of the Optimal Ate Pairing over Barreto–Naehrig Curves</title>
            <seriesInfo name="DOI" value="10.1007/978-3-642-17455-1_2" />
            <seriesInfo name="Pairing 2010" value="pp. 21-39" />
            <author initials="J." surname="Beuchat" fullname="Jean-Luc Beuchat">
              <organization />
            </author>
            <author initials="J." surname="González-Díaz" fullname="Jorge E. González-Díaz">
              <organization />
            </author>
            <author initials="S." surname="Mitsunari" fullname="Shigeo Mitsunari">
              <organization />
            </author>
            <author initials="E." surname="Okamoto" fullname="Eiji Okamoto">
              <organization />
            </author>
            <author initials="F." surname="Rodríguez-Henríquez" fullname="Francisco Rodríguez-Henríquez">
              <organization />
            </author>
            <author initials="T." surname="Teruya" fullname="Tadanori Teruya">
              <organization />
            </author>
            <date year="2010" />
          </front>
        </reference>

        <reference anchor="SG19" target="https://eprint.iacr.org/2018/193.pdf">
          <front>
            <title>A New Family of Pairing-Friendly elliptic curves</title>
            <seriesInfo name="Cryptology ePrint Archive" value="Report 2019/193" />
            <author initials="M." surname="Scott">
              <organization />
            </author>
            <author initials="A." surname="Guillevic">
              <organization />
            </author>
            <date year="2019" />
          </front>
        </reference>

        <reference anchor="BCM15" target="https://eprint.iacr.org/2015/247.pdf">
          <front>
            <title>Subgroup security in pairing-based cryptography</title>
            <seriesInfo name="Cryptology ePrint Archive" value="Report 2015/247" />
            <author initials="P. S. L. M." surname="Barreto">
              <organization />
            </author>
            <author initials="C." surname="Costello">
              <organization />
            </author>
            <author initials="R." surname="Misoczki">
              <organization />
            </author>
            <author initials="M." surname="Naehrig">
              <organization />
            </author>
            <author initials="G. C. C. F. " surname="Pereira">
              <organization />
            </author>
            <author initials="G. " surname="Zanon">
              <organization />
            </author>

            <date year="2015" />
          </front>
        </reference>

        <reference anchor="Montgomery" target="https://www.ams.org/journals/mcom/1987-48-177/S0025-5718-1987-0866113-7/S0025-5718-1987-0866113-7.pdf">
          <front>
            <title>Speeding the Pollard and Elliptic Curve Methods of Factorization</title>
            <seriesInfo name="MATHEMATICS OF COMPUTATION" value=", January" />
            <author initials="P." surname="Montgomery">
              <organization />
            </author>
            <date year="1987" />
          </front>
        </reference>

        <reference anchor="MAF19" target="https://doi.org/10.1504/IJACT.2020.107167">
          <front>
            <title>Computing the Optimal Ate Pairing over Elliptic Curves with Embedding Degrees 54 and 48 at the 256-bit security level</title>
            <seriesInfo name="International Journal of Applied Cryptography" value="vol. 4, no. 1, pp. 45-59" />
            <author initials="N.B." surname="Mbang">
              <organization />
            </author>
            <author initials="D.F." surname="Aranha">
              <organization />
            </author>
            <author initials="E." surname="Fouotsa">
              <organization />
            </author>
            <date year="2020" />
          </front>
        </reference>

        <reference anchor="KSS08">
          <front>
            <title>Constructing Brezing-Weng Pairing-Friendly Elliptic Curves Using Elements in the Cyclotomic Field</title>
            <seriesInfo name="DOI" value="10.1007/978-3-540-85538-5_9" />
            <seriesInfo name="Pairing 2008" value="pp. 126-135" />
            <author initials="E." surname="Kachisa">
              <organization />
            </author>
            <author initials="E." surname="Schaefer">
              <organization />
            </author>
            <author initials="M." surname="Scott">
              <organization />
            </author>
            <date year="2008" />
          </front>
        </reference>

        <reference anchor="AFKMR12">
          <front>
            <title>Implementing Pairings at the 192-Bit Security Level</title>
            <seriesInfo name="DOI" value="/10.1007/978-3-642-36334-4_11" />
            <seriesInfo name="Pairing 2012" value="pp. 177-195" />
            <author initials="D.F." surname="Aranha">
              <organization />
            </author>
            <author initials="L." surname="Fuentes-Castaneda">
              <organization />
            </author>
            <author initials="E." surname="Knapp">
              <organization />
            </author>
            <author initials="A." surname="Menezes">
              <organization />
            </author>
            <author initials="F." surname="Rodríguez-Henríquez">
              <organization />
            </author>
            <date year="2012" />
          </front>
        </reference>

        <reference anchor="FM19" target="https://eprint.iacr.org/2019/555.pdf">
          <front>
            <title>Optimal TNFS-secure pairings on elliptic curves with composite embedding degree</title>
            <seriesInfo name="Cryptology ePrint Archive" value="Report 2019/555" />
            <author initials="G." surname="Fotiadis">
              <organization />
            </author>
            <author initials="C." surname="Martindale">
              <organization />
            </author>
            <date year="2019" />
          </front>
        </reference>

        <reference anchor="FK18" target="https://eprint.iacr.org/2018/1017.pdf">
          <front>
            <title>TNFS Resistant Families of Pairing-Friendly Elliptic Curves</title>
            <seriesInfo name="Cryptology ePrint Archive" value="Report 2018/1017" />
            <author initials="G." surname="Fotiadis">
              <organization />
            </author>
            <author initials="E." surname="Konstantinou">
              <organization />
            </author>
            <date year="2018" />
          </front>
        </reference>

        <reference anchor="CLN09" target="https://eprint.iacr.org/2009/615.pdf">
          <front>
            <title>Faster Pairing Computations on Curves with High-Degree Twists</title>
            <seriesInfo name="Cryptology ePrint Archive" value="Report 2009/615" />
            <author initials="C." surname="Costello">
              <organization />
            </author>
            <author initials="T." surname="Lange">
              <organization />
            </author>
            <author initials="M." surname="Naehrig">
              <organization />
            </author>
            <date year="2009" />
          </front>
        </reference>

        <reference anchor="S86">
          <front>
            <title>The arithmetic of elliptic curves</title>
            <seriesInfo name="Springer GTM" value="106" />
            <author initials="J. H." surname="Silverman">
              <organization />
            </author>
            <date year="1986" />
          </front>
        </reference>

        <reference anchor="MNT01">
          <front>
            <title>New explicit conditions of Elliptic Curve Traces under FR reduction</title>
            <seriesInfo name="IEICE Trans. Fundamentals. E84-A(5)" value="pp. 1234-1243" />
            <author initials="A." surname="Miyaji">
              <organization />
            </author>
            <author initials="M." surname="Nakabayashi">
              <organization />
            </author>
            <author initials="S." surname="Takano">
              <organization />
            </author>
            <date year="2001" />
          </front>
        </reference>

        <reference anchor="Freeman06">
          <front>
            <title>Constructing pairing-friendly elliptic curves with embedding degree 10</title>
            <seriesInfo name="DOI" value="10.1007/11792086_32" />
            <seriesInfo name="ANTS 2006" value="pp. 452-465" />
            <author initials="D." surname="Freeman">
              <organization />
            </author>
            <date year="2006" />
          </front>
        </reference>

        <reference anchor="AdjointLib" target="https://github.com/adjoint-io/pairing">
          <front>
            <title>Optimised bilinear pairings over elliptic curves</title>
            <author>
              <organization>Adjoint Inc.</organization>
            </author>
            <date year="2018" />
          </front>
        </reference>

        <reference anchor="AMCLv2" target="https://github.com/miracl/amcl/tree/master/version22">
          <front>
            <title>Old version of the Apache Milagro Cryptographic Library</title>
            <author>
              <organization>The Apache Software Foundation</organization>
            </author>
            <date year="2016" />
          </front>
        </reference>

        <reference anchor="blst" target="https://github.com/supranational/blst">
          <front>
            <title>blst: BLS12-381 signature library</title>
            <author>
              <organization>Supranational LLC</organization>
            </author>
            <date year="2020" />
          </front>
        </reference>

        <reference anchor="gnark-crypto" target="https://github.com/consensys/gnark-crypto">
          <front>
            <title>gnark-crypto: Elliptic curve cryptography and pairing library</title>
            <author>
              <organization>Consensys Software Inc.</organization>
            </author>
            <date year="2020" />
          </front>
        </reference>

        <reference anchor="noble-curves" target="https://github.com/paulmillr/noble-curves">
          <front>
            <title>noble-curves: Audited &amp; minimal JS implementation of elliptic curve cryptography</title>
            <author initials="P." surname="Miller">
              <organization />
            </author>
            <date year="2022" />
          </front>
        </reference>

        <reference anchor="arkworks" target="https://github.com/arkworks-rs">
          <front>
            <title>arkworks: A Rust ecosystem for zkSNARK programming</title>
            <author>
              <organization>arkworks contributors</organization>
            </author>
            <date year="2020" />
          </front>
        </reference>

        <reference anchor="constantine" target="https://github.com/mratsim/constantine">
          <front>
            <title>Constantine: Constant-time cryptographic library</title>
            <author initials="M." surname="Ratsimbazafy">
              <organization />
            </author>
            <date year="2020" />
          </front>
        </reference>

        <reference anchor="zig-pairings" target="https://github.com/jedisct1/zig-pairings">
          <front>
            <title>zig-pairings: Pairing-friendly curves in pure Zig (BLS12-381, BN462)</title>
            <author initials="F." surname="Denis" fullname="Frank Denis">
              <organization />
            </author>
            <date year="2026" />
          </front>
        </reference>

        <reference anchor="pfcurve-js" target="https://github.com/jc-lab/pfcurve.js">
          <front>
            <title>pfcurve.js: A pairing-friendly curve library for Node.js and the browser</title>
            <author>
              <organization>JC-Lab</organization>
            </author>
            <date year="2020" />
          </front>
        </reference>

        <reference anchor="Filecoin" target="https://filecoin.io">
          <front>
            <title>Filecoin: A Decentralized Storage Network</title>
            <author>
              <organization>Protocol Labs</organization>
            </author>
            <date year="2017" />
          </front>
        </reference>

        <reference anchor="EIP2537" target="https://eips.ethereum.org/EIPS/eip-2537">
          <front>
            <title>EIP-2537: Precompile for BLS12-381 curve operations</title>
            <author initials="A." surname="Vlasov">
              <organization />
            </author>
            <date year="2020" />
          </front>
        </reference>

        <reference anchor="HHT20" target="https://eprint.iacr.org/2020/875">
          <front>
            <title>Efficient Final Exponentiation via Cyclotomic Structure for Pairings over Families of Elliptic Curves</title>
            <author initials="D." surname="Hayashida">
              <organization />
            </author>
            <author initials="K." surname="Hayasaka">
              <organization />
            </author>
            <author initials="T." surname="Teruya">
              <organization />
            </author>
            <date year="2020" />
          </front>
          <seriesInfo name="Cryptology ePrint Archive" value="Paper 2020/875" />
        </reference>

        <reference anchor="SBCK09" target="https://doi.org/10.1007/978-3-642-03298-1_6">
          <front>
            <title>On the Final Exponentiation for Calculating Pairings on Ordinary Elliptic Curves</title>
            <author initials="M." surname="Scott">
              <organization />
            </author>
            <author initials="N." surname="Benger">
              <organization />
            </author>
            <author initials="M." surname="Charlemagne">
              <organization />
            </author>
            <author initials="L.J." surname="Dominguez Perez">
              <organization />
            </author>
            <author initials="E.J." surname="Kachisa">
              <organization />
            </author>
            <date year="2009" />
          </front>
          <seriesInfo name="Pairing 2009, LNCS" value="vol. 5671, pp. 78-88" />
        </reference>

        <reference anchor="AKLGL11" target="https://eprint.iacr.org/2010/526">
          <front>
            <title>Faster Explicit Formulas for Computing Pairings over Ordinary Curves</title>
            <author initials="D.F." surname="Aranha">
              <organization />
            </author>
            <author initials="K." surname="Karabina">
              <organization />
            </author>
            <author initials="P." surname="Longa">
              <organization />
            </author>
            <author initials="C.H." surname="Gebotys">
              <organization />
            </author>
            <author initials="J." surname="López">
              <organization />
            </author>
            <date year="2011" />
          </front>
          <seriesInfo name="EUROCRYPT" value="2011" />
        </reference>

        <reference anchor="FCKR11" target="https://doi.org/10.1007/978-3-642-28496-0_25">
          <front>
            <title>Faster Hashing to G2</title>
            <author initials="L." surname="Fuentes-Castañeda">
              <organization />
            </author>
            <author initials="E." surname="Knapp">
              <organization />
            </author>
            <author initials="F." surname="Rodríguez-Henríquez">
              <organization />
            </author>
            <date year="2011" />
          </front>
          <seriesInfo name="SAC 2011, LNCS" value="vol. 7118, pp. 412-430" />
        </reference>

      </references>
    </references>

{:/nomarkdown}

# Computing the Optimal Ate Pairing  {#comp_pairing}

Before presenting the computation of the optimal Ate pairing e(P, Q) satisfying the properties shown in {{pairing}}, we give the subfunctions used for the pairing computation.

The following algorithm, Line_function, shows the computation of the line function. It takes Q_1 = (x_1, y_1), Q_2 = (x_2, y_2) in G_2, and P = (x, y) in G_1 as input, and outputs an element of G_T.

~~~~~~~~~~
if (Q_1 = Q_2) then
    l := (3 * x_1^2) / (2 * y_1);
else if (Q_1 = -Q_2) then
    return x - x_1;
else
    l := (y_2 - y_1) / (x_2 - x_1);
end if;
return (l * (x - x_1) + y_1 - y);
~~~~~~~~~~

When implementing the line function, implementers should consider the isomorphism of E and its twist curve E' so that one can reduce the computational cost of operations in G_2 {{CLN09}}{{KIK17}}. We note that Line_function does not consider such an isomorphism; i.e., the above pseudocode operates on coordinates in untwisted form.

The computation of the optimal Ate pairing uses the p-power Frobenius endomorphism pi of {{pairing}}, applied to points Q = (x, y) over E'. The pseudocode below writes it with the characteristic as an explicit argument, pi(p, Q) = (x^p, y^p).

## Optimal Ate Pairings over Barreto-Naehrig Curves  {#optimal-ate-pairings-over-barreto-naehrig-curves}

Let c = 6 * t + 2 for a parameter t and c_0, c_1, ... , c_N in {-1,0,1} such that the sum of c_i * 2^i (i = 0, 1, ..., N) equals c.

The following algorithm shows the computation of the optimal Ate pairing on BN curves. It takes P in G_1, Q in G_2, an integer c, c_0, ...,c_N in {-1,0,1} such that the sum of c_i * 2^i (i = 0, 1, ..., N) equals c, and the order r of G_1 as input, and outputs e(P, Q).

~~~~~~~~~~
f := 1; T := Q;
if (c_N = -1) then
    T := -T;
end if
for i = N-1 downto 0
    f := f^2 * Line_function(T, T, P); T := T + T;
    if (c_i = 1) then
        f := f * Line_function(T, Q, P); T := T + Q;
    else if (c_i = -1) then
        f := f * Line_function(T, -Q, P); T := T - Q;
    end if
end for
Q_1 := pi(p, Q); Q_2 := pi(p, Q_1);
f := f * Line_function(T, Q_1, P); T := T + Q_1;
f := f * Line_function(T, -Q_2, P);
f := f^{(p^k - 1) / r}
return f;
~~~~~~~~~~

## Optimal Ate Pairings over Barreto-Lynn-Scott Curves  {#optimal-ate-pairings-over-barreto-lynn-scott-curves}

Let c = t for a parameter t and c_0, c_1, ... , c_N in {-1,0,1} such that the sum of c_i * 2^i (i = 0, 1, ..., N) equals c.

The following algorithm shows the computation of the optimal Ate pairing on Barreto-Lynn-Scott curves. It takes P in G_1, Q in G_2, an integer c, c_0, ...,c_N in {-1,0,1} such that the sum of c_i * 2^i (i = 0, 1, ..., N) equals c, and the order r of G_1 as input, and outputs e(P, Q).

~~~~~~~~~~
f := 1; T := Q;
if (c_N = -1) then
    T := -T;
end if
for i = N-1 downto 0
    f := f^2 * Line_function(T, T, P); T := T + T;
    if (c_i = 1) then
        f := f * Line_function(T, Q, P); T := T + Q;
    else if (c_i = -1) then
        f := f * Line_function(T, -Q, P); T := T - Q;
    end if
end for
f := f^{(p^k - 1) / r};
return f;
~~~~~~~~~~


# Implementation Notes  {#implementation-notes}

This appendix is informative. It documents implementation considerations discovered through verification of this memo's pseudocode against production-grade pairing libraries (mcl, noble-curves, blst), and does not standardize any algorithm.

## Production Library Cofactors  {#production-library-cofactors}

Some implementations evaluate a fixed multiple of the final exponent. Their output differs from the literal output of the pseudocode in {{comp_pairing}} by a curve-specific exponent in G_T:

~~~~~~~~~~
e_lib(P, Q) = e_pseudocode(P, Q)^alpha
~~~~~~~~~~

where alpha is:

- BLS12_381: alpha = 3 {{HHT20}}
- BN462: alpha = 2t(6t^2 + 3t + 1) mod r {{FCKR11}}
- BLS48_581: alpha = 3 {{HHT20}}

Because gcd(alpha, r) = 1 for all three curves, the following properties hold:

- Bilinearity is preserved: e_lib([K]P, [L]Q) = e_lib(P, Q)^{K * L}.
- Verification equations of the form e(A, B) = e(C, D) hold using e_lib if and only if they hold using e_pseudocode.
- Direct byte-comparison between e_lib output and the test vectors in {{test-vectors-of-optimal-ate-pairing}} will not match. Implementations seeking byte-level reproducibility of those test vectors should evaluate the pseudocode in {{comp_pairing}} literally, without applying the cofactor optimization.

## Final Exponentiation Decomposition  {#final-exponentiation-decomposition}

The pseudocode in {{comp_pairing}} writes the final exponentiation as a single step, f := f^((p^k - 1) / r). In practice, implementations compute this via an easy/hard split: an easy part computed cheaply via the Frobenius endomorphism, and a hard part computed via an addition chain over the curve parameter t.

Standard references for the hard-part addition chain include {{SBCK09}} (the original approach for BLS curves), {{AKLGL11}} (for BN curves), {{FCKR11}} (the BN cofactor variant used by mcl), and {{HHT20}} (a more recent, general treatment via cyclotomic structure, used for BLS12_381 above).

# Test Vectors of Optimal Ate Pairing  {#test-vectors-of-optimal-ate-pairing}

We provide test vectors for Optimal Ate Pairing e(P, Q) given in {{comp_pairing}} for the curves BLS12_381, BN462 and BLS48_581 given in {{secure_params}}. Here, the inputs P = (x, y) and Q = (x', y') are the corresponding base points BP and BP' given in {{secure_params}}.

Note: The G_2 base points Q = (x', y') in this appendix are given in twisted form, i.e., as coordinates over E'(GF(p^(k/d))), which gives a compact representation. The pseudocode in Appendix A operates on points of the untwisted curve E(GF(p^k)). Implementations invoking that pseudocode directly must first apply the untwist isomorphism psi defined in {{secure_params}} to lift Q from E' to E(GF(p^k)). Most production libraries perform this lifting implicitly by using twisted-form variants of Line_function, which are mathematically equivalent and more efficient.

For BLS12_381 and BN462, Q = (x', y') is given by

~~~~~~~~~~
x' = x'_0 + x'_1 * u and
y' = y'_0 + y'_1 * u,
~~~~~~~~~~

where u is an indeterminate and x'_0, x'_1, y'_0, y'_1 are elements of GF(p).

For BLS48_581, Q = (x', y') is given by

~~~~~~~~~~
x' = x'_0 + x'_1 * u + x'_2 * v + x'_3 * u * v
    + x'_4 * w + x'_5 * u * w + x'_6 * v * w + x'_7 * u * v * w and
y' = y'_0 + y'_1 * u + y'_2 * v + y'_3 * u * v
    + y'_4 * w + y'_5 * u * w + y'_6 * v * w + y'_7 * u * v * w,
~~~~~~~~~~

where u, v and w are indeterminates and x'_0, ..., x'_7 and y'_0, ..., y'_7 are elements of GF(p).

In addition, we use the notation e_i (i = 0, ..., k-1) for the coefficients of e(P, Q) over GF(p), with respect to the tower of extension fields defined for each curve in {{secure_params}}. The basis is the set of products of powers of the indeterminates of that tower, indexed so that each indeterminate contributes a stride equal to the degree over GF(p) of the field it extends.

For BLS12_381 and BN462 (k = 12), with the towers of {{tower_bls12_381}} and {{tower_bn462}}, this gives

~~~~~~~~~~
e(P, Q) = e_0 + e_1 * u + e_2 * v + e_3 * u * v + e_4 * v^2
        + e_5 * u * v^2 + e_6 * w + e_7 * u * w + e_8 * v * w
        + e_9 * u * v * w + e_10 * v^2 * w + e_11 * u * v^2 * w.
~~~~~~~~~~

For BLS48_581 (k = 48), with the tower of {{tower_bls48_581}}, the same rule gives the 48 basis elements u^i1 * v^i2 * w^i3 * z^i4 * s^i5, where i1, i2, i3 and i5 are in {0, 1} and i4 is in {0, 1, 2}. The coefficient of that basis element is e_i with

~~~~~~~~~~
i = i1 + 2 * i2 + 4 * i3 + 8 * i4 + 24 * i5.
~~~~~~~~~~

BLS12_381:

Input x value:
:   0x17f1d3a73197d7942695638c4fa9ac0fc3688c4f9774b905a14e3a3f171bac586c55e83ff97a1aeffb3af00adb22c6bb

Input y value:
:   0x08b3f481e3aaa0f1a09e30ed741d8ae4fcf5e095d5d00af600db18cb2c04b3edd03cc744a2888ae40caa232946c5e7e1

Input x'_0 value:
:   0x024aa2b2f08f0a91260805272dc51051c6e47ad4fa403b02b4510b647ae3d1770bac0326a805bbefd48056c8c121bdb8

Input x'_1 value:
:   0x13e02b6052719f607dacd3a088274f65596bd0d09920b61ab5da61bbdc7f5049334cf11213945d57e5ac7d055d042b7e

Input y'_0 value:
:   0x0ce5d527727d6e118cc9cdc6da2e351aadfd9baa8cbdd3a76d429a695160d12c923ac9cc3baca289e193548608b82801

Input y'_1 value:
:   0x0606c4a02ea734cc32acd2b02bc28b99cb3e287e85a763af267492ab572e99ab3f370d275cec1da1aaa9075ff05f79be

e_0:
:   0x11619b45f61edfe3b47a15fac19442526ff489dcda25e59121d9931438907dfd448299a87dde3a649bdba96e84d54558

e_1:
:   0x153ce14a76a53e205ba8f275ef1137c56a566f638b52d34ba3bf3bf22f277d70f76316218c0dfd583a394b8448d2be7f

e_2:
:   0x095668fb4a02fe930ed44767834c915b283b1c6ca98c047bd4c272e9ac3f3ba6ff0b05a93e59c71fba77bce995f04692

e_3:
:   0x16deedaa683124fe7260085184d88f7d036b86f53bb5b7f1fc5e248814782065413e7d958d17960109ea006b2afdeb5f

e_4:
:   0x09c92cf02f3cd3d2f9d34bc44eee0dd50314ed44ca5d30ce6a9ec0539be7a86b121edc61839ccc908c4bdde256cd6048

e_5:
:   0x111061f398efc2a97ff825b04d21089e24fd8b93a47e41e60eae7e9b2a38d54fa4dedced0811c34ce528781ab9e929c7

e_6:
:   0x01ecfcf31c86257ab00b4709c33f1c9c4e007659dd5ffc4a735192167ce197058cfb4c94225e7f1b6c26ad9ba68f63bc

e_7:
:   0x08890726743a1f94a8193a166800b7787744a8ad8e2f9365db76863e894b7a11d83f90d873567e9d645ccf725b32d26f

e_8:
:   0x0e61c752414ca5dfd258e9606bac08daec29b3e2c57062669556954fb227d3f1260eedf25446a086b0844bcd43646c10

e_9:
:   0x0fe63f185f56dd29150fc498bbeea78969e7e783043620db33f75a05a0a2ce5c442beaff9da195ff15164c00ab66bdde

e_10:
:   0x10900338a92ed0b47af211636f7cfdec717b7ee43900eee9b5fc24f0000c5874d4801372db478987691c566a8c474978

e_11:
:   0x1454814f3085f0e6602247671bc408bbce2007201536818c901dbd4d2095dd86c1ec8b888e59611f60a301af7776be3d

BN462:

Input x value:
:   0x21a6d67ef250191fadba34a0a30160b9ac9264b6f95f63b3edbec3cf4b2e689db1bbb4e69a416a0b1e79239c0372e5cd70113c98d91f36b6980d

Input y value:
:   0x0118ea0460f7f7abb82b33676a7432a490eeda842cccfa7d788c659650426e6af77df11b8ae40eb80f475432c66600622ecaa8a5734d36fb03de

Input x'_0 value:
:   0x0257ccc85b58dda0dfb38e3a8cbdc5482e0337e7c1cd96ed61c913820408208f9ad2699bad92e0032ae1f0aa6a8b48807695468e3d934ae1e4df

Input x'_1 value:
:   0x1d2e4343e8599102af8edca849566ba3c98e2a354730cbed9176884058b18134dd86bae555b783718f50af8b59bf7e850e9b73108ba6aa8cd283

Input y'_0 value:
:   0x0a0650439da22c1979517427a20809eca035634706e23c3fa7a6bb42fe810f1399a1f41c9ddae32e03695a140e7b11d7c3376e5b68df0db7154e

Input y'_1 value:
:   0x073ef0cbd438cbe0172c8ae37306324d44d5e6b0c69ac57b393f1ab370fd725cc647692444a04ef87387aa68d53743493b9eba14cc552ca2a93a

e_0:
:   0x0cf7f0f2e01610804272f4a7a24014ac085543d787c8f8bf07059f93f87ba7e2a4ac77835d4ff10e78669be39cd23cc3a659c093dbe3b9647e8c

e_1:
:   0x00ef2c737515694ee5b85051e39970f24e27ca278847c7cfa709b0df408b830b3763b1b001f1194445b62d6c093fb6f77e43e369edefb1200389

e_2:
:   0x04d685b29fd2b8faedacd36873f24a06158742bb2328740f93827934592d6f1723e0772bb9ccd3025f88dc457fc4f77dfef76104ff43cd430bf7

e_3:
:   0x090067ef2892de0c48ee49cbe4ff1f835286c700c8d191574cb424019de11142b3c722cc5083a71912411c4a1f61c00d1e8f14f545348eb7462c

e_4:
:   0x1437603b60dce235a090c43f5147d9c03bd63081c8bb1ffa7d8a2c31d673230860bb3dfe4ca85581f7459204ef755f63cba1fbd6a4436f10ba0e

e_5:
:   0x13191b1110d13650bf8e76b356fe776eb9d7a03fe33f82e3fe5732071f305d201843238cc96fd0e892bc61701e1844faa8e33446f87c6e29e75f

e_6:
:   0x07b1ce375c0191c786bb184cc9c08a6ae5a569dd7586f75d6d2de2b2f075787ee5082d44ca4b8009b3285ecae5fa521e23be76e6a08f17fa5cc8

e_7:
:   0x05b64add5e49574b124a02d85f508c8d2d37993ae4c370a9cda89a100cdb5e1d441b57768dbc68429ffae243c0c57fe5ab0a3ee4c6f2d9d34714

e_8:
:   0x0fd9a3271854a2b4542b42c55916e1faf7a8b87a7d10907179ac7073f6a1de044906ffaf4760d11c8f92df3e50251e39ce92c700a12e77d0adf3

e_9:
:   0x17fa0c7fa60c9a6d4d8bb9897991efd087899edc776f33743db921a689720c82257ee3c788e8160c112f18e841a3dd9a79a6f8782f771d542ee5

e_10:
:   0x0c901397a62bb185a8f9cf336e28cfb0f354e2313f99c538cdceedf8b8aa22c23b896201170fc915690f79f6ba75581f1b76055cd89b7182041c

e_11:
:   0x20f27fde93cee94ca4bf9ded1b1378c1b0d80439eeb1d0c8daef30db0037104a5e32a2ccc94fa1860a95e39a93ba51187b45f4c2c50c16482322

BLS48_581:

Input x value:
:   0x02af59b7ac340f2baf2b73df1e93f860de3f257e0e86868cf61abdbaedffb9f7544550546a9df6f9645847665d859236ebdbc57db368b11786cb74da5d3a1e6d8c3bce8732315af640

Input y value:
:   0x0cefda44f6531f91f86b3a2d1fb398a488a553c9efeb8a52e991279dd41b720ef7bb7beffb98aee53e80f678584c3ef22f487f77c2876d1b2e35f37aef7b926b576dbb5de3e2587a70

Input x'_0 value:
:   0x05d615d9a7871e4a38237fa45a2775debabbefc70344dbccb7de64db3a2ef156c46ff79baad1a8c42281a63ca0612f400503004d80491f510317b79766322154dec34fd0b4ace8bfab

Input x'_1 value:
:   0x07c4973ece2258512069b0e86abc07e8b22bb6d980e1623e9526f6da12307f4e1c3943a00abfedf16214a76affa62504f0c3c7630d979630ffd75556a01afa143f1669b36676b47c57

Input x'_2 value:
:   0x01fccc70198f1334e1b2ea1853ad83bc73a8a6ca9ae237ca7a6d6957ccbab5ab6860161c1dbd19242ffae766f0d2a6d55f028cbdfbb879d5fea8ef4cded6b3f0b46488156ca55a3e6a

Input x'_3 value:
:   0x0be2218c25ceb6185c78d8012954d4bfe8f5985ac62f3e5821b7b92a393f8be0cc218a95f63e1c776e6ec143b1b279b9468c31c5257c200ca52310b8cb4e80bc3f09a7033cbb7feafe

Input x'_4 value:
:   0x038b91c600b35913a3c598e4caa9dd63007c675d0b1642b5675ff0e7c5805386699981f9e48199d5ac10b2ef492ae589274fad55fc1889aa80c65b5f746c9d4cbb739c3a1c53f8cce5

Input x'_5 value:
:   0x0c96c7797eb0738603f1311e4ecda088f7b8f35dcef0977a3d1a58677bb037418181df63835d28997eb57b40b9c0b15dd7595a9f177612f097fc7960910fce3370f2004d914a3c093a

Input x'_6 value:
:   0x0b9b7951c6061ee3f0197a498908aee660dea41b39d13852b6db908ba2c0b7a449cef11f293b13ced0fd0caa5efcf3432aad1cbe4324c22d63334b5b0e205c3354e41607e60750e057

Input x'_7 value:
:   0x0827d5c22fb2bdec5282624c4f4aaa2b1e5d7a9defaf47b5211cf741719728a7f9f8cfca93f29cff364a7190b7e2b0d4585479bd6aebf9fc44e56af2fc9e97c3f84e19da00fbc6ae34

Input y'_0 value:
:   0x00eb53356c375b5dfa497216452f3024b918b4238059a577e6f3b39ebfc435faab0906235afa27748d90f7336d8ae5163c1599abf77eea6d659045012ab12c0ff323edd3fe4d2d7971

Input y'_1 value:
:   0x0284dc75979e0ff144da6531815fcadc2b75a422ba325e6fba01d72964732fcbf3afb096b243b1f192c5c3d1892ab24e1dd212fa097d760e2e588b423525ffc7b111471db936cd5665

Input y'_2 value:
:   0x0b36a201dd008523e421efb70367669ef2c2fc5030216d5b119d3a480d370514475f7d5c99d0e90411515536ca3295e5e2f0c1d35d51a652269cbc7c46fc3b8fde68332a526a2a8474

Input y'_3 value:
:   0x0aec25a4621edc0688223fbbd478762b1c2cded3360dcee23dd8b0e710e122d2742c89b224333fa40dced2817742770ba10d67bda503ee5e578fb3d8b8a1e5337316213da92841589d

Input y'_4 value:
:   0x0d209d5a223a9c46916503fa5a88325a2554dc541b43dd93b5a959805f1129857ed85c77fa238cdce8a1e2ca4e512b64f59f430135945d137b08857fdddfcf7a43f47831f982e50137

Input y'_5 value:
:   0x07d0d03745736b7a513d339d5ad537b90421ad66eb16722b589d82e2055ab7504fa83420e8c270841f6824f47c180d139e3aafc198caa72b679da59ed8226cf3a594eedc58cf90bee4

Input y'_6 value:
:   0x0896767811be65ea25c2d05dfdd17af8a006f364fc0841b064155f14e4c819a6df98f425ae3a2864f22c1fab8c74b2618b5bb40fa639f53dccc9e884017d9aa62b3d41faeafeb23986

Input y'_7 value:
:   0x035e2524ff89029d393a5c07e84f981b5e068f1406be8e50c87549b6ef8eca9a9533a3f8e69c31e97e1ad0333ec719205417300d8c4ab33f748e5ac66e84069c55d667ffcb732718b6

e_0:
:   0x0e26c3fcb8ef67417814098de5111ffcccc1d003d15b367bad07cef2291a93d31db03e3f03376f3beae2bd877bcfc22a25dc51016eda1ab56ee3033bc4b4fec5962f02dffb3af5e38e

e_1:
:   0x069061b8047279aa5c2d25cdf676ddf34eddbc8ec2ec0f03614886fa828e1fc066b26d35744c0c38271843aa4fb617b57fa9eb4bd256d17367914159fc18b10a1085cb626e5bedb145

e_2:
:   0x02b9bece645fbf9d8f97025a1545359f6fe3ffab3cd57094f862f7fb9ca01c88705c26675bcc723878e943da6b56ce25d063381fcd2a292e0e7501fe572744184fb4ab4ca071a04281

e_3:
:   0x0080d267bf036c1e61d7fc73905e8c630b97aa05ef3266c82e7a111072c0d2056baa8137fba111c9650dfb18cb1f43363041e202e3192fced29d2b0501c882543fb370a56bfdc2435b

e_4:
:   0x03c6b4c12f338f9401e6a493a405b33e64389338db8c5e592a8dd79eac7720dd83dd6b0c189eeda20809160cd57cdf3e2edc82db15f553c1f6c953ea27114cb6bd8a38e273f407dae0

e_5:
:   0x016e46224f28bfd8833f76ac29ee6e406a9da1bde55f5e82b3bd977897a9104f18b9ee41ea9af7d4183d895102950a12ce9975669db07924e1b432d9680f5ce7e5c67ed68f381eba45

e_6:
:   0x008ddce7a4a1b94be5df3ceea56bef0077dcdde86d579938a50933a47296d337b7629934128e2457e24142b0eeaa978fd8e70986d7dd51fccbbeb8a1933434fec4f5bc538de2646e90

e_7:
:   0x060ef6eae55728e40bd4628265218b24b38cdd434968c14bfefb87f0dcbfc76cc473ae2dc0cac6e69dfdf90951175178dc75b9cc08320fcde187aa58ea047a2ee00b1968650eec2791

e_8:
:   0x0c3943636876fd4f9393414099a746f84b2633dfb7c36ba6512a0b48e66dcb2e409f1b9e150e36b0b4311165810a3c721525f0d43a021f090e6a27577b42c7a57bed3327edb98ba8f8

e_9:
:   0x02d31eb8be0d923cac2a8eb6a07556c8951d849ec53c2848ee78c5eed40262eb21822527a8555b071f1cd080e049e5e7ebfe2541d5b42c1e414341694d6f16d287e4a8d28359c2d2f9

e_10:
:   0x07f19673c5580d6a10d09a032397c5d425c3a99ff1dd0abe5bec40a0d47a6b8daabb22edb6b06dd8691950b8f23faefcdd80c45aa3817a840018965941f4247f9f97233a84f58b262e

e_11:
:   0x0d3fe01f0c114915c3bdf8089377780076c1685302279fd9ab12d07477aac03b69291652e9f179baa0a99c38aa8851c1d25ffdb4ded2c8fe8b30338c14428607d6d822610d41f51372

e_12:
:   0x0662eefd5fab9509aed968866b68cff3bc5d48ecc8ac6867c212a2d82cee5a689a3c9c67f1d611adac7268dc8b06471c0598f7016ca3d1c01649dda4b43531cffc4eb41e691e27f2eb

e_13:
:   0x0aad8f4a8cfdca8de0985070304fe4f4d32f99b01d4ea50d9f7cd2abdc0aeea99311a36ec6ed18208642cef9e09b96795b27c42a5a744a7b01a617a91d9fb7623d636640d61a6596ec

e_14:
:   0x0ffcf21d641fd9c6a641a749d80cab1bcad4b34ee97567d905ed9d5cfb74e9aef19674e2eb6ce3dfb706aa814d4a228db4fcd707e571259435393a27cac68b59a1b690ae8cde7a94c3

e_15:
:   0x0cbe92a53151790cece4a86f91e9b31644a86fc4c954e5fa04e707beb69fc60a858fed8ebd53e4cfd51546d5c0732331071c358d721ee601bfd3847e0e904101c62822dd2e4c7f8e5c

e_16:
:   0x0202db83b1ff33016679b6cfc8931deea6df1485c894dcd113bacf564411519a42026b5fda4e16262674dcb3f089cd7d552f8089a1fec93e3db6bca43788cdb06fc41baaa5c5098667

e_17:
:   0x070a617ed131b857f5b74b625c4ef70cc567f619defb5f2ab67534a1a8aa72975fc4248ac8551ce02b68801703971a2cf1cb934c9c354cadd5cfc4575cde8dbde6122bd54826a9b3e9

e_18:
:   0x070e1ebce457c141417f88423127b7a7321424f64119d5089d883cb953283ee4e1f2e01ffa7b903fe7a94af4bb1acb02ca6a36678e41506879069cee11c9dcf6a080b6a4a7c7f21dc9

e_19:
:   0x058a06be5a36c6148d8a1287ee7f0e725453fa1bb05cf77239f235b417127e370cfa4f88e61a23ea16df3c45d29c203d04d09782b39e9b4037c0c4ac8e8653e7c533ad752a640b233e

e_20:
:   0x0dfdfaaeb9349cf18d21b92ad68f8a7ecc509c35fcd4b8abeb93be7a204ac871f2195180206a2c340fccb69dbc30b9410ed0b122308a8fc75141f673ae5ec82b6a45fc2d664409c6b6

e_21:
:   0x0d06c8adfdd81275da2a0ce375b8df9199f3d359e8cf50064a3dc10a592417124a3b705b05a7ffe78e20f935a08868ecf3fc5aba0ace7ce4497bb59085ca277c16b3d53dd7dae5c857

e_22:
:   0x0708effd28c4ae21b6969cb9bdd0c27f8a3e341798b6f6d4baf27be259b4a47688b50cb68a69a917a4a1faf56cec93f69ac416512c32e9d5e69bd8836b6c2ba9c6889d507ad571dbc4

e_23:
:   0x09da7c7aa48ce571f8ece74b98431b14ae6fb4a53ae979cd6b2e82320e8d25a0ece1ca1563aa5aa6926e7d608358af8399534f6b00788e95e37ef1b549f43a58ad250a71f0b2fdb2bf

e_24:
:   0x080fa69eaed5d5e9044c43db6c168066772604f3a94c4e7db2f92252ebed3011df4c9bcf4ffccbc7e514566fc6462ca39d4da73705a404bce6b7e2751037e5cd279df81de00e653134

e_25:
:   0x0a3a604cd354f6a763481558b087c55b1e6df4aca073f89ee4c2f7eea48091ef2b75369a7819f39472a1d518474fb11998a3d10e4cc3aaba54b5531e8dd777b9b5503283f15f2cbc53

e_26:
:   0x0ee80fd1b6256c379e78d3a966f78a1b75ea21a33ffd4a2b758a6a0fe9c95fb597700b44eda1f05ff3dc6705178624c83eff49c2e1a109e7af3ef6eaf2138d81b9934c6c6dac264587

e_27:
:   0x0baa734a9d454bc8507eaf40d71e100339988eac975139f667cf426e4a856d1190a9f6a2ea07693108526be00ca0d94a0fb7d59b63497d81f97786159ee25bb0e8384e61403094adca

e_28:
:   0x050ab6cabefbfeafa89331de62bb2c7441607dccabe12fa3302bf74e5cea09b6e2a44e8a32ba04b8393cae776984e015b6aecd3151695907b83feb8fb1683b52bdac298802a1820bb5

e_29:
:   0x07da541017034b7f7150a4cc42eb8e32c8e4a91641cb5d71cc6ba5297d85efa21400a44a121bb0b8a3ad1a780722a46ae4a0d7101f78986f29aab9688af8302c0a7cc1e16b95ddb4da

e_30:
:   0x013985a6999ae046842851e80d88dae0c26a5b1cab6a70f97d36d41693efcf80e5ecfe8fceb61b22afc07140de05f5342dd8c04050afd6649f10e0bf2180f6ff4e14562558e21cdd9e

e_31:
:   0x00dd7f830773ae0a3569af18f7fa31794a413c91b576c48a775bf25b6a078876c8bbb247c5169d8871812d70418f43cf56bef6ce6adfc90558610ba732e3447107c4c0c56a018f6d63

e_32:
:   0x06f5b22dfef5b5047297208ef02b482fadf2f7a05fde3ce1a740991176a359ffbdb2a781ef01d5b9d5fa38d7ce3bd946bb3ca36d131d3b01e658ce48cb0c16b44acb14896a9e0794a8

e_33:
:   0x09779469c05944d969e2db181a1caf60dffe3fb23afc7a95d03641b8aeadf0aa0c6db009ee6e40c9cee370db20a40be7d2b4e8d3bb5ded36c08c6a236823d175269a4209446e8ba161

e_34:
:   0x094f2f80f6c6c25fb2dbc6b49778ba5768ae8d6e3e355a7782d0360e40fe74fc488d1cdf61efd091ecf453900d677009a016a0978ae88a214319432047ff820c29aa07cd87f6f118de

e_35:
:   0x1076373833daf844b5a78fb8d6a87d5bce51be2aa85a8fb9d05aba01e08e527c0d336a498fb22a8eabcf73444a5dc80b8e8196d1b6843b1e905ba7e158ddddc080291f2fbb432684af

e_36:
:   0x06760a326a3b3425d0e8ee8533684d4149d27c221becb781e24786cf9b55cfc21511eb1f3b237a9a8f35c90feb5b059b0540a34e6517412a79b7bb9cd243b070846a549d7ab484aa92

e_37:
:   0x03897dadb1a5b4c58d7c6f0ff099885371d896a6d7546dff0b0ab940d61e77f04922532ef46bf90c018a5e7f148330c17e02318b387772ba8a65626c6275ff4df09f5bf5d8d4dec847

e_38:
:   0x111a9376b2770c3d32b28b5452058eca6ec30e1b97ecd98d641165b007e960375c1c0cf586dc47829d7f4da0f5494bf13fcf10da1d7630cf3b1e96771bd3e125abed5d7a4b07ff2b2f

e_39:
:   0x00fc4faae2597cd2fc5206a40cb0a70646fb4237727fe75e51e6f6a591a5e2adc28b28b6c47e0fcc0953f9aab4d3daf926249f9930550f55554d6d380c3b4c875a6aa4e1ef525be43e

e_40:
:   0x0072af98ab6e2c8ba452e0acca6dbc50afe97b037ba3586cf2f3a8f4b70f393462a72d7efcf87f8f94e9941e9998f4f66e989df87822ea5ce094055105827af2d5d86ef324cc139811

e_41:
:   0x10158908b23e827e39978dacbf0d1f489c1f89de0bdc8b6614cf5b1c78c03d01fa57d1141e6f21a24d80ff6c596e891c0a5f99a6305820c3ba5192fd18dd9b422976adccdaacc823ba

e_42:
:   0x0e651105c3a12b4b91b5346cad5de5ff5126c1c2b94c1246882798d44eb0ca93655491c753f3492e5eb1097b7a6c88f3e5c064551fa6660460a6275caf7a672c0886574c254e8ec0b1

e_43:
:   0x0040de9ec4386438b63e6cbea88022e58a9163bf374460b3c6dae8887f56882db2fa40e1a0acc723d8e6ada12bc0ca838c932d74e199d6b006b92dd204eaf2e1186b6e91434ed6a24e

e_44:
:   0x053ec95b4b13256c02702930617ab4b3c2d3069bb4d08414c7de5b33578c4ddd1bd4dcb3cf18a3ca40206edc917ab98928bdb62df970f3537910dfc2423dcebc561ed93b3ed4e0d346

e_45:
:   0x0167007aacbba10275709297b940b601b1ed49d629e35faee61cf188a6da8ec4e39ae91dc326cd54617a5fa3330637c9ab0ed46f32e472eef36f30228bb2c3ad3cc03d5d49a8794a50

e_46:
:   0x0c624cb15846e9faedc2f8cddd3dd894eaee57ed85a76167559fd68e818184df146aa913a44ffb7e9c520ab84c6a2ad0ac525cb1bbb00afc02c79603375060b2ddeac8eec0e4ce49a7

e_47:
:   0x07a369e7095b629da43956d18650a88bb0342dabc1e04265ae4133cdd537c3d9cdc0b99518aa6e8b6942c4a958d3c7df4a18211c91bc4e9455f28a49c7e0ee07dc2f97fd5ce4edf7a6


# Test Vectors for Serialization  {#point-serialization-test-vectors}
This appendix gives test vectors for the serialization procedures defined in {{point-serialization}}. The point vectors are computed for the base points BP and BP' given in {{secure_params}}, in the compressed form (C_bit = 1) and in the uncompressed form (C_bit = 0). The scalar vectors are given for all three curves, since {{scalar-serialization}} applies to BN462 as well.

In each uncompressed string, the first half is the corresponding compressed string with the three metadata bits cleared, and the second half is the y-coordinate laid out by the rule of step 3 of {{point-serialization-procedure}}. Hex strings are broken at a whole number of bytes.

## BLS12_381 Points

G_1 (BP), compressed (48 bytes):

~~~~~~~~~~
97f1d3a73197d7942695638c4fa9ac0fc3688c4f9774b905a14e3a3f171bac58
6c55e83ff97a1aeffb3af00adb22c6bb
~~~~~~~~~~

G_1 (BP), uncompressed (96 bytes):

~~~~~~~~~~
17f1d3a73197d7942695638c4fa9ac0fc3688c4f9774b905a14e3a3f171bac58
6c55e83ff97a1aeffb3af00adb22c6bb08b3f481e3aaa0f1a09e30ed741d8ae4
fcf5e095d5d00af600db18cb2c04b3edd03cc744a2888ae40caa232946c5e7e1
~~~~~~~~~~

G_2 (BP'), compressed (96 bytes):

~~~~~~~~~~
93e02b6052719f607dacd3a088274f65596bd0d09920b61ab5da61bbdc7f5049
334cf11213945d57e5ac7d055d042b7e024aa2b2f08f0a91260805272dc51051
c6e47ad4fa403b02b4510b647ae3d1770bac0326a805bbefd48056c8c121bdb8
~~~~~~~~~~

G_2 (BP'), uncompressed (192 bytes):

~~~~~~~~~~
13e02b6052719f607dacd3a088274f65596bd0d09920b61ab5da61bbdc7f5049
334cf11213945d57e5ac7d055d042b7e024aa2b2f08f0a91260805272dc51051
c6e47ad4fa403b02b4510b647ae3d1770bac0326a805bbefd48056c8c121bdb8
0606c4a02ea734cc32acd2b02bc28b99cb3e287e85a763af267492ab572e99ab
3f370d275cec1da1aaa9075ff05f79be0ce5d527727d6e118cc9cdc6da2e351a
adfd9baa8cbdd3a76d429a695160d12c923ac9cc3baca289e193548608b82801
~~~~~~~~~~

Identity on E: 48 zero bytes with the leading byte set to 0xc0 (C_bit = 1, I_bit = 1) when compressed, and 96 zero bytes with the leading byte set to 0x40 (I_bit = 1) when uncompressed.

Identity on E': 96 zero bytes with the leading byte set to 0xc0 when compressed, and 192 zero bytes with the leading byte set to 0x40 when uncompressed.

## BLS48_581 Points

G_1 (BP), compressed (73 bytes):

~~~~~~~~~~
a2af59b7ac340f2baf2b73df1e93f860de3f257e0e86868cf61abdbaedffb9f7
544550546a9df6f9645847665d859236ebdbc57db368b11786cb74da5d3a1e6d
8c3bce8732315af640
~~~~~~~~~~

G_1 (BP), uncompressed (146 bytes):

~~~~~~~~~~
02af59b7ac340f2baf2b73df1e93f860de3f257e0e86868cf61abdbaedffb9f7
544550546a9df6f9645847665d859236ebdbc57db368b11786cb74da5d3a1e6d
8c3bce8732315af6400cefda44f6531f91f86b3a2d1fb398a488a553c9efeb8a
52e991279dd41b720ef7bb7beffb98aee53e80f678584c3ef22f487f77c2876d
1b2e35f37aef7b926b576dbb5de3e2587a70
~~~~~~~~~~

G_2 (BP'), compressed (584 bytes):

~~~~~~~~~~
8827d5c22fb2bdec5282624c4f4aaa2b1e5d7a9defaf47b5211cf741719728a7
f9f8cfca93f29cff364a7190b7e2b0d4585479bd6aebf9fc44e56af2fc9e97c3
f84e19da00fbc6ae340b9b7951c6061ee3f0197a498908aee660dea41b39d138
52b6db908ba2c0b7a449cef11f293b13ced0fd0caa5efcf3432aad1cbe4324c2
2d63334b5b0e205c3354e41607e60750e0570c96c7797eb0738603f1311e4ecd
a088f7b8f35dcef0977a3d1a58677bb037418181df63835d28997eb57b40b9c0
b15dd7595a9f177612f097fc7960910fce3370f2004d914a3c093a038b91c600
b35913a3c598e4caa9dd63007c675d0b1642b5675ff0e7c5805386699981f9e4
8199d5ac10b2ef492ae589274fad55fc1889aa80c65b5f746c9d4cbb739c3a1c
53f8cce50be2218c25ceb6185c78d8012954d4bfe8f5985ac62f3e5821b7b92a
393f8be0cc218a95f63e1c776e6ec143b1b279b9468c31c5257c200ca52310b8
cb4e80bc3f09a7033cbb7feafe01fccc70198f1334e1b2ea1853ad83bc73a8a6
ca9ae237ca7a6d6957ccbab5ab6860161c1dbd19242ffae766f0d2a6d55f028c
bdfbb879d5fea8ef4cded6b3f0b46488156ca55a3e6a07c4973ece2258512069
b0e86abc07e8b22bb6d980e1623e9526f6da12307f4e1c3943a00abfedf16214
a76affa62504f0c3c7630d979630ffd75556a01afa143f1669b36676b47c5705
d615d9a7871e4a38237fa45a2775debabbefc70344dbccb7de64db3a2ef156c4
6ff79baad1a8c42281a63ca0612f400503004d80491f510317b79766322154de
c34fd0b4ace8bfab
~~~~~~~~~~

G_2 (BP'), uncompressed (1168 bytes):

~~~~~~~~~~
0827d5c22fb2bdec5282624c4f4aaa2b1e5d7a9defaf47b5211cf741719728a7
f9f8cfca93f29cff364a7190b7e2b0d4585479bd6aebf9fc44e56af2fc9e97c3
f84e19da00fbc6ae340b9b7951c6061ee3f0197a498908aee660dea41b39d138
52b6db908ba2c0b7a449cef11f293b13ced0fd0caa5efcf3432aad1cbe4324c2
2d63334b5b0e205c3354e41607e60750e0570c96c7797eb0738603f1311e4ecd
a088f7b8f35dcef0977a3d1a58677bb037418181df63835d28997eb57b40b9c0
b15dd7595a9f177612f097fc7960910fce3370f2004d914a3c093a038b91c600
b35913a3c598e4caa9dd63007c675d0b1642b5675ff0e7c5805386699981f9e4
8199d5ac10b2ef492ae589274fad55fc1889aa80c65b5f746c9d4cbb739c3a1c
53f8cce50be2218c25ceb6185c78d8012954d4bfe8f5985ac62f3e5821b7b92a
393f8be0cc218a95f63e1c776e6ec143b1b279b9468c31c5257c200ca52310b8
cb4e80bc3f09a7033cbb7feafe01fccc70198f1334e1b2ea1853ad83bc73a8a6
ca9ae237ca7a6d6957ccbab5ab6860161c1dbd19242ffae766f0d2a6d55f028c
bdfbb879d5fea8ef4cded6b3f0b46488156ca55a3e6a07c4973ece2258512069
b0e86abc07e8b22bb6d980e1623e9526f6da12307f4e1c3943a00abfedf16214
a76affa62504f0c3c7630d979630ffd75556a01afa143f1669b36676b47c5705
d615d9a7871e4a38237fa45a2775debabbefc70344dbccb7de64db3a2ef156c4
6ff79baad1a8c42281a63ca0612f400503004d80491f510317b79766322154de
c34fd0b4ace8bfab035e2524ff89029d393a5c07e84f981b5e068f1406be8e50
c87549b6ef8eca9a9533a3f8e69c31e97e1ad0333ec719205417300d8c4ab33f
748e5ac66e84069c55d667ffcb732718b60896767811be65ea25c2d05dfdd17a
f8a006f364fc0841b064155f14e4c819a6df98f425ae3a2864f22c1fab8c74b2
618b5bb40fa639f53dccc9e884017d9aa62b3d41faeafeb2398607d0d0374573
6b7a513d339d5ad537b90421ad66eb16722b589d82e2055ab7504fa83420e8c2
70841f6824f47c180d139e3aafc198caa72b679da59ed8226cf3a594eedc58cf
90bee40d209d5a223a9c46916503fa5a88325a2554dc541b43dd93b5a959805f
1129857ed85c77fa238cdce8a1e2ca4e512b64f59f430135945d137b08857fdd
dfcf7a43f47831f982e501370aec25a4621edc0688223fbbd478762b1c2cded3
360dcee23dd8b0e710e122d2742c89b224333fa40dced2817742770ba10d67bd
a503ee5e578fb3d8b8a1e5337316213da92841589d0b36a201dd008523e421ef
b70367669ef2c2fc5030216d5b119d3a480d370514475f7d5c99d0e904115155
36ca3295e5e2f0c1d35d51a652269cbc7c46fc3b8fde68332a526a2a84740284
dc75979e0ff144da6531815fcadc2b75a422ba325e6fba01d72964732fcbf3af
b096b243b1f192c5c3d1892ab24e1dd212fa097d760e2e588b423525ffc7b111
471db936cd566500eb53356c375b5dfa497216452f3024b918b4238059a577e6
f3b39ebfc435faab0906235afa27748d90f7336d8ae5163c1599abf77eea6d65
9045012ab12c0ff323edd3fe4d2d7971
~~~~~~~~~~

Identity on E: 73 zero bytes with the leading byte set to 0xc0 (C_bit = 1, I_bit = 1) when compressed, and 146 zero bytes with the leading byte set to 0x40 (I_bit = 1) when uncompressed.

Identity on E': 584 zero bytes with the leading byte set to 0xc0 when compressed, and 1168 zero bytes with the leading byte set to 0x40 when uncompressed.

## Scalars  {#scalar-test-vectors}
The values below are the boundary cases of {{scalar-serialization}}: the zero scalar, the smallest nonzero scalar, the largest scalar the procedure accepts, and the smallest value it rejects. The value kappa = 1 distinguishes the byte order, and kappa = r exercises the rejection of values that are not less than r.

BLS12_381 (n_s = 32):

~~~~~~~~~~
kappa = 0
0000000000000000000000000000000000000000000000000000000000000000
kappa = 1
0000000000000000000000000000000000000000000000000000000000000001
kappa = r - 1
73eda753299d7d483339d80809a1d80553bda402fffe5bfeffffffff00000000
kappa = r  (deserialization returns INVALID)
73eda753299d7d483339d80809a1d80553bda402fffe5bfeffffffff00000001
~~~~~~~~~~

BN462 (n_s = 58):

~~~~~~~~~~
kappa = 0
0000000000000000000000000000000000000000000000000000000000
0000000000000000000000000000000000000000000000000000000000
kappa = 1
0000000000000000000000000000000000000000000000000000000000
0000000000000000000000000000000000000000000000000000000001
kappa = r - 1
240480360120023ffffffffff6ff0cf6b7d9bfca0000000000d812908e
e1c201f7fffffffff6ff66fc7bf717f7c0000000002401b007e010800c
kappa = r  (deserialization returns INVALID)
240480360120023ffffffffff6ff0cf6b7d9bfca0000000000d812908e
e1c201f7fffffffff6ff66fc7bf717f7c0000000002401b007e010800d
~~~~~~~~~~

BLS48_581 (n_s = 65):

~~~~~~~~~~
kappa = 0
000000000000000000000000000000000000000000000000000000000000000000
0000000000000000000000000000000000000000000000000000000000000000
kappa = 1
000000000000000000000000000000000000000000000000000000000000000000
0000000000000000000000000000000000000000000000000000000000000001
kappa = r - 1
2386f8a925e2885e233a9ccc1615c0d6c635387a3f0b3cbe003fad6bc972c2e6e7
41969d34c4c92016a85c7cd0562303c4ccbe599467c24da118a5fe6fcd671c00
kappa = r  (deserialization returns INVALID)
2386f8a925e2885e233a9ccc1615c0d6c635387a3f0b3cbe003fad6bc972c2e6e7
41969d34c4c92016a85c7cd0562303c4ccbe599467c24da118a5fe6fcd671c01
~~~~~~~~~~
> Note: the BLS12_381 compressed values above are independently confirmed against multiple third-party implementations (e.g. zkcrypto/bls12_381, arkworks). The uncompressed values are derived from the same coordinates by the procedure of {{point-serialization-procedure}}, and have been checked to deserialize back to those coordinates. The BLS48_581 values are newly computed for this document by applying the sign_GF_p^8 function of {{I-D.ietf-cose-bls-key-representations}} to the BP' coordinates in {{secure_params}}; independent cross-validation from COSE/BBS implementers is welcome ahead of RGLC.

# Point Serialization for BN462  {#bn462-serialization-notes}

This appendix is informative. It records why the metadata placement of {{point-serialization}} does not fit BN462, which encodings existing BN462 implementations use, and which variants were considered and set aside. It defines no format, standardizes no algorithm, and places no requirement on implementations. Its purpose is to spare implementers of BN462 the effort of rediscovering the constraint described below, and to record the state of practice at the time of writing.

The observations below were obtained by reading the source code of the cited libraries in August 2026. They describe what those libraries emit and accept; no survey of deployed protocols or products was carried out.

## The Constraint  {#bn462-constraint}

The format of {{point-serialization}} carries three metadata bits (C_bit, I_bit, S_bit) in the three most significant bits of the first byte of a serialized coordinate. This requires the canonical representation of an element of GF(p) to leave at least three unused bits in its leading byte.

BN462 has a 462-bit characteristic p, so n = ceil(462 / 8) = 58 bytes, i.e. 464 bits, and only 464 - 462 = 2 bits are unused: one bit short, as stated in {{bn462-applicability}}.

The same criterion appears in implementations. {{MIRACL}} selects between two point encodings at compile time using the condition (MBITS - 1) mod 8 <= 4, where MBITS is the bit length of p; this condition holds exactly when the leading byte of a coordinate has three or more unused bits. When it holds, the library packs a compression flag (0x80) and a sign flag (0x20) into that byte, reserving 0x40; when it does not, it falls back to the {{SEC1}} encoding described in {{bn462-leading-byte}}. For BN462, (462 - 1) mod 8 = 5, so the fallback is used. The diagnosis above is therefore not particular to this document: an implementation supporting BN462 reaches the same conclusion by the same test. (The bit-packing path is additionally gated on a build-time option that the shipped configuration does not enable, so the default build uses the fallback encoding for every curve it supports.)

## Encodings Used by Existing Implementations  {#bn462-existing}

Few libraries implement BN462 at all. Among those surveyed for this document, it is absent from {{RELIC}}, {{blst}}, {{gnark-crypto}}, {{noble-curves}} and {{arkworks}}, each of which does implement BLS12_381. The following do provide it:

| Implementation | G_1 uncompressed | G_1 compressed | Metadata placement | Coordinate byte order |
|---|---|---|---|---|
| {{MIRACL}} | 117 bytes | 59 bytes | {{SEC1}} type byte (0x04 / 0x02 / 0x03) | big-endian |
| {{pfcurve-js}} | 117 bytes | 59 bytes | {{SEC1}} type byte (0x04 / 0x02 / 0x03) | big-endian |
| {{zig-pairings}} | 117 bytes | 59 bytes | dedicated flag byte, see {{bn462-leading-byte}} | big-endian |
| {{mcl}} | 116 bytes | 58 bytes | none / packed into the coordinate | little-endian by default |

For {{MIRACL}} and {{zig-pairings}}, the corresponding G_2 sizes are 233 bytes uncompressed and 117 bytes compressed.

In {{mcl}}, BN462 is marked deprecated. Its affine serialization writes x followed by y with no metadata at all, its compressed form stores the parity of y in the most significant bit of the most significant byte of the coordinate, and the identity element is represented by an all-zero string. {{MIRACL}} likewise has no distinct representation for the identity element: its coordinates are zero and the ordinary type byte is emitted, so the identity element is recognized by inspecting the coordinate values rather than by a metadata bit. This differs from the approach of {{point-serialization}}, which represents the identity element explicitly through I_bit and leaves the decision of whether to accept it to the calling protocol ({{identity-point-handling}}).

Two observations follow. First, implementations that support BN462 have converged on a dedicated leading byte rather than on packing metadata into the coordinate. Second, none of the implementations examined uses the two spare bits of the coordinate to carry metadata.

## Alternative Considered: Two Metadata Bits  {#bn462-two-bit}

S_bit is meaningful only for compressed points (see Step 1 of {{point-serialization-procedure}}). An encoding restricted to uncompressed points therefore needs only C_bit and I_bit, which fit in the two spare bits that BN462 does have. This possibility was raised by Frank Denis during review of this document. Its appeal is that BN462 would remain within the same bit-packing family as the other two curves: a decoder written for BLS12_381 could be extended to BN462 by masking two metadata bits instead of three, rather than by acquiring a second, leading-byte parser used only for BN462.

It carries a pitfall. Step 2 of {{point-deserialization-procedure}} determines the curve jointly from C_bit and the length of the string. For BN462 (n = 58, and E' represented over GF(p^2)), an uncompressed point on E and a compressed point on E' both occupy 116 bytes. Under a two-bit variant, compressed forms cannot be represented and C_bit is always 0, so the two cases never both arise; but a decoder that reuses Step 2 unmodified would accept a 116-byte string with C_bit set and interpret it as a compressed point on E'. An implementation of such a variant would have to reject a nonzero C_bit before the length-based determination in Step 2. An encoding with a dedicated leading byte ({{bn462-leading-byte}}) does not have this hazard, because the type byte separates the two cases structurally.

The variant would also forgo point compression, which is the form most protocols transmit. No implementation of it was found.

## Alternative Considered: A Dedicated Leading Byte  {#bn462-leading-byte}

Moving the metadata out of the coordinate and into a byte of its own removes the constraint of {{bn462-constraint}} entirely and keeps point compression available, at a cost of one byte. Two mutually incompatible variants are in use:

- The {{SEC1}} type byte: 0x04 for an uncompressed point, 0x02 or 0x03 for a compressed point with the value itself carrying the sign of y, followed by the coordinates in big-endian order. This is what {{MIRACL}} and {{pfcurve-js}} emit.

- A flag byte carrying the same three metadata bits as {{point-serialization-procedure}} in the same positions -- C_bit at 0x80, I_bit at 0x40, S_bit at 0x20, with the remaining five bits zero and checked on input -- followed by the coordinates in big-endian order. This is what {{zig-pairings}} emits. These are the same bit positions that {{MIRACL}} uses in its bit-packing path, so this variant amounts to relocating the bit assignment of {{point-serialization}} into a leading byte, leaving the rest of the format unchanged.

Both variants yield the same encoded lengths (117 and 59 bytes for G_1, 233 and 117 bytes for G_2), so length alone does not distinguish them. The leading byte does: an uncompressed point other than the identity element begins with 0x04 in the first variant and 0x00 in the second. A decoder that validates the leading byte rather than skipping over it will therefore reject a string in the other format instead of misparsing it.

## Why This Document Does Not Define a Format  {#bn462-not-defined}

{{bn462-applicability}} gives the reasons this document does not specify a BN462 point format. Two of them bear on what is recorded above. No specification examined defines or requires a BN462 point encoding, so the encodings listed here are library conventions rather than a format that a consumer of this document needs; and choosing among them would be encoding design rather than the recording of established practice.

Should BN462 point encodings converge, or should a protocol specification come to require one, a separate specification can define one. Of the encodings recorded above, the {{SEC1}} type byte is the most widely implemented among the libraries examined, and is the form an implementer is most likely to encounter when interoperating with existing BN462 code.

# Adoption Status of Pairing-Friendly Curves with the 100-bit Security Level  {#adoption_status_100bit_security}

BN curves including BN254 that were estimated as the 128-bit security level before exTNFS ensure no more than the 100-bit security level by the effect of exTNFS. The following table summarizes the adoption status of the parameters with a security level lower than the "Arnd 128-bit" range. Please refer to {{secure_params}} for the naming conventions for each curve.

| Category | Name | Supported 100-bit Curves |
|:---:|:---:|:---:|
| Standard | ISO/IEC | BN256I |
| Standard | TCG | BN256I |
| Standard | FIDO/W3C | BN256I |
| Standard | FIDO/W3C | BN256D |
| Library | mcl | BN254N |
| Library | mcl | BN_SNARK1 |
| Library | TEPLA | BN254B |
| Library | TEPLA | BN254N |
| Library | RELIC | BN254N |
| Library | RELIC | BN256D |
| Library | AMCL | BN254N |
| Library | AMCL | BN254CX |
| Library | AMCL | BN256I |
| Library | Intel IPP | BN256I |
| Library | MIRACL | BN254N |
| Library | MIRACL | BN254CX |
| Library | MIRACL | BN256I |
| Library | Adjoint | BN_SNARK1 |
| Library | Adjoint | BN254B |
| Library | Adjoint | BN254N |
| Library | Adjoint | BN254S1 |
| Library | Adjoint | BN254S2 |
| Application | Zcash | BN_SNARK1 |
| Application | DFINITY | BN254N |
| Application | DFINITY | BN_SNARK1 |

