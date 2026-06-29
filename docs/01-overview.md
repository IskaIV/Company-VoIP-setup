# 01 — Overview

## The client and the goal

The client is an AI systems provisioning company that wanted a professional phone
presence: a toll-free number answered on a physical **desk phone**. They
specifically asked for a deskphone rather than a software-only setup, so the
end state is a single **Yealink SIP-T43U** handset reachable at the client's
toll-free DID.

This was a **solo deployment that spanned three days of work**:

- **Day 1 — Discovery.** Determine the client's needs and define what the
  solution had to do.
- **Day 2 — Research and planning.** Research the provider and hardware, test
  POP servers for response rate and stability, and draw up the step-by-step plan
  to follow.
- **Day 3 — On-site setup.** Set up the account, order the number, configure the
  service, provision the phone, and verify calls.

## The stack

| Layer | Choice | Role |
|-------|--------|------|
| SIP provider (ITSP) | **VoIP.ms** | Supplies the DID and the SIP credentials (sub-accounts) endpoints register to. Pay-as-you-go. |
| Phone number (DID) | Toll-free `(888) XXX-XXXX` | Public-facing inbound number, routed to a sub-account. |
| Test softphone | **Jami** (SIP-account mode) | Temporary endpoint to confirm the service worked; removed afterward. |
| Desk phone | **Yealink SIP-T43U** | The permanent endpoint, provisioned manually via its web UI. |

## Why this design

- **No PBX to maintain.** VoIP.ms is a "bring your own device" SIP provider —
  endpoints register *directly* to a POP server with SIP credentials. For one
  number and one desk phone, a PBX (Asterisk/FreePBX/3CX) would be pure overhead
  with nothing to maintain it for.
- **Why VoIP.ms.** Great rates and a lot of customizability. Setup and operation
  are more complex than turnkey services like Ooma or Dialpad, but if you have
  the experience and can navigate wiki-style documentation, the control and cost
  make it a strong choice.
- **Why Jami for testing.** Before touching the hardware, the service needs to be
  proven. Jami was ideal as a throwaway test endpoint: free, requires no personal
  information, runs on nearly every platform, and has the fewest restrictions of
  the common options. It registered to the VoIP.ms sub-account, confirmed the DID
  and credentials worked, then was removed once the Yealink took over.
- **Why the Yealink SIP-T43U.** The client wanted a desk phone. The T43U is a
  business-class SIP handset with a clear web interface, so **manual
  provisioning** of a single unit is quick and predictable. (Auto-provisioning
  wins at fleet scale, but is unnecessary for one phone.)

## Scope

One DID with direct endpoint registration. Ring groups, IVRs/auto-attendants,
call queues, and auto-provisioning servers are supported by VoIP.ms but are out
of scope for this deployment yet.

## Terminology

- **ITSP** — Internet Telephony Service Provider (here, VoIP.ms).
- **DID** — the phone number (Direct Inward Dialing).
- **Sub-account** — a set of SIP credentials under your VoIP.ms account that an
  endpoint registers with.
- **POP server** — the regional VoIP.ms server an endpoint registers to (the SIP
  registrar/proxy), e.g. `<POP_SERVER>`.
- **Registration** — an endpoint authenticating to the POP server and announcing
  it is reachable.
