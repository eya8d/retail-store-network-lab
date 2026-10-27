# Retail Store Network Lab

I built this network in Cisco Packet Tracer to practise the networking topics I'd studied in a setting with clear requirements: a retail store with six checkout PCs, an office, a warehouse, an internal server and guest Wi-Fi.

My aim was to make the right services reachable from each department, then check that traffic which should be blocked really was blocked.

![Retail store topology](docs/topology.png)

## Try the lab

Download [Retail-Store-Network.pkt](Retail-Store-Network.pkt) and open it in Cisco Packet Tracer. Give the links a moment to come up.

- From a POS or Warehouse PC, open `http://portal.retail.test`.
- From an Office PC or Guest laptop, open `http://www.public.test`.
- From a Guest laptop, try `http://192.168.40.10`. It should be blocked.
- From an Office PC, try SSH to `192.168.60.3`. Then repeat from Admin-01 with the lab account.

The ISP and public website are simulated inside Packet Tracer. They don't connect to the real internet.

## How I split the network

| VLAN | Department | Subnet | Gateway |
|---|---|---|---|
| 10 | POS / checkout | 192.168.10.0/24 | 192.168.10.1 |
| 20 | Office | 192.168.20.0/24 | 192.168.20.1 |
| 30 | Warehouse | 192.168.30.0/24 | 192.168.30.1 |
| 40 | Servers | 192.168.40.0/24 | 192.168.40.1 |
| 50 | Guest Wi-Fi | 192.168.50.0/24 | 192.168.50.1 |
| 60 | Network management | 192.168.60.0/24 | 192.168.60.1 |

I used /24 networks to keep the addressing easy to follow while working on VLANs and access rules. It's more space than this store needs; using VLSM would be a useful follow-up.

Two 2960 switches connect the store devices. A 2911 router handles inter-VLAN routing through 802.1Q subinterfaces. A second 2911 represents the ISP.

## What I configured

DHCP supplies addresses to POS, Office, Warehouse and Guests. The internal server provides DNS and an HTTP page representing the store portal. Guests use a separate simulated external DNS server.

Office and Guest traffic uses PAT to share `203.0.113.2`. POS and Warehouse can reach DNS and HTTP on the internal server, but can't browse the external site or access the other departments through the router.

I restricted SSH on the store router and switches to Admin-01. On wired endpoint ports, I added sticky MAC port security, PortFast and BPDU Guard, and disabled unused ports.

The [address plan](docs/ADDRESSING.md), [port map](docs/TOPOLOGY.md) and [access rules](docs/POLICY.md) have the details.

## Problems I ran into

**Office could still log in to SW2.** I'd applied the source ACL to VTY15 rather than the full range. Checking the running configuration made the mistake clear. I applied the settings to all the configured VTY lines, then repeated the allowed and denied SSH tests.

**Warehouse PCs received an IP but no DNS.** I had several differently spelled DHCP pool names. The DNS setting was in a different pool from the one serving VLAN30. I corrected the working pool and renewed the leases. I kept the original spelling, `WEARHOUSE`, while cleaning up the unused pools.

**A different PC shut down a checkout port.** This was my port-security test: learn the intended MAC, connect a different endpoint and send traffic. The port entered Secure-shutdown. I reconnected the original PC and used shutdown/no shutdown to recover it.

I've kept the [troubleshooting notes](docs/TROUBLESHOOTING.md) and [screenshots](evidence/README.md) alongside the project.

## Checks I used

| Check | Expected result |
|---|---|
| POS / Warehouse → internal portal by name | Opens |
| Office / Guest → external website by name | Opens |
| Guest → internal server | Blocked |
| POS / Warehouse → Office printer | Blocked |
| Office → Office printer | Reachable |
| Admin-01 → store device SSH | Allowed |
| Office → store device SSH | Blocked |
| Different MAC on secured POS port | Port shuts down |

The screenshots cover the tests I captured during the build. After the cleanup I also repeated Warehouse portal access, Guest external browsing and Guest access blocking. The [test checklist](tests/validation.csv) is there for a full repeat run; I haven't filled every row.

A failed ping to the internal server from POS or Warehouse is expected: their ACLs allow DNS and HTTP, not ICMP echo to the server.

## Finding your way around

- [configs/observed/](configs/observed/) — my earlier configuration exports, with password hashes removed.
- [configs/build/](configs/build/) — cleaned configurations for rebuilding on fresh devices. These include extra management settings; see the [configuration notes](configs/README.md) before using them.
- [docs/](docs/) — addressing, topology, access rules and rebuild notes.
- [evidence/](evidence/) — screenshots of services, ACL tests, PAT and port security.
- [tests/](tests/) — the test checklist and screenshot register.

For a rebuild, start with [REBUILD.md](docs/REBUILD.md). The router exports show IOS 15.1 and the switch exports show IOS 15.0. I still need to record the exact Packet Tracer application version.

## What I'd improve next

I'd like to add a second branch, use OSPF between sites, and test redundant uplinks with EtherChannel.

For now, this is a single-store lab. The ACLs are stateless and don't isolate hosts within the same VLAN. There is no stateful edge firewall or link redundancy. The portal is a test page rather than a sales application.

## License

The lab files and documentation are available under the [MIT License](LICENSE).

— Eyad Ahmed Gaafar
