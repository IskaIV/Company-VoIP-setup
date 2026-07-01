# 08 — Lessons Learned

Field notes from the deployment — time, cost, and what to do differently next
time.

## Time

The job took **three days**. That was longer than the hands-on work strictly
required, and the reasons are worth stating:

- **First time doing a VoIP deployment.** I needed to research the landscape
  before committing the client to anything.
- **Provider evaluation for the client.** Part of the work was comparing VoIP
  options and presenting the trade-offs (e.g. VoIP.ms vs. turnkey services like
  Ooma or Dialpad) so the client could make an informed choice.
- **Discovery, planning, and execution as distinct phases** — see the day-by-day
  breakdown in [Overview](01-overview.md).

A second, similar deployment would be substantially faster now that the research
and a repeatable plan exist.

## Cost

- **Handsets:** the Yealink SIP-T43U runs roughly **$100–150 each new** — but the
  **client already owned theirs**, so there was no hardware spend on this job.
- **Service:** about **$20 to initially fund the VoIP.ms account**, then
  **pay-as-you-go** from there (per-minute usage plus the monthly DID fee).

For a small setup like this, the ongoing cost is minimal compared to turnkey
per-seat plans — one of the main reasons VoIP.ms made sense.

## What I'd do differently / gotchas

- **Bring USB-to-Ethernet adapters.** The biggest surprise was having no wall
  Ethernet and no reachable router, which forced the Internet Connection Sharing
  (ICS) workaround. Next time I'll carry USB-to-Ethernet adapters so I can set up
  ICS (or a direct link) on the spot when cabling isn't available.
- **Treat ICS as temporary.** Passing the network through the client PC works,
  but the phone is only reachable while that PC is on. Where uptime matters,
  recommend proper cabling or a dedicated network path.
- **Set a static IP on the phone.** Under ICS the phone's address can change,
  breaking the bookmarked web UI. A static IP keeps the management address
  stable.
- **Don't promise an 800 number.** True `(800)` numbers are effectively
  unavailable — taken, or resold by brokers at a premium. Set client
  expectations early; any toll-free prefix (`888`, `877`, etc.) works the same
  for callers.
- **Toll-free has no 911.** Communicate up front that emergency calling isn't
  available on a toll-free DID, and plan a separate path if the client needs it.
- **Follow fields, not screenshots.** The VoIP.ms wiki lagged the current Jami
  UI. Vendor docs drift — match the required fields (server, username, password)
  rather than the exact menu layout.
- **Test POP servers; don't assume nearest.** Measuring response rate and
  stability across candidates paid off — the best server isn't always the
  closest one.
- **Keep a credential/extension map.** With multiple sub-accounts and handsets,
  record which username/extension maps to which phone before you start — it saves
  confusion during provisioning and later support.
- **Change default phone credentials immediately.** The first web-UI action on
  each Yealink should be replacing the factory login.
- **Verify each sub-account before touching hardware.** Proving the service with
  a free softphone (Jami) first isolated provider-side issues from phone-side
  ones and made the hardware step predictable.
