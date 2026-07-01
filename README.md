# Company VoIP Setup

A field guide to standing up a client's Voice over IP (VoIP) phone service with
**VoIP.ms** and deploying **Yealink SIP-T43U** desk phones — no PBX required.

VoIP.ms is a cloud SIP provider (ITSP): you create a **sub-account** (a set of
SIP credentials) and route a **DID** (phone number) to it, then register an
endpoint directly to a VoIP.ms POP server using those credentials. A **Jami**
softphone was used as a temporary endpoint to confirm the service worked, then
removed once the Yealink was in place.

> Identifiers in this guide are placeholders. Replace values such as
> `<SUBACCOUNT_USER>`, `<SIP_PASSWORD>`, `<POP_SERVER>`, and `(888) XXX-XXXX`
> with your real values.

## Architecture at a glance

```
            PSTN / caller dials (888) XXX-XXXX
                          │
                ┌─────────▼──────────┐
                │      VoIP.ms       │   Cloud SIP provider (ITSP)
                │  POP: <POP_SERVER> │   DID routed to sub-account
                └─────────┬──────────┘
                          │  SIP register (UDP 5060) + RTP media
              ┌───────────┴────────────┐
              │                        │
     ┌────────▼─────────┐    ┌─────────▼─────────┐
     │ Yealink T43U ×3  │    │   Jami softphone  │
     │  (permanent)     │    │   (temp / test)   │
     └──────────────────┘    └───────────────────┘
```

## Contents

| # | Document | Purpose |
|---|----------|---------|
| 01 | [Overview](docs/01-overview.md) | The client, the goal, the stack, and why this design |
| 02 | [Architecture](docs/02-architecture.md) | Call flow, POP server selection, ports, and codecs |
| 03 | [VoIP.ms account setup](docs/03-voipms-account-setup.md) | Sub-accounts, toll-free DID, routing, and 911 caveat |
| 04 | [Softphone: Jami](docs/04-softphone-jami.md) | Temporary SIP test endpoint and the echo test |
| 05 | [Yealink SIP-T43U](docs/05-yealink-t43u.md) | Internet Connection Sharing, web UI registration, and codecs |
| 06 | [Testing & call quality](docs/06-testing-and-call-quality.md) | Echo test, inbound/outbound checks, and ring-timeout tuning |
| 07 | [Troubleshooting](docs/07-troubleshooting.md) | ICS/IP issues, registration, one-way audio, and more |
| 08 | [Lessons learned](docs/08-lessons-learned.md) | Time, cost, gotchas, and what to do differently |
