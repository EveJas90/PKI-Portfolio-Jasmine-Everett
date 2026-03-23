# Lab 01 — Inspect X.509 Certificate Fields

## Overview
Briefly describe what this lab was about in your own words.
What PKI concept were you investigating?

This lab's purpose was to show how to read and interpet the key fields of a X.509 certificate.

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
| Serial Number        | aa:23:02:42:8e:f4:39:7e:10:bb:2c:32:93:1c:fc:2e|
| Signature Algorithm  | ecdsa-with-SHA256      |
| Issuer               | C=US, O=Google Trust Services, CN=WE2|
| Subject              | CN=*.google.com|
| Not Before           | Feb 23 18:19:56 2026 GMT|
| Not After            | May 18 18:19:55 2026 GMT|
| Public Key Algorithm | id-ecPublicKey          |

---

## Observations

1. Who issued the certificate?
   
   Google Trust Services
  
3. What domain or organization does it represent?
   
   It represents google.com
   
5. When does it expire?
   
   It expires May 18th
   
7. What public key algorithm is used?
   
   The public key algorithm used was ecdsa-with-SHA256
   
9. Why does the Issuer field matter in a PKI system?
    
   The issuer field matters in PKI because it tells who the oganization is that issued the certificate. 
