# Evidence index

These are original screenshots supplied during the build. They prove the specific historical observations listed below; they do not prove a retest after the corrected configuration files in this package. No images were generated or edited.

| File | Observation | Boundary |
|---|---|---|
| internal-portal-office.png | Office browser opened portal.retail.test | Office only, historical |
| guest-public-web-by-name.png | Guest browser opened www.public.test | DNS/HTTP path, historical |
| pat-office-and-guests.png | NAT table shows Office and Guests sharing 203.0.113.2 | Established translations at capture time |
| office-ssh-denied.png | Office SSH attempts to store router and both switches rejected | Pair with final Admin success retest |
| guest-internal-access-blocked.png | Guest gateway responds; internal probes rejected | Specific tested destinations only |
| port-security-learned-mac.png | Secure-up, one sticky MAC on SW1 Fa0/1 | Before violation |
| port-security-violation.png | Secure-shutdown, violation count 1 | Controlled test, not final restored state |
| sw1-port-security-summary.png | Seven secure ports with learned addresses | Historical summary |
| sw2-port-security-summary.png | Six learned addresses; Fa0/7 had zero | Later supplied running-config includes Fa0/7 sticky MAC; final live state still needs test |
| warehouse-dhcp-name-incident.png | Working network pool and mistakenly named DNS-only pools | Troubleshooting evidence, not fixed-state proof |

`topology-final.png` is a copy of the final full topology image already uploaded to the repository root. The original root image is preserved.

The builder later reported successful post-cleanup Warehouse portal access, Guest external web access, and denied Guest access to the internal server. These confirmations are recorded in the main README; the older screenshots above retain their historical labels.
