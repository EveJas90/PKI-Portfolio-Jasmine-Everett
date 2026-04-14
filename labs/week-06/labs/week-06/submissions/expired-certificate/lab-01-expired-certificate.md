# Lab [1] — [Expired Certificate]

**Week 6 · PKI Incident Diagnosis & Troubleshooting**
**CVI PKI Career Pathway — Phase 1 Foundations**

---

## Incident Summary

**Target system:** portal.metrogeneral.org (simulated via expired.badssl.com)

**Diagnosed by:** Jasmine Everett

**Date of diagnosis:** 4/12/26

---

### What failed

The TLS failure was caused by an expired certificate.

---

### Evidence

 ![Description](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/5388cffd88e0b0c91a901c0cbd23ef8062389aed/assets/screenshots/week-06/NotB4NotAfter.png)
 
![Description](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/b55aa90db12e54c8b01ca36c812256a021cfde06/assets/screenshots/week-06/VerificationError.png)

---

### Why it failed

The certificate has an notBefore=Apr  9 00:00:00 2015 GMT and notAfter=Apr 12 23:59:59 2015 GMT vadility date. Since the certificate expired 3 days ago the certificate chain could not be validated. This can be also vailidated by verification error which shows on this certificate as: Verification error: certificate has expired (See Screenshot in Evidence). 

---

### Chain status

The certificate chain was structurally intact. I could confirm this from the output after I used OpenSSL to verify the certificate chain. I counted 3 certficate blocks that started with -----BEGIN CERTIFICATE-----. There were no other chain-related issues that I could see.

---

### Remediation path

This is what I would do to restore the failing system:

-Create a 2048-bit RSA private key using OpenSSL and then generate a Certificate Signing Request. Make sure to accurately provide the correct fields for the distingushed name as this will be the name of your certificate's subject.

-Once the Certificate Signing Request(CSR) is completed and you have verifed the distingushed name matches the information I provided I would then use my organization's Certificate Lifecycle Management (CLM) system to send the certificate to the Certificate Authority to be signed. 

-Once the Certificate Authority verifes and approves my certificate request it will return me a signed certificate.

-Once I recieve the certificate I would compare the orginal Certificate Signing Request(CSR) with the signed one from the Certificate Authority by extracting and comparing their public keys using the "diff" command in OpenSSL.

-I can confirm the integrity of this new certificate if I recieve a blank output in the terminal.

-Then I would inventory the new certificate, making sure that the ownership, expiration dates, etc are accurately documented to prevent a failure like this from happening again.

-Finally I would deploy the leaf and intermediate certificate together to the server. It is important to deploy the intermediate certificate with the leaf certificate because if you do not the certificate chain would be broken. This is a common mistake and will lead to a TLS failure down the line. 

---

### Prevention 

One concrete thing the organization could do differently to preven this failure is to have some sort of Certificate Lifecyle Management system in place. This system would be able to catch a certificate before it expired because there would be a record of this certificate which could easily be reissued. 

---

## Diagnostic Steps

Document each step of the PKI Diagnostic Framework as you worked through it.

### Step 1 — Retrieve

**Command used:**

```
openssl s_client -connect expired.badssl.com:443 -showcerts </dev/null 2>/dev/null\
| openssl x509 -outform PEM > expired_cert.pem
```

**What you observed:**

The ouput in the terminal was blank but on my local machine I located a text file with the name "expired_cert.pem". When I opened the file in a text editor I can also confirm that this is the cert because the text is in a readable format.

---

### Step 2 — Parse

**Command used:**

```
openssl x509 -in expired_cert.pem -text -noout
```

**Key fields from the certificate:**

| Field | Value |
|---|---|
| Subject CN |*.badssl.com |
| Issuer | C=GB, ST=Greater Manchester, L=Salford, O=COMODO CA Limited, CN=COMODO RSA Domain Validation Secure Server CA |
| Not Before |Apr  9 00:00:00 2015 GMT |
| Not After | Apr 12 23:59:59 2015 GMT|
| SAN entries | N/A |

**What you found:**

The parsed certificate told me that the failure was caused by the certificate expiring.

---

### Step 3 — Validate the Chain

**Command used:**

```
openssl s_client -connect expired.badssl.com:443 -showcerts </dev/null 2>/dev/null
```

**Result:**

Using the above command the chain structure of the certificate was valid. I can confirm this because I was able to cound 3 -----BEGIN CERTIFICATE----- blocks. The error that this certificate recieved was: Verification error: certificate has expired


**What you found:**

This step confirmed that the TLS failure was caused due to an expired certificate.

---

### Step 4 — Check Revocation and Trust

**Command used:**

```
openssl x509 -in expired_cert.pem -noout -text | grep -A1 "OCSP"

```
```
openssl x509 -in cert.pem -noout -issuer
openssl x509 -in issuer.pem -noout -subject


```

```
 openssl ocsp \
  -issuer issuer.pem \
  -cert cert.pem \
  -url $(openssl x509 -in cert.pem -noout -ocsp_uri) \
  -resp_text \
  -noverify
```

**What you found:**

When I ran the above command the terminal produced the ouput below. An OSCP url was present as:http://ocsp.comodoca.com. I tried to check the revocation status of the OSCP URL but I kept getting Responder Error: unauthorized (6).  I verifed that the issuer and subject of the leaf and intermediate certs that I pulled from the expired cert was correct but since the certificate was expired the error I recievd from the responder was expected.


![Description](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/b55aa90db12e54c8b01ca36c812256a021cfde06/assets/screenshots/week-06/OCSP%20Query.png)

---

## Reflection

This lab reinforced my understanding of how certificates are structured, how the TLS handshake works to validate certificates as it moves up the chain, and how to remediate a expired certificate. I had to slow down and think about how to do an OSCP query. I forgot to include the intermeidate certificate when I was trying to connect to the OSCP responder. Once I figured that out I was able to continue troubleshooting.

---

*CVI PKI Career Pathway — Foundations Phase*
