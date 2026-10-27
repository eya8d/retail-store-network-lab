# Configuration notes

- **observed/** contains my lab configurations, updated with the final cleanup. Password hashes and serial identifiers are omitted.
- **build/** contains configurations for rebuilding on fresh matching devices. These create the VLANs explicitly and let port security learn the MACs of the endpoints you attach.

I removed the unused DHCP pools, empty ACLs, malformed DHCP exclusion and unused subinterface. SW1 uses `retail.test`, and the unused SW2 G0/2 port is disabled.

The build files also add a full-admin account, console login and an enable secret. Those extra settings belong to the rebuild version; they aren't an exact credential or management-settings inventory for the uploaded .pkt.

Open [Retail-Store-Network.pkt](../Retail-Store-Network.pkt) to run the saved lab, or follow [REBUILD.md](../docs/REBUILD.md) to start from scratch.

All 26 checks in [validation.csv](../tests/validation.csv) were completed manually and confirmed by Eyad.
