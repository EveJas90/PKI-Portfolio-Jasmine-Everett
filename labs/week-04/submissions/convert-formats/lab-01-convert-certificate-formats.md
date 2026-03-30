# Lab 01 — Convert Certificate Formats

## Overview
Briefly describe what this lab was about in your own words.
What PKI concept or system behavior were you investigating?

---

## Environment
- Operating System: Windows 11
- Terminal Used: GIT Bash
- OpenSSL Version (`openssl version`):

---

## Steps Performed
Summarize the key steps you performed to complete the lab.
Do not copy the lab instructions — describe what you actually did.

  To complete this lab, I first retrieved a certificate from a public website and saved it locally in PEM format. I then converted the PEM certificate into DER format using OpenSSL and verified the conversion by inspecting the file and confirming it could still be validated.

Next, I converted the DER file back into PEM format and compared it with the original file to ensure the conversion process preserved the certificate data accurately.

Finally, I created a PFX bundle by generating a test key pair and a self-signed certificate. This step required manually entering certificate details such as the Distinguished Name fields. After generating the certificate and private key, I bundled them into a PFX file with a password and verified it by successfully opening the file.
   
---

## Results
Include the important outputs or findings from the lab.

- What did the PEM file look like compared to the DER file?
  
  The PEM file was in a readable formated. It had a distingushed header and footer such as BEGIN and END. I could easily decifer the characters used whereas in the DER file the characters where in an unrecongnizable format.

- What happened when you opened the .der file in a text editor?
  The file opened but I was unable to read what the characters said.
  
- What did the diff output show after converting PEM → DER → PEM?
- My output was blank. Each time. To me that just verifed that it sucessfully converted and I was able to confirm this by looking at both files.
- 
- What information was displayed when you verified the PFX?

"MAC: sha256, Iteration 2048
MAC length: 32, salt length: 8
PKCS7 Encrypted data: PBES2, PBKDF2, AES-256-CBC, Iteration 2048, PRF hmacWithSHA256
Certificate bag
PKCS7 Data
Shrouded Keybag: PBES2, PBKDF2, AES-256-CBC, Iteration 2048, PRF hmacWithSHA256"


---

## Key Findings
Document the most important observations from the lab.

• Its easy to convert the PEM file to DER and vice versa
• You will always need to provide a Distingushed name when self-signing a certficate.

---

## Explanation
Explain why the results matter.

- Why does a PFX require a password?
- PFX requires a password because it combines both the private and public key to sign a certficate. If someone got ahold of the password they would be able to sign and create many certficates under your name.
  
- In what real-world scenario would you choose PEM vs DER vs PFX?
- 
  PEM = readable & flexible (Linux/web)
  DER = strict & binary (Java/system-specific)
  PFX = portable & secure bundle (Windows/enterprise)
  
- Why is it important never to commit private key files to GitHub?
  GitHub is a public resource and anybody can take your private key and act as you

---

## Artifacts
List the files generated during this lab.

- leaf_cert.pem
- leaf_cert.der
- leaf_cert_restored.pem
- test_cert.pem
- test_bundle.pfx
