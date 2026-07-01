# 04 — Softphone: Jami (temporary test endpoint)

Before provisioning any hardware, the VoIP.ms service and each sub-account need to
be proven working. **Jami** served as a throwaway SIP endpoint for exactly that —
it is free, needs no personal information, runs on nearly every platform, and has
the fewest restrictions of the common softphones. Each sub-account was tested on
its own Jami account, then the accounts were removed once the Yealinks took over.

> Jami is primarily a peer-to-peer app, but it includes a standard **SIP
> account** type. That is the only mode used here — no Jami-network features are
> involved.

## 1. Add a SIP account

1. Open Jami.
2. On the account-creation screen, click **Advanced Option**.
3. Choose **[Add a SIP Account]**.
4. Enter the sub-account credentials:

   | Field | Value | Notes |
   |-------|-------|-------|
   | Server | `<POP_SERVER>` | The POP server chosen in [Architecture](02-architecture.md). |
   | Proxy | *(leave empty)* | Not needed for direct registration. |
   | Username | `<SUBACCOUNT_USER>` | The VoIP.ms sub-account. |
   | Password | `<SIP_PASSWORD>` | The sub-account's SIP password. |

5. Set a profile name so each test is identifiable — `TEST_ACCOUNT_1`,
   `TEST_ACCOUNT_2`, `TEST_ACCOUNT_3`.

**One account per device.** Because each Yealink gets its own sub-account, each
sub-account was set up and tested on a separate Jami account, one at a time.

No codec, SRTP, or other tweaks were required — Jami registers with sensible
defaults and the setup is deliberately simple.

## 2. Set outbound caller ID

From the main dashboard: **Settings → Account → Advanced options → Outbound
Caller ID**, and set it to the DID. Voicemail was intentionally left unconfigured
for these tests.

## 3. Verify with the echo test

With the account registered, dial **4443** to reach the VoIP.ms **echo test**.
It plays back your own audio — if you hear yourself clearly with no delay or
dropouts, registration, outbound calling, and two-way audio are all working for
that sub-account.

## 4. Remove the account

Once the sub-account is confirmed, delete the Jami account and repeat for the
next device. After all three are verified, Jami has done its job and is removed
entirely — the permanent endpoints are the Yealink handsets in
[section 05](05-yealink-t43u.md).
