# Lab 04 — Detect Certificate Misconfigurations

## Overview
Briefly describe what this lab was about in your own words.
What PKI concept were you investigating?

In this lab I am going over various senarios analyzing the certificate configurations on each and explain why it would fail or cause problems in a production system. This further expands on PKI concepts based on trust chain validation. 

---

## Scenario 1 — Missing Subject Alternative Name

**Would modern browsers trust this certificate?**
No modern bowsers would not trust this certifcate.

**Analysis:**
[Explain why SAN is required, why CN is not sufficient, and what error users would see]

A SAN (Subject Alternative Name) is required because it futher expands on the domain names that a website may have which supports multiple domains on one certificate. CN (Subject Common Name) is not sufficient because it only supports one domain and its ignored by modern browsers if a SAN exists. 

---

## Scenario 2 — Incorrect Extended Key Usage

**Would a browser accept this certificate for a web server?**

Yes a browser would accept this certificate for a web server because it supports Client Authorization

**Analysis:**
[Explain what EKU defines, what value is required for HTTPS, and what error users would see]

EKU (Extended Key Usage) determines the specific purposes for which a certificate is allowed to be used. It is an extension in a certificate that restricts how the public key can be used, such as for server authentiation, client authentication, digital signing etc. TLS Web Server Authentication is required for HTTPS because validates that the certificate is valid for authenticating a web server during a TLS handshake. This allows secure encrypted communication between the browser and the server. If a certificate does not include the correct EKU browsers will reject it for HTTPS connections.

Users may see errors like "your connection isn't private" or ERR_CERT_AUTHORITY_INVALID.

---

## Scenario 3 — Expired Certificate

**What happens if this certificate is used today?**

Because the current date is after the certificate’s “Not After” date (May 1, 2023), the certificate is expired. As a result, TLS validation will fail and the certificate will not be trusted for establishing a secure connection.

**Analysis:**
[Explain why expiration fails validation, why lifecycle management matters, and what users would see]

TLS validation includes a check that ensures the certificate is within its valid date range. When a certificate is expired browsers reject it because it is no longer considered trustworthy even if the rest of the chain is valid. Lifecycle management matters because it reduces risk if a certificate or its private key is compromised, replacecs the certificates service downtime which also ties into them being renewed before they are expired. 

Users would see something like "Your connection is not private" or NET::ERR_CERT_DATE_INVALID

---

## Scenario 4 — Missing Intermediate Certificate

**What error would a browser likely display?**
If the server does not include the intermediate certificate, the browser will likely fail to build a complete certificate chain and display an error such as:

“Your connection is not private”
NET::ERR_CERT_AUTHORITY_INVALID (

**Analysis:**
[Explain how chains establish trust, why servers must include intermediates, and why browser behavior varies]

Certificate trust in PKI is built as a chain:

--The server (leaf) certificate is signed by an intermediate CA
--The intermediate CA is signed by a trusted root CA
--The browser already trusts the root CA (stored in its trust store)

To validate the server certificate, the browser:

--Verifies the leaf certificate was signed by a known intermediate
--Verifies the intermediate was signed by a trusted root
--Confirms the chain leads back to a trusted root CA

Servers are responsible for presenting the full certificate chain (except the root) during the TLS handshake. If the intermediate certificate is missing the browser cannot complete the chain.I noticed this in lab 03 when I was troubleshooting why I kept producing an error when trying to verify the trust chain. 

---

## Key Takeaway
In 2-3 sentences, explain why certificate misconfiguration is one of the most common causes of PKI outages.

Missing intermediate certificates, incorrect SAN entries, expired certificates, or improper EKU settings can break the certificate chain or fail validation during the TLS handshake. When this happens,browsers can't establish trust with the server, resulting in immediate connection failures or security warnings. Since certificates are widely distributed across systems and require precise configuration minor mistakes can quickly impact many users and services.
