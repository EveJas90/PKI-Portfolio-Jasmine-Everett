# Lab 01 — Inspect X.509 Certificate Fields

## Overview
Briefly describe what this lab was about in your own words.
What PKI concept were you investigating?

This lab's purpose was to show how to read and interpet the key fields of a x.509 certificate.

---

## Environment
- OS: Windows 11
- Terminal used (Mac Terminal / Git Bash / WSL): Git Bash
- OpenSSL version (`openssl version`): OpenSSL 3.5.5 27 Jan 2026

---

## Certificate Fields

| Field                | Value from your output |
|----------------------|------------------------|
| Version              | 3 (0x2)                |
| Serial Number        | 7f:e5:30:bf:33:13:43:be:dd:82:16:10:49:3d:8a:1b|
| Signature Algorithm  | sha256WithRSAEncryption|
| Issuer               | C=BE, O=GlobalSign nv-sa, OU=Root CA, CN=GlobalSign Root CA|
| Subject              | C=US, O=Google Trust Services LLC, CN=GTS Root R4|
| Not Before           | Nov 15 03:43:21 2023 GMT|
| Not After            | Jan 28 00:00:42 2028 GMT|
| Public Key Algorithm | id-ecPublicKey          |

---

## Observations

1. Who issued the certificate?
   Global Sign
  
3. What domain or organization does it represent?
   It represents Google Trust Services LLC
   
5. When does it expire?
   It expires Jan 28
   
7. What public key algorithm is used?
   The public key algorithm used was sha256WithRSAEncryption
   
9. Why does the Issuer field matter in a PKI system?
   The issuer field matters in PKI because it tells who the oganization is that issued the certificate. 
