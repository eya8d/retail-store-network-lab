# Configuration notes

I kept two sets of files:

- **observed/** contains the exports I took during the build, before the last cleanup. I removed password hashes, but kept the old misspellings and unused entries so the troubleshooting notes still make sense.
- **build/** contains cleaned configurations for fresh matching devices. These create the VLANs explicitly and let port security learn the MACs of the PCs you attach.

The build files also add consistent admin privileges, console login and an enable secret. I haven't applied all of those extra management changes to the uploaded .pkt, so don't use the build files as an exact copy of its current settings.

I applied the targeted R1 cleanup, corrected the SW1 domain and closed unused SW2 G0/2. I haven't uploaded replacement exports after those changes yet.

To run the existing lab, open [Retail-Store-Network.pkt](../Retail-Store-Network.pkt). To rebuild it, follow [the rebuild notes](../docs/REBUILD.md) and check [the demonstration credentials](../docs/LAB-CREDENTIALS.md).
