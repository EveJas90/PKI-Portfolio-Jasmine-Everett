
# Lab — Symmetric Encryption (Confidentiality)

## Overview

This lab purpose is to teach how symmetric encryption works by creating a plaintext file and then excrypting it by using AES 256 using Open SSL. This lab helps demonstrates how encryption keeps data secure by making it unreadable without the correct password and shows how symmetric encryption is used to keep information secure during transmission


---

## Environment

- Operating System: Windows
- Terminal Used: GIT
- OpenSSL Version (if applicable): OpenSSL 3.5.5 27 Jan 2026 (Library: OpenSSL 3.5.5 27 Jan 2026)

---

## Steps Performed
Summarize the key steps you performed to complete the lab.

Do **not copy the lab instructions**.  
Describe what you actually did.

1.  First I created a folder on my computer titled labs/02-week-02-cryptography-fundamentals/submissions/encrypted
   
3.  I then created a plaintext file in the encrypted folder using the **echo** command.
   
4.  I wanted to see what the file looked like before I encrypted it so I used the **cat** command to view the contents of the plaintext file.
   
5.  I then encrypted the file using AES 256  and I used the **cat** command again to view the file. I knew that the encryption worked because the output were in characters that were unreadable.
   
6. Once I confirmed that my files were ecrypted I then went and used the password I chose at the time I encrypted my file and was able to sucessfully see the contents of the file again.


---

## Results
Include the important outputs or findings from the lab.

Viewing the file before decrypting

![Verification results](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/a57820ceceb11ca3c37d3eda40715afe41b51ea7/assets/screenshots/week-02/verification-results.png)

Decrypting and viewing the decrypted file

![Verification results](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/a57820ceceb11ca3c37d3eda40715afe41b51ea7/assets/screenshots/week-02/verification-results2.png)

---

## Key Findings
Document the most important observations from the lab.

•  The file was encrypted using AES 256 through Open SSL in Git Bash.

•  When attempting to view the encrypted file directly it produced unreadable characters, showing that the data was sucessfully encrypted.

•  I used the OpenSSL command with the -d option to decrypt the file and entered the correct password when prompted. I then used the cat command to view the contents of the decrypted file. 

---

## Explanation
Explain **why the results matter**.

The results show that symmetric encryption sucessfully protects the confidentiality of data. When the file was encrypted using AES with Open SSL, the contents became unreadable. This demonstrates that encryption would prevent unauthorized users from being able to veiw the contents of the file. 

By using the correct password to decrypt the file this also confirms that only an authorized person who has the password or key to the file would be able to view it's contents. When the cat command was used this further provided evidence that the encryption and decryption process worked as intended.

This matters because it shows a real world example of how encryption works to both store and protect data during transmission. If the wrong password was used the file would not decrypt properly, which shows that encryption works to prevent an unauthorized person from accessing the file.

---

## Challenges / Troubleshooting
Document any issues encountered during the lab and how you resolved them.

I experienced an issue with this lab initially because I never had experience using GIT Bash before.  Once I installed the software on my computer I was able to follow the instructions of the lab and understand the outputs I was given. 

---

## Artifacts
List the files generated during this lab.

Examples:

plaintext.decrypted.txt  
plaintext.txt  
plaintext.txt.enc

---

CVI PKI Career Pathway — Foundations Phase
