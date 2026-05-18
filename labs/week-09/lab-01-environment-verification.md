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

```
I did not expirence any issues signing into the Virtual Machines.
```

---

## Step 2 – VM Connectivity Test

**Command run on PKI-SRV01:**

```powershell
Test-Connection -ComputerName DC01 -Count 2

```

**Output received:**

```
<img width="844" height="113" alt="DC01 Output" src="https://github.com/user-attachments/assets/dc8bf14b-6d83-4c41-a6a7-d50ff9c24d8d" />
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
<img width="525" height="182" alt="DC01 Output 2" src="https://github.com/user-attachments/assets/2fdaa898-a356-41cf-a109-3a7e8977817f" />

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

```
The "Computer Tower"icon with a green check is also another way to check if the service is running.If the "Computer Tower" icon had a red x instead this would mean the service is inactive or not running.


<img width="754" height="520" alt="CertSrv Screenshot" src="https://github.com/user-attachments/assets/ad156d2e-df0e-4b67-a94e-1012667e1430" />

```

---

## Step 5 – Certificate Log File Path

**Command run on PKI-SRV01:**

```powershell
Get-ChildItem "C:\Windows\System32\CertLog"
```

**Output received:**

```
I'm getting an error when running the command tha says "Get-ChildItem : Access to the path 'C:\Windows\System32\CertLog' is denied"
```

**Files found in CertLog:**

| File Name | Approximate Size |
|-----------|-----------------|
| | |
| | |

---

## Reflection

**One thing that went well during this lab:**

```
I have created and actively used using virtual machines in my own personal homelabs so this excercise refreshed my memory on concepts I have already attempted before on my own. I also do alot of this stuff already at work so with this excercise it put me back in the mindset of being at work lol. 
```

**One thing that was confusing or unexpected:**

```
I would say getting the error trying to get the Child Items out of the CertLog folder using PowerShell was unexpected. I expected the command to produce an output since the other commands processed as expected. 
```

---

## Submission Checklist

- [ ] All five steps completed
- [ ] All command outputs pasted
- [ ] Reflection section filled in
- [ ] File saved as `lab-01-environment-verification.md`
- [ ] File committed to my portfolio repo under `labs/week-09/`
