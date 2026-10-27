# Review and prepared corrections

Basis: four device running-config exports supplied by the user. A static review was performed. The final .pkt and full topology screenshot are now uploaded at the repository root, but the .pkt has not been executed by the documentation reviewer.

| Finding | Correction prepared | Live status |
|---|---|---|
| Malformed extra exclusion .20.1 to .10.20 | Remove exact malformed line; retain correct .20.1–.20.20 | Prepared in files; see status note below |
| DNS-only WERAHOUSE / WAREHOUSES pools | Remove; preserve working WEARHOUSE | Prepared in files; see status note below |
| Empty WERHOUSE-INNN / WERHOUSE-IN ACLs | Remove; preserve applied WAREHOUSE-IN | Prepared in files; see status note below |
| Unused G0/0.200 | Remove | Prepared in files; see status note below |
| SW1 retial.test typo | Correct to retail.test | Prepared in files; see status note below |
| SW2 unused G0/2 not shut down | Disable after confirming uncabled | Prepared in files; see status note below |
| Switch account privilege inconsistent | Set netadmin privilege15 for this single-admin lab | Prepared in files; see status note below |
| No enable secret or console authentication | Add public lab-only enable secret and local console login | Prepared in files; see status note below |
| ISP unused remote management | Disable VTY transport | Prepared in files; see status note below |
| Exports omit VLAN creation and key material | Fresh templates create VLANs; RSA keys generated separately | Rebuild instructions provided |

The observed files retain original network settings but omit hashes, license serials, CLI prompts/errors and repeated blank separators. Their username lines are comments, so they are not complete restore scripts.

Corrected build templates remove old sticky bindings to relearn MACs from new endpoints. They explicitly set one-MAC maximum and shutdown violation mode, matching the defaults in the source behavior. Existing-lab patches preserve the learned MAC bindings and working ACL policy.

All VTY ranges present in the source exports are restricted by ACL99 on store devices. No claim is made that every possible connection is filtered by interface ACLs. See POLICY.md.

Normal-range VLAN state can reside in vlan.dat and may not appear as creation statements in running-config. Capture show vlan brief and include explicit VLAN steps when rebuilding. SSH RSA private keys also require separate generation.

Primary sources:
- Cisco 2960 VLAN configuration: https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst2960/software/release/15-0_1_se/configuration/guide/scg2960/swvlan.html
- Cisco ACL configuration: https://www.cisco.com/c/en/us/support/docs/security/ios-firewall/23602-confaccesslists.html

No production deployment, complete security audit or final test pass is implied.

## Subsequent builder-reported status

The builder reported applying the targeted R1 cleanup, the SW1 domain correction, and the SW2 unused-port shutdown. Warehouse portal access and the Guest external-allow/internal-deny tests were reported successful afterward. Replacement running-config exports have not been supplied, so these changes are reported, not independently re-exported or executed by the reviewer. The extra management hardening in the fresh-build templates (privilege 15, enable secret, console login, and ISP VTY disabling) has NOT been confirmed applied to the uploaded simulation.
