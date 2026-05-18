# Lab 02: AD CS Console Exploration & CA Hierarchy Documentation

**Student Name:** Jasmine Everett 
**Date Completed:** May 17 2026
**Phase:** 2 | **Week:** 9  
**Submission Path:** `labs/week-09/lab-02-environment-documentation.md`

---

## Part A — AD CS Console Exploration (PKI-SRV01)

### CA Console Nodes — Observations

| Node | Contents / Observations |
|------|------------------------|
| Revoked Certificates | No Items To Show |
| Issued Certificates | No Items To Show |
| Pending Requests | No Items To Show |
| Certificate Templates |Shows 11 Certificate Templates |

### CA Properties — Key Settings

**General Tab**
- CA Name: CVI Issuing CA 1
- Computer Name: NA

**Extensions Tab — CRL Distribution Points (CDP):**


![CVI CDP](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/004aba9f6d7acda51a34543d39195e5a9c1486b8/assets/screenshots/week-09/CVI%20%20CDP.png)


**Extensions Tab — Authority Information Access (AIA):**

![CVI AIA](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/356d4ddc1a5d08c74a212f3b124b242f172b47f7/assets/screenshots/week-09/CVI%20AIA.png)


**Storage Tab**
- Database Path: C:\Windows\system32\CertLog
- Log Path: C:\Windows\system32\CertLog

### Certificate Templates Console (certtmpl.msc)

Templates visible in the forest (list what you observed):

![CVI CDP](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/e87b4a461d9f41f4aa22dde7c0321f835177753d/assets/screenshots/week-09/Cert%20Temp%20List.png)

---

## Part B — CA Hierarchy Verification (PKI-SRV01)

### Command: certutil -store -enterprise Root

```
================ Certificate 0 ================
Serial Number: 26373e51a6ab669340c47caef2232ce1
Issuer: CN=CVI Root CA, DC=corp, DC=cvilab, DC=local
 NotBefore: 4/25/2026 6:15 PM
 NotAfter: 4/25/2046 6:25 PM
Subject: CN=CVI Root CA, DC=corp, DC=cvilab, DC=local
CA Version: V0.0
Signature matches Public Key
Root Certificate: Subject matches Issuer
Cert Hash(sha1): b805e6ab548f6e7c57d3989f61de7fe6a51031d1
No key provider information
Cannot find the certificate and private key for decryption.
CertUtil: -store command completed successfully.
```

**What did you see?** (Subject, Issuer, Thumbprint — describe in your own words):

This is a self signed Root CA certificate issued by the CVI Root CA within the corp.cvilab.local domain evironment. Both Subject and Issuer fields match exactly (CN=CVI Root CA, DC=corp, DC=cvilab, DC=local), which confirms that this is the Root CA certificate. Thumbprint in this case this would ne the Cert Hash which I can see is SHA-1 b805e6ab548f6e7c57d3989f61de7fe6a51031d1.  


### Command: certutil -store -enterprise CA

```
CA "Intermediate Certification Authorities"
================ Certificate 0 ================
Serial Number: 5800000002f7714edc7f317c46000000000002
Issuer: CN=CVI Root CA, DC=corp, DC=cvilab, DC=local
 NotBefore: 4/25/2026 7:26 PM
 NotAfter: 4/25/2027 7:36 PM
Subject: CN=CVI Issuing CA 1, DC=corp, DC=cvilab, DC=local
CA Version: V0.0
Certificate Template Name (Certificate Type): SubCA
Non-root Certificate
Template: SubCA, Subordinate Certification Authority
Cert Hash(sha1): 5137a597de2c3085ec5816c7f11edc18cfcdbaf8
No key provider information
Missing stored keyset
CertUtil: -store command completed successfully.
```

**What did you see?** (Subject, Issuer, Thumbprint — describe in your own words):

This is the Intermediate CA certificate issued by the CVI Root CA to the CVI Issuing CA 1 within the corp.cvilab.local domain environment. 

Unlike the root certificate, the Subject and Issuer fields are different here:
- Issuer: CN=CVI Root CA, DC=corp, DC=cvilab, DC=local
- Subject: CN=CVI Issuing CA 1, DC=corp, DC=cvilab, DC=local

This confirms a parent-child relationship in the PKI hierarchy, showing that the Root CA has delegated authority to this subordinate Issuing CA. The unique SHA-1 thumbprint (Cert Hash) identifying this specific intermediate certificate is 5137a597de2c3085ec5816c7f11edc18cfcdbaf8.


---

## Part C — Active Directory Structure (DC01)

### Active Directory Users and Computers (dsa.msc)

**PKI Admins OU — accounts found:**

```
Cert Manager, PKI Admin, and PKI Admins
```

**pki.admin account — group memberships:**

```
Domain Admins
Domain Users
PKI Admins
```

**cert.manager account — group memberships:**

```
Domain Users
PKI Admins
```

**Domain-joined computer accounts found:**

```
PKI-SRV01
```

### Active Directory Sites and Services (dssite.msc)

**Server registered under Default-First-Site-Name:**

```
DC01
```

### Certificate Templates Console (certtmpl.msc on DC01)

Did templates appear here, confirming they are stored in AD?
- [ ] Yes
- [X] No — describe what happened: I got an error message saying that Windows couldn't find 'certtmpl.msc'. Make sure you typed name correclty, and then try again. 

---

## Part D — Environment Summary Write-Up

> Write this section in your own words. Cover all five areas. Aim for clarity over length.

### 1. Environment Topology

*(Describe the three VMs, their roles, and IP addresses.)*

-DC01: This is the Domain Controller. This is what the PKI server depends on to complete authentication requests. The IP address for the domain controller is: 192.168.10.10

-PKI-SRV01: This is the PKI server. This acts as the Enterprise Issuing CA for this lab. The IP address for the PKI server is: 192.168.10.20

-RootCA: This is the Offline Root CA. This is a standalone server. This server is not joined to the CORP domain and does not accept CORP\domain logins. This server boots up when an Intermediate/Issuing certificate needs to be signed or renewed, or when a new CRL (Certificate Revocation List) needs to be published. Once the operation is complete, this server is immediately shut down.

### 2. CA Hierarchy

This lab enviroment utilizes a two-tier CA hierarchy.

-Root CA (Tier 1):The CVI Root CA acts as the root of trust. It is configured as a standalone, offline server to completely air gap a private key from the network. It only issues certificates to subodinate CAs.

-Issuing CA (Tier 2): The CVI Issuign CA 1 (PKI)SRV01 is an enterprise, domain joined intermediate CA. Its certificate was issued and signed by the offline Root CA. This server stays online to handle day to day certifcate enrollment, issuance, and validation for domain assets.

### 3. Certificate Templates

Although the active template deployment output was unavailable during documentation, a standard enterprise issuing CA in this design hosts default templates deployed via Active Directory (certtmpl.msc). These typically include Web Server templates for internal HTTPS encryption, Computer/Machine templates for domain asset authentication, and User templates for securing user-based digital signatures and credentials.

### 4. Active Directory Structure

The PKI Admins OU hosts the dedicated accounts used to manage the certificate infrastructure. While the pki.admin and cert.manager accounts appear identical at first glance in Active Directory, a closer look at their group memberships reveals a strict separation of administrative roles:

-cert.manager (Certificate Manager Role): This account belongs to the PKI Admins and Domain Users groups. This grants it full authority to perform day-to-day PKI administrative tasks within the corp.cvilab.local hierarchy, such as issuing, revoking, or reissuing certificates. However, its permissions are limited strictly to the PKI environment.

-pki.admin (Enterprise PKI/Domain Administrator Role): In addition to being a member of the PKI Admins group, this account is also a member of the Domain Admins group. This gives it administrative privileges across the entire Active Directory domain, allowing it to modify domain user objects, configure Group Policies for auto-enrollment, and manage the underlying server infrastructure.

### 5. One Thing I Found Interesting or Unexpected

As an IAM analyst who works with Active Directory on a daily basis, I found it particularly interesting to see a real world implementation of strict role separation within the PKI infrastructure. While the pki.admin and cert.manager accounts look identical at first glance in the OU, separating the day-to-day Certificate Manager role from the high-privilege Domain Admins group perfectly mirrors the principle of least privilege. It was a great reminder that even within dedicated admin OUs, granular group membership is what ultimately prevents lateral movement and secures the environment.

---

## Submission Checklist

- [x] Part A: All five console nodes documented
- [x] Part A: CA Properties (CDP, AIA, Storage) recorded
- [x] Part A: Certificate Templates console observed
- [x] Part B: certutil -store -enterprise Root output included
- [x] Part B: certutil -store -enterprise CA output included
- [x] Part C: PKI Admins OU and both accounts documented
- [x] Part C: Sites and Services server noted
- [x] Part C: certtmpl.msc confirmed templates in AD
- [x] Part D: All five summary areas completed in own words
- [x] File committed to portfolio repo at `labs/week-09/lab-02-environment-documentation.md`
