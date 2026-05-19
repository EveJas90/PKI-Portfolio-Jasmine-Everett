## OpenSSL CSR Generation Failing to Prompt for Interactive Distinguished Name Attributes

*Give your runbook a clear, specific name that describes the scenario.*

> Example: *Certificate Verification Failure Due to Expired Intermediate CA*

**Runbook title:** OpenSSL CSR Generation Failing to Prompt for Interactive Distinguished Name Attributes

---

## Problem Statement

*In one to two sentences: what did you expect to happen, and what happened instead?*

When attempting to generate a Certificate Signing Request (CSR), I expected the OpenSSL terminal to interactively prompt me to input the required Distinguished Name (DN) attributes. Instead, the command executed without providing the interactive prompts, resulting in an incomplete or misconfigured CSR setup.

---

## Environment

*Note the relevant context so someone reading this knows whether it applies to their situation.*

**Tool(s) used:** OpenSSL CLI

**Certificate type or component involved:** Certificate Signing Request (CSR) generation via Private Key

**Any relevant configuration details:** Standard local terminal environment executing cryptographic commands against a pre-generated private key ("test_key.pem")

---

## Symptoms

*List the specific signals that indicated something was wrong. Paste exact error messages or command output where possible.*

-The terminal bypasses or fails to display the interactive prompt fields (e.g., "Country Name (2 letter code)", "State or Province Name", "Locality Name", "Organization Name", "Common Name")

- The generated CSR file is either empty, missing crucial identity fields, or errors out due to missing DN configurations.

---

## Diagnostic Steps

*Walk through what you did to identify the root cause. Number each step. Include the exact commands you ran and what their output told you.*

1. Checked the original OpenSSL syntax being used to ensure it wasn't accidentally pointing to a quiet/silent configuration file or missing an execution switch.
   
2. Verified that the private key ("test_key.pem") existed in the specified directory and was syntactically valid.
   
3. Isolated the execution parameters to strip out unnecessary config flags, forcing OpenSSL to default back to its standard interactive shell behavior.

---

## Resolution

*Describe exactly what fixed the issue — the specific command, config change, or action taken.*

To fix the issue and force the terminal to successfully produce the interactive prompts to input the required setup fields, isolate and execute the creation request using explicitly targeted flags. Use the following clean two-line layout:
 
 ```
bash
openssl req -new \
-key labs/week-05/submissions/generate-csr/test_key.pem \
-out labs/week-05/submissions/generate-csr/test_csr.pem
 ```
Running this syntax strips away competing configuration arguments and ensures OpenSSL freezes the terminal state to properly ingest the identity parameters required to successfully complete the CSR.


---

## Prevention Note

*One to two sentences: how to avoid this issue in the future, or what to check first if it happens again.*

When generating CSRs manually via the CLI, avoid combining complex config files unless they are explicitly pre-mapped with default values. To guarantee a successful interactive prompt setup, stick to a stripped-back, direct "-new", "-key", and "-out" syntax structure.

---

**Lab this scenario is drawn from:** `labs/week-05/submissions/generate-csr.md

---

*CVI PKI Career Pathway — Phase 1 Foundations*
