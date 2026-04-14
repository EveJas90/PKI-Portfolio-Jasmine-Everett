# Lab [02] — [Diagnose a Broken Certificate Chain]

**Week 6 · PKI Incident Diagnosis & Troubleshooting**
**CVI PKI Career Pathway — Phase 1 Foundations**

---

## Incident Summary

**Target system:** Radiology imaging platform (simulated via incomplete-chain.badssl.com)
**Diagnosed by:** Jasmine Everett
**Date of diagnosis:** 4/13/26

---

### What failed

The TLS failure was caused by an incomplete certificate chain due to a missing issuer certificate.

---

### Evidence

![Description](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/48e697f10a8aa7f5136836284da39283fec78e0d/assets/screenshots/week-06/Chain%20Validation.png)
![Description](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/48e697f10a8aa7f5136836284da39283fec78e0d/assets/screenshots/week-06/AIA%20URI.png)
![Description](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/48e697f10a8aa7f5136836284da39283fec78e0d/assets/screenshots/week-06/Chain%20Val-2.png)

---

### Why it failed

The TLS connection for the Radiology imaging platform was caused due to a incomplete certificate chain. The issuer certificate was missing from the server so the issuer of the certificate was unable to be verifed.

---

### Chain status

The certificate chain was not structurally intact. The intermediate or issuer certificate was missing which caused the certificate to fail during the TLS handshake.

---

### Remediation path

To restore the failing system, I first identified that the certificate chain was incomplete by observing the verify return code 21. I then extracted the issuer information from the leaf certificate to determine the missing intermediate CA. I used the command in OpenSSL to find the AIA URI and navigated to that url to get the intermediate certificate.  After verifying that the intermediate matched the issuer, I would rebuild the certificate chain and updated the server configuration to include the full chain. Finally, once I restarted the service I would confirm successful validation with a verify return code of 0.

---

### Prevention

Once concrete thing that theorganization could do differently to prevent this failure is to make sure the intermediate/issuer cert is sent with the leaf cert. This would insure that the certificate chain stays in tact. 

---

## Diagnostic Steps

Document each step of the PKI Diagnostic Framework as you worked through it.

### Step 1 — Retrieve

**Command used:**

```
openssl s_client -connect incomplete-chain.badssl.com:443 -showcerts </dev/null 2>/dev/null \
| openssl x509 -outform PEM > leaf_cert.pem

openssl s_client -connect incomplete-chain.badssl.com:443 </dev/null 2>&1
```

**What you observed:**

The first command I used produced a leaf_cert.pem text file on my local directory. The second command produced the full out put of the certficate where I could visually verify that the TLS connection failed due to certificate chain the verify return code 21. This code means that the server was unable to verify the leaf certificate.

---

### Step 2 — Parse

**Command used:**

```
openssl x509 -in leaf_cert.pem -text -noout
```

**Key fields from the certificate:**

| Field | Value |
|---|---|
| Subject CN |*.badssl.com|
| Issuer |C=US, O=Let's Encrypt, CN=R13|
| Not Before |Mar 24 20:02:52 2026 GMT|
| Not After |Jun 22 20:02:51 2026 GMT |
| SAN entries |*.badssl.com |

**What you found:**

The parsed certificate told me that the failure isn't related to an expired certificate. The certificate doesn't expire until Not Before Mar 24 20:02:52 2026 GMT and Not After Jun 22 20:02:51 2026 GMT.


---

### Step 3 — Validate the Chain

**Command used:**

```
openssl verify -untrusted issuer_cert.pem leaf_cert.pem
```

**Result:**

The first command was used to validate the certificate relationship between the leaf certificate and the intermediate (issuer) certificate. The output leaf_cert.pem: OK indicates that the leaf certificate can be correctly chained to the provided intermediate certificate, meaning the signing relationship between those two certificates is valid.

**What you found:**
This step confirms that the TLS failure was due to a broken certificate chain because of the output Verify return code 21. In previous steps we were able to validate the certificate relationship between the leaf and intermediate certificates showing that the intermediate certificate is missing to fully complete the TLS handshake.

### Step 4 — Check Revocation and Trust

**Command used:**

```
openssl s_client -connect incomplete-chain.badssl.com:443 -showcerts </dev/null 2>/dev/null \
| grep "Verify return code"
```

**What you found:**

This command tested the full TLS certificate chain as it would be seen by a client connecting to the server. The output Verify return code: 21 (unable to verify the first certificate) indicates that the server is missing a required intermediate certificate or that the client cannot build a complete trust chain to a trusted root in its certificate store.

---

## Reflection


This lab helped reinforce how to validate that a certficate is configured correctly by following verify the certificate chain and understanding the errors that are produced by the terminal. This helped clarify how to effectively remediated a certificate when the certificate chain is broken.

---

*CVI PKI Career Pathway — Foundations Phase*
