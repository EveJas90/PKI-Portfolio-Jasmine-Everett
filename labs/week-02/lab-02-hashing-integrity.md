# Lab — Hashing & Integrity

## Overview
Briefly describe the purpose of this lab in your own words.  
What PKI concept or system behavior were you investigating?

The purpose of this lab is to learn how to generate a SHA-256 has of a file using Open SSL and to understand how hashing is used to verify data integrity. It demonstrates how even the smallest change to a file will produce a different hash, helping users to detect if an file has been altered.

---

## Environment
Document the environment used to complete the lab.

- Operating System: Windows 11
- Terminal Used: Git Bash
- OpenSSL Version (if applicable):

---

## Steps Performed
Summarize the key steps you performed to complete the lab.

1.  Created a sample text file (message.txt) to work with
2.  Used Open SSL to generate a SHA-256 hash of the file.
3.  Modifed the hashed file with the word "tampered".
4. Observed the findigs of the new hash and the old hash.
5. Both hashes are diffrent which confirms that the process was sucessfull.   

---

## Results
Include the important outputs or findings from the lab.

Examples may include:

- command outputs
- certificate fields
- verification results
- screenshots (if applicable)

If you include screenshots, store them in the **assets folder** and reference them here.

Example:

![Certificate Output](assets/certificate-output.png)

---

## Key Findings
Document the most important observations from the lab.

•  Small changes to the original file would rest in a completly diffrent hash, demonstrating how hashing works to keep data integrity.
•  The hashing output appeared as a long string of characters
•  When generating a hash a new file will be created for comparision later.

---

## Explanation
Explain **why the results matter**.

The results matter because they easily demonstrate how hashing can be used to detect of data has been altered. By generating a hash using SHA-256 with Open SSL, a small change, would produce a different hash vaule. This would allow a user to verify the integrity of the data to make sure it hasn't been tampered with.

---


---

CVI PKI Career Pathway — Foundations Phase
