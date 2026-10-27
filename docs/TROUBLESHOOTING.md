# Troubleshooting journal

## Incident 1 — Office could SSH to SW2

- Symptom: an Office PC established an SSH session to SW2 despite the Admin-only requirement.
- Evidence: historical SW2 VTY output had access-class only under `line vty 15`; ranges 0–4 and 5–14 lacked it.
- Cause: selecting a single line rather than all intended lines.
- Fix: apply login local, SSH-only transport and ACL99 under `line vty 0 15`.
- Subsequent evidence: Office SSH rejection screenshots; final supplied exports contain ACL99 on ranges 0–4 and 5–15 of both switches.
- Final acceptance: new SSH sessions succeed from Admin-01 and fail from Office on all three store devices.

## Incident 2 — Warehouse received no DNS

- Symptom: a Warehouse PC received 192.168.30.x and gateway .30.1, but DNS was 0.0.0.0.
- Cause: DNS was added under differently spelled pool names rather than the pool containing network 192.168.30.0/24.
- Resolution: update **WEARHOUSE** and renew the endpoint lease. The supplied final router export includes DNS under the working pool. Extra pools still remain in the supplied export and have proposed removals.
- Acceptance: both Warehouse clients receive DNS 192.168.40.10 and open portal.retail.test.

## Exercise — Port security

Historical evidence showed Secure-up with the intended sticky MAC, followed by Secure-shutdown after a different device transmitted. Restore the original endpoint before shutdown/no shutdown. Final static exports contain sticky bindings, but the running status/counters still require evidence.

## Controlled extra exercise — Missing VLAN on trunk

Do this only in a separate copy of the working `.pkt`. Capture baseline portal access first. On SW1's inter-switch uplink G0/2, remove VLAN30 from the allowed list. Test a Warehouse client's gateway and portal; inspect access VLAN, trunk allowlist, and interface state to locate the fault. Restore VLAN30 on that same trunk and repeat tests. Revert the experiment entirely before final release.

For every future incident record: date, symptom, affected scope, hypothesis, diagnostic commands, actual root cause, minimal fix, before/after evidence, and prevention. Do not mark an exercise completed until you execute it.
