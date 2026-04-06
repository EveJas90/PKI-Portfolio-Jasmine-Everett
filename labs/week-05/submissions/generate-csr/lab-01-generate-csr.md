# Lab 01 — Generate a CSR and Simulate the Issuance Workflow

## Overview
Briefly describe what this lab was about in your own words.

This lab focused on the process of generating a Certificate Signing Request (CSR) and simulating how a Certificate Authority (CA) validates and signs that request to issue a certificate. It demonstrated the end to end workflow of certificate creation, from key generation to certificate issuance.

The primary PKI concept being investigated is certificate-based authentication using asymmetric cryptography. Specifically, it involves how an RSA key pair (2048-bit) is used to create a CSR, how the public key is embedded in an X.509 certificate, and how digital signatures ensure the authenticity and integrity of that certificate.

---

## Environment
- Operating System:Windows 11
- Terminal Used: GIT Bash
- OpenSSL Version (`openssl version`):OpenSSL 3.5.5 27 Jan 2026 (Library: OpenSSL 3.5.5 27 Jan 2026)

---

## Steps Performed
Summarize the key steps you performed to complete the lab.
Do not copy the lab instructions — describe what you actually did.

To begin, I created a directory on my local computer to store all files generated during this lab. I then generated a 2048-bit RSA private key using OpenSSL, which I confirmed by reviewing the terminal output.: 
![Description](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/a146f6e58e862943ce9fb50389c7026d5e4842a3/assets/screenshots/week-05/Private%20Key%20RSA%20Type.png). 

Next, I generated a Certificate Signing Request (CSR). I initially experienced issues using the provided command, but I was able to successfully create the CSR by entering the commands step by step and manually completing the Distinguished Name fields.
![Description](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/7627f67027ac96088e13665a7345003a1fb63916/assets/screenshots/week-05/CSR%20Generation.png). 

After generating the CSR, I inspected it and confirmed that the Distinguished Name values matched the information I had entered. I then simulated the signing process. In a real-world environment, the CSR would be sent to a Certificate Authority (CA), which would verify the request, approve it, and return a signed certificate.

![Description](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/083ba8e2437b0b83edee772405d3fe683810798d/assets/screenshots/week-05/Inspected%20CSR.png)

Based on the certificate output, I verified that the subject name matched the CSR. Since this was a self-signed certificate, the issuer matched the same Distinguished Name. In a real scenario, the issuer would instead reflect the CA. I also confirmed the validity period (“Not Before” and “Not After”) matched the -days 365 flag (screenshot is provided below), meaning the certificate is valid for one year.

![Description](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/b5e35895f78f8a0021b336e337613b6fd2564389/assets/screenshots/week-05/Self%20Sign%20Cert.png)

Finally, I compared the original CSR and the signed certificate by extracting and comparing their public keys. The empty diff result confirmed that they matched, verifying the integrity of the key pair and certificate.

![Description](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/d613f0e2b00619d982a791c69f69791132d05c6e/assets/screenshots/week-05/Diff%20Output.png)




---

## Results
Include the important outputs or findings from the lab.

- What Subject fields did you include in your CSR, and what does each one represent?

![Description](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/083ba8e2437b0b83edee772405d3fe683810798d/assets/screenshots/week-05/Inspected%20CSR.png)

  
| Field | Abbreviation | Value in Your CSR |
|---|---|---|
| Common Name | CN | lab01.cvi.internal |
| Organization | O |Cybervisionaries |
| Organizational Unit | OU | PKI Career Pathway |
| Country | C | US|
| State | ST |California |
| Locality | L | San Francisco |

- What is the Issuer field in a self-signed certificate, and why does it match the Subject?
  
In a self-signed certificate, the Issuer field is the same as the Distinguished Name listed in the Subject. This is because the certificate is signed by the same entity that created it, meaning it is acting as its own Certificate Authority. In a real-world scenario, the Issuer would instead reflect the trusted Certificate Authority that signed the certificate.
  
- What did the public key comparison (diff) show, and what does that mean?

![Description](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/d613f0e2b00619d982a791c69f69791132d05c6e/assets/screenshots/week-05/Diff%20Output.png)

The diff output was blank, which indicates there were no differences between the two public keys. This confirms that the public key in the CSR and the public key in the self-signed certificate are identical. This verifies that the certificate was correctly generated from the original private key and maintains the integrity of the key pair.

- How do the fields in your signed certificate connect to what you learned about X.509 in Week 3?

The fields in the signed certificate directly relate to what I learned about X.509 certificates in Week 3, such as the Subject, Issuer, validity period, and public key. This lab reinforced my understanding of how these fields are created and used in practice, helping me connect the theory of certificate structure to the real process of generating, signing, and validating certificates in an environment. 


---

## Key Findings
Document the most important observations from the lab.

• A self-signed certificate will show the subject as the issuer

• A certificate signed by a trusted Certificate Authority will show the trusted Certificate Authority's information as the issuer instead of the distingushed name

• Diff outputs should produce no results if the public keys from both the CSR and signed certifcate match

---

## Explanation
Explain why the results matter.

- What is the purpose of a CSR in the real certificate issuance workflow?

The purpose of a CSR (Certificate Signing Request) is to provide a Certificate Authority (CA) with the necessary information to issue an X.509 certificate. This includes the public key and identifying details such as the Distinguished Name. The CSR proves possession of the private key, but it does not include the private key itself. While it may include requested attributes, the CA ultimately determines the validity period and other certificate details.
  
- Why does the CA receive a CSR rather than a private key?
  
The CA receives a CSR instead of a private key to ensure the private key remains secure and only accessible to the owner. If a private key were shared, it could be compromised and used to impersonate the owner or create fraudulent certificates. Keeping the private key private is a fundamental principle of public key infrastructure (PKI) security.
  
- What would it mean if the public key in the certificate did NOT match the private key?
  
If the public key in the certificate does not match the private key, the certificate would not function correctly. This means the system would be unable to establish trust, as it could not properly encrypt/decrypt data or verify signatures. In practice, this would result in failed TLS connections or authentication errors, effectively making the certificate unusable.

---

## Challenges / Troubleshooting
Document any issues encountered and how you resolved them.

The only issue that I expierenced was during creation of the CSR when it came time to input the distingushed name attributes. I had to only use the two commands:

openssl req -new \
  -key labs/week-05/submissions/generate-csr/test_key.pem \
  -out labs/week-05/submissions/generate-csr/test_csr.pem \

  To have the terminal produce the prompts to input the fields needed to complete the setup of the CS. You can also see a example of the prompts in earlier steps of this lab.

---

## Artifacts
List the files generated during this lab.

- test_csr.pem
- test_cert.pem
