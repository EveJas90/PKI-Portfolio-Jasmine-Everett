# Lab 02 — CA Recovery Simulation: Snapshot Restore

**Student Name:**

**Date Completed:**

**Phase:** 2 | **Week:** 13

**Submission Path:** `labs/week-13/lab-02-ca-recovery-simulation.md`

## Pre-Lab Verification

- Lab 01 backup files present in C:\CABackup: [ ] Yes / [ ] No

- certutil -ping output:

- certutil -CRL output:

## Part A — Pre-Failure Snapshot

- Pre-snapshot CA state (certutil -CRL, thumbprint, last RequestID):

- Hypervisor: [ ] VirtualBox / [ ] UTM

- Snapshot name:

- Snapshot taken successfully: [ ] Yes

## Part B — Destructive Operation

- Option chosen: [ ] Delete database files / [ ] Rename CertLog folder

- Get-Service CertSvc (failed state):

- Event log errors (Event IDs and messages):

- Failure description in your own words:

## Part C — Snapshot Restore

- PKI-SRV01 powered off before restore: [ ] Yes

- Snapshot restore completed: [ ] Yes

- VM started and login successful: [ ] Yes

## Part D — Post-Recovery Verification

- Get-Service CertSvc:

- certutil -ping:

- certutil -CRL:

- CRL accessible at HTTP CDP: [ ] Yes

- Issued certificate count matches pre-failure baseline: [ ] Yes

- Event log clean: [ ] Yes / [ ] Errors found:

- Recovery completed at (date and time):

- Time from snapshot restore to CA fully operational:

## Part E — Lab Report

1. [Describe the failure state — Get-Service output, Event IDs, what a real admin would see]

2. [Walk through the snapshot restore procedure — what did the hypervisor do?]

3. [Walk through post-recovery verification — why isn't it sufficient to just check the service?]

4. [If the last snapshot was 72 hours ago in production — what data would be lost and why does it matter?]

5. [Compare snapshot restore to file-based restore — primary advantage and when snapshot restore is not available]

## Submission Checklist

- [ ] Logged in as CORP\pki.admin

- [ ] Lab 01 backup files confirmed present before snapshot

- [ ] Pre-failure CA state recorded

- [ ] Snapshot taken — name documented

- [ ] Destructive operation performed — failure state documented

- [ ] Snapshot restore completed without errors

- [ ] Post-recovery: certutil -ping successful

- [ ] Post-recovery: certutil -CRL successful

- [ ] Post-recovery: CRL accessible at HTTP CDP

- [ ] Post-recovery: database count matches baseline

- [ ] Post-recovery: event log clean

- [ ] Recovery time recorded

- [ ] All five lab report questions answered

**File path:** `labs/week-13/lab-02-ca-recovery-simulation.md`
