# Lab [03] — [Diagnose a Hostname and SAN Mismatch]

**Week 6 · PKI Incident Diagnosis & Troubleshooting**
**CVI PKI Career Pathway — Phase 1 Foundations**

---

## Incident Summary

**Target system:** Staff scheduling portal (simulated via wrong.host.badssl.com)
**Diagnosed by:** Jasmine Everett
**Date of diagnosis:** 4/13/26

---

### What failed

The TLS connection was tested against a misconfigured hostname simulating a DNS or certificate mismatch scenario.

---

### Evidence

**Command used:**

```
$ openssl s_client -connect wrong.host.badssl.com:443 -servername wrong.host.badssl.com </dev/null 2>&1

```

<img width="262" height="36" alt="San Mismatch Evidence" src="https://github.com/user-attachments/assets/1e2ab28d-dfb0-4b01-a1c9-2739d8a460f7" />
<img width="220" height="30" alt="San Mismatch Evidence2" src="https://github.com/user-attachments/assets/474660a7-7418-4ae8-87c0-f090985ca322" />

-Verify return code: 0 (ok)

-Subject CN: *.badssl.com

-Issuer: Let’s Encrypt R13

-Certificate chain successfully validated without errors


---

### Why it failed

The TLS handshake succeeded because the certificate chain was valid and trusted by the system’s root CA store. However, the hostname mismatch between the SNI value and the expected domain demonstrates that certificate validation and hostname verification are separate processes. In real browser environments, this mismatch would typically trigger a security warning even if the certificate chain itself is valid.

---

### Chain status

The certificate chain was structurally complete, consisting of the leaf certificate, intermediate CA (Let’s Encrypt R13), and root CA (ISRG Root X1). No chain-building or trust issues were detected, and verification returned “ok”.

---

### Remediation path

The correct remediation would be to ensure that the certificate is issued for the exact hostname being used in DNS and SNI. If DNS records were updated, a new certificate request should be generated for the updated hostname. The updated certificate would then need to be deployed to the server or load balancer handling TLS termination.

---

### Prevention

Implement automated certificate management tied to DNS changes, ensuring that any hostname updates trigger certificate reissuance and deployment. Additionally, enforce hostname validation testing during TLS configuration validation.

---

## Diagnostic Steps

Document each step of the PKI Diagnostic Framework as you worked through it.

### Step 1 — Retrieve

**Command used:**

```
openssl s_client -connect wrong.host.badssl.com:443 -servername wrong.host.badssl.com
```

**What you observed:**

Successful TLS connection and certificate retrieval from the server.

---

### Step 2 — Parse

**Command used:**

```
openssl x509 -in mismatch_cert.pem -text -noout
```

**Key fields from the certificate:**

| Field | Value |
|---|---|
| Subject CN | *.badssl.com |
| Issuer | Let's Encrypt R13 |
| Not Before |Mar 24 20:02:52 2026 GMT |
| Not After |Jun 22 20:02:51 2026 GMT |
| SAN entries |*.badssl.com|

**What you found:**

The certificate is valid for *.badssl.com, confirming it does not strictly match the mismatched hostname.

---

### Step 3 — Validate the Chain

**Command used:**

```
openssl verify mismatch_cert.pem
```

**Result:**

Verify return code: 0 (ok)

**What you found:**

The certificate chain was valid and fully trusted, with no intermediate or root trust issues

---

### Step 4 — Check Revocation and Trust

**Command used:**

```
[paste command here]
```

**What you found:**

No OCSP or CRL check was performed in this step. However, the certificate chain was successfully validated against trusted root authorities, indicating no immediate trust or revocation issues were detected during the handshake.

---

## Reflection

This lab reinforced that TLS validation involves multiple independent checks, including certificate chain verification and hostname identity matching. One area that required careful interpretation was understanding why the certificate chain could validate successfully even when the requested hostname did not match exactly. It also highlighted how the connection request (SNI) influences which certificate is presented, and how that is separate from certificate validation itself.

---

*CVI PKI Career Pathway — Foundations Phase*
