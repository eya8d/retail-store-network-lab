# Retail Store Network Lab

A Cisco Packet Tracer project by **Eyad Ahmed Gaafar**, built to practice designing, configuring, securing, and troubleshooting a retail store network.

The lab connects checkout terminals, office staff, warehouse PCs, internal services, guest Wi-Fi, and a dedicated network administrator. An ISP router and external server simulate internet access entirely inside Packet Tracer.

## Network topology

![Retail store network topology](Screenshot%202026-10-28%20003924.png)

## Download and run

1. [Download the Packet Tracer project](Retail-Store-Network.pkt) using GitHub's **Download raw file** button.
2. Open it in Cisco Packet Tracer.
3. Allow links and network services to converge.
4. Use the tests below to explore allowed and blocked traffic.

The project file and topology image are included in this repository. Router configuration exports identify Cisco 2911 devices with IOS 15.1; switches are 2960-24TT devices with exported IOS 15.0. The exact Packet Tracer application version has not been recorded, so cross-version compatibility is not guaranteed.

## VLAN and addressing plan

All LANs use a /24 mask (`255.255.255.0`).

| VLAN | Department | Network | Default gateway |
|---|---|---|---|
| 10 | Checkout / POS | 192.168.10.0/24 | 192.168.10.1 |
| 20 | Office | 192.168.20.0/24 | 192.168.20.1 |
| 30 | Warehouse | 192.168.30.0/24 | 192.168.30.1 |
| 40 | Internal servers | 192.168.40.0/24 | 192.168.40.1 |
| 50 | Guest Wi-Fi | 192.168.50.0/24 | 192.168.50.1 |
| 60 | Network management | 192.168.60.0/24 | 192.168.60.1 |

| Infrastructure | Address |
|---|---|
| Internal HTTP/DNS server | 192.168.40.10 |
| Office printer | 192.168.20.10 |
| SW1 management | 192.168.60.2 |
| SW2 management | 192.168.60.3 |
| Admin-01 | 192.168.60.10 |
| Store WAN interface | 203.0.113.2/30 |
| ISP WAN interface | 203.0.113.1/30 |
| ISP external-server interface | 198.51.100.1/24 |
| External HTTP/DNS server | 198.51.100.10 |

DHCP serves POS, Office, Warehouse, and Guests. Addresses .1 through .20 are reserved from dynamic allocation in each of those networks. Infrastructure uses static addresses.

## Configuration features

- **VLANs and trunks:** six VLANs with 802.1Q trunks between switches and the store router.
- **Inter-VLAN routing:** router-on-a-stick using one subinterface per VLAN.
- **DHCP and DNS:** automatic client addressing and name resolution for internal and simulated external web services.
- **NAT/PAT:** Office and Guest clients share the store WAN address using overload.
- **Extended ACLs:** POS and Warehouse can access internal DNS and HTTP; other routed traffic is denied except explicit DHCP/ICMP allowances.
- **Guest separation:** guest traffic to private routed destinations is blocked, with exceptions for DHCP and pinging its own gateway.
- **SSH:** store router and switch remote login is restricted to Admin-01 using a VTY access list.
- **Switch protection:** sticky MAC port security, PortFast, BPDU Guard, and shutdown of unused ports.

## Services and validation

| Source | Test | Expected result |
|---|---|---|
| POS, Office, Warehouse | Open `http://portal.retail.test` | Internal portal opens |
| Office and Guests | Open `http://www.public.test` | Simulated external site opens |
| Guests | Open `http://192.168.40.10` | Blocked |
| POS and Warehouse | Ping office printer | Blocked |
| Office | Ping office printer | Allowed |
| Admin-01 | SSH to 192.168.60.1, .2, and .3 | Allowed with valid lab credentials |
| Office | SSH to those management addresses | Blocked |
| Guest client | Renew DHCP lease | Receives guest subnet, gateway, and external DNS |
| Unauthorized endpoint on secured POS port | Send traffic using a different MAC | Port-security shutdown |

Tests demonstrated during the build included internal/external HTTP and DNS, DHCP, PAT translations, SSH source restrictions, guest blocking, and a controlled port-security violation. After cleanup, the builder confirmed Warehouse portal access, Guest external web access, and blocked Guest access to the internal server. This is a learning-lab record, not an independent automated test report.

The internal server intentionally does not answer POS/Warehouse ping requests under the configured ACLs; test its permitted services using the browser instead.

## Troubleshooting lessons

**VTY range mismatch:** an access list was initially applied only to VTY15. Inspecting all VTY ranges revealed why an Office PC could still log in. Applying the source restriction across all configured VTY ranges corrected the access policy.

**Warehouse DNS:** differently spelled DHCP pool names created separate pools. The pool actually serving VLAN30 needed the DNS setting. Correcting that pool and renewing leases restored name resolution.

**Port security:** replacing an authorized endpoint triggered a violation. Recovery involved reconnecting the intended endpoint and re-enabling the interface.

## Documentation and configuration files

| Folder | Contents |
|---|---|
| [docs/](docs/) | Address plan, topology, policy, rebuild guide and troubleshooting |
| [configs/](configs/) | Configuration guide explaining historical exports versus fresh-build templates |
| [configs/observed/](configs/observed/) | Sanitized configuration snapshots supplied before cleanup |
| [configs/build/](configs/build/) | Corrected templates for fresh matching devices; includes optional management hardening |
| [evidence/](evidence/) | Original build/test screenshots and an evidence index |
| [tests/](tests/) | Acceptance checklist and historical evidence register |

The **observed** files preserve earlier mistakes for troubleshooting context. The **build** files are authored templates, not a new export from the uploaded simulation; their optional credential/console settings have not been confirmed applied to the `.pkt`. See [configuration notes](configs/README.md) and [lab credentials](docs/LAB-CREDENTIALS.md).

## Useful verification commands

Run the commands relevant to each device:

```text
show vlan brief
show interfaces trunk
show ip interface brief
show ip route
show ip dhcp binding
show access-lists
show ip nat translations
show port-security
show port-security address
```

## Scope and future improvements

This is a **single-store educational simulation**, with HTTP pages representing business services. It does not implement a real sales application or payment processing.

Router ACLs are directional and stateless. They do not filter traffic between hosts in the same VLAN. Office, Server, and Management networks do not have complete destination allowlists. PAT is not a stateful firewall. No redundancy or payment-security compliance is claimed.

Future extensions could include a second branch, OSPF, redundant uplinks, EtherChannel, and stronger endpoint isolation.
