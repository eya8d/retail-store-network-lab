# Configuration files

- [observed/](observed/): sanitized snapshots of the four exports supplied during the build, before the final cleanup. Historical misspellings and redundant objects are deliberately retained for troubleshooting context. Credential hashes are removed and the username lines are comments.
- [build/](build/): corrected templates for rebuilding on fresh matching devices. They explicitly create VLANs and relearn endpoint MACs. These are authored templates, not exports from the final .pkt. They include optional management hardening and public demonstration credentials described in [LAB-CREDENTIALS.md](../docs/LAB-CREDENTIALS.md).

The user subsequently reported applying the targeted R1 cleanup, correcting the SW1 domain, and shutting down unused SW2 G0/2, then confirmed the three post-cleanup service tests listed in the main README. New final configuration exports have not been supplied; the observed files must not be presented as the final cleaned state.

The runnable simulation is [Retail-Store-Network.pkt](../Retail-Store-Network.pkt). Do not bulk-paste fresh-build templates over the existing topology.
