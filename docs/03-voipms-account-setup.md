# 03 — VoIP.ms Account Setup

This section covers the provider side: the main account, the sub-accounts the
endpoints register with, and the toll-free DID that routes to them.

## 1. Main account

Sign up at VoIP.ms and fund the account (it is prepaid/pay-as-you-go). The main
account is the billing and management identity, and it also carries its own SIP
credentials. In this deployment **one handset registers to the main account**,
and the remaining handsets register to **sub-accounts** created under it — four
phones in total.

## 2. Sub-accounts

**Main menu → Sub Accounts → Create Sub Account.**

For this deployment I created **three sub-accounts**, each authenticating with a
**username and password** and each assigned its own **internal extension
number**, so the endpoints can call each other extension-to-extension as well as
place outside calls.

| Setting | Value used | Notes |
|---------|-----------|-------|
| Authentication type | **User / Password** | Simplest for endpoints behind dynamic IPs. |
| Username | `<SUBACCOUNT_USER>` | VoIP.ms prefixes it with the main account ID. |
| Password | `<SIP_PASSWORD>` | Use a strong, unique password per sub-account. |
| Device type | IP phone | Matches the Yealink handsets. |
| Internal extension | `<EXT>` (e.g. 101, 102, 103) | For internal dialing between endpoints. |
| Allowed codecs | PCMU, G.729, Opus | See [Architecture](02-architecture.md#codecs). |

Each sub-account was connected to a **similar Yealink device**. Repeat the create
step per endpoint; keep a record of which username/extension maps to which phone.

## 3. The DID (toll-free number)

**Main menu → DID Numbers → Order DID(s) → Toll-Free.**

### 800 vs 888

I wanted a `(800)` number, but discovered that **true `(800)` numbers are almost
never available** — they are largely taken, or held and resold by third-party
brokers rather than offered by providers directly. Every toll-free prefix
(`800/888/877/866/855/844/833`) is functionally equivalent to callers, so I took
an available **`(888) XXX-XXXX`** from VoIP.ms instead.

> If a client specifically wants an `800`, set expectations early: it will likely
> mean buying from a broker at a premium, not ordering one from the provider.

## 4. Routing the DID

**Main menu → DID Numbers → Manage DID(s) → (edit the DID).**

The DID is routed **straight to a sub-account** — no IVR, ring group, or
forwarding layer in between. VoIP.ms allows **multiple sub-accounts to be wired
to the same DID**, so the number can ring more than one registered endpoint.

## 5. 911 on toll-free — important

**911 / e911 service is not offered on toll-free DIDs.** A toll-free number
cannot be used to reach emergency services. If the client needs 911, that must be
handled separately (for example, a local DID with e911 registered, or the
premises' existing lines). Make sure the client understands this limitation
before relying on the toll-free number as their phone.

## Result

- The main account plus three sub-accounts — four registrations in total, each
  with username/password credentials and an internal extension.
- One toll-free `(888)` DID routed directly to the account(s).
- Ready for an endpoint to register — continue to
  [Jami](04-softphone-jami.md) or [Yealink](05-yealink-t43u.md).
