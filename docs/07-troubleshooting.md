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
