# Lab 01: Explore and Duplicate a Certificate Template

**Student Name:** Jasmine Everett
**Date Completed:** 6/10/26  
**Phase:** 2 | **Week:** 10  
**Submission Path:** `labs/week-10/lab-01-template-exploration.md`

---

## Pre-Lab Verification

If you can log into PKI-SRV01 as CORP\pki.admin, you are communicating with DC01 and the environment is ready. Proceed to Part A.

---

## Part A — Explore Three Built-in Templates

### Step 1 — Open the Certificate Templates Console

1. On PKI-SRV01, press **Win + R**, type `certtmpl.msc`, and press **Enter**
2. The Certificate Templates console opens showing all templates installed on the forest
3. Scroll through the list to get familiar with what exists before locating your three target templates

### Step 2 — Explore the User Template

1. Scroll to find the **User** template in the list
2. Right-click **User** → select **Properties**
3. Work through each tab below and record what you observe

**General tab:**

| Field | Value |
|-------|-------|
| Template Display Name |User |
| Template Name (internal) |User |
| Validity Period |1 year |
| Renewal Period |6 years|
| Schema Version |1 3.1|

**Request Handling tab — Purpose:**

```
Signature and encryption
```

**Subject Name tab:**

```
Built from information in Active Directory
```

**Extensions tab — Key Usage:**

```
Digital Signature, Allow key echnage only with key encryption, Critical extension.
```

**Extensions tab — Application Policies (EKU):**

```
Encrypting File System
Secure Email
Client Authentication
```

4. Close the Properties window when done

### Step 3 — Explore the Computer Template

1. Scroll to find the **Computer** template (note: the internal name is "Machine")
2. Right-click **Computer** → select **Properties**
3. Record the same tabs

**General tab:**

| Field | Value |
|-------|-------|
| Template Display Name |Computer|
| Template Name (internal) |Machine|
| Validity Period |1 year|

**Request Handling tab — Purpose:**

```
Signature and encryption
```

**Subject Name tab:**

```
Built from information in Active Directory
```

**Extensions tab — Key Usage:**

```
Digital Signature, Allow key echnage only with key encryption, Critical extension
```

**Extensions tab — Application Policies (EKU):**

```
Client Server
Server Authentication
```

4. Close the Properties window when done

### Step 4 — Explore the Web Server Template

1. Scroll to find the **Web Server** template
2. Right-click **Web Server** → select **Properties**

**General tab:**

| Field | Value |
|-------|-------|
| Template Display Name |Web Server |
| Template Name (internal) |WebServer|
| Validity Period |2 year|

**Request Handling tab — Purpose:**

```
Signature and Encryption
```

**Subject Name tab:**

```
Supplied in the request
```

**Extensions tab — Key Usage:**

```
Digital Signature, Allow key echnage only with key encryption, Critical extension
```

**Extensions tab — Application Policies (EKU):**

```
Server Authentication
```

3. Close the Properties window when done

### Step 5 — Template Comparison Questions

In your own words — what is the most significant difference between the User, Computer, and Web Server templates? Address both the subject name source and the EKU differences in your answer.

```
The most significant difference between the User, Computer, and Web Server templates that I observed would be the subject names and the EKUs. For the User and Computer templates the subject names were built from Active Directory, where as the Web Server template, the subject name comes from the request. In regards to the EKUs, I noticed that only the User/Computer templates include Client Authentication and the Web Server template only requires Server Authentication.

```

Why does the Web Server template use "Supplied in the request" for the subject name rather than building it from Active Directory?

```
The Web Server template uses the "Supplied in request" for the subject name rather than building it from Active Directory because when it comes to web server's certificates the subject name is supplied on the CSR when the certificate is being sent to the server to be signed. Active Directory is more so an data entry software that only reflects what is manually inputed or synced thru automation. In regards to the lab specifically, AD knows that the internal server computer name is PKI-SRV01.local, but the external web server needs a certificate matching its public web URL like secure.cvi.labs.com or www.company.com. AD does not automatically track public domain names.

```

---

## Part B — Duplicate the Web Server Template

### Step 1 — Open the Duplicate Dialog

1. In `certtmpl.msc`, locate the **Web Server** template
2. Right-click **Web Server** → **Duplicate Template**
3. The Duplicate Template dialog appears

### Step 2 — Set Compatibility Settings

1. In the dialog, set the following compatibility settings:
   - **Certification Authority:** `Windows Server 2012 R2`
   - **Certificate Recipient:** `Windows 7 / Server 2008 R2` (or later)
2. Click **OK** on any informational dialog that appears

**Compatibility settings selected:**
- Certification Authority: Windows Server 2012 R2
- Certificate Recipient: Windows 7 Server 2008 R2

### Step 3 — Configure the General Tab

1. A new template Properties window opens — go to the **General** tab
2. Change **Template display name** to: `CVI-WebServer`
3. Confirm the **Template name** (internal name, no spaces) reads exactly: `CVI-WebServer`
4. Set **Validity period** to `1` year
5. Leave **Renewal period** at the default (6 weeks)

> **Important:** The internal name cannot contain spaces. If your display name has a space (e.g., "CVI WebServer"), Windows strips it in the internal name, producing "CVIWebServer" instead of "CVI-WebServer". This will cause `certutil -template CVI-WebServer` to fail in Part C. Confirm the internal name field shows exactly `CVI-WebServer` with a hyphen before saving.

**General tab — changes made:**

| Setting | Original Value | New Value |
|---------|---------------|-----------|
| Template display name | Web Server | CVI-WebServer |
| Template name (internal) | WebServer | CVI-WebServer |
| Validity period | 2 years |1 year|
| Renewal period |6 weeks|6 weeks|

**Rationale for validity period chosen:**

```
By setting the validity period to 1 year, it ensures that the certificate reduces the security risk of key compromise by limiting its operational lifespan. This short duration minimizes the time a leaked key is valid and ensures that encryption standards are updated regularly during the annual renewal cycle.
```

### Step 4 — Verify the Subject Name Tab

1. Click the **Subject Name** tab
2. Confirm **Supply in the request** is selected — this was inherited from the Web Server base template and must stay unchanged

**Supply in the request is selected:**
- [X] Yes

### Step 5 — Confirm Security Tab Permissions

1. Click the **Security** tab
2. Confirm **Domain Computers** have **Enroll** checked
3. Confirm **Authenticated Users** have **Read** (not Enroll)
4. Click **Apply**

**Security tab — permissions confirmed:**

| Group / Account | Read | Enroll | Autoenroll |
|-----------------|------|--------|------------|
| Authenticated Users |X | | |
| Domain Computers |X |X| |

### Step 6 — Save the Template

1. Click **OK** to close the Properties window and save the template
2. Verify **CVI-WebServer** now appears in the `certtmpl.msc` list. Press **F5** to refresh if it does not appear within 30 seconds.

**CVI-WebServer visible in certtmpl.msc:**
- [X] Yes

---

## Part C — Inspect the Duplicate Template with certutil

### Step 1 — Run the certutil Command

1. Open **PowerShell** on PKI-SRV01 as CORP\pki.admin
2. Run:

```powershell
certutil -template CVI-WebServer
```

3. Copy the full output and paste it below

**Full certutil output:**

```
  Name: Active Directory Enrollment Policy
  Id: {41635678-B3E8-4BD7-8FE7-D49A1E336991}
  Url: ldap:
35 Templates:

  Template[9]:
  TemplatePropCommonName = CVI-WebServer
  TemplatePropFriendlyName = CVI-WebServer
  TemplatePropEKUs =
1 ObjectIds:
    1.3.6.1.5.5.7.3.1 Server Authentication

  TemplatePropCryptoProviders =
    0: Microsoft RSA SChannel Cryptographic Provider
    1: Microsoft DH SChannel Cryptographic Provider

  TemplatePropMajorRevision = 64 (100)
  TemplatePropDescription = Computer
  TemplatePropSchemaVersion = 2
  TemplatePropMinorRevision = 6
  TemplatePropRASignatureCount = 0
  TemplatePropMinimumKeySize = 800 (2048)
  TemplatePropOID =
    1.3.6.1.4.1.311.21.8.15886664.4298044.8996776.14853544.7902291.169.8802149.7201620 CVI-WebServer

  TemplatePropV1ApplicationPolicy =
1 ObjectIds:
    1.3.6.1.5.5.7.3.1 Server Authentication

  TemplatePropEnrollmentFlags = 0

  TemplatePropSubjectNameFlags = 1
    CT_FLAG_ENROLLEE_SUPPLIES_SUBJECT -- 1

  TemplatePropPrivateKeyFlags = 4060000 (67502080)
    CTPRIVATEKEY_FLAG_ATTEST_NONE -- 0
    TEMPLATE_SERVER_VER_THRESHOLD<<CTPRIVATEKEY_FLAG_SERVERVERSION_SHIFT -- 60000 (393216)
    TEMPLATE_CLIENT_VER_WIN8<<CTPRIVATEKEY_FLAG_CLIENTVERSION_SHIFT -- 4000000 (67108864)

  TemplatePropGeneralFlags = 20241 (131649)
    CT_FLAG_ENROLLEE_SUPPLIES_SUBJECT -- 1
    CT_FLAG_MACHINE_TYPE -- 40 (64)
    CT_FLAG_ADD_TEMPLATE_NAME -- 200 (512)
    CT_FLAG_IS_MODIFIED -- 20000 (131072)

  TemplatePropSecurityDescriptor = O:S-1-5-21-3975454498-3980183307-2685672490-1105G:S-1-5-21-3975454498-3980183307-2685672490-519D:PAI(OA;;CR;0e10c968-78fb-11d2-90d4-00c04f79dc55;;DC)(OA;;RPWPCR;0e10c968-78fb-11d2-90d4-00c04f79dc55;;DA)(OA;;RPWPCR;0e10c968-78fb-11d2-90d4-00c04f79dc55;;S-1-5-21-3975454498-3980183307-2685672490-519)(A;;LCRPRC;;;DC)(A;;CCDCLCSWRPWPDTLOSDRCWDWO;;;DA)(A;;CCDCLCSWRPWPDTLOSDRCWDWO;;;S-1-5-21-3975454498-3980183307-2685672490-519)(A;;CCDCLCSWRPWPDTLOSDRCWDWO;;;S-1-5-21-3975454498-3980183307-2685672490-1105)(A;;LCRPLORC;;;AU)

    Allow Enroll        CORP\Domain Computers
    Allow Enroll        CORP\Domain Admins
    Allow Enroll        CORP\Enterprise Admins
    Allow Read  CORP\Domain Computers
    Allow Full Control  CORP\Domain Admins
    Allow Full Control  CORP\Enterprise Admins
    Allow Full Control  CORP\pki.admin
    Allow Read  NT AUTHORITY\Authenticated Users


  TemplatePropExtensions =
4 Extensions:

  Extension[0]:
    1.3.6.1.4.1.311.21.7: Flags = 0, Length = 31
    Certificate Template Information
        Template=CVI-WebServer(1.3.6.1.4.1.311.21.8.15886664.4298044.8996776.14853544.7902291.169.8802149.7201620)
        Major Version Number=100
        Minor Version Number=6

  Extension[1]:
    2.5.29.37: Flags = 0, Length = c
    Enhanced Key Usage
        Server Authentication (1.3.6.1.5.5.7.3.1)

  Extension[2]:
    2.5.29.15: Flags = 1(Critical), Length = 4
    Key Usage
        Digital Signature, Key Encipherment (a0)

  Extension[3]:
    1.3.6.1.4.1.311.21.10: Flags = 0, Length = e
    Application Policies
        [1]Application Certificate Policy:
             Policy Identifier=Server Authentication

  TemplatePropValidityPeriod = 1 Years
  TemplatePropRenewalPeriod = 6 Weeks
CertUtil: -Template command completed successfully.
```

### Step 2 — Extract Key Fields

From the certutil output, fill in the table below. No cells should be left blank.

| Field | Value from certutil Output |
|-------|---------------------------|
| Template Name |CVI-WebServer|
| Template OID |1.3.6.1.4.1.311.21.8.15886664.4298044.8996776.14853544.7902291.169.8802149.7201620|
| Schema Version |2|
| Key Usage |Digital Signature, Key Encipherment|
| Enhanced Key Usage (EKU) |Server Authentication|
| Validity Period |1 year |
| Subject Name flags |CT_FLAG_ENROLLEE_SUPPLIES_SUBJECT -- 1|

> **Finding Key Usage:** Look for `Key Usage=` in the output — it shows a hex value like `0xa0`, which represents Digital Signature (0x80) + Key Encipherment (0x20).
>
> **Finding the Subject Name flag:** Look for `CT_FLAG_ENROLLEE_SUPPLIES_SUBJECT` — this confirms "Supply in request" is set.

---

## Reflection

**Why does AD CS require you to duplicate a built-in template rather than modifying it directly?**

```
(your answer here — what is the consequence of modifying a built-in template, and why does the duplicate workflow protect against this?)

AD CS requires you to duplicate a built-in template beacuse the built-in template is being used by the computer or server in the background and it also acts as a guide. If you modify a built-in template you run the risk of not being able to create new templates.
```

**One setting in the template you found unexpected or want to explore further:**

```
I would like to learn more about when to use the Cryptogaphy tab and waht situations would cause someone to want to update those fields.
```

---

## Submission Checklist

- [X] Pre-lab verification completed — all three checks documented
- [X] Part A: User template — General, Request Handling, Subject Name, and Extensions tabs recorded
- [X] Part A: Computer template — same tabs recorded
- [X] Part A: Web Server template — same tabs recorded
- [X] Part A: Comparison question answered in own words (3–5 sentences)
- [X] Part A: "Supply in the request" question answered with reasoning
- [X] Part B: Duplicate dialog opened from Web Server template
- [X] Part B: Compatibility settings recorded
- [X] Part B: Internal name confirmed as `CVI-WebServer` (hyphen, no spaces)
- [X] Part B: Validity period rationale provided
- [X] Part B: Security tab permissions documented
- [X] Part B: Template visible in certtmpl.msc confirmed
- [X] Part C: `certutil -template CVI-WebServer` output pasted in full
- [X] Part C: Key fields extracted (OID, schema version, Key Usage hex, EKU OID, subject name flags)
- [X] Reflection section completed
- [X] File saved as `lab-01-template-exploration.md`
- [X] File committed to portfolio repo under `labs/week-10/`
- [X] File saved as `lab-01-template-exploration.md`
- [X] File committed to portfolio repo under `labs/week-10/`
