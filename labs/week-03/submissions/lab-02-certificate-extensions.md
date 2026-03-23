# Lab 02 — Investigate Certificate Extensions

## Overview
Briefly describe what this lab was about in your own words.
What PKI concept were you investigating?

This lab is about how to investigate a certifcate further by reading the Certificate Extentions to determine how it can be used and validated. The PKI concept that I'm investigating is certificate validation and trust enforcement.

---

## Environment
- OS: Windows 11
- Terminal used (Mac Terminal / Git Bash / WSL): Git Bash
- OpenSSL version (`openssl version`): OpenSSL 3.5.5 27 Jan 2026

---

## Extensions Found

### Subject Alternative Name (SAN)
Paste the value from your output:

  X509v3 Subject Alternative Name:
                DNS:*.google.com, DNS:*.appengine.google.com, DNS:*.bdn.dev, DNS:*.origin-test.bdn.dev, DNS:*.cloud.google.com, DNS:*.crowdsource.google.com, DNS:*.datacompute.google.com, DNS:*.google.ca,......
                
### Key Usage
Paste the value from your output:

 X509v3 Key Usage: critical
                Digital Signature


### Extended Key Usage (EKU)
Paste the value from your output:

X509v3 Extended Key Usage:
                TLS Web Server Authentication

### Basic Constraints
Paste the value from your output:

 X509v3 Basic Constraints: critical
                CA:FALSE


---

## Observations

1. What domains appear in the SAN field?

   There are alot of dmoains that appear in the SAN field. Here are a few: *.google.com, DNS:*.appengine.google.com, DNS:*.bdn.dev, DNS:*.origin-test.bdn.dev, DNS:*.cloud.google.com, DNS:*.crowdsource.google.com, DNS:*.datacompute.google.com, DNS:*.google.ca, DNS:*.google.cl, DNS:*.google.co.in, DNS:*.google.co.jp, DNS:*.google.co.uk,

3. What operations are permitted by Key Usage?

   The key usage extention permits Digital Signiature. This allows the certficate to be used to sign data being transmitted during the TLS handshake to prove the server's identity.
  
5. What applications are authorized by EKU?

   The only appplicaions that are authorized by EKU is TLS Web Server Authentication. This means that its intended to authenticate a web server during HTTP connections. 
   
7. Can this certificate issue other certificates? How do you know?

   No this certificate can not issue other certificates. I know because the basic constraints is set to CA:FALSE which indicates its not a Certificate Authority. This means that this certficate is not allowed  to sign or issue certficates. 
   
9. Why are these extensions important for TLS validation?

    These extentions are important for TLS validation because they can be used to esure trust during a TLS handshake. Key Usage restricts the cryptographic functions a certificate can perfom. Preventing the certficate from being used for anything except its intended purpose. Extended Key Usage further expands on this by provided more context on what the certificate is allowed to do. Basic Constraints determines whether a certificate is a certificate authority. Which controls whether the certificate can issue other certificates. Together they all work together to enforce proper certificate usage and helps validate that a certificate is trustworthy and able to provide a secure connection. 
