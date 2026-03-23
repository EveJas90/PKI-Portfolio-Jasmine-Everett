# Lab 03 — Verify a Certificate Chain

## Overview
Briefly describe what this lab was about in your own words.
What PKI concept were you investigating?

This lab focuses on retrieving a digital certificate and validating it by examining its contents using OpenSSL. I inspected certificate fields such as subject and issuer, and verifed that the certificate chain was properly constructed by linking the lear certificate to an intermediate CA and then to a trusted root CA.

The PKI concept that I am investigating is chain of trust verification.

---

## Environment
- OS: Windows 11
- Terminal used (Mac Terminal / Git Bash / WSL): Git Bash
- OpenSSL version (`openssl version`): OpenSSL 3.5.5 27 Jan 2026
- Website used: github.com

---

## Chain Verification Result
Paste the output of your `openssl verify` command:

server.pem: OK

---

## Certificate Roles

| Certificate | Subject | Issuer | CA:TRUE/FALSE |
|---|---|---|---|
| server.pem |CN=github.com | C=GB, O=Sectigo Limited, CN=Sectigo Public Server Authentication CA DV E36 |CA:FALSE |
| intermediate.pem | C=GB, O=Sectigo Limited, CN=Sectigo Public Server Authentication CA DV E36 |C=GB, O=Sectigo Limited, CN=Sectigo Public Server Authentication Root E46 |CA:TRUE |
| root.pem |  C=GB, O=Sectigo Limited, CN=Sectigo Public Server Authentication Root E46|C=US, ST=New Jersey, L=Jersey City, O=The USERTRUST Network, CN=USERTrust ECC Certification Authority |CA:TRUE |

---

## Observations

1. Which certificate is the root CA?

   The root.pem certificate is the root CA.

   
3. Which is the intermediate CA?

   The intermediate.pem is the intermediate CA.
   
5. Which is the leaf certificate?

   The leaf certificate is server.pm
   
7. How does the Issuer field connect the chain?
   
   From what I observed each Issuer atched the subject of the chain as the chain went further up.
   
9. Why do intermediate certificates exist?

    Intermediate certificates exist because they protect the root CA.  

---

## Challenges / Troubleshooting
Document any issues encountered and how you resolved them.
