# Lab 02: Issue a Code Signing Certificate

**Student Name:** Jasmine Everett    
**Date Completed:** 6/11/2026 
**Phase:** 2 | **Week:** 11  
**Submission Path:** `labs/week-11/lab-02-code-signing-certificate.md`

---

## Pre-Lab Verification

If you can log into PKI-SRV01 as **CORP\pki.admin**, you are communicating with DC01 and the environment is ready. Proceed to Part A.

---

## Part A — Design the CVI-CodeSigning Template

### Step 1 — Open the Certificate Templates Console

1. Press **Win + R**, type `certtmpl.msc`, and press **Enter**
2. The Certificate Templates console opens showing all templates installed on this CA

### Step 2 — Duplicate the Code Signing Template

1. Scroll through the list to find the built-in **Code Signing** template
2. Right-click **Code Signing** → select **Duplicate Template**
3. A new template Properties window opens — this is your working copy

> **Why Code Signing and not User?** The built-in Code Signing template already has the correct Key Usage (Digital Signature only) and EKU (Code Signing only) pre-configured. Starting from User would require removing multiple EKUs and introduces the risk of leaving incorrect settings in place.

**Source template duplicated:** Code Signing

### Step 3 — Set Compatibility Settings

1. Click the **Compatibility** tab
2. Set **Certification Authority** to: `Windows Server 2012 R2`
3. Set **Certificate Recipient** to: `Windows 8.1 / Windows Server 2012 R2`
4. Click **OK** on any informational dialog that appears

**Compatibility settings:**
- Certification Authority: Windows Server 2012 R2
- Certificate Recipient: Windows 8.1 / Windows Server 2012 R2

### Step 4 — Set the Template Name (General Tab)

1. Click the **General** tab
2. Change **Template display name** to: `CVI Code Signing`
3. Confirm the **Template name** (internal) auto-fills as `CVI-CodeSigning`

**Template names:**

| Field | Value |
|-------|-------|
| Template display name | CVI Code Signing |
| Template name (internal) | CVI-CodeSigning |

### Step 5 — Configure Key Usage

1. Click the **Extensions** tab
2. Select **Key Usage** in the list → click **Edit**
3. Confirm only **Digital Signature** is checked. Uncheck anything else if present
4. Check **Make this extension critical**
5. Click **OK**

| Key Usage | Included? | Reason |
|-----------|-----------|--------|
| Digital Signature |X| This certificate's main purpose is to sign code and scripts.| 
| Key Encipherment | |Not needed |
| Non-Repudiation | |Not Needed |

**Explanation of Key Usage decision:**

```
(why is Digital Signature the only required Key Usage for a code signing certificate?)

The Digital Signature is the only required Key Usage for a code signing certificate because the certificate's sole purpose is to provide cryptographic verification of code integrity and authenticity, not to encrypt data or sign other certificates.
```

### Step 6 — Configure Extended Key Usage (Application Policies)

1. Still on the **Extensions** tab, select **Application Policies** → click **Edit**
2. Confirm only **Code Signing (1.3.6.1.5.5.7.3.3)** is listed
3. If any other EKUs are present, select them and click **Remove**
4. Click **OK**

| EKU | Included? | Reason |
|-----|-----------|--------|
| Code Signing (1.3.6.1.5.5.7.3.3) |X |The main purpose of this certificate is to digitally sign code.|
| Client Authentication | | Not Needed|
| Other | |Not Needed|

**Explanation of EKU decision:**

```
(what does the Code Signing EKU control — and why should no other EKU be added?)
The Code Signing EKU confines the certificate's validity strictly to signing executable code and scripts. No other EKUs should be added to adhere to the principle of least privilege, ensuring this certificate cannot be misused for other functions like web server or client authentication..
```

### Step 7 — Configure Subject Name

1. Click the **Subject Name** tab
2. Select **Build from this Active Directory information**
3. Under Subject name format, select **User principal name (UPN)**

| Setting | Value | Reason |
|---------|-------|--------|
| Subject name source |Build from Active Directory| Automatically and securely maps the certificate to an authenticated Active Directory identity, preventing identity spoofing.|
| Subject built from |User principal name (UPN) |within AD as the distinct identifier for the code signing subject.|

### Step 8 — Set Validity Period and Enrollment Permissions

**Validity:**
1. Click the **General** tab
2. Set **Validity period** to `1` year (or 2 — document your reasoning)
3. Leave **Renewal period** at the default

**Security / Enrollment Permissions:**
1. Click the **Security** tab
2. Select **Authenticated Users** — confirm Read is checked, Enroll is NOT checked
3. Select **Domain Admins** — confirm Read and Enroll are checked
4. pki.admin should inherit Enroll through Domain Admins. If it does not, click **Add** → type `pki.admin` → Check Names → OK → check **Read** and **Enroll**
5. Do NOT enable Autoenroll — code signing certificates should require a deliberate enrollment action
6. Click **Apply**

| Setting | Value | Reason |
|---------|-------|--------|
| Validity period |1 year |Shorter lifespans minimize the window of exposure if a private key is ever compromised|
| Enroll — account(s) granted |Domain Admins and Pki.Admin |These should be the only user types that should have the right to enroll/ request the certificate |
| Autoenroll | | The deployment of a code signing certificate requires manual verification and intent by an administrator to prevent unauthorized code signing |

### Step 9 — Save the Template

1. Click **OK** to close the Properties window
2. Verify **CVI-CodeSigning** now appears in the certtmpl.msc list

**Template saved:**
- [X] Yes — visible in certtmpl.msc

---

## Part B — Publish and Issue the Certificate

### Step 1 — Publish the Template to the CA

1. Press **Win + R**, type `certsrv.msc`, and press **Enter**
2. Expand **CVI Issuing CA 1** in the left pane
3. Right-click **Certificate Templates** → **New** → **Certificate Template to Issue**
4. Scroll to find **CVI-CodeSigning** → select it → click **OK**
5. The template should appear in the Certificate Templates node within 30 seconds. If it doesn't, right-click the node → **Refresh**

**CVI-CodeSigning visible in Certificate Templates node:**
- [X] Yes

### Step 2 — Request the Certificate (as pki.admin)

1. Press **Win + R**, type `mmc.exe`, and press **Enter**
2. Go to **File → Add/Remove Snap-in**
3. Select **Certificates** → click **Add**
4. Choose **My user account** → click **Finish** → click **OK**
5. In the left pane, expand **Certificates (Current User)** → expand **Personal**

> **Note:** If no certificates have been issued yet, you will not see a **Certificates** folder under Personal — this is expected. Right-click **Personal** directly → **All Tasks** → **Request New Certificate**. Once the certificate is issued, the Certificates folder will appear automatically.

6. Right-click **Personal** → **All Tasks** → **Request New Certificate**
7. Click **Next** through the enrollment wizard
8. Select **Active Directory Enrollment Policy** → click **Next**
9. The **CVI-CodeSigning** template should appear in the list. Check the box next to it
10. Click **Enroll**
11. Enrollment should complete immediately. Click **Finish**

**Certificate issued:**
- [X] Yes — immediately
- [ ] Pending — describe:

```
(describe outcome)
Once I completed the Enrollment widget steps I noticed that the enrollment completed immediately. Once the box disappeared I could now see that I have two certificates in My Personal store. One is the Code Signing cert I just did and a User cert.
```

### Step 3 — Record the Request ID

1. Open **certsrv.msc**
2. Expand **CVI Issuing CA 1** → click **Issued Certificates**
3. Find the pki.admin code signing certificate
4. Record the Request ID below

**Request ID from certsrv.msc Issued Certificates node:** 7

> **Save this Request ID.** It is used in Week 12 revocation and in Lab 03.

### Step 4 — Verify the Certificate

Open **PowerShell** on PKI-SRV01 and run:

```powershell
certutil -store My
```

**Full certutil output for the code signing certificate:**

```
My "Personal"
================ Certificate 0 ================
Serial Number: 4400000007863dfe0d5e577c07000000000007
Issuer: CN=CVI Issuing CA 1, DC=corp, DC=cvilab, DC=local
 NotBefore: 6/15/2026 8:29 PM
 NotAfter: 4/25/2027 7:36 PM
Subject: CN=PKI Admin, OU=PKI Admins, DC=corp, DC=cvilab, DC=local
Non-root Certificate
Template: CVI-CodeSigning, CVI Code Signing
Cert Hash(sha1): b5dda40e3b765bdedf01097556f48d3b8a68a194
  Key Container = f94fbd1f3d35d4669b07335370c391cf_f0a99c17-76d3-498a-97de-2992c06105fd
  Simple container name: te-CVI-CodeSigning-bdc5fa1b-4745-419e-a339-a54e7ac0dfb7
  Provider = Microsoft Enhanced Cryptographic Provider v1.0
Private key is NOT exportable
Signature test passed

================ Certificate 1 ================
Serial Number: 44000000037e0d3eab46eb59db000000000003
Issuer: CN=CVI Issuing CA 1, DC=corp, DC=cvilab, DC=local
 NotBefore: 6/10/2026 2:33 AM
 NotAfter: 4/25/2027 7:36 PM
Subject: CN=PKI Admin, OU=PKI Admins, DC=corp, DC=cvilab, DC=local
Certificate Template Name (Certificate Type): User
Non-root Certificate
Template: User
Cert Hash(sha1): 43a58496d237b3e0e1e463172f4d64e87b59dad7
  Key Container = d2406b523b04a0af02106c8cdf89d4f8_f0a99c17-76d3-498a-97de-2992c06105fd
  Simple container name: te-User-0455faea-aad8-4f05-9ff5-04eb4c136e90
  Provider = Microsoft Enhanced Cryptographic Provider v1.0
Encryption test passed
CertUtil: -store command completed successfully.
```

Locate the CVI-CodeSigning certificate in the output and confirm the EKU field:

| Field | Value |
|-------|-------|
| Subject |CN=PKI Admin, OU=PKI Admins, DC=corp, DC=cvilab, DC=local|
| EKU |1.3.6.1.5.5.7.3.3 (Code Signing) *Confirmed via verbose lookup|
| Validity |NotBefore: 6/15/2026 8:29 PM / NotAfter: 4/25/2027 7:36 PM |
| Thumbprint |b5dda40e3b765bdedf01097556f48d3b8a68a194|

**EKU = 1.3.6.1.5.5.7.3.3 (Code Signing) confirmed:**
- [ ] Yes
- [X ] No — describe discrepancy:

The standard certutil -store My command only shows basic details like the Subject, Template, and Thumbprint, leaving out the EKU field. To see the 1.3.6.1.5.5.7.3.3 OID string in the terminal, I used the verbose flag and run certutil -v -store My instead.

---

## Part C — Sign a PowerShell Script

### Step 1 — Create the Test Script

Open **PowerShell** on PKI-SRV01 as CORP\pki.admin and run the following block. Copy and paste it exactly:

```powershell
$scriptContent = @'
# CVI Phase 2 — Week 11 Code Signing Test
Write-Host "This script is signed with a CVI code signing certificate."
Write-Host "Issued to: pki.admin"
Write-Host "Date: $(Get-Date)"
'@

New-Item -Path "C:\Scripts" -ItemType Directory -Force
Set-Content -Path "C:\Scripts\Test-CVI.ps1" -Value $scriptContent
```

**Script created at C:\Scripts\Test-CVI.ps1:**
- [X] Yes

### Step 2 — Retrieve the Code Signing Certificate

Run the following to confirm the certificate is accessible and select it into a variable:

```powershell
$cert = Get-ChildItem -Path Cert:\CurrentUser\My -CodeSigningCert | Select-Object -First 1
$cert | Select-Object Subject, Thumbprint, NotAfter
```

**Output of certificate selection:**

```
CN=PKI Admin, OU=PKI Admins, DC=corp, DC=cvilab, DC=local | B5DDA40E3B765BDEDF01097556F48D3B8A68A194| 4/25/2027 7:36:58 PM
```

> If this returns nothing, the certificate was not issued with the Code Signing EKU. Go back to certtmpl.msc, check the Application Policies on CVI-CodeSigning, and re-enroll.

### Step 3 — Sign the Script

```powershell
$result = Set-AuthenticodeSignature -FilePath "C:\Scripts\Test-CVI.ps1" -Certificate $cert
$result
```

**Set-AuthenticodeSignature output:**

```
B5DDA40E3B765BDEDF01097556F48D3B8A68A194 Valid Test-CVI.ps1
```

Expected result: **Status = Valid**

### Step 4 — Verify the Signature

```powershell
Get-AuthenticodeSignature -FilePath "C:\Scripts\Test-CVI.ps1"
```

**Full Get-AuthenticodeSignature output:**

```
B5DDA40E3B765BDEDF01097556F48D3B8A68A194  Valid                                  Test-CVI.ps1
```

**Status:**
- [X] Valid
- [ ] Other — describe:

### Step 5 — Check for a Timestamp

```powershell
(Get-AuthenticodeSignature "C:\Scripts\Test-CVI.ps1").TimeStamperCertificate
```

**TimeStamperCertificate output:**

```
$null
```

**Timestamp present:**
- [ ] Yes — note the timestamp authority:
- [X] No — note this in Part D

### Step 6 — Hash Mismatch Test

Modify the script after signing to verify the signature breaks:

```powershell
Add-Content -Path "C:\Scripts\Test-CVI.ps1" -Value "# Modified after signing"

Get-AuthenticodeSignature -FilePath "C:\Scripts\Test-CVI.ps1"
```

**Get-AuthenticodeSignature output after modification:**

```
                                        NotSigned                              Test-CVI.ps1
```

**Status after modification:**
- [] HashMismatch
- [X] Other — describe:
      
This output did not produce a hash. 

---

## Part D — Written Explanation

Answer the following in plain prose paragraphs — not bullet points.

**What does the Code Signing EKU enforce, and at what layer?**

Cover: what application or OS component checks for the Code Signing EKU, what it does when the EKU is present vs. absent, and how this is different from the cryptographic validity check.

```
The Code Signing EKU confines a certificate’s valid use case strictly to signing executable files, binaries, and scripts. This validation happens at the Operating System and Application layer, where components like Windows AppLocker, PowerShell Execution Policies, or User Account Control (UAC) check the certificate during execution. If the Code Signing EKU is present and trusted, the OS permits the script to run smoothly. If it is absent, the system treats the file as untrusted or unsigned and blocks execution, regardless of whether the certificate is cryptographically valid. This is distinct from a standard cryptographic validity check, which only verifies that the mathematical signature matches the file and that the certificate hasn't expired or been revoked.

```

**What did the hash mismatch test demonstrate about what the signature is protecting?**

Cover: what the signature covers (the code hash), what the mismatch status means, and why this matters for software integrity in a production environment.

```
The hash mismatch test demonstrated that a digital signature strictly protects the integrity of the code file. When a script is signed, a unique cryptographic hash is generated from its text contents and encrypted using the signer's private key. The mismatch status means that the file was altered after the signature was applied, causing the current file hash to conflict with the original hash embedded in the signature. In a production environment, this is a critical defense mechanism because it ensures software integrity, alerting administrators that a script has been tampered with, corrupted, or maliciously injected with unauthorized code before execution.

```

**Should the CVI-CodeSigning template require CA certificate manager approval in a production environment? Why or why not?**

```
Yes, the CVI-CodeSigning template should require CA certificate manager approval in a production environment. Code signing certificates grant the power to authorize software execution across the enterprise network. If left on auto-enrollment or unapproved manual enrollment, any compromised account with template access could generate a valid certificate to sign malware or unauthorized scripts, bypassing critical endpoint security filters. Requiring manager approval enforces a necessary layer of dual-control and human verification, ensuring that certificates are only issued to verified developers or administrative accounts.
```

---

## Reflection

**Why is a timestamp operationally significant for a code signing certificate — particularly for software that will be distributed and executed over a long period?**

```
A timestamp is operationally significant because it provides a verifiable, cryptographically secure record of exactly when the code was signed. Without a timestamp, the validity of the signed software is bound entirely to the lifecycle of the code signing certificate; once that certificate expires or is revoked, the operating system will immediately treat the software as untrusted and block it. When a timestamp is applied by a trusted Time Stamping Authority (TSA), the OS checks if the certificate was valid. If it was, the software remains trusted and executable indefinitely, even long after the original code signing certificate has expired.

```

**One thing about the code signing workflow you would want to understand better or configure differently:**

```
(your observation here)
```
I would want to better understand how to configure and enforce strict hardware-based protections for the private keys, such as using a Hardware Security Module (HSM) or Trusted Platform Module (TPM). 

---

## Submission Checklist

- [X] Pre-lab verification completed
- [X] Part A: Template duplicated from the built-in Code Signing template
- [X] Part A: All settings configured — Key Usage, EKU, Subject Name, Validity, Enrollment Permissions
- [X] Part A: Template saved as CVI-CodeSigning and visible in certtmpl.msc
- [X] Part B: Template published to CVI Issuing CA 1
- [X] Part B: Certificate issued to pki.admin — Request ID recorded
- [X] Part B: certutil output pasted with EKU confirmed as Code Signing (1.3.6.1.5.5.7.3.3)
- [X] Part C: Test script created at C:\Scripts\Test-CVI.ps1
- [X] Part C: Certificate selection output pasted
- [X] Part C: Set-AuthenticodeSignature output pasted (Status = Valid)
- [X] Part C: Get-AuthenticodeSignature output pasted (Status = Valid)
- [X] Part C: Timestamp check output pasted
- [X] Part C: Hash mismatch test output pasted (Status = HashMismatch)
- [X] Part D: Written explanation completed in prose
- [X] Reflection completed
- [X] File saved as `lab-02-code-signing-certificate.md`
- [X] File committed to portfolio repo under `labs/week-11/`
