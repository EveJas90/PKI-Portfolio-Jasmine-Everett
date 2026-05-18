# Lab 01: Environment Verification & VM Connectivity Check

**Student Name: Jasmine Everett 
**Date Completed: 5/10/26 
**Phase:** 2 | **Week:** 9  
**Submission Path:** `labs/week-09/lab-01-environment-verification.md`

---

## Step 1 – VM Startup & Login

**VMs started in correct order (DC01 → PKI-SRV01 → Root-CA):**
- [X] Yes
- [ ] No — describe what happened:

**Login credentials used:**

| VM | Account Used | Login Successful? |
|----|-------------|-------------------|
| DC01 | CORP\pki.admin |Yes|
| PKI-SRV01 | CORP\pki.admin |Yes|
| Root-CA | .\Administrator |Yes|

**Notes / issues encountered:**

I did not expirence any issues signing into the Virtual Machines.


---

## Step 2 – VM Connectivity Test

**Command run on PKI-SRV01:**

```powershell
Test-Connection -ComputerName DC01 -Count 2

```

**Output received:**

```
Source        Destination     IPV4Address      IPV6Address                              Bytes    Time(ms)
------        -----------     -----------      -----------                              -----    --------
PKI-SRV01     DC01            192.168.10.10                                             32       0
PKI-SRV01     DC01            192.168.10.10                                             32       0
```

**DC01 responded successfully:**
- [X] Yes
- [ ] No — troubleshooting steps taken:

---

## Step 3 – CertSvc Service Status

**Command run on PKI-SRV01:**

```powershell
Get-Service -Name CertSvc
```

**Output received:**

```
Status   Name               DisplayName
------   ----               -----------
Running  CertSvc            Active Directory Certificate Services

```

**CertSvc status shown:** Running

**Service was Running:**
- [X] Yes
- [ ] No — action taken:

---

## Step 4 – Certification Authority Console

**Steps completed on PKI-SRV01:**
- [X] Opened certsrv.msc via Run dialog
- [X] Confirmed CA name visible: CVI Issuing CA 1
- [X] Confirmed CA status: Running
- [X] Expanded left pane — folders visible (Revoked Certificates, Issued Certificates, etc.)

**Screenshot or description of what you observed in certsrv.msc:**

The "Computer Tower" icon with a green check is also another way to check if the service is running. If the "Computer Tower" icon had a red x instead this would mean the service is inactive or not running.

![CertSrv Example](https://github.com/EveJas90/PKI-Portfolio-Jasmine-Everett/blob/e9767a54b85f97bfb2ad3896a0bfe0d815944df6/assets/screenshots/week-09/CertSrv%20Screenshot.png)



---

## Step 5 – Certificate Log File Path

**Command run on PKI-SRV01:**

```powershell
Get-ChildItem "C:\Windows\System32\CertLog"
```

**Output received:**

I'm getting an error when running the command tha says "Get-ChildItem : Access to the path 'C:\Windows\System32\CertLog' is denied"


**Files found in CertLog:**

| File Name | Approximate Size |
|-----------|-----------------|
| | |
| | |

---

## Reflection

**One thing that went well during this lab:**

I have created and actively used using virtual machines in my own personal homelabs so this excercise refreshed my memory on concepts I have already attempted before on my own. I also do alot of this stuff already at work so with this excercise it put me back in the mindset of being at work lol. 


**One thing that was confusing or unexpected:**

I would say getting the error trying to get the Child Items out of the CertLog folder using PowerShell was unexpected. I expected the command to produce an output since the other commands processed as expected. 


---

## Submission Checklist

- [X] All five steps completed
- [X] All command outputs pasted
- [X] Reflection section filled in
- [X] File saved as `lab-01-environment-verification.md`
- [X] File committed to my portfolio repo under `labs/week-09/`
