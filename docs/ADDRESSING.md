# Address plan

All LAN masks: 255.255.255.0 (/24), with .0 network and .255 broadcast.

| VLAN | Role | Network | Gateway | Allocation / DNS |
|---|---|---|---|---|
| 10 | POS | 192.168.10.0/24 | 192.168.10.1 | POS pool, .21–.254; DNS 192.168.40.10 |
| 20 | Office | 192.168.20.0/24 | 192.168.20.1 | OFFICE pool, .21–.254; DNS 192.168.40.10 |
| 30 | Warehouse | 192.168.30.0/24 | 192.168.30.1 | WEARHOUSE pool, .21–.254; DNS 192.168.40.10 |
| 40 | Servers | 192.168.40.0/24 | 192.168.40.1 | Static internal server .10 |
| 50 | Guests | 192.168.50.0/24 | 192.168.50.1 | GUESTS pool, .21–.254; DNS 198.51.100.10 |
| 60 | Network management | 192.168.60.0/24 | 192.168.60.1 | SW1 .2; SW2 .3; Admin-01 .10, static |

DHCP excludes .1–.20 and starts dynamic allocation at .21. The working Warehouse pool keeps its original name, WEARHOUSE.

| External segment | Device | Address |
|---|---|---|
| Store–ISP 203.0.113.0/30 | R1 G0/1 | 203.0.113.2/30 |
| Store–ISP | ISP G0/0 | 203.0.113.1/30 |
| Public server segment 198.51.100.0/24 | ISP G0/1 | 198.51.100.1/24 |
| Public server segment | PUBLIC-WEB | 198.51.100.10/24; gateway .1 |

Printer: 192.168.20.10/24, gateway 192.168.20.1. Server, printer and AP settings are configured in their Packet Tracer windows rather than in the IOS exports. The external addresses represent documentation networks in a closed simulation, not real public connectivity.
