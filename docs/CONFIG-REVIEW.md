# Cleanup notes

After the network was working, I went through the running configurations to remove leftovers from the build.

## Changes I made

On R1 I removed:

- The extra DHCP exclusion running from `192.168.20.1` to `192.168.10.20`. The correct Office exclusion is `.20.1` through `.20.20`.
- The DNS-only pools `WERAHOUSE` and `WAREHOUSES`. The working pool is `WEARHOUSE`.
- The empty ACLs `WERHOUSE-INNN` and `WERHOUSE-IN`. The applied Warehouse ACL is `WAREHOUSE-IN`.
- The unused `GigabitEthernet0/0.200` subinterface.

I also corrected SW1's domain from `retial.test` to `retail.test` and shut down SW2's unused G0/2.

Afterward, I checked Warehouse portal access, Guest access to the external site, and blocked Guest access to the internal server. Those checks passed.

## Things to remember when rebuilding

The earlier switch exports don't contain the VLAN creation commands. On a 2960, normal-range VLAN information can be stored in vlan.dat. I included explicit VLAN creation in the build files so a fresh switch has what it needs.

Sticky MAC entries belong to the original endpoints. If you create new PCs, let the switch learn their MACs rather than pasting the old bindings.

SSH keys also need to be generated on fresh devices.

## Management settings

The original router account has privilege 15, while the switch accounts don't set an explicit privilege. The build files use a single full-admin lab account, local console login and an enable secret. They also disable unused ISP remote login. Those extra settings are for the rebuild; I haven't confirmed them all in the uploaded simulation.

The [observed exports](../configs/observed/) remain the earlier snapshots. I still need to replace them with a separate set of final exports after cleanup.

## References I used for the notes

- [Cisco 2960 VLAN configuration](https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst2960/software/release/15-0_1_se/configuration/guide/scg2960/swvlan.html)
- [Cisco IP access lists](https://www.cisco.com/c/en/us/support/docs/security/ios-firewall/23602-confaccesslists.html)
