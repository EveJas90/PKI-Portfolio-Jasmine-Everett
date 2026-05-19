# Phase 1 Reflection Project

---

## Before You Begin

Review all of your weekly reflections (`reflections/week-01.md` through `reflections/week-07.md`) before writing this.
Your final submission should build on those entries — not repeat them.

Choose your submission format:
- [X] Written reflection (500–800 words)
- [ ] Video explanation (5–7 minutes) — paste your link below

**Video link (if applicable):**

---

## Part 1 — Your Phase 1 Journey

*How has your understanding of PKI changed from Week 1 to now? What did you think PKI was before you started? What was the first thing that genuinely surprised you?*

When I first started this corhort I did not know anything about PKI. I knew that cryptography was apart of PKI but I didn't know what PKI was in its entirety. The first thing that genuinely suprised me was that certficates are the key factor in how websites get secured. 

---

## Part 2 — How the Pieces Connect

*Walk through how the Phase 1 concepts connect as a system — cryptographic foundations, certificates, trust, lifecycle, troubleshooting, and real-world deployment. Show that you understand the architecture, not just the individual topics.*

Phase 1 showed me that PKI isn't just a collection of isolated tools, but a carefully connected ecosystem designed to establish trust. It all starts at the cryptographic layer with asymmetric encryption, which generates the public and private key pairs. To make a public key usable and verifiable, it is bound to an identity inside a digital certificate.

We ensure the integrity of this certificate using hashing and digital signatures; a Certificate Authority (CA) signs the certificate to prove it hasn't been tampered with. Our systems only trust this certificate if they can validate its entire "chain of trust" back to a trusted Root CA. Finally, because these components exist in a live environment, understanding the certificate lifecycle (like expiration and revocation) and troubleshooting broken chains is what allows us to securely deploy and maintain this architecture in a real enterprise. 

---

## Part 3 — A Lab That Made It Real

*Choose one lab from any week. Describe what you did, what you observed, and what that output taught you about how PKI works in a real environment. Reference the specific lab file in your repo.*

**Lab referenced:** labs/week-05/submissions/generate-csr/lab-01-generate-csr.md

This lab focused on the process of generating a Certificate Signing Request (CSR) and simulating how a Certificate Authority (CA) validates and signs that request to issue a certificate. It demonstrated the end-to-end workflow of certificate creation, from key generation to certificate issuance.

In this lab, I used OpenSSL to generate a private key and its corresponding CSR. I observed how the CSR packages critical identity information, such as the Common Name (CN), Organization, and location—alongside my public key into a standardized format. I then simulated the CA role by taking that CSR and signing it with a CA private key to issue a valid, usable X.509 certificate (test_cert.pem).This output taught me that a CSR acts as the essential bridge between an applicant and a trust provider. In a real-world enterprise environment, an administrator doesn't just hand over a private key to get a certificate; they keep the private key strictly secure on their own server and send only the CSR to a public or internal CA. Seeing the CA parse my CSR and output a cryptographically signed certificate reinforced how identity verification and the chain of trust are practical, enforceable operations, not just theoretical concepts.

---

## Part 4 — Explaining PKI to Someone Who Doesn't Know It

*What is the most important thing someone in cybersecurity misses when they think PKI is just "SSL certificates"? How would you explain what PKI actually is without losing them in the first 30 seconds?*

The most important thing people miss when they think PKI is "just SSL certificates" is that the certificate itself is useless without the entire backend infrastructure supporting it. A certificate is just a piece of paper; PKI is the entire legal, organizational, and technical system that proves that paper is real.

If I had to explain it to a non-technical person, I’d tell them to think of PKI like the security system at a high-security corporate office building:

-The Employee Badge (The Digital Certificate): Your badge has your name, photo, and company ID on it. It proves who you are to anyone inside the building.

-The HR Department / Badge Office (The Certificate Authority): You can't just print your own badge at home. A trusted internal authority (HR) has to verify your hiring paperwork, background check your identity, and physically print the badge for you.

-The Turnstile / Gate Guard (The Verification/Handshake): When you swipe your badge at the turnstile, the system instantly verifies that the badge hasn't expired, hasn't been revoked (like if an employee was fired), and was actually signed and issued by HR.

PKI is not just the badge itself; it is the entire ecosystem—the guard, the badge printer, the background checks, and the database—working together behind the scenes to verify your identity through various checkpoints before letting you through the gate.

---

## Part 5 — Where You Go From Here

*Reflect on the PKI Career Landscape from Week 7. What roles interest you? What in Phase 1 felt most relevant to where you want to go? What do you want to understand better — in Phase 2 or beyond?*

As a current Identity and Access Management (IAM) Analyst looking to transition into the engineering side of my career, Phase 1 has felt incredibly relevant. The concepts of internal and external identity verification align closely with the work I do now, particularly within the framework of a Zero Trust architecture. In Zero Trust, we operate under the principle of "never trust, always verify"—we don't grant access simply because a user or device claims an identity or had access previously. Every single connection must be explicitly verified.

PKI is the technical backbone that makes this continuous, explicit verification possible. Moving forward into Phase 2, the aspect that resonates most with my engineering goals is learning how to design and automate these trust systems at scale. Since I already understand the "why" behind strict identity verification, I am eager to dive deeper into the implementation side—such as configuring automated certificate issuance, managing enterprise certificate lifecycles without manual friction, and engineering robust authentication pipelines that ensure every checkpoint in the network is cryptographically secure.

---

## Required Visual

*Attach or embed your diagram below. Your visual must show PKI as a connected system. Choose one:*
- A TLS handshake with the full certificate chain labeled at each step
- A complete certificate lifecycle from key generation through revocation
- A PKI trust hierarchy with root CA, intermediate CA, and leaf certificate annotated

**Visual file:** reflections/visual/Phase 1 Project.png

*Brief description of what your diagram shows:*

My diagram illustrates a three-tier PKI trust hierarchy, mapping out the chain of trust from a Root CA down to an Intermediate (Issuing) CA, and finally to an end-entity leaf certificate. It highlights how the Root CA sits at the top as the ultimate trust anchor (typically kept offline for security), which delegates signing authority to the Intermediate CA. The Intermediate CA then explicitly verifies and signs the individual leaf certificates used by servers or users. This layout perfectly reflects the "never trust, always verify" mindset, demonstrating that a system must validate every link in the chain upwards to the Root before granting explicit access.

---

## Second Lab Reference

*Reference at least one more lab — different from Part 3. What did you do and what did it reinforce?*

**Lab referenced:** labs/03-week-03-certificate-fields/submissions/lab-03-certificate-chain.md

In this lab, I analyzed and validated a multi-tier certificate chain to see how a system traces trust from a leaf certificate back up to a trusted root. I inspected the "Issuer" and "Subject" fields across different certificates in the path to verify that they linked correctly, and checked how digital signatures were used to cryptographically lock each layer together.

This lab reinforced the reality of the trust hierarchies we discussed in class. It showed me that a server certificate doesn't just stand on its own; it requires a perfectly intact cryptographic chain. If any intermediate link is missing, misconfigured, or broken, the entire verification process fails immediately. This is highly relevant to my goals in IAM engineering, as maintaining the integrity of these trust paths is non-negotiable for keeping automated authentication pipelines completely secure.


---

*CVI PKI Career Pathway — Phase 1 Foundations*
