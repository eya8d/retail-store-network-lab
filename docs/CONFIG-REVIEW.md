# Final configuration review

I finished the cleanup and completed the manual test checklist.

## Configuration checked

- Each of the four DHCP pools has the correct network, gateway and DNS server.
- The .1–.20 DHCP exclusions reserve infrastructure addresses.
- POS, Warehouse and Guest ACLs are applied to their matching subinterfaces.
- PAT selects Office and Guests and uses the WAN interface address.
- SSH source ACL99 covers the configured VTY ranges on the store devices.
- SW1's domain is `retail.test`.
- Unused SW2 G0/2 is disabled.
- Unused DHCP pools, empty ACLs and G0/0.200 are removed.

The [observed configurations](../configs/observed/) include those cleanup changes. Password hashes and serial identifiers are omitted.

## Rebuild notes

The switch VLAN database and SSH keys need attention when starting with fresh devices. The build files create the VLANs explicitly; RSA keys are generated on each store network device.

Sticky MAC entries belong to the original endpoints. On a new topology, let port security learn the new endpoints.

The build files include additional full-admin, console-login and enable-secret settings. Check [LAB-CREDENTIALS.md](LAB-CREDENTIALS.md) for that version.

## Validation

All 26 checks in [validation.csv](../tests/validation.csv) passed during manual testing confirmed by Eyad. Selected screenshots are listed in [the evidence index](../evidence/README.md).

## References

- [Cisco 2960 VLAN configuration](https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst2960/software/release/15-0_1_se/configuration/guide/scg2960/swvlan.html)
- [Cisco IP access lists](https://www.cisco.com/c/en/us/support/docs/security/ios-firewall/23602-confaccesslists.html)
