# Lab 02 — Inspect Your Trust Store

## Overview
Briefly describe what this lab was about in your own words.
What PKI concept or system behavior were you investigating?

This lab was about exploring the system’s certificate trust store and understanding how trusted root Certificate Authorities are managed locally. I viewed the installed root certificates on my machine and identified which CAs are trusted by default. I then used OpenSSL to validate a certificate against the local trust store to confirm that it chains back to a trusted CA.

The main PKI concept being investigated was certificate trust and validation through the system trust store, specifically how operating systems and browsers determine whether a certificate is trusted based on installed root Certificate Authorities.

---

## Environment
- Operating System: Windows 11
- Terminal Used: Powershell, Git Bash
- OpenSSL Version (`openssl version`):

---

## Steps Performed
Summarize the key steps you performed to complete the lab.
Do not copy the lab instructions — describe what you actually did.

In this lab, I examined the trusted root certificates installed on my local machine. I opened the Certificate Manager using the Windows Run command and navigated to the Trusted Root Certification Authorities store, where I viewed the list of certificates trusted by the system.

Next, I used a command line tool to extract and list the common names of the trusted root certificates, which helped me better understand which certificate authorities my system trusts.

Finally, I used OpenSSL to validate a certificate against the trusted root store on my machine. This confirmed that the certificate chain could be verified using the locally installed root certificate

---

## Results
Include the important outputs or findings from the lab.

- How many trusted root CAs did you find on your system?
  
  I found 54 trusted root CAs on my machine.

  ![Trust Store](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/a9c6762f0485a887627d33065425e0351ed486a0/assets/screenshots/week-04/Cert%20Store.png)

-  Name at least one specific root CA you inspected. Include its Subject and expiration date.

I looked at a Microsoft Root Authority certificate. Based on what I see in my screenshot it has the following information:

Issuer:
CN = Microsoft Root Authority
OU = Microsoft Corporation
OU = Copyright (c) 1997 Microsoft Corp.

It was valid ‎‎Friday, ‎January ‎10, ‎1997 3:00:00 AM to ‎Thursday, ‎December ‎31, ‎2020 3:00:00 AM

![Cert Preview](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/a9c6762f0485a887627d33065425e0351ed486a0/assets/screenshots/week-04/Cert%20Preview.png)

- What did the verify return code output tell you?
  
Verify return code: 0 (ok) this means that the certificate validatio was sucessful and fully trusted.

![Verify Return 1](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/a9c6762f0485a887627d33065425e0351ed486a0/assets/screenshots/week-04/Return%20results-1.png)

![Verify Reutrn 2](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/a9c6762f0485a887627d33065425e0351ed486a0/assets/screenshots/week-04/Return%20results-2.png)

## Key Findings
Document the most important observations from the lab.

• A machine may have many certificates
• A certificate may appear more than once for the same Issuer


---

## Explanation
Explain why the results matter.

- Why does your browser trust a certificate from a website you have never visited before?

My browser trusts a certificate from an unfamiliar website because it relies on the system’s trust store. If a website’s certificate traces back to a trusted root CA already installed on my machine the browser can validate it automatically without needing prior interaction with that website.

- What would happen if an enterprise's internal root CA was NOT in the trust store?

If an enterprise’s internal root CA is not in the trust store the browser or application will not be able to validate the certificate chain. As a result, users would receive a security warning or error indicating that the connection is not trusted or secure.

- What surprised you about how many roots are pre-installed on your system?
  
I was surprised by the number of preinstalled trusted root certificates on my system. I also noticed several well known Certificate Authorities, such as DigiCert, already present. It was interesting to see how many organizations are trusted by default and how often they appear across different certificates during this cohort.

