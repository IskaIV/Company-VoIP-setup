# 07 — Troubleshooting

Issues actually encountered during this deployment, followed by general checks for
this stack (VoIP.ms + Yealink T43U, no PBX).

## Issues hit during this deploy

### Jami interface didn't match the VoIP.ms guide

The VoIP.ms wiki's softphone walkthrough was written against an older Jami
layout, so the screens and labels didn't line up with the current version. It's
intuitive enough to work through regardless — follow the *fields* (server,
username, password) rather than the exact menu path in the guide. See
[Softphone: Jami](04-softphone-jami.md) for the current flow.

### No nearby Ethernet port / router

The site had no wall Ethernet and the router/modem was too far to cable. The
fix was a **workaround, not a permanent design**: pass the network through the
client PC with Internet Connection Sharing (ICS). See
[Yealink SIP-T43U](05-yealink-t43u.md#getting-the-phone-on-the-network-internet-connection-sharing).
Because the phone then depends on the PC being on, treat this as temporary and
recommend proper cabling where uptime matters.

### Phone not getting an IP through the PC (ICS)

On first boot the Yealink sometimes failed to pull an IP over the shared
connection. Fix:

1. Open **`ncpa.cpl`** → Wi-Fi adapter → **Properties → Sharing**.
2. **Uncheck** "Allow other network users to connect through this computer's
   Internet connection" and click **OK**.
3. Re-open the same dialog, **re-check** the box, and click **OK**.
4. **Power-cycle the phone** (off and back on).

This re-initializes ICS and the phone picks up a `192.168.137.X` address.

### Web UI IP keeps changing

Unless the phone is given a **static IP**, the ICS DHCP lease can hand it a
different address, so the old gateway IP you bookmarked will throw a
connection error. Options:

- Re-check the phone's current IP on the handset (network status menu) before
  opening the web UI, **or**
- Assign the phone a **static IP** so the web UI address stays constant.

## General checks

### Account won't register

- Confirm **username, register name, and password** exactly match the VoIP.ms
  sub-account (case-sensitive).
- Confirm the **SIP server** is the correct POP server, **port 5060**, transport
  **UDP**.
- Make sure the sub-account isn't already registered on another endpoint (e.g. a
  leftover Jami test account) — deregister the old one first.
- Verify the PC/phone actually has internet (open a page from the shared
  connection).

### No incoming calls (but outbound works)

- Check the phone still shows **Registered** — registration can expire if the
  connection drops. Re-register or power-cycle.
- Confirm the **DID is routed to the right sub-account** in VoIP.ms.
- If it rings only briefly before dropping, raise the **ring timeout** (see
  [Testing](06-testing-and-call-quality.md#4-tuning-ring-timeout)).

### One-way or no audio

- Usually a **NAT / SIP ALG** problem on the router. If the client's router runs
  **SIP ALG**, disable it. (On this deploy nothing needed disabling, but other
  networks differ.)
- Confirm both ends share a **common codec** (PCMU is the safe baseline).

### Choppy or robotic audio

- Check available **bandwidth** — VoIP needs roughly 85–100 kbps per concurrent
  call with PCMU. Congested Wi-Fi upstream (as in the ICS setup) is a common
  cause.
- Fall back to **G.729** on constrained links to lower bandwidth per call.

### Can't reach the web UI

- Get the phone's **current IP** from its on-screen network menu (it may have
  changed — see above), then browse to that address.
- Make sure you're on the **same network** as the phone (the ICS host PC).

### Reminder: no 911 on toll-free

If someone reports 911 doesn't work, that's expected — **toll-free DIDs do not
support 911/e911**. See [VoIP.ms account setup](03-voipms-account-setup.md#5-911-on-toll-free--important).
