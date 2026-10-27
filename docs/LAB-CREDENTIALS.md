# Public demonstration credentials

The corrected build templates deliberately use these **public, lab-only** credentials:

| Purpose | Value |
|---|---|
| Username | netadmin |
| Login password | RetailLab2026! |
| Enable secret | RetailEnable2026! |

They are not private secrets and must never be reused outside this simulated lab. Existing observed exports have credential hashes removed. The .pkt itself must use lab-only credentials before you publish it.

Applying a fresh-build template sets the demonstrated netadmin password and grants privilege 15 on store devices and the ISP console. Console login uses the local account. This single-administrator model is a deliberate learning-lab choice, not role separation. R1/SW1/SW2 SSH retains source ACL99; ISP remote VTY transport is disabled. Do not close your direct CLI until you have tested new access.

`service password-encryption` is basic configuration obfuscation for applicable password fields; it does not replace SSH encryption or secure secret storage.

These credentials describe the authored fresh-build templates only. They are not a verified credential inventory for the uploaded .pkt.
