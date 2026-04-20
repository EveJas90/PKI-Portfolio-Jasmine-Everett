# Lab 01 — Analyze a Live Enterprise Certificate Deployment

## Overview
Briefly describe the purpose of this lab in your own words.
What PKI concept or system behavior were you investigating?

This lab focuses on acting as a PKI engineer at Metro General Hospital to understand what a mature enterprise PKI looks like in production. The goal is to analyze the public-facing TLS deployment of a well known enterprise environment by examining its certificates and how they are used in practice.

---

## Target
The target I choose for this lab is https://www.okta.com/

## Certificate Summary

| Field | Value |
|---|---|
| Issuer |C=US, O=Amazon, CN=Amazon RSA 2048 M01|
| Not Before |Nov  3 00:00:00 2025 GMT|
| Not After |Dec  2 23:59:59 2026 GMT|
| Certificte Type |DV(Domain Validation)|
| SAN count | 3 |
| Wildcard Usage |No|

---

## Chain Analysis
Number of certificates in chain, intermediate CA identity, root CA identity, chain completeness.

<img width="417" height="138" alt="Cert Chain" src="https://github.com/user-attachments/assets/1087449e-4ddd-44dd-a40a-ec6645697972" />


There are three certificates in the chain: a leaf certificate (CN=okta.com), an intermediate CA certificate (CN=Amazon RSA 2048 M01), and a root CA certificate (CN=Amazon Root CA 1). The chain is complete because each certificate is properly signed by the next certificate in the hierarchy, ultimately terminating at a trusted root certificate in the system trust store. The verification output shows a successful path from the leaf certificate (depth=0) through the intermediate (depth=1) to the root certificate (depth=2), confirming a valid and trusted certificate chain.


---

## Termination Analysis
Where TLS appears to terminate (app server / load balancer / CDN) and the evidence that supports your conclusion.

<img width="723" height="346" alt="Server Response" src="https://github.com/user-attachments/assets/e51d30c5-7d1e-4d12-b3c7-561b6f0af775" />

TLS terminates at a CDN/load balancer layer. This is supported by multiple Anycast IP addresses and the fact that the TLS handshake fails before any HTTP response is returned, indicating no direct connection to an application server.


---

## TLS Configuration
SSL Labs grade, TLS versions, HSTS, OCSP stapling.

Tools used: sslabs.com

<img width="1316" height="797" alt="SSL Labs Grade" src="https://github.com/user-attachments/assets/b4329e15-6484-4c79-aa46-ece857aa6e02" />
<img width="1232" height="365" alt="TLS Versions" src="https://github.com/user-attachments/assets/48c6365f-7f61-4976-a695-7e638d23651e" />
<img width="422" height="65" alt="HTSOCSP" src="https://github.com/user-attachments/assets/200e195d-7b0e-4908-aa91-43f7a4553ed0" />




- SSL Labs overall grade: B
- TLS versions supported (TLS 1.0, 1.1, 1.2, 1.3): This server supports TLS 1.0, 1.1, 1.2, 1.3
- Are deprecated TLS versions (1.0 or 1.1) still supported?: Yes
- Is HSTS (HTTP Strict Transport Security) configured? No
- Is OCSP stapling enabled? Yes




---

## CT Log Analysis
CA consistency, any unexpected issuers, certificate validity period pattern

During the Certificate Transparency (CT) log analysis portion of the lab, I was unable to access crt.sh due to a bad gateway error, which prevented me from retrieving live CT log data. As a result, I was not able to complete the full external certificate search and analysis as intended. However, the purpose of this step was to review publicly logged certificates to understand how certificate issuance can be monitored and validated through CT logs.

---

## Architecture Assessment
In 2–3 sentences, describe what this certificate deployment tells you about the organization's PKI architecture and operational approach. 

This certificate setup shows the organization is using a modern, cloud based PKI system where certificates are issued by a public Certificate Authority and managed automatically. The presence of a full certificate chain and a DV certificate suggests that TLS is handled at the edge using services like a CDN or load balancer instead of individual application servers. Overall, it shows a mostly automated and standardized approach to managing certificates rather than a manual or on-premises PKI system.

---

*CVI PKI Career Pathway — Foundations Phase*
