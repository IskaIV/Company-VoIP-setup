# Company VoIP Setup

A field guide to standing up a client's Voice over IP (VoIP) phone service with
**VoIP.ms** and deploying a **Yealink SIP-T43U** desk phone — no PBX required.

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
     │  Yealink T43U    │    │   Jami softphone  │
     │  (permanent)     │    │   (temp / test)   │
     └──────────────────┘    └───────────────────┘
```

## Contents

| # | Document | Purpose |
|---|----------|---------|
| 01 | [Overview](docs/01-overview.md) | The client, the goal, the stack, and why this design |
