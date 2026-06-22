# Lab 02: Issue Your First Certificate from a Custom Template

**Student Name:** Jasmine Everett  
**Date Completed:** 6/17/26
**Phase:** 2 | **Week:** 10  
**Submission Path:** `labs/week-10/lab-02-first-issuance.md`

---

## Pre-Lab Verification

If you can log into PKI-SRV01 as **CORP\pki.admin**, you are communicating with DC01 and the environment is ready. Also confirm the following before starting:

```powershell
Get-Service -Name CertSvc
certutil -ping
```

**CertSvc status:** Active 
**CA responding (certutil -ping):**
- [X] Yes
- [ ] No — action taken:

**CVI-WebServer template visible in certtmpl.msc (from Lab 01):**
- [X] Yes
- [ ] No — complete Lab 01 before proceeding

> **Lab 01 is required before Lab 02.** The CVI-WebServer template must exist in certtmpl.msc before you can publish it to the CA in Part A.

---

## Part A — Publish the Template to the CA

Publishing the template makes it available for certificate requests. The template definition lives in Active Directory — this step tells the CA to read it.

### Step 1 — Open the CA Management Console

1. On PKI-SRV01, press **Win + R**, type `certsrv.msc`, and press **Enter**
2. The Certification Authority management console opens

### Step 2 — Publish the Template

1. In the left pane, expand **CVI Issuing CA 1**
2. Right-click **Certificate Templates** → **New** → **Certificate Template to Issue**
3. The Enable Certificate Templates dialog opens
4. Scroll the list to find **CVI-WebServer**
5. Select it → click **OK**
6. **CVI-WebServer** should appear in the Certificate Templates node within 30 seconds. If it does not appear, right-click the node → **Refresh**

**CVI-WebServer visible in Certificate Templates node:**
- [X] Yes
- [ ] No — describe what happened:

```
(describe here
```

**What you observe in the Certificate Templates node after publishing:**

```
(describe what you see in certsrv.msc — how many templates are listed? does CVI-WebServer appear?)
After adding the CVI-WebServer template I notice that I now have 12 certificate templates including the CVI-WebServer showing in the Certificate Templates node.

```

---

## Part B — Request the Certificate via MMC

### Step 1 — Open MMC and Add the Certificates Snap-in

1. On PKI-SRV01 logged in as CORP\pki.admin, press **Win + R**, type `mmc.exe`, and press **Enter**
2. Go to **File → Add/Remove Snap-in**
3. In the Available snap-ins list, select **Certificates** → click **Add**
4. When prompted, select **My user account** → click **Finish**
5. Click **OK** to close the Add/Remove Snap-in dialog

### Step 2 — Navigate to the Certificate Store

1. In the left pane, expand **Certificates (Current User)**
2. Expand **Personal**

> **Note:** If no certificates have been issued yet to this account, you will not see a **Certificates** folder under Personal — this is expected. Right-click **Personal** directly → **All Tasks** → **Request New Certificate**. Once the certificate is issued, the Certificates folder will appear.

### Step 3 — Launch the Certificate Enrollment Wizard

1. Right-click **Personal** (or **Certificates** if the folder is visible) → **All Tasks** → **Request New Certificate**
2. Click **Next** through the "Before You Begin" screen

### Step 4 — Select Enrollment Policy

1. Select **Active Directory Enrollment Policy**
2. Click **Next**

**Enrollment policy selected:**

```
Active Directory Enrollment Policy
```

### Step 5 — Select the CVI-WebServer Template

1. The template list appears. Review all visible templates
2. Locate **CVI-WebServer** in the list

**Templates visible in the wizard:**

```
I now see CVI-Webserver and a subordinate certificate in the certificate template wizard.
```

**CVI-WebServer visible in the wizard:**
- [X] Yes
- [ ] No — troubleshooting steps taken:

3. If a yellow warning triangle appears next to CVI-WebServer, click the link below the template name that reads **"More information is required to enroll for this certificate. Click here to configure settings."**
4. In the Certificate Properties dialog that opens, go to the **Subject** tab
5. Under Subject name, set **Type** = `Common name`, **Value** = `webserver.corp.cvilab.local`
6. Click **Add** → click **OK**

> **Why the yellow triangle?** The CVI-WebServer template uses "Supply in the request" — the CA requires you to provide the subject name yourself before it can issue the certificate. The common name you supply here (webserver.corp.cvilab.local) becomes the CN in the issued certificate.

**Subject name entered:** 

```
webserver.corp.cvilab.local
```

### Step 6 — Enroll

1. Check the box next to **CVI-WebServer** → click **Enroll**
2. Enrollment should complete immediately (Status: Succeeded)
3. Click **Finish**

**Certificate request outcome:**
- [X] Issued immediately (Status: Succeeded)
- [ ] Pending manager approval — describe resolution:
- [ ] Error encountered:

```
(paste error here if applicable)
```

---

## Part C — Inspect the Issued Certificate

### Step 1 — Inspect in the MMC Certificates Snap-in

1. In the MMC left pane, expand **Certificates (Current User)** → **Personal** → **Certificates**
2. You should see the newly issued certificate in the list
3. Double-click it to open the Certificate dialog

**General tab:**

| Field | Value |
|-------|-------|
| Issued to |webserver.corp.cvilab.local |
| Issued by |CVI Issuing CA 1 |
| Valid from |6/21/2026 |
| Valid to |4/25/2027 |

4. Click the **Details** tab and scroll through the fields

**Details tab — record the following:**

| Field | Value |
|-------|-------|
| Serial Number |4400000008f90ab707cd423ea0000000000008|
| Signature Algorithm |sha256|
| Subject |CN = webserver.corp.cvilab.local|
| Key Usage |Digital Signature, Key Encipherment (a0) |
| Enhanced Key Usage |Server Authentication (1.3.6.1.5.5.7.3.1) |
| Subject Alternative Name (if present) | |
| Thumbprint |d6a36ba06bb386d1b984fb4432ffbf38e59e53d3 |

### Step 2 — Inspect with certutil

1. Copy the **Thumbprint** value from the Details tab (40 hex characters)
2. Open **PowerShell** on PKI-SRV01 as CORP\pki.admin
3. Run the following, replacing `<thumbprint>` with the value you copied (remove any spaces):

```powershell
certutil -store My "d6a36ba06bb386d1b984fb4432ffbf38e59e53d3"
```

> If that returns no results, the request was processed under your user account — try:
> ```powershell
> certutil -store -user My "<thumbprint>"
> ```

**Full certutil output:**

```
My "Personal"
================ Certificate 0 ================
Serial Number: 4400000008f90ab707cd423ea0000000000008
Issuer: CN=CVI Issuing CA 1, DC=corp, DC=cvilab, DC=local
 NotBefore: 6/21/2026 7:26 PM
 NotAfter: 4/25/2027 7:36 PM
Subject: CN=webserver.corp.cvilab.local
Non-root Certificate
```

### Step 3 — Confirm in certsrv.msc Issued Certificates Node

1. Open `certsrv.msc` (or switch back to it if already open)
2. Expand **CVI Issuing CA 1** → click **Issued Certificates**
3. Click the **Request ID** column header to sort by most recent
4. Locate the certificate you just requested

**Certificate visible in Issued Certificates node:**
- [X] Yes

**Record from the Issued Certificates node:**

| Column | Value |
|--------|-------|
| Request ID |8 |
| Requester Name |CORP.PKI-SRV01$ |
| Certificate Template |CVI-Webserver |
| Issued Common Name |webserver.corp.cvilab.local |
| Certificate Expiration Date |4/25/27 7:36 PM |

> **Save your Request ID.** You will use it in Week 12 to revoke this certificate, and it is also needed for the Week 11 Lab 03 profile comparison exercise. If you lose it later, run on PKI-SRV01: `certutil -view -out RequestId,CommonName,CertificateTemplate`

---

## Part D — Write-Up: The Issuance Workflow

Describe the full certificate issuance workflow in your own words, in plain prose paragraphs (not bullet points). Cover all four stages:

1. **What happened in Active Directory when you published the template** — what did publishing actually do?
2. **What the MMC Certificate Enrollment wizard sent to the CA** — what is a CSR and what does it contain?
3. **What the CA evaluated before issuing the certificate** — what checks did it perform?
4. **Where the issued certificate was placed and why** — where does it live now, and what other record was created?

```
When the certificate template was published to Active Directory, it was added directly to the AD database's configuration partition. This made it a globally replicated object across the entire domain, advertising its cryptographic requirements and security rules to all domain joined machines. Because of this, when a user or machine opens the enrollment wizard, their system automatically queries Active Directory to see what templates they have permission to use, which is how the MMC snap-in dynamically shows the available options. Once the CVI-Webserver template was selected and the subject name (webserver.corp.cvilab.local) was filled out, the local machine generated its own public/private key pair and bundled everything into a Certificate Signing Request (CSR). The machine then signed this request with its new private key to prove ownership to the CA without ever exposing the private key itself.

Before actually issuing anything, the CA put the CSR through a strict review process. It verified the digital signature to make sure the request hadn't been messed with, and checked Active Directory to confirm that the identity (CORP.PKI-SRV01$) had the right permissions to enroll. It also made sure the request matched all the template's specific constraints, like validity periods and intended uses. After passing these checks, the CA generated the final certificate, signed it using its own private key, and logged the transaction as Request ID 8 in its internal database. Finally, the certificate was sent back to the local machine and dropped into the Personal (My) certificate store, pairing it up with the local private key so it's fully ready to be bound to web services.
```

**One thing about the issuance process you did not expect or want to understand better:**

```
I didn't expect that the private key never actually touches or travels to the Certificate Authority during enrollment. It was interesting to learn that the local machine handles key generation entirely on its own, and the CA only signs the public portion.
```

---

## Submission Checklist

- [X] Pre-lab verification completed — CertSvc running, CA responding, CVI-WebServer template confirmed in certtmpl.msc
- [X] Part A: CVI-WebServer template published to CVI Issuing CA 1 via certsrv.msc
- [X] Part A: Template visible in Certificate Templates node — confirmed
- [X] Part B: MMC opened with Certificates snap-in (My user account)
- [X] Part B: Active Directory Enrollment Policy selected
- [X] Part B: CVI-WebServer template located in wizard — yellow triangle behavior documented if encountered
- [X] Part B: Subject name supplied (webserver.corp.cvilab.local)
- [X] Part B: Certificate enrollment outcome documented
- [X] Part C: Certificate details recorded from MMC (General + Details tabs)
- [X] Part C: certutil -store My output pasted in full
- [X] Part C: Certificate confirmed in certsrv.msc Issued Certificates node
- [X] Part C: Request ID recorded
- [X] Part D: Issuance workflow write-up completed in own words — covers all 4 stages
- [X] File saved as `lab-02-first-issuance.md`
- [X] File committed to portfolio repo under `labs/week-10/`
