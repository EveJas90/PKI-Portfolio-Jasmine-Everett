# Lab 02 — Check Certificate Revocation Status with OCSP

## Overview
Briefly describe what this lab was about in your own words.
What PKI concept or system behavior were you investigating?

This lab focused on understanding how certificate validation works in a real world enviroment using PKI by inspecting a live website certificate and performing an OCSP revocation check. I retrieved and analyzed the leaf and intermediate certificates, identified the certificate chain, and used OpenSSL to query the OCSP responder to confirm whether the certificate was still valid.

The PKI concept being investigated is certificate validation and revocation checking in X.509 public key infrastructure, specifically how OCSP is used to ensure a certificate has not been revoked before it is trusted in a secure connection.

---

## Environment
- Operating System: Windows 11
- Terminal Used: Git Bash
- OpenSSL Version (`openssl version`): OpenSSL 3.5.5 27 Jan 2026 (Library: OpenSSL 3.5.5 27 Jan 2026)
- Target site used: github.com

---

## Steps Performed
Summarize the key steps you performed to complete the lab.
Do not copy the lab instructions — describe what you actually did.

To complete this lab, I first created a directory on my local machine to store all generated files. I then connected to GitHub using OpenSSL and retrieved the leaf certificate from the website. I verified the certificate by inspecting its subject, issuer, and validity dates, confirming that the subject was github.com and that it was issued by Sectigo.

![Description](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/f34e3ec79cb05bb0d375f794031ffcbf3192e1b5/assets/screenshots/week-05/Leaf%20Cert%20Subject%20Name-0502.png)

Next, I prepared to perform an OCSP (Online Certificate Status Protocol) check. I retrieved the full certificate chain from the server and extracted the intermediate certificate (issuer certificate) from the chain file. After saving it separately, I verified that it was a valid Certificate Authority by inspecting its details and confirming that its subject and issuer differed, indicating it was signed by a higher-level (root) CA.

![Description](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/d5a9379cce48dc496515f6724e95bfb958f06e57/assets/screenshots/week-05/Issuer%20Cert%20Output.png)

In this output I notice that the subject and issuer is different from the leaf certificate. The subject nad issuer is different here because this is the intermediate certificate. Meaning this certificate was signed by the root certificate authority.

![Description](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/b6b0e21d36e7f108c983d3cbf408b2b87619c421/assets/screenshots/week-05/OCSP%20responder%20URL.png)

I then examined the leaf certificate’s extensions to locate the OCSP responder URL. Using that URL, I constructed and executed an OCSP query in OpenSSL, providing both the leaf and issuer certificates.

Finally, I reviewed the OCSP response and confirmed the certificate status. I also analyzed the “This Update” and “Next Update” fields, which showed that the OCSP response is cached and refreshed on a regular interval (approximately one week).

![Description](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/c4d4295d8e88b2662dbadc057dd12991a7ad38f1/assets/screenshots/week-05/OCSP%20response%20data.png)

---

## Results
Include the important outputs or findings from the lab.

- What was the Subject and Issuer of the leaf certificate?
  
  The subject and issuer of the leaf certificate was:
   subject=CN=github.com
issuer=C=GB, O=Sectigo Limited, CN=Sectigo Public Server Authentication CA DV E36

![Description](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/f34e3ec79cb05bb0d375f794031ffcbf3192e1b5/assets/screenshots/week-05/Leaf%20Cert%20Subject%20Name-0502.png)

- What OCSP URL did you find in the Authority Information Access extension?
  
![Description](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/b6b0e21d36e7f108c983d3cbf408b2b87619c421/assets/screenshots/week-05/OCSP%20responder%20URL.png)

I found http://ocsp.sectigo.com in the Authority Information Access extension

- What CRL Distribution Point URL did you find?
  
  ![Description](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/73acd566f3a93d1e6d18803534c772a83011740e/assets/screenshots/week-05/CRL%20Distribution%20Point.png)

The CRL Distribution URL I found was the same http://ocsp.sectigo.com.

- What was the OCSP response status for the certificate?

  ![Description](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/c4d4295d8e88b2662dbadc057dd12991a7ad38f1/assets/screenshots/week-05/OCSP%20response%20data.png)

 OCSP Response Status: successful (0x0)
  
- What do "This Update" and "Next Update" tell you?

  It tells me that the next update for this certificate is done weekly.

---

## Key Findings
Document the most important observations from the lab.

• The OCSP and CRL distribution URL is the same

• OCSP status should read successful if the query was performed correctly


---

## Explanation
Explain why the results matter.

- Why does an OCSP query require both the leaf certificate and the issuer certificate?
  
An OCSP query requires both the leaf certificate and the issuer certificate because the OCSP request must be validated against the correct Certificate Authority (CA). The leaf certificate identifies the specific certificate being checked for revocation, while the issuer certificate provides the public key needed to verify the OCSP response signature. Without the issuer certificate, the system cannot confirm that the OCSP response is authentic and actually comes from a trusted CA.
  
- What is the difference between OCSP and CRL in practice?
  
OCSP (Online Certificate Status Protocol) provides real-time validation by querying a CA responder for the current status of a specific certificate. CRL (Certificate Revocation List), on the other hand, is a downloadable list of all revoked certificates issued by a CA. In practice, OCSP is faster and more efficient for checking individual certificates, while CRLs are bulk lists that must be downloaded and periodically updated, which can be slower and less efficient for real-time validation.

- What would happen if a system trusted a revoked certificate because OCSP was unavailable?
  
  If OCSP is unavailable and the system incorrectly continues to trust a revoked certificate, it could allow an attacker to impersonate a trusted service or decrypt sensitive communication. This creates a serious security risk because certificate revocation is meant to prevent compromised or invalid certificates from being used. Depending on system configuration this could lead to unauthorized access or a full compromise of secure communications.

---

## Challenges / Troubleshooting
Document any issues encountered and how you resolved them.

I did not encounter any issues completing this lab.

---

## Artifacts
List the files generated during this lab.

- leaf_cert.pem
- issuer_cert.pem
- ocsp_response.txt
