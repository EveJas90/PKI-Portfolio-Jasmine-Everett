# Lab — Digital Signatures (Integrity + Authenticity)

## Overview
Briefly describe the purpose of this lab in your own words.  
What PKI concept or system behavior were you investigating?

The purpose of this lab was to create a digital signature using Open SSL by generating a private key and extracting a public key. I used the private key to sign a file and then used the public key to verify the signature. After modifying the file, the verification failed as expected. 

This lab demonstrates the PKI concept of Digital Signature. Digital Signature ensures data integrity and authenticity becuase it can confirm if a file has not been tampered with and it comes from a trusted source. 

---

## Environment
Document the environment used to complete the lab.

- Operating System: Windows 11
- Terminal Used: GIT Bash
- OpenSSL Version (if applicable):

---

## Steps Performed
Summarize the key steps you performed to complete the lab.

Do **not copy the lab instructions**.  
Describe what you actually did.

1. Created a plain text file using Open SSL in GIT Bash
2. Generated a private key using Open SSL
3. Extracted the corresponding public key from the private key
4. Used the private key to create a Digital Signature for the file
5. Used the public key to confirm the file's authenticity
6. Modified the file and tried to verify the file again
7. Observed that the verfication failed after the file was altered.

---

## Results

![Certificate Output]([assets/screenshots/week-02/hashing-integrity.png](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/0ac7a5b25dedaa496b0f5450634ef2894610da0e/assets/screenshots/week-02/hashing-integrity.png)

---

## Key Findings
Document the most important observations from the lab.

•  A digitial signature was created using the private key

•  The orginal file passed verification with the public key

•  After modifying the file the signature verification failed.

---

## Explanation
Explain **why the results matter**.

The results matter because they demonstrate how Digital Signature ensures both data integrity and authentictiy. When the file was unchanged, the verification passed. This proves that the file came from a trusted source and was not altered. After tampering with the file, the verfication failed. Thus proving once again that digital signatures can also detect changes. 

---

CVI PKI Career Pathway — Foundations Phase
