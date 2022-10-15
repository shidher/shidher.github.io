---
title: "sidh notes"
date: 2022-08-10T00:07:01-07:00
tags:
  - cryptography
show_summary: false
---

## beginning glossary

**SIDH** - supersingular isogeny diffie hellman.

**Supersingular** - an elliptic curve with maximal order.

**Elliptic curve** - this definition is simplified. It's an equation of the form \\(y^2 \equiv x^3 + ax + b \bmod p\\) where \\(a, b, p\in \mathbb{Z}\\).

**Isogeny** - a morphism, not an isomorphism, between two elliptic curves.

**Diffie-Hellman** - a key exchange that relies on the hardness of dlog.

**dlog** - stands for the discrete logarithm problem. Given \\(g, b, p\\), find \\(a\\) given \\(g^a \equiv b \bmod p\\). You can't do it in reasonable time if the given variables are large enough.

Two elliptic curves are isomorphic iff they have the same **j-invariant**.

## sidh key exchange

The public key consists of:

1. a prime \\(p = \omega_A^{e_A}\omega_B^{e_B}f \pm 1 \\\), where \\(\omega_A^{e_A} \approx \omega_B^{e_B}\\) and \\(\omega_A, \omega_B\\) are usually \\(2, 3\\).
2. a supersingular elliptic curve \\(E\\) over \\(\mathbb{F}\_{p^2}\\). We extend the finite field of order \\(p\\) over the complex numbers
3. \\(P_A, Q_A, P_B, Q_B\\) are points on \\(E\\) such that the order of \\(P_A, Q_A = \omega_A^{e_A}\\) and the order of \\(P_B, Q_B = \omega_B^{e_B}\\). If the order of a point \\(P\\) is \\(a\\), that means \\(aP = I\\) where \\(I\\) is the identity or the point of infinity.

The key exchange between parties A and B will be as follows:

1. A creates random integers \\(m_A, n_A < \omega_A^{e_A}\\).
2. A computes \\(R_A = m_AP_A + n_AQ_A\\).
3. A computes an isogeny \\(\phi_A : E\rightarrow E_A\\) where \\(R_A\\) is the kernel of \\(\phi_A\\). The kernel of an isogeny \\(\phi_A : (x, y)\rightarrow (f(x), f(x, y))\\) is the set of points such that the denominator of \\(f(x)\\) or \\(f(x, y)\\) are \\(0\\).
4. A sends to B \\(E_A, \phi_A(P_B), \phi_A(Q_B)\\).
5. Repeat steps 1-4 switching A and B.
6. A computes \\(S\_{BA} = m_A(\phi_B(P_A)) + n_A(\phi_B(Q_A))\\)
7. A computes an isogeny \\(\phi\_{BA} : E \rightarrow E\_{BA}\\) where \\(S\_{BA}\\) is its kernel.
8. A computes K, the j-invariant of \\(E\_{BA}\\), which is the shared secret key.
9. Repeat steps 6-8 switching B and A.
