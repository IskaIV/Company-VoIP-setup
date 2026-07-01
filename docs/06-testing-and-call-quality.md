# 06 — Testing & Call Quality

With the phones registered, confirm the deployment actually works end to end:
audio in both directions, correct caller ID, and calls that ring long enough to
be answered.

## 1. Echo test

Dial **4443** on the phone to reach the VoIP.ms **echo test**. It plays your own
audio back to you — clear playback with no delay or dropouts confirms
registration, outbound calling, and two-way audio for that handset.

## 2. Inbound call

Call the toll-free DID **from a personal phone** and confirm the Yealink rings
and connects. Check that:

- **Audio is two-way** — both parties hear each other clearly.
- **Caller ID** shows the calling number.

## 3. Outbound call

**Place a call to a personal phone** from the Yealink and confirm the same:

- **Two-way audio** is clean.
- **Caller ID** presents the DID (the number, as configured).

## 4. Tuning: ring timeout

By default, an unanswered call can drop sooner than a person can realistically
get to the phone. In VoIP.ms, the **no-answer / ring timeout was increased to
longer than 60 seconds**, so calls ring long enough to be answered before they
time out.

## Result

Echo test clean, inbound and outbound calls connecting with two-way audio and
correct caller ID, and a ring timeout long enough for real-world answering. If
any of these fail, see [Troubleshooting](07-troubleshooting.md).
