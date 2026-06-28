# Lab 01 — Full CA Backup: Database, Keys, and System State

**Student Name:**

**Date Completed:**

**Phase:** 2 | **Week:** 13

**Submission Path:** `labs/week-13/lab-01-ca-backup.md`

## Pre-Lab Verification

- whoami output:

- Get-Service CertSvc output:

- certutil -ping output:

## Part A — Backup Folder

- C:\CABackup created via File Explorer: [ ] Yes

- dir C:\CABackup (confirm empty):

## Part B — CA Database and Private Key Backup

- certutil -backup -p output (full):

- .p12 file — full path, size, timestamp:

- .edb file — full path, size, timestamp:

- certutil -dump output (first 20 lines):

- Password storage location (not the password):

## Part C — Windows System State Backup

- Get-WindowsFeature output:

- wbadmin output (or documented limitation):

- wbadmin get versions output:

## Part D — Post-Backup Verification

- Get-Service CertSvc:

- certutil -ping:

- certutil -CRL:

## Part E — Lab Report

1. [Describe the three backup components and what recovery requires if each is missing]

2. [Explain what VSS does and why it is better than stopping the CA service]

3. [Where did you store the password, and why is storing it in C:\CABackup a security problem?]

4. [What does the system state backup capture that certutil -backup does not?]

5. [Explain the relationship between backup frequency and RPO]

## Submission Checklist

- [ ] Logged in as CORP\pki.admin

- [ ] C:\CABackup created via File Explorer and confirmed empty

- [ ] certutil -backup -p completed — full output included

- [ ] .p12 and .edb files confirmed — sizes recorded

- [ ] certutil -dump confirms .p12 is readable

- [ ] Password stored separately — location documented

- [ ] Windows Server Backup installed

- [ ] wbadmin completed (or limitation documented)

- [ ] CA operational after backup — all three commands succeeded

- [ ] All five lab report questions answered

**File path:** `labs/week-13/lab-01-ca-backup.md`
