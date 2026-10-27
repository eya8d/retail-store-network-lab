# Implemented access policy and boundaries

| Source | Allowed routed traffic | Other routed traffic |
|---|---|---|
| POS / VLAN10 | DHCP requests; echo to own gateway; DNS UDP/TCP53 and HTTP TCP80 to 192.168.40.10; echo-reply to Admin-01 | Denied by POS-IN |
| Warehouse / VLAN30 | Same pattern with gateway 192.168.30.1 | Denied by WAREHOUSE-IN |
| Guests / VLAN50 | DHCP requests; echo to own gateway; traffic from guest subnet to non-RFC1918 destinations | Private destinations denied by GUEST-IN; other unmatched sources denied |
| Office / VLAN20 | No inbound interface ACL; PAT selected for external access | Not a full destination allowlist |
| Servers / VLAN40 | No inbound interface ACL | Not a full destination allowlist |
| Management / VLAN60 | No inbound interface ACL | Not a full destination allowlist |

VTY ACL99 accepts source 192.168.60.10 only on R1/SW1/SW2. It restricts remote login, not all IP traffic to a management address.

DHCP rule uses UDP68→67 and any source to support initial discovery. Numeric equivalents: bootpc=68, bootps=67, domain=53, www=80.

Router ACLs are stateless and directional. Inbound POS/Warehouse rules also filter endpoint replies to connections initiated from other VLANs; only the listed response traffic is allowed. Admin ping is explicitly supported using echo-reply, but arbitrary admin application access to endpoints is not guaranteed.

Same-VLAN traffic bypasses router ACLs: POS-to-POS, Office-to-printer, and guest-to-guest isolation are not enforced here. Port security limits learned source MACs; it is not user identity authentication and is not protection against all MAC spoofing.

Guests are permitted more than HTTP/DNS to simulated nonprivate destinations. NAT-SOURCES selects translation; it is not a firewall. There is no inbound WAN interface ACL or stateful firewall. Do not claim protection against every unsolicited WAN packet or production payment compliance.

Single router, one trunk per uplink and one ISP are deliberate learning-lab availability limits. OSPF, EtherChannel, failover, DHCP snooping, endpoint isolation and a stateful firewall are future extensions, not implemented features.
