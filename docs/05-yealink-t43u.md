# 05 — Yealink SIP-T43U

The Yealink SIP-T43U handsets are the permanent endpoints. Each is provisioned
**manually through its web interface** and registered to its own VoIP.ms
sub-account. There are three phones; the steps below are repeated once per phone.

## Getting the phone on the network (Internet Connection Sharing)

The client site had **no Ethernet ports in the walls**, and the router/modem was
too far to run a cable. The workaround was to pass the network **through the
client PC** to the phone using Windows **Internet Connection Sharing (ICS)** over
a USB-to-Ethernet adapter.

> **Caveat:** with ICS, the phone's connectivity depends on the host PC being
> powered on and connected. It works, but it is not ideal for a phone that needs
> to be reachable at all times — flag this to the client (see
> [Lessons learned](08-lessons-learned.md)).

### Steps

1. Connect an Ethernet cable from the PC's USB-Ethernet adapter to the Yealink's
   **Internet port** — **NOT** the **PC port**. (The T43U has both; the Internet
   port is the uplink.)
2. On the PC, press **Windows + R**, type **`ncpa.cpl`**, and press Enter to open
   **Network Connections**.
3. You should see two adapters: the **Wi-Fi adapter** (the PC's internet) and the
   **USB-Ethernet adapter** (the link to the phone).
4. Right-click the **Wi-Fi adapter → Properties → Sharing** tab.
5. Check **"Allow other network users to connect through this computer's Internet
   connection"**, then click **OK**.
6. Plug the Yealink into power and wait for **"Obtaining IP Address"** to finish,
   then press **OK** on the phone.
7. If sharing is working, the phone's IP will be in the ICS range —
   **`192.168.137.X`**.
