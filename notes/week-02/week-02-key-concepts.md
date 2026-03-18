# Week 2 Lesson Notes — Cryptography Fundamentals (Just Enough)

## 1. Core Concept

Explain the foundational concept clearly and practically.

This week focused on three main cryptographic concepts: Symmetric Encryption, Cryptographic Hash Function, and Digital Signature. Symmetric encryption protects data by converting it into unreadable text using a shared key. Hashing creates a unique fixed length vaule to verify data intergrity. Digital signatures ensure both the integrity and authenticity of data using public and private keys.

---

## 2. Why It Matters

How this concept appears in real enterprise environments.

These concepts matter in real enterpise enviroments because they protect sensitive data and establish trust. Encryption keeps data confidential, hashing ensures files are not altered, and a digital signature verifies the identity of the sender.


---

## 3. Technical Breakdown

- Symmetric encryption: Uses one key for both encryption and decryption
- Hashing: Converts data into a fixed lenght value and cannot be reversed
- Digital signature: Uses private key to sign data and a public key to verify it
- Components:
  - Encryption algorithm like Advanced Encryption Standard (AES-256)
  - Hashing algorithm like SHA-256
  - Public/private key pair
  - Tool: OpenSSL

 - Flow

  1. Data is encrypted using a symmetric key to protect confidentiality
  2. A hash is generated to create a unqiue signature of the data
  3. The hash is signed using a private key to create a digital signature
  4. The reciever decrypts the data, verfies the hash and signature using the public key

---

## 4. Common Misconceptions

- Misconception 1: Encryption, hashing, and signing do the same thing.
  They serve diffrent purposes depending which part of the CIA traid you are using
  
- Misconception 2: Hashing can be reveresed
  Hashing is only one way and can not be reversed.

---

## 5. Where This Shows Up

This shows up in everyday tasks like browsing the web. HTTPS uses encryption, hashing, and digital signatures together. Week 1 showed us this with certificates. I think of them like a package version of all the cryptography functions together. 

---

## Mental Model

Short summary that ties the week back to:

- Identity: Digital Signatures prove who sent the data
- Trust: Built by knowing that only the holder of the private key could create a vaild signature
- Verification: Done through hashing with SHA-256 and encryption like AES ensuring that the data has not been altered. 
