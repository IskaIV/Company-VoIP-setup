# 02 — Architecture

## How a call flows

There is no PBX in the path. The DID lives at VoIP.ms, is routed to a
sub-account, and the endpoint registers directly to a VoIP.ms POP server.

```
   Caller dials (888) XXX-XXXX
            │
            ▼
       VoIP.ms  ── DID routed to ──►  Sub-account <SUBACCOUNT_USER>
            │
            │  rings whatever endpoint is registered to that sub-account
            ▼
   POP server <POP_SERVER>  ◄── SIP REGISTER ──  Yealink SIP-T43U
            │
            └── RTP media (voice) ──────────────► (two-way audio)
```

- **Inbound:** caller → VoIP.ms → DID routing → sub-account → the registered
  endpoint rings.
- **Outbound:** endpoint → POP server → VoIP.ms → PSTN, presenting the DID as
  caller ID.
- **Registration** is what makes the endpoint reachable: it authenticates to the
  POP server with the sub-account credentials and keeps a live binding so
  VoIP.ms knows where to send inbound calls.

## Choosing the POP server

VoIP.ms offers POP servers in many regions, and the nearest one is not always the
best one. Before committing, I **tested candidate servers and ranked them** by
response rate, registration stability, and latency, then registered the endpoint
to the top performer. The chosen server is referenced throughout as
`<POP_SERVER>`.

> Don't assume geography. Run your own tests against a few candidates and pick on
> measured stability, not just distance.

## Protocols and ports

| Function | Protocol / Port | Notes |
|----------|-----------------|-------|
| SIP signaling | **UDP 5060** | Registration and call setup. |
| RTP media | UDP (dynamic range) | The actual voice; ports negotiated per call. |

**Transport: plain UDP on 5060.** No TLS/SRTP in this deployment.

### Firewall / NAT

On this client's network, **nothing had to be disabled and no ports were
forwarded** — the phone registered and called out cleanly behind the existing
router. SIP registration keeps an outbound binding open, so inbound calls return
through the same path without manual port forwarding.

> This won't always be the case. Some routers run SIP ALG or aggressive NAT that
> breaks SIP; see [Troubleshooting](07-troubleshooting.md) if registration drops
> or audio is one-way.

## Codecs

| Codec | Use | Notes |
|-------|-----|-------|
| **PCMU** (G.711 µ-law) | Primary | Standard North American narrowband; reliable, ~64 kbps. |
| **G.729** | Low-bandwidth fallback | Compressed, lower bandwidth at some quality cost. |
| **Opus** | HD audio | Configured wideband — **Opus-WB, 16 kHz** sample rate. |

PCMU guarantees compatibility, G.729 covers constrained links, and Opus-WB
provides HD voice where both ends support it.
