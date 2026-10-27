# Test screenshots

These are screenshots I took while building and troubleshooting the lab. Some show a fault on purpose, such as the port-security shutdown and the DHCP pool spelling mismatch.

Start with [the full topology](../docs/topology.png).

| Screenshot | What it shows |
|---|---|
| [Internal portal](internal-portal-office.png) | Office opening portal.retail.test |
| [External site by name](guest-public-web-by-name.png) | Guest HTTP and DNS working together |
| [PAT translations](pat-office-and-guests.png) | Office and Guest clients sharing 203.0.113.2 |
| [SSH denied](office-ssh-denied.png) | Office rejected by the store router and switches |
| [Guest blocking](guest-internal-access-blocked.png) | Gateway reachable, internal destinations blocked |
| [Learned MAC](port-security-learned-mac.png) | One sticky MAC on SW1 Fa0/1 |
| [Port-security violation](port-security-violation.png) | A different endpoint triggering Secure-shutdown |
| [SW1 summary](sw1-port-security-summary.png) | Learned addresses on seven secured ports |
| [SW2 summary](sw2-port-security-summary.png) | Six addresses learned at that point; Admin-01 hadn't sent traffic yet |
| [Warehouse DNS issue](warehouse-dhcp-name-incident.png) | Similar pool names split across separate DHCP entries |

The later SW2 export contains Admin-01's sticky MAC too. After cleanup I repeated the Warehouse portal and Guest allow/block checks. I haven't captured a replacement screenshot for every test in the checklist.
