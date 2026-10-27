# Rebuilding the lab

The quickest way to try the network is to open [Retail-Store-Network.pkt](../Retail-Store-Network.pkt).

If you want to build it yourself, here's the order I would follow.

1. Place and cable the devices using the [port map](TOPOLOGY.md).
2. Apply the matching files from [configs/build/](../configs/build/) to fresh devices. The switch files create all six VLANs.
3. Generate RSA keys on R1, SW1 and SW2 with `crypto key generate rsa`, choosing 2048 bits when prompted.
4. Configure the servers, printer, AP and Admin-01 using the settings below.
5. Set the user PCs to DHCP and connect the Guest laptops to the AP.
6. Generate traffic from the intended wired endpoints so sticky MAC learning can populate.
7. Check services, ACLs, SSH and port security before saving.

The build files include extra management settings described in [LAB-CREDENTIALS.md](LAB-CREDENTIALS.md). I wouldn't paste them over a working copy without making a backup first.

## Static devices and services

| Device | Settings |
|---|---|
| Internal server | 192.168.40.10/24; gateway 192.168.40.1; DNS itself |
| PUBLIC-WEB | 198.51.100.10/24; gateway 198.51.100.1 |
| Printer | 192.168.20.10/24; gateway 192.168.20.1 |
| Admin-01 | 192.168.60.10/24; gateway 192.168.60.1; DNS 192.168.40.10 |

Enable HTTP and DNS on both servers.

On the internal DNS server, add these A records:

- `portal.retail.test` → `192.168.40.10`
- `www.public.test` → `198.51.100.10`

On PUBLIC-WEB, add `www.public.test` → `198.51.100.10`.

These records are entered manually on each server; I didn't configure DNS forwarding.

## Guest Wi-Fi

Use SSID `Retail-Guest` and WPA2-PSK/AES where the selected AP supports it. Set a lab passphrase and use the same settings on the laptops. Connect the AP to SW1 Fa0/8, which is an access port in VLAN50.

The AP bridges traffic. DHCP comes from the store router, so don't add another DHCP server or NAT router for Guests.

## Before saving

Useful checks:

```text
show ip interface brief
show interfaces trunk
show vlan brief
show ip dhcp binding
show access-lists
show port-security address
```

Run the browser and SSH tests in the main README. For POS/Warehouse, use HTTP to check the server; their server ping requests are intentionally blocked.

After the intended MAC addresses have been learned and the tests pass:

```text
copy running-config startup-config
```

Save the .pkt under a new name, reopen it and repeat a few service and login tests. The [completed test results](../tests/validation.csv) show the checks I used; repeat them when rebuilding on new devices.
