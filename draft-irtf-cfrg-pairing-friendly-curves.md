---
title: "Pairing-Friendly Curves"
docname: draft-irtf-cfrg-pairing-friendly-curves-latest
category: info
ipr: trust200902
area: IRTF
workgroup: CFRG
keyword: Internet-Draft

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

Currently, Boneh-Lynn-Shacham (BLS) signature schemes are being standardized {{I-D.boneh-bls-signature}} and utilized in several blockchain projects such as Ethereum {{Ethereum}}, Algorand {{Algorand}}, Chia Network {{Chia}}, and DFINITY {{DFINITY}}. The aggregation functionality of BLS signatures is effective for their applications of decentralization and scalability.

## Motivation and Contribution  {#goal}

At CRYPTO 2016, Kim and Barbulescu proposed an efficient number field sieve (NFS) algorithm for the discrete logarithm problem in a finite field GF(p^k) {{KB16}}. The attack improves the polynomial selection that is the first step in the number field sieve algorithm for discrete logarithms in GF(p^k). The idea is applicable when the embedding degree k is a composite that satisfies k = i*j (gcd (i, j) = 1, i, j> 1). The basic idea is based on the equality GF(p^k) = (GF(p^i)^j) and one of the improvement for reducing the amount of cost for solving the discrete logarithm problem is using sub-field calculation. Several types of pairing-friendly curves such as Barreto-Naehrig curves (BN curves){{BN05}} and Barreto-Lynn-Scott curves (BLS curves){{BLS02}} are affected by the attack, since a pairing-friendly curve suitable for cryptographic applications requires that the discrete logarithm problem is sufficiently difficult. Please refer to {{KB16}} for detailed ideas and calculation algorithms of the attack by Kim. In particular, BN254, which is a BN curve with a 254-bit characteristic effective for pairing calculations, was adopted by a lot of cryptographic libraries as a parameter of the 128-bit security level, however, BN254 ensures no more than the 100-bit security level due to the effect of the attack, where the security levels described in this memo correspond to the security strength of NIST recommendation {{NIST}}.

To resolve this effect immediately, several research groups and implementers re-evaluated the security of pairing-friendly curves and they respectively proposed various curves that are secure against the attack {{BD18}} {{BLS12_381}}.

In this memo, we list the security levels of certain pairing-friendly curves, and motivate our choices of curves. First, we summarize the adoption status of pairing-friendly curves in international standards, libraries and applications, and classify them in the 128-bit, 192-bit, and 256-bit security levels. Then, from the viewpoints of "security" and "widely used", pairing-friendly curves corresponding to each security level are selected in accordance with the security evaluation by Barbulescu and Duquesne {{BD18}}.

As a result, we recommend the BLS curve with 381-bit characteristic of embedding degree 12 and the BN curve with the 462-bit characteristic for the 128-bit security level, and the BLS curves of embedding degree 48 with the 581-bit characteristic for the 256-bit security level. This memo shows their specific test vectors.

## Requirements Terminology  {#requirements-terminology}

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in BCP 14 {{RFC2119}} {{RFC8174}} when, and only when, they appear in all capitals, as shown here.


# Preliminaries  {#preliminaries}

## Elliptic Curves  {#elliptic-curve}

Let p be a prime number and q = p^n for a natural number n > 0, where p at least 5. Let GF(q) be a finite field. The curve defined by the following equation E is called an elliptic curve:

~~~~~~~~~~

   E : y^2 = x^3 + a * x + b,

~~~~~~~~~~

and a and b in GF(q) satisfy the discriminant inequality 4 * a^3 + 27 * b^2 != 0 mod q. This is called the Weierstrass normal form of an elliptic curve.

A solution (x,y) to the equation E can be thought of as a point on the corresponding curve. For a natural number k, we define the set of (GF(q^k))-rational points of E, denoted by E(GF(q^k)), to be the set of all solutions (x,y) in GF(q^k), together with a 'point at infinity' O_E, which is defined to lie on every vertical line passing through the curve E.

The set E(GF(q^k)) forms a group under a group law that can be defined geometrically as follows. For P and Q in E(GF(q^k)) define P + Q to be the reflection around the x-axis of the unique third point R of intersection of the straight line passing through P and Q with the curve E. If the straight line is tangent to E, we say that it passes through that point twice. The identity of this group is the point at infinity O_E. We also define scalar multiplication [K]P for a positive integer K as the point P added to itself (K-1) times. Here, [0]P becomes the point at infinity O_E and the relation [-K]P = -([K]P) is satisfied.

## Pairings  {#pairing}

A pairing is a bilinear map defined on two subgroups of rational points of an elliptic curve. Examples include the Weil pairing, the Tate pairing, the optimal Ate pairing {{Ver09}}, and so on. The optimal Ate pairing is considered to be the most efficient to compute and is the one that is most commonly used for practical implementation.

Let E be an elliptic curve defined over a prime field GF(p). Let k be the minimum integer for which r is a divisor of p^k - 1; this is called the embedding degree of E over GF(p). Let G_1 be a cyclic subgroup of E(GF(p)) of order r, there also exists a cyclic subgroup of E(GF(p^k)) of order r, define this to be G_2. Let d be a divisor of k and E' be an elliptic curve defined over GF(p^(k/d)). If an isomorphism from E' to E(GF(p^k)) exists, then E' is called the twist of E. It can sometimes be convenient for efficiency to do the computations of G_2 in the twist E', and so consider G_2 to instead be a subgroup of E'. Let G_T be an order r subgroup of the multiplicative group (GF(p^k))^*; this exists by definition of k.

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
:   a multiplicative group of GF(p).

(GF(p^k))*:
:   a multiplicative group of GF(p^k).

b:
:   a primitive element of the multiplicative group (GF(p))^*.

O_E:
:   the point at infinity over an elliptic curve E.

E(GF(p^k)):
:   the group of GF(p^k)-rational points of E.

\#E(GF(p^k)):
:   the number of GF(p^k)-rational points of E.

r:
:   the order of G_1 and G_2.

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

In the case of BN curves, we can use twists of the degree 6. If m is an element that is neither a square nor a cube in an extension field GF(p^2), the twist E' of E is defined over an extension field GF(p^2) by the equation E': y^2 = x^3 + b' with b' = b / m or b' = b * m. BN curves are called D-type if b' = b / m, and M-type if b' = b * m. The embedding degree k is 12.

A pairing e is defined by taking G_1 as a subgroup of E(GF(p)) of order r, G_2 as a subgroup of E'(GF(p^2)), and G_T as a subgroup of a multiplicative group (GF(p^12))^* of order r.

## Barreto-Lynn-Scott Curves  {#BLSdef}

A BLS curve {{BLS02}} is a another family of pairing-frinedly curves proposed in 2002. Similar to BN curves, a pairing over BLS curves constructs optimal Ate pairings.

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

A pairing e is defined by taking G_1 as a subgroup of E(GF(p)) of order r, G_2 as an order r subgroup of E'(GF(p^2)) for BLS12 and of E'(GF(p^8)) for BLS48, and G_T as an order r subgroup of a multiplicative group (GF(p^12))^* for BLS12 and of a multiplicative group (GF(p^48))^* for BLS48.

## Representation Convention for an Extension Field  {#representation-convention-for-an-extension-field}

Pairing-friendly curves use a tower of some extension fields. In order to encode an element of an extension field, focusing on interoperability, we adopt the representation convention shown in Appendix J.4 of {{I-D.ietf-lwig-curve-representations}} as a standard and effective method. Note that the big-endian encoding is used for an element in GF(p) which follows to mcl {{mcl}}, ISO/IEC 15946-5 {{ISOIEC15946-5}} and etc.

Let GF(p) be a finite field of characteristic p and GF(p^d) = GF(p)(i) be an extension field of GF(p) of degree d.

~~~~~~~~~~
For an element s in GF(p^d) such that s = s_0 + s_1 * i + ... +
s_{d - 1} * i^{d - 1} where s_0, s_1, ... , s_{d - 1} in the
basefield GF(p), s is represented as octet string by
oct(s) = s_0 || s_1 || ... || s_{d - 1}.
~~~~~~~~~~

Let GF(p^d') = GF(p^d)(j) be an extension field of GF(p^d) of degree d' / d.

~~~~~~~~~~
For an element s' in GF(p^d') such that s' = s'_0 + s'_1 * j + ...
+ s'_{d' / d - 1} * j^{d' / d - 1} where s'_0, s'_1, ...,
s'_{d' / d - 1} in the basefield GF(p^d), s' is represented as
integer by oct(s') = oct(s'_0) || oct(s'_1) || ... ||
oct(s'_{d' / d - 1}), where oct(s'_0), ... , oct(s'_{d' / d - 1})
are octet strings encoded by above convention.
~~~~~~~~~~

In general, one can define encoding between integer and an element of any finite field tower by inductively applying the above convention.

The parameters and test vectors of extension fields described in this memo are encoded by this convention and represented in an octet stream.

When applications communicate elements in an extension field, using the compression method {{MP04}} may be more effective. In that case, care for interoperability must be taken.


# Security of Pairing-Friendly Curves  {#security_pfc}

## Evaluating the Security of Pairing-Friendly Curves  {#evaluating-the-security-of-pairing-friendly-curves}

The security of pairing-friendly curves is evaluated by the hardness of the following discrete logarithm problems:

- The elliptic curve discrete logarithm problem (ECDLP) in G_1 and G_2
- The finite field discrete logarithm problem (FFDLP) in G_T

There are other hard problems over pairing-friendly curves used for proving the security of pairing-based cryptography. Such problems include the computational bilinear Diffie-Hellman (CBDH) problem, the bilinear Diffie-Hellman (BDH) problem, the decision bilinear Diffie-Hellman (DBDH) problem, the gap DBDH problem, etc. {{ECRYPT}}. Almost all of these variants are reduced to the hardness of discrete logarithm problems described above and are believed to be easier than the discrete logarithm problems.

Although it would be sufficient to attack any of these problems to attack pairing-based crytography, the only known attacks thus far attack the discrete logarithm problem directly, so we focus on the discrete logarithm in this memo.

The security levels of pairing-friendly curves are estimated by the computational cost of the most efficient algorithm for solving the above discrete logarithm problems. The best-known algorithms for solving the discrete logarithm problems are based on Pollard's rho algorithm {{Pollard78}} and Index Calculus {{HR83}}. To make index calculus algorithms more efficient, number field sieve (NFS) algorithms are utilized.

## Impact of Recent Attacks  {#impact}

In 2016, Kim and Barbulescu proposed a new variant of the NFS algorithms, the extended tower number field sieve (exTNFS), which drastically reduces the complexity of solving FFDLP {{KB16}}. The exTNFS improves the polynomial selection that is the first step in the number field sieve algorithm for discrete logarithms in GF(p^k). The idea is applicable when the embedding degree k is a composite that satisfies k = i * j (gcd (i, j) = 1, i, j> 1). Since the above condition is satisfied especially when k = 2^n*3^m (n, m> 1), BN curves and BLS curves whose embedding degree is divisible by 6 are affected by the exTNFS. The basic idea of the exTNFS is based on the equality GF(p^k) = (GF(p^i)^j) and one of the improvement for reducing the amount of cost for solving FFDLP is using sub-field calculation. Please refer to {{KB16}} for detailed ideas and calculation algorithms of exTNFS. Due to exTNFS, the security levels of certain pairing-friendly curves asymptotically dropped down. For instance, Barbulescu and Duquesne estimated that the security of the BN curves, which had been believed to provide 128-bit security (BN256, for example) was reduced to approximately 100 bits {{BD18}}. Here, the security levels described in this memo correspond to the security strength of NIST recommendation {{NIST}}.

There has since been research into the minimum bit length of the parameters of pairing-friendly curves for each security level when applying exTNFS as an attacking method for FFDLP. For 128-bit security, Barbulescu and Duquesne estimated the minimum bit length of p of BN curves and BLS12 curves after exTNFS as 461 bits {{BD18}}. For 256-bit security, Kiyomura et al. estimated the minimum bit length of p^k of BLS48 curves as 27,410 bits, which indicated 572 bits of p {{KIK17}}.


# Selection of Pairing-Friendly Curves  {#secure_params}

In this section, we introduce some of the known secure pairing-friendly curves that consider the impact of exTNFS.

First, we show the adoption status of pairing-friendly curves in standards, libraries and applications, and classify them in accordance with the 128-bit, 192-bit, and 256-bit security levels. Then, from the viewpoints of "security" and "widely used", pairing-friendly curves corresponding to each security level are selected and their parameters are indicated.

In our selection policy, it is important that selected curves are shown in peer-reviewed papers for security and that they are widely used in cryptographic libraries. In addition, "efficiency" is one of the important aspects but greatly dependant on implementations, so we choose to prioritize "security" and "widely used" over "efficiency" in consideration of future interconnections and interoperability over the internet.

As a result, we recommend the BLS curve with 381-bit characteristic of embedding degree 12 and the BN curve with the 462-bit characteristic for the 128-bit security level, and the BLS curves of embedding degree 48 with the 581-bit characteristic for the 256-bit security level. On the other hand, we do not show the parameters for 192-bit security here because there are no curves that match our selection policy.

## Adoption Status of Pairing-friendly Curves  {#impl}

<!-- NOTE: The comprehensive adoption status table (Table 1 in v12 XML) has been
     removed from this section due to its size and feedback from CFRG members
     that it may not be suitable for inclusion in the I-D body.
     The table data is preserved in notes/table_adoption.xml and
     notes/table_adoption.md for reference.
     TODO: Determine the appropriate publication venue for the full table
     (e.g., companion document, external resource, or appendix).
     See GitHub issue for tracking. -->

We show the pairing-friendly curves that have been selected by existing standards, cryptographic libraries, and applications.

The adoption status of pairing-friendly curves is surveyed in standards, libraries and applications. In this survey, "Arnd" is an abbreviation for "Around". The curves categorized as 'Arnd 128-bit', 'Arnd 192-bit' and 'Arnd 256-bit' for each label show that their security levels are within the range of plus/minus 5 bits for each security level. Other labels shown with '~' mean that the security level of the categorized curve is outside the range of each security level. Specifically, the security level of the categorized curves is more than the previous column and is less than the next column. The details are described as the following subsections. A BN curve with a XXX-bit characteristic p is denoted as BNXXX and a BLS curve of embedding degree k with a XXX-bit p is denoted as BLSk_XXX.

This section omits parameters with security levels below the "Arnd 128-bit" range due to space limitations and viewpoints of secure usage of parameters. On the other hand, indicating which standards, libraries, and applications use these lower security level parameters would be useful information for implementers, therefore {{adoption_status_100bit_security}} shows these parameters. In addition, the full version is available at [https://lepidum.co.jp/blog/2020-03-27/ietf-draft-pfc/](https://lepidum.co.jp/blog/2020-03-27/ietf-draft-pfc/).

The security level for each curve is evaluated in accordance with {{BD18}}, {{GMT19}}, {{MAF19}} and {{FK18}}. Note that the Freeman curves and MNT curves are not included in this survey because {{BD18}} does not show the security levels of these curves.

### International Standards  {#standardization}

ISO/IEC 15946 series specifies public-key cryptographic techniques based on elliptic curves. ISO/IEC 15946-5 {{ISOIEC15946-5}} shows numerical examples of MNT curves{{MNT01}} with 160-bit p and 256-bit p, Freeman curves {{Freeman06}} with 224-bit p and 256-bit p, and BN curves with 160-bit p, 192-bit p, 224-bit p, 256-bit p, 384-bit p, and 512-bit p. These parameters do not take into account the effects of the exTNFS. On the other hand, the parameters may be revised in future versions since ISO/IEC 15946-5 is currently under development. As described below, BN curves with 256-bit p and 512-bit p specified in ISO/IEC 15946-5 used by other standards and libraries, these curves are especially denoted as BN256I and BN512I. The suffix 'I' of BN256I and BN512I are given from the initials of the standard name ISO.

TCG adopts the BN256I and a BN curve with 638-bit p specified by their own{{TPM}}. FIDO Alliance {{FIDO}} and W3C {{W3C}} adopt BN256I, BN512I, the BN638 by TCG, and the BN curve with 256-bit p proposed by Devegili et al.{{DSD07}} (named BN256D). The suffix 'D' of BN256D is given from the initials of the first author's name of the paper which proposed the parameter.

### Cryptographic Libraries  {#cryptographic_libraries}

There are a lot of cryptographic libraries that support pairing calculations.

PBC is a library for pairing-based cryptography published by Stanford University that supports BN curves, MNT curves, Freeman curves, and supersingular curves {{PBC}}. Users can generate pairing parameters by using PBC and use pairing operations with the generated parameters.

mcl{{mcl}} is a library for pairing-based cryptography that supports four BN curves and BLS12_381 {{GMT19}}. These BN curves include BN254 proposed by Nogami et al. {{NASKM08}} (named BN254N), BN_SNARK1 suitable for SNARK applications{{libsnark}}, BN382M, and BN462. The suffix 'N' of BN256N and the suffix 'M' of BN382M are respectively given from the initials of the first author's name of the proposed paper and the library's name mcl. Kyushu University published a library that supports the BLS48_581 {{BLS48}}. The University of Tsukuba Elliptic Curve and Pairing Library (TEPLA) {{TEPLA}} supports two BN curves, BN254N and BN254 proposed by Beuchat et al. {{BGMORT10}} (named BN254B). The suffix 'B' of BN254B is given from the initials of the first author's name of the proposed paper. Intel published a cryptographic library named Intel Integrated Performance Primitives (Intel-IPP) {{Intel-IPP}} and the library supports BN256I.

RELIC {{RELIC}} uses various types of pairing-friendly curves including six BN curves (BN158, BN254R, BN256R, BN382R, BN446, and BN638), where BN254R, BN256R, and BN382R are RELIC specific parameters that are different from BN254N, BN254B, BN256I, BN256D, and BN382M. The suffix 'R' of BN382R is given from the initials of the library's name RELIC. In addition, RELIC supports six BLS curves (BLS12_381, BLS12_446, BLS12_445, BLS12_638, BLS24_477, and BLS48_575 {{MAF19}}), Cocks-Pinch curves of embedding degree 8 with 544-bit p{{GMT19}}, pairing-friendly curves constructed by Scott et al. {{SG19}} based on Kachisa-Scott-Schaefer curves with embedding degree 54 with 569-bit p (named K54_569){{MAF19}}, a KSS curve {{KSS08}} of embedding degree 18 with 508-bit p (named KSS18_508) {{AFKMR12}}, Optimal TNFS-secure curve {{FM19}} of embedding degree 8 with 511-bit p(OT8_511), and a supersingular curve {{S86}} with 1536-bit p (SS_1536).

Apache Milagro Crypto Library (AMCL){{AMCL}} supports four BLS curves (BLS12_381, BLS12_461, BLS24_479 and BLS48_556) and four BN curves (BN254N, BN254CX proposed by CertiVox, BN256I, and BN512I). In addition to AMCL's supported curves, MIRACL {{MIRACL}} supports BN462 and BLS48_581.

Adjoint published a library that supports the BLS12_381 and six BN curves (BN_SNARK1, BN254B, BN254N, BN254S1, BN254S2, and BN462) {{AdjointLib}}, where BN254S1 and BN254S2 are BN curves adopted by an old version of AMCL {{AMCLv2}}. The suffix 'S' of BN254S1 and BN254S2 are given from the initials of developper's name because he proposed these parameters.

The Celo foundation published the bls12377js library {{bls12377js}}. The supported curve is the BLS12_377 curve which is shown in {{BCGMMW20}}.

### Applications  {#applications}

Zcash uses a BN curve (named BN128) in their library libsnark {{libsnark}}. In response to the exTNFS attacks, they proposed new parameters using BLS12_381 {{BLS12_381}} {{GMT19}}and published its experimental implementation {{zkcrypto}}.

Ethereum 2.0 adopted BLS12_381 and uses the implementation by Meyer {{pureGo-bls}}. Chia Network published their implementation {{Chia}} by integrating the RELIC toolkit {{RELIC}}. DFINITY uses mcl, and Algorand published an implementation which supports BLS12_381.

## For 128-bit Security  {#for-128-bits-of-security}

The survey in {{impl}} shows a lot of cases of adopting BN and BLS curves. Among them, BLS12_381 and BN462 match our selection policy. Especially, the one that best matches the policy is BLS12_381 from the viewpoint of "widely used" and "efficiency", so we introduce the parameters of BLS12_381 in this memo.

On the other hand, from the viewpoint of the future use, the parameter of BN462 is also introduced. As shown in recent security evaluations for BLS12_381{{BD18}} {{GMT19}}, its security level close to 128-bit but it is less than 128-bit. If the attack is improved even a little, BLS12_381 will not be suitable for the curve of the 128-bit security level. As curves of 128-bit security level are currently the most widely used, we recommend both BLS12_381 and BN462 in this memo in order to have a more efficient and a more prudent option respectively.

### BLS Curves for the 128-bit security level (BLS12_381)  {#parameter-BLS12_381}

In this part, we introduce the parameters of the Barreto-Lynn-Scott curve of embedding degree 12 with 381-bit p that is adopted by a lot of applications such as Zcash {{Zcash}}, Ethereum {{Ethereum}}, and so on.

The BLS12_381 curve is shown in {{BLS12_381}} and it is defined by the parameter

~~~~~~~~~~

    t = -2^63 - 2^62 - 2^60 - 2^57 - 2^48 - 2^16

~~~~~~~~~~

where the size of p becomes 381-bit length.

For the finite field GF(p), the towers of extension field GF(p^2), GF(p^6) and GF(p^12) are defined by indeterminates u, v, and w as follows:

~~~~~~~~~~

    GF(p^2) = GF(p)[u] / (u^2 + 1)
    GF(p^6) = GF(p^2)[v] / (v^3 - u - 1)
    GF(p^12) = GF(p^6)[w] / (w^2 - v).

~~~~~~~~~~

Defined by t, the elliptic curve E and its twist E' are represented by E: y^2 = x^3 + 4 and E': y^2 = x^3 + 4(u + 1). BLS12_381 is categorized as M-type.

We have to note that the security level of this pairing is expected to be 126 rather than 128 bits {{GMT19}}.

Parameters of BLS12_381 are given as follows.

- G_1 is the largest prime-order subgroup of E(GF(p)) - BP = (x,y) : a 'base point', i.e., a generator of G_1
- G_2 is an r-order subgroup of E'(GF(p^2)) - BP' = (x',y') : a 'base point', i.e., a generator of G_2 (encoded with {{I-D.ietf-lwig-curve-representations}}) - x' = x'_0 + x'_1 * u (x'_0, x'_1 in GF(p)) - y' = y'_0 + y'_1 * u (y'_0, y'_1 in GF(p)) - h' : the cofactor #E'(GF(p^2))/r

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

As mentioned above, BLS12_381 is adopted in a lot of applications. Since it is expected that BLS12_381 will continue to be widely used more and more in the future, {{zcash_rep_bls12_381}} shows the serialization format of points on an elliptic curve as useful information. This serialization format is also adopted in {{I-D.boneh-bls-signature}} {{zkcrypto}}.

In addition, many pairing-based cryptographic applications use a hashing to an elliptic curve procedure that outputs a rational point on an elliptic curve from an arbitrary input. A standard specification of ciphersuites for a hashing to an elliptic curve, including BLS12_381, is under discussion in the IETF {{I-D.irtf-cfrg-hash-to-curve}} and it will be valuable information for implementers.

### BN Curves for the 128-bit security level (BN462)  {#bn-curves}

A BN curve with the 128-bit security level is shown in {{BD18}}, which we call BN462. BN462 is defined by the parameter

~~~~~~~~~~

    t = 2^114 + 2^101 - 2^14 - 1

~~~~~~~~~~

for the definition in {{BNdef}}.

For the finite field GF(p), the towers of extension field GF(p^2), GF(p^6) and GF(p^12) are defined by indeterminates u, v, and w as follows:

~~~~~~~~~~

    GF(p^2) = GF(p)[u] / (u^2 + 1)
    GF(p^6) = GF(p^2)[v] / (v^3 - u - 2)
    GF(p^12) = GF(p^6)[w] / (w^2 - v).

~~~~~~~~~~

Defined by t, the elliptic curve E and its twist E' are represented by E: y^2 = x^3 + 5 and E': y^2 = x^3 - u + 2, respectively. The size of p becomes 462-bit length. BN462 is categorized as D-type.

We have to note that BN462 is significantly slower than BLS12_381, but has 134-bit security level {{GMT19}}, so may be more resistant to future small improvements to the exTNFS attack.

We note also that CP8_544 is about 20% faster that BN462 {{GMT19}}, has 131-bit security level, and that due to its construction will not be affected by future small improvements to the exTNFS attack. However, as this curve is not widely used (it is only implemented in one library), we instead chose BN462 for our 'safe' option.

We give the following parameters for BN462.

- G_1 is the largest prime-order subgroup of E(GF(p)) - BP = (x,y) : a 'base point', i.e., a generator of G_1
- G_2 is an r-order subgroup of E'(GF(p^2)) - BP' = (x',y') : a 'base point', i.e., a generator of G_2 (encoded with {{I-D.ietf-lwig-curve-representations}}) - x' = x'_0 + x'_1 * u (x'_0, x'_1 in GF(p)) - y' = y'_0 + y'_1 * u (y'_0, y'_1 in GF(p)) - h' : the cofactor #E'(GF(p^2))/r

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

For the finite field GF(p), the towers of extension field GF(p^2), GF(p^4), GF(p^8), GF(p^24) and GF(p^48) are defined by indeterminates u, v, w, z, and s as follows:

~~~~~~~~~~

    GF(p^2) = GF(p)[u] / (u^2 + 1)
    GF(p^4) = GF(p^2)[v] / (v^2 + u + 1)
    GF(p^8) = GF(p^4)[w] / (w^2 + v)
    GF(p^24) = GF(p^8)[z] / (z^3 + w)
    GF(p^48)= GF(p^24)[s] / (s^2 + z).

~~~~~~~~~~

The elliptic curve E and its twist E' are represented by E: y^2 = x^3 + 1 and E': y^2 = x^3 - 1 / w. BLS48_581 is categorized as D-type.

We then give the parameters for BLS48_581 as follows.

- G_1 is the largest prime-order subgroup of E(GF(p)) - BP = (x,y) : a 'base point', i.e., a generator of G_1
- G_2 is an r-order subgroup of E'(GF(p^8)) - BP' = (x',y') : a 'base point', i.e., a generator of G_2 (encoded with {{I-D.ietf-lwig-curve-representations}}) - x' = x'_0 + x'_1 * u + x'_2 * v + x'_3 * u * v + x'_4 * w + x'_5 * u * w + x'_6 * v * w + x'_7 * u * v * w (x'_0, ..., x'_7 in GF(p)) - y' = y'_0 + y'_1 * u + y'_2 * v + y'_3 * u * v + y'_4 * w + y'_5 * u * w + y'_6 * v * w + y'_7 * u * v * w (y'_0, ..., y'_7 in GF(p)) - h' : the cofactor #E'(GF(p^8))/r

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


# Security Considerations  {#security-considerations}

The recommended pairing-friendly curves are selected by considering the exTNFS proposed by Kim et al. in 2016 {{KB16}} and they are categorized in each security level in accordance with {{BD18}}. Implementers who will newly develop pairing-based cryptography applications SHOULD use the recommended parameters. As of 2020, as far as we've investigated the top cryptographic conferences in the past, there are no fatal attacks that significantly reduce the security of pairing-friendly curves after exTNFS.

BLS curves of embedding degree 12 typically require a characteristic p of 461 bits or larger to achieve the 128-bit security level {{BD18}}. Note that the security level of BLS12_381, which is adopted by a lot of libraries and applications, is slightly below 128 bits because a 381-bit characteristic is used {{BD18}} {{GMT19}}.

BN254 is used in most of the existing implementations as shown in {{impl}} and {{adoption_status_100bit_security}}, however, BN curves that were estimated as the 128-bit security level before exTNFS including BN254 ensure no more than the 100-bit security level by the effect of exTNFS.

In addition, implementors should be aware of the following points when they implement pairing-based cryptographic applications using recommended curves. Regarding the use case and applications of pairing-based cryptographic applications, please refer {{applications-of-pairing-based-cryptography}}.

In applications such as key agreement protocols, users exchange the elements in G_1 and G_2 as public keys. To check these elements are so-called sub-group secure {{BCM15}}, implementors should validate if the elements have the correct order r. Specifically, for public keys P in G_1 and Q in G_2, a receiver should calculate scalar multiplications [r]P and [r]Q, and check the results become points at infinity.

The pairing-based protocols, such as the BLS signatures, use a scalar multiplication in G_1, G_2 and an exponentiation in G_3 with the secret key. In order to prevent the leakage of secret key due to side channel attacks, implementors should apply countermeasure techniques such as montgomery ladder {{Montgomery}} {{CF06}} when they implement modules of a scalar multiplication and an exponentiation. Please refer {{Montgomery}} and {{CF06}} for the detailed algorithms of montgomery ladder.

When converting between an element in extension field and an octet string, implementors should check that the coefficient is within an appropriate range {{IEEE1363}}. If the coefficient is out of range, there is a possible that security vulnerabilities such as the signature forgery may occur.

Recommended parameters are affected by the Cheon's attack which is a solving algorithm for the strong DH problem {{Cheon06}}. The mathematical problem that provides the security of the strong DH problem is called ECDLP with Auxiliary Inputs (ECDLPwAI). In ECDLPwAI, given rational points P, [K]P, [K^i]P, for i=1,...,n, then we find a secret K. Since the complexity of ECDLPwAI is given as O(sqrt((r-1)/n + sqrt(n)) where n divides r-1 by using Cheon's algorithm whereas the complexity of ECDLP is given as O(sqrt(r)), the complexity of ECDLPwAI with the ideal value n becomes dramatically smaller than that of ECDLP. Please refer {{Cheon06}} for the details of Cheon's algorithm. Therefore, implementers should be careful when they design cryptographic protocols based on the strong DH problem. For example, in the case of Short Signatures, they can prevent the Cheon's attack by carefully setting the maximum number of queries which corresponds to the parameter n.


# IANA Considerations  {#iana-considerations}

This document has no actions for IANA.


# Acknowledgements  {#acknowledgements}

The authors would like to appreciate a lot of authors including Akihiro Kato for their significant contribution to early versions of this memo. The authors would also like to acknowledge Kim Taechan, Hoeteck Wee, Sergey Gorbunov, Michael Scott, Chloe Martindale as an Expert Reviewer, Watson Ladd, Armando Faz, Rene Struik, and Satoru Kanno for their valuable comments.


--- back

{::nomarkdown}
<references xmlns:xi="http://www.w3.org/2001/XInclude">
      <name>References</name>
      <references>
        <name>Normative References</name>
        <xi:include href="https://xml2rfc.tools.ietf.org/public/rfc/bibxml/reference.RFC.2119.xml" />
        <xi:include href="https://xml2rfc.tools.ietf.org/public/rfc/bibxml/reference.RFC.8174.xml" />
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
        <xi:include href="https://xml2rfc.tools.ietf.org/public/rfc/bibxml/reference.RFC.8017.xml" />

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

        <reference anchor="ZCashRep" target="https://github.com/zkcrypto/pairing/blob/master/src/bls12_381/README.md">
          <front>
            <title>BLS12-381</title>
            <author>
              <organization>Electric Coin Company</organization>
            </author>
            <date year="2017" month="July" />
          </front>
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

        <xi:include href="https://xml2rfc.tools.ietf.org/public/rfc/bibxml3/reference.I-D.draft-ietf-lwig-curve-representations-08.xml" />
        <xi:include href="https://xml2rfc.tools.ietf.org/public/rfc/bibxml3/reference.I-D.draft-boneh-bls-signature-00.xml" />
        <xi:include href="https://xml2rfc.tools.ietf.org/public/rfc/bibxml3/reference.I-D.draft-irtf-cfrg-hash-to-curve-09.xml" />

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
            <title>ISO/IEC 15946-5:2017</title>
            <seriesInfo name="ISO/IEC" value="Information technology -- Security techniques -- Cryptographic techniques based on elliptic curves -- Part 5: Elliptic curve generation" />
            <author>
              <organization>ISO/IEC</organization>
            </author>
            <date year="2017" />
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
        <reference anchor="pureGo-bls" target="https://github.com/phoreproject/bls">
          <front>
            <title>Pure GO bls library</title>
            <author initials="J." surname="Meyer">
              <organization />
            </author>
            <date year="2019" />
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

        <reference anchor="MP04" target="https://eprint.iacr.org/2004/032.pdf">
          <front>
            <title>Cocks–Pinch curves of embedding degrees five to eight and optimal ate pairing computation</title>
            <seriesInfo name="Cryptology ePrint Archive" value="Report 2019/431" />
            <author initials="A." surname="Guillevic">
              <organization />
            </author>
            <author initials="S." surname="Masson">
              <organization />
            </author>
            <author initials="E." surname="Thome">
              <organization />
            </author>
            <date year="2019" />
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

        <reference anchor="MAF19" target="https://www.researchgate.net/publication/337011283_Computing_the_Optimal_Ate_Pairing_over_Elliptic_Curves_with_Embedding_Degrees_54_and_48_at_the_256-bit_security_level">
          <front>
            <title>Computing the Optimal Ate Pairing over Elliptic Curves with Embedding Degrees 54 and 48 at the 256-bit security level</title>
            <seriesInfo name="International Journal of Applied Cryptography" value="to appear" />
            <author initials="N.B." surname="Mbiang">
              <organization />
            </author>
            <author initials="D.F." surname="Aranha">
              <organization />
            </author>
            <author initials="E." surname="Fouotsa">
              <organization />
            </author>
            <date year="2019" />
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

      </references>
    </references>

{:/nomarkdown}

# Computing the Optimal Ate Pairing  {#comp_pairing}

Before presenting the computation of the optimal Ate pairing e(P, Q) satisfying the properties shown in {{pairing}}, we give the subfunctions used for the pairing computation.

The following algorithm, Line_Function shows the computation of the line function. It takes Q_1 = (x_1, x_2), Q_2 = (x_2, y_2) in G_2, and P = (x, y) in G_1 as input, and outputs an element of G_T.

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

When implementing the line function, implementers should consider the isomorphism of E and its twist curve E' so that one can reduce the computational cost of operations in G_2 {{CLN09}}{{KIK17}}. We note that Line_function does not consider such an isomorphism.

The computation of the optimal Ate pairing uses the Frobenius endomorphism. The p-power Frobenius endomorphism pi for a point Q = (x, y) over E' is pi(p, Q) = (x^p, y^p).

## Optimal Ate Pairings over Barreto-Naehrig Curves  {#optimal-ate-pairings-over-barreto-naehrig-curves}

Let c = 6 * t + 2 for a parameter t and c_0, c_1, ... , c_L in {-1,0,1} such that the sum of c_i * 2^i (i = 0, 1, ..., L) equals c.

The following algorithm shows the computation of the optimal Ate pairing on BN curves. It takes P in G_1, Q in G_2, an integer c, c_0, ...,c_L in {-1,0,1} such that the sum of c_i * 2^i (i = 0, 1, ..., L) equals c, and the order r of G_1 as input, and outputs e(P, Q).

~~~~~~~~~~

    f := 1; T := Q;
    if (c_L = -1) then
        T := -T;
    end if
    for i = L-1 downto 0
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

Let c = t for a parameter t and c_0, c_1, ... , c_L in {-1,0,1} such that the sum of c_i * 2^i (i = 0, 1, ..., L) equals c.

The following algorithm shows the computation of the optimal Ate pairing on Barreto-Lynn-Scott curves. It takes P in G_1, Q in G_2, an integer c, c_0, ...,c_L in {-1,0,1} such that the sum of c_i * 2^i (i = 0, 1, ..., L) equals c, and the order r of G_1 as input, and outputs e(P, Q).

~~~~~~~~~~

    f := 1; T := Q;
    if (c_L = -1) then
        T := -T;
    end if
    for i = L-1 downto 0
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


# Test Vectors of Optimal Ate Pairing  {#test-vectors-of-optimal-ate-pairing}

We provide test vectors for Optimal Ate Pairing e(P, Q) given in {{comp_pairing}} for the curves BLS12_381, BN462 and BLS48_581 given in {{secure_params}}. Here, the inputs P = (x, y) and Q = (x', y') are the corresponding base points BP and BP' given in {{secure_params}}.

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

where u, v and w are indeterminates and x'_0, ..., x'_7 and y'_0, ..., y'_7 are elements of GF(p). The representation of Q = (x', y') given below is followed by {{I-D.ietf-lwig-curve-representations}}.

In addition, we use the notation e_i (i = 0, ..., k-1) to represent each element in e(P, Q), where the extension field that e(P, Q) belongs is constructed according to {{I-D.ietf-lwig-curve-representations}}.

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

e_0:
:   0x0728370dfbe5b77483ce4be9cba465ce0a624407b17be0d4a68ce60f7bece07aedee994e4ee9c5b4970136ab4d09e1957538037f290fdb875f137cce19812172f171694f9ae7aadbda

e_1:
:   0x08153d69faf7476a66dc870abb71cff7e2d90cd6b5969bb92eb04c282d83e528d796faf28497114ce34c60130b48e03fd06681555b2ef9ec654f4a5fb9a2ce4b49ddb80c0fc9f845ab

e_2:
:   0x0394a48406ac0d6583a9b7c1b43eae6e32fea940f9b24eb03bcba914f99998e9fb698ebec6ef1f9abcce67a15c812e01541e73fcba359babf5e8830e313dbb6716de23fb074ccc8bd3

e_3:
:   0x063b8c23ade1f4674806b55661e57ef6d749e791560b38e55d2aa746ebf75799bb2e16e6d33117b9553e232c43872951b20efdaa0c7b93fc2e20c30a841b1b985aca97e720d814e87f

e_4:
:   0x10244cbd679f194c45965a50869479062301721b57429930fc8ba3b3149f4204a3ff91181afa8fcd22b718e56a72d9b2b0c164ce32956ca3506b8210c3a8e4724d08a1baaf4464958e

e_5:
:   0x0e9258fbc8e42fd5d0226dad77384b51ebeb58b7419bbf350369eb84a5089f06a2c4a5849a379ad7d11680699ade3288f0086616e93213d473b709e3f6edf1967eb94442a45c90ebbb

e_6:
:   0x06e134492deb4396f6c9c80738d0af481dc5b181d718ed56155cb6c8599a18b93b58e9fb782a392b9e571277cd89c8de5a3d0a93628e2a2b1b7edbe94cb84dcf7b1f090ca39bd6f364

e_7:
:   0x07de5e32e7289ac7873a933c4708466a99f117c61a2ee14f94327cd73af436318c2032dc75545cdfa271347ab7cbcdf2dad37b672130c0342984a5f197b4093088e411273f3dfe417e

e_8:
:   0x0de32640097a9acce11fa2ae92b4d9372e114bc8bfd31d2aec38a1cc75565ca1926142ce8bed385cd72e1c60b7aef66a337279682f832c7309fcb29fa84b459cbe65240cdc1af3e6c7

e_9:
:   0x06d3acae2d679917ecf0e8216e14e8bf4f24e0fb5673b4247056f84b7da26960d36db96b704075d01e807e1b971a47905c95b4d0667f9319e97614dc246716bd202152420f32dd768b

e_10:
:   0x046e7f9077ace54376cab916b83d3b30dd8114ef02cc443c259f8b904ec21bfe7964f4ea6c65c181e41cfe368def11874dcdba6eb7299359a2d213c2e444bdec4cbaa79c5cadc7ea92

e_11:
:   0x0b371eeb284afe2b375c899589a3141f51c40c52fce2c166a493f4c403ffd4cc5d4e0d4d27dc60923613f39288b4aab576719555e2b14e52888c76cd70297fb398a0a64852a5bfd40d

e_12:
:   0x0a2bd6b1b811c704e258eefb47fce7172ffc3277f710da2ab13257b0d02addf3a65757058edb746e18cecccbfc17bc4376ce8031ba12d871aa8e0debede2d497bef0e00a5c0e706a3e

e_13:
:   0x0fd0ff5e67cc61f810c9babd05b3693472da111df2f66479f4f076ebedfebfc854a6642288cb82c65ee44a67af11b1855756704aefc25453327e0692f480bc9a4a46e9128d06d38af0

e_14:
:   0x02124bc2c807a8033e462a4f1352c6098ee56930953622e92e0d037da27258fbf67af6ad3a8a99a262c32f15bd90e90612b8cec6b20c1b374dbb932d1e9c43fbeeb61b02982899cde2

e_15:
:   0x080213072f8830aab2f299e808fc275b342acd465c16bc39c215c0f727f6f9ed81e6f7b927b6524dd7bb246e1332f21555fe9482d8f6b9781ef89b70b1b14b7eda497a9459204e9f0b

e_16:
:   0x015da7065c8d4a232746542a7ca008a46d65a777a86cffbd5c64e29c7ed25092aa0a13c5ca6789a064306264bb65b9cce72cb81b3229a805546c70a30abafb7484f4b482ea56ccc07e

e_17:
:   0x0eb3b6aa48b99bfb400d83252f1ccec395b564847e02fc31ef71abfaedd94193fc0fac3321ee8c7402948e128c87436da9bdbcb437350e5169771bc39f4af509c0800a69e5ef6913aa

e_18:
:   0x0754210c9887c9416fa7c486f33d1ca06344f395537ef9cf8d39e2058f8f050437ee6d6e566ac3adfc9d9dd53fe48b440560e080291e439d29c4cfeb0a6fe0593c5700bbffa6c523b7

e_19:
:   0x09a028b784dafdb4d0756d1020efe3c1500e40db3276bf5dc46d451b814aedbb03f4613b95174a32a326483970d3c4339bba5ba51d198d7c121abed82c3523162445a0276f48bf89ff

e_20:
:   0x116a0f7f6792d653084bb65e6e88db3cfbdea4e7e8b6ab6450ce59cf44a0e6e0b14965e3494a02c6839ada9b1eb54909ac295538e0f183c61f78524d894bb2a0da124b1fe9b83495db

e_21:
:   0x0d7dad6094d2348e788664c42b558efd83abe5e0dd2974f71f5be2f91537544ee4f9361444786d28f69914b44208035d3d6fc2bb4ec6c246fa00fb4751c2599ea49e0cbfa063764466

e_22:
:   0x0aaaa489911ca73b50ffa76b6c259661296dcd60c9fe0046890eca9e1347c7973e18cc517f2af15bda018a6fb31b5d2d274fc9cddb4dc9652cfe2141651325f4039819826724b4beaf

e_23:
:   0x0fb49f14ba49afa5d6f59a314a592e07f8d5bf177cb8210dcb6c778c757cb1b5dd78ca03ee84c5bbb445bd6273245eec49eeefef70b35afb1e2914f617c019291383a69ee210ae519e

e_24:
:   0x0aee140954f99c762830f952df644afa9b79ddbf340ee544dc3b8858b857715d7bd9822ef61baa1ab166bf263db8849048593c8caa6f129a488a359eeca33ee231231d14013958af23

e_25:
:   0x0acc6c4ea0c92d932c05a1f2e4e2db503b70d661ac395d64830f73726a7abebedfa44891a77302dd1864a6cd0d253ea08178a27bc2dbd0dd8066c022e9bd7946000dfa7d9614157925

e_26:
:   0x050deec1fa36e12c4c4077859abd06b6220daf70f921e262283abf30b5aee64e6889b9b352339785c8d326abdb47f3dce0fa6afde2d15c3799fe82233f93971b1103ec74a4861acc6e

e_27:
:   0x1230408a5e21add1ec9b1a80c48e11f93376d28790b46f20b5fc62eb4e65d61b92918e4d7363615e1c404bc4de4163f5662acb9853ea97802a3367a52dd18d0d2b80f5dcae12bfada1

e_28:
:   0x0798bb2076792ef2c410c5d7d04cd8c23c6ae903a3036f89b6b95238a1da6340831bb4954ae0680e79fa168711436cd75fb59066d700461f09cd777efb21cfae7378de3ff104914e0e

e_29:
:   0x0e415108b64594f34071373b75a074e6a704f71d97a9297c96f6a496b2554cc37a0c0f593a98e5d450959144302e4b8928df6cef8e27dbda83812c5b4a5ca8dcb902151eff1eaf1935

e_30:
:   0x04f59c80adb5919935615d23fa2d3a2edede7c37b4a4b40b867e69855426f820285a0ce2d3e42675ed3ea9945976627cd6730e8da68bb38a9add565fe442a584bed3ef4b6907875544

e_31:
:   0x034ceeb2e39c61a5732ebee4e80714a97f82cd0cc174d5f4dfd19034f8c3dc0d25fc16112456345e7f22a02e898948798e18b297adc534072dad9ec6b6f57e3b85c598b45d860b0209

e_32:
:   0x07c4a289d9f064dc9b9cb7e07345abcd341ebfe63ba33671563f73f3a1430f96767cad3c9b1c96216968f530e2a26afc67abc3cbd68d64d6ab924cd7b00e1dac94b6d334a1804e51ac

e_33:
:   0x04ba54ae9b7eff857175045310b4b5c1b12fb63bf89786bbf43963f615c347d9b86852c89b0f12d447d3c7ac0645dc34a08f2e40af5564ac16609da1c5a9155090ab4ea65ff1f395a5

e_34:
:   0x088297a3f699e1920875ef0bb319312236e65d19ec729bbdaecdd78ad99332b3420ded29fb0c25d985f6032e946554fdc8e95b8126d08dd236504fc4f3cf43dc20ec76a9a54b56c08b

e_35:
:   0x1122ea6890e187b23e25041f49940a771373bc7b88154ccce6c9c2fa1e3e3d342abd03918c9741fa1712eadc42614a9de0466c459d92fbad01852a2199981f03f7ad75731cb65078c0

e_36:
:   0x0e47723170a6efb1cfda14f5f1e733af065cbc55b52add250f9ffe92726c8bfabb24b3e7fcefe9c83dad6fe243a2eaea00f3556fcfc79cdf90d3947c9317fbee25490778e42a0db50a

e_37:
:   0x0b8a8b3d05b9826e28a3392bcb84d786ff40746c1ad9daf4da585f1a075c68e7a17ce525f8fd3f2d6e379bb67f4f89f8c0d42532c0065a9d23fa23efb9ff560515bf694de2b3ce2724

e_38:
:   0x00a403cc73a4449a59995a0bcbd924b40f1d6bca65033acd6e7f71d3452a0843774b0d8f7948f5045db5aa81927a222ca411b63c763a88fdde8e150d0bb5b300f77c95f4281450575a

e_39:
:   0x0881c6d9d3a2e972d2491f8d436d4c351fc02c33ad7d83ef14515a560041febc0a1a9a5ecaa987850dc5240fe0521ac611feb7a73a29915368ad73025dfe6a2823f833b18a9ec48ad9

e_40:
:   0x0d8fe3d94bd8e4927a1518dbacc7939d941b9547e5dc63ac1fdc84f806a933b93a537f5688a13931563c760487c8cfb2c16fd1ac6abc4f181d9b547ef8e48b7e45546c801afc0ec30c

e_41:
:   0x03d1bbb1efd04933769302a2cb1cf46f009aac17598036a24e350a622a84c54d42391e398c1454841913c5ebeb2a519219ea093368d7a6416d1806cd2edc7ddc647ece0809c24b96e9

e_42:
:   0x0a436fb468257035cf1aacb2390c4a8c560c1411b72d8b3a14fc5cc7b7d9cd6a1c8447e8496ba1d859d539a284c6d988f9d291dfc18cedc7ddde66c0572531d3fe1a3eacf67065b4c2

e_43:
:   0x0a0067f37999d6d9c6a4be4917cd723e26b7c51e204578a4fc4f68a253fb83e22c84f77189ccb151fb3b6cf9a7742817dbac70e5d10b55178210ac92fbee9a19924fa90fbe7505d8ce

e_44:
:   0x0b74c455ff1b2fb483d34ce165dd95cc3496dd2502278d3c29037184822c367fb58e65af3e2448f156e1bb455da57ec187eb809d5e3ba4541aa9685fa9f9a2805e45b4404169c7ac72

e_45:
:   0x0818fdfb70557e572d3b12b5fa4fd355b54aec27a4cb4a875d01401c1ea9d0ab3c4c758f6c26e0e79efc0fce3c1b9c0ec20d6c201e1f3ef5a45673bd29dd6d6ac01adb3e6be648413c

e_46:
:   0x042b061a336224d0435d40195c2994864df20cf62f2d08d0792ca250e46226f72cb8bec3f0ee38c5f263baa20de278f178d3c589bb23952a798ef778923b40c2e0b5d921efcbeb9036

e_47:
:   0x0f2a3120c31e22b435ccbc3b4cb67776fd4f9b24fb0361ea147a0f44b9356f94417c030893c8bdc2d205e61f4c41e74be9561f03009056e5b8e9a12c5b8b90cb89375433dbc3f2df3f


# ZCash serialization format for BLS12_381  {#zcash_rep_bls12_381}

This section describes the serialization format defined by {{ZCashRep}}. It is not officially standardized by the standards organization, however we show it in this appendix as a useful reference for implementers. This format applies to points on the BLS12_381 elliptic curves E and E', whose parameters are given in {{parameter-BLS12_381}}. Note that this serialization method is based on the representation shown in {{SEC1}} and it is a tiny tweak so as to apply to GF(p^m).

At a high level, the serialization format is defined as follows:

- Serialized points include three metadata bits that indicate whether a point is compressed or not, whether a point is the point at infinity or not, and (for compressed points) the sign of the point's y-coordinate.
- Points on E are serialized into 48 bytes (compressed) or 96 bytes (uncompressed). Points on E' are serialized into 96 bytes (compressed) or 192 bytes (uncompressed).
- The serialization of a point at infinity comprises a string of zero bytes, except that the metadata bits may be nonzero.
- The serialization of a compressed point other than the point at infinity comprises a serialized x-coordinate.
- The serialization of an uncompressed point other than the point at infinity comprises a serialized x-coordinate followed by a serialized y-coordinate.

Below, we give detailed serialization and de-serialization procedures. The following notation is used in the rest of this section:

- Elements of GF(p^2) are represented as polynomial with GF(p) coefficients like .
- For a byte string str, str[0] is defined as the first byte of str.
- The function sign_GF_p(y) returns one bit representing the sign of an element of GF(p). This function is defined as follows:

~~~~~~~~~~

    sign_GF_p(y) := { 1 if y > (p - 1) / 2, else
                   { 0 otherwise.

~~~~~~~~~~

- The function sign_GF_p^2(y') returns one bit representing the sign of an element in GF(p^2). This function is defined as follows:

~~~~~~~~~~

    sign_GF_p^2(y') := { sign_GF_p(y'_0) if y'_1 equals 0, else
                      { 1 if y'_1 > (p - 1) / 2, else
                      { 0 otherwise.

~~~~~~~~~~

## Point Serialization Procedure  {#zcash_rep_bls12_381_seri_procedure}

The serialization procedure is defined as follows for a point P = (x, y). This procedure uses the I2OSP function defined in {{RFC8017}}.

1. Compute the metadata bits C_bit, I_bit, and S_bit, as follows:
   - C_bit is 1 if point compression should be used, otherwise it is 0.
   - I_bit is 1 if P is the point at infinity, otherwise it is 0.
   - S_bit is 0 if P is the point at infinity or if point compression is not used. Otherwise (i.e., when point compression is used and P is not the point at infinity), if P is a point on E, S_bit = sign_GF_p(y), else if P is a point on E', S_bit = sign_GF_p^2(y).
2. Let m_byte = (C_bit * 2^7) + (I_bit * 2^6) + (S_bit * 2^5).
3. Let x_string be the serialization of x, which is defined as follows:
   - If P is the point at infinity on E, let x_string = I2OSP(0, 48).
   - If P is a point on E other than the point at infinity, then x is an element of GF(p), i.e., an integer in the inclusive range [0, p - 1]. In this case, let x_string = I2OSP(x, 48).
   - If P is the point at infinity on E', let x_string = I2OSP(0, 96).
   - If P is a point on E' other than the point at infinity, then x can be represented as (x_0, x_1) where x_0 and x_1 are elements of GF(p), i.e., integers in the inclusive range [0, p - 1] (see discussion of vector representations above). In this case, let x_string = I2OSP(x_1, 48) concatenated with I2OSP(x_0, 48). Notice that in all of the above cases, the 3 most significant bits of x_string[0] are guaranteed to be 0.
4. If point compression is used, let y_string be the empty string. Otherwise (i.e., when point compression is not used), let y_string be the serialization of y, which is defined in Step 3.
5. Let s_string be the concatenation of x_string and y_string.
6. Set s_string[0] = x_string[0] OR m_byte, where OR is computed bitwise. After this operation, the most significant bit of s_string[0] equals C_bit, the next bit equals I_bit, and the next equals S_bit. (This is true because the three most significant bits of x_string[0] are guaranteed to be zero, as discussed above.)
7. Output s_string.

## Point deserialization procedure  {#zcash_rep_bls12_381_deseri_procedure}

The deserialization procedure is defined as follows for a string s_string. This procedure uses the OS2IP function defined in {{RFC8017}}.

1. Let m_byte = s_string[0] AND 0xE0, where AND is computed bitwise. In other words, the three most significant bits of m_byte equal the three most significant bits of s_string[0], and the remaining bits are 0. If m_byte equals any of 0x20, 0x60, or 0xE0, output INVALID and stop decoding. Otherwise:
   - Let C_bit equal the most significant bit of m_byte,
   - Let I_bit equal the second most significant bit of m_byte, and
   - Let S_bit equal the third most significant bit of m_byte.
2. If C_bit is 1:
   - If s_string has length 48 bytes, the output point is on the curve E.
   - If s_string has length 96 bytes, the output point is on the curve E'.
   - If s_string has any other length, output INVALID and stop decoding.

   If C_bit is 0:
   - If s_string has length 96 bytes, the output point is on E.
   - If s_string has length 192 bytes, the output point is on E'.
   - If s_string has any other length, output INVALID and stop decoding.
3. Let s_string[0] = s_string[0] AND 0x1F, where AND is computed bitwise. In other words, set the three most significant bits of s_string[0] to 0.
4. If I_bit is 1:
   - If s_string is not the all zeros string, output INVALID and stop decoding.
   - Otherwise (i.e., if s_string is the all zeros string), output the point at infinity on the curve that was determined in step 2 and stop decoding.

   Otherwise, I_bit must be 0. Continue decoding.
5. If C_bit is 0:
   - Let x_string be the first half of s_string.
   - Let y_string be the last half of s_string.
   - Let x = OS2IP(x_string).
   - Let y = OS2IP(y_string).
   - If the point P = (x, y) is not a valid point on the curve that was determined in step 2, output INVALID and stop decoding.
   - Otherwise, output the point P = (x, y) and stop decoding.

   Otherwise, C_bit must be 1. Continue decoding.
6. Let x = OS2IP(s_string).
7. If the curve that was determined in step 2 is E:
   - Let y2 = x^3 + 4 in GF(p).
   - If y2 is not square in GF(p), output INVALID and stop decoding.
   - Otherwise, let y = sqrt(y2) in GF(p) and let Y_bit = sign_GF_p(y).

   Otherwise, (i.e., when the curve that was determined in step 2 is E'):
   - Let y2 = x^3 + 4 * (u + 1) in GF(p^2).
   - If y2 is not square in GF(p^2), output INVALID and stop decoding.
   - Otherwise, let y = sqrt(y2) in GF(p^2) and let Y_bit = sign_GF_p^2(y).
8. If S_bit equals Y_bit, output P = (x, y) and stop decoding. Otherwise, output P = (x, -y) and stop decoding.


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

