# Mobile ID
### Designing identity verification around an API that wouldn't bend

---

**Product** — DottedSign, a B2B e-signature platform by Kdan Mobile
**My role** — Product Designer. Flow design, interaction rules, failure-state design,
functional spec. Visual execution by our UI designer.
**Team** — PM, product designer (me), UI designer, front-end, back-end, QA
**Integration** — TWCA (Taiwan-CA), the national certificate authority
**Status** — Shipped

---

## Why this feature existed

An electronic signature is only worth as much as the certainty that the right person
made it. Everything else in the product — the audit trail, the tamper-proofing, the
legal weight — rests on that.

Email links and passwords don't establish identity. They establish access to an inbox.

Kdan had a roadmap for stronger identity verification, and Mobile ID was the first
piece I designed. It verifies a signer through their mobile carrier: the phone number,
the SIM, and the national ID number are checked against carrier records, confirming a
real, specific person is holding the device.

I later designed a second method on the same roadmap using Taiwan's mobile Citizen
Digital Certificate.

## The constraint that decided the entire design

TWCA's API does one thing when verification succeeds: it stamps a digital certificate
onto the signed document.

Stamping the certificate seals the document. Nothing about it can change afterwards.

That single fact removed most of my design space. Verification could not happen when
the signer opens the task, and it could not happen before they place their signature —
either would seal a document that wasn't finished yet.

Verification had to be the last thing that happens, immediately after the signer
submits.

I've come to think this is the most honest kind of design problem. There was no user
research that would have changed this. The work was to accept the constraint and then
make sure the signer never had to feel it.

## Design decisions

### 1. Split the feature across two moments, for two different people

Mobile ID isn't one screen. It's a requirement set by one person and satisfied by
another, days apart.

**At task creation**, the initiator marks a signer as requiring Mobile ID verification,
and can optionally pre-fill that signer's phone number. Pre-filling tightens the
requirement: only the person holding that number can complete the signature.

**At signing**, the signer completes verification.

### 2. Make the phone number optional, because initiators often don't know it

The strict version of this feature would require the initiator to specify the phone
number every time. It's more secure, and it's what the security model wants.

But initiators frequently don't have that information. A contract goes to a counterparty
whose email you know and whose mobile number you don't. Requiring it would have made the
feature unusable in exactly the external-party scenarios where identity verification
matters most.

So the field is optional, and it changes what the signer sees:

- **Number pre-filled** → shown to the signer, locked, not editable. They must verify as
  that specific person.
- **Number left blank** → the signer enters their own. The verification still proves a
  real person with a real ID is signing; it just doesn't pin down *which* person in
  advance.

Two security levels, one field, and the initiator picks based on what they actually know.

### 3. Use a QR code to move the signer to the phone — and skip it when they're already there

Mobile ID needs the phone. Most enterprise signing happens on a desktop.

On submit, a modal presents a QR code. The signer scans it and continues on their phone.

If they were signing on a mobile browser to begin with, there is no QR code. The
verification screen opens directly.

Same feature, two paths, and neither one makes the user think about which one they're on.

### 4. Keep the signer inside our product

The obvious way to build this is to redirect the signer to TWCA's own verification page.
It's less work, and it's what most integrations do.

We didn't. Our back end calls TWCA's API, and every screen the signer sees is ours.

That decision mattered more than it sounds. A redirect to an unfamiliar government-adjacent
page, in the middle of signing a legal document, is exactly where people stop and ask
whether they've been phished. Owning the screens meant I could keep one voice, one
visual language, and one explanation of what was happening, from the moment they hit
submit to the moment the task advanced.

### 5. Tell people to turn off Wi-Fi — and explain why

Carrier verification only works over the mobile data network. If the phone is on Wi-Fi,
it fails.

This is an absurd thing to ask a user, and it's non-negotiable. So the instruction sits
in the verification screen before they submit anything, with the reason attached rather
than as a bare command. A user who understands *why* Wi-Fi breaks it will turn it off.
A user told only *that* it breaks it assumes the product is broken.

### 6. Design the failure chain, not just the success path

Wrong digits are the most common failure, and they're recoverable. Everything else isn't.

- **Attempts 1–5** — the signer can correct their input and retry
- **After 5 failures** — verification locks for 24 hours
- **The task moves to pending**, rather than failing or expiring
- **The initiator is emailed** that their task is on hold

That last step is the one I'd defend hardest. Without it, a locked-out signer produces
a task that silently stops moving, and the initiator finds out days later when they
chase it. Routing the failure to the person who can actually resolve it — by calling
the signer, or by switching them to a different verification method — turns a dead end
into a handoff.

For system-level errors, the screen shows an error code the signer can bring to support,
instead of a generic apology that gives our team nothing to work with.

### 7. Decide what the system should *not* solve

Some signers don't have Mobile ID at all.

I chose not to build a system path for this. There's no in-product fallback, no
alternative-method picker at the point of failure.

If a signer can't verify this way, the right resolution is a conversation with the
initiator, who can reissue the task with a different method. Building an in-flow
alternative would have meant letting the signer downgrade the security level the
initiator deliberately chose — which is the one thing the feature exists to prevent.

Adding a screen there would have looked more thorough and been worse.

## Outcome

The feature shipped and worked. Sales reported that clients found the flow clear and
easy to follow in demos — which, for a verification step involving a QR handoff, a
carrier network requirement, and a national ID, was the bar.

Adoption was low.

## What I take from it

Mobile ID was a paid add-on, positioned as an upgrade available to any customer. Most
customers didn't take it.

The easy read is that the market wasn't ready — and there's something to that. Plenty of
users didn't feel they needed a digital certificate on their documents at all. Security
maturity in the market genuinely lagged behind what the product could offer, and building
ahead of that curve was a deliberate strategic bet, not an accident.

But that framing lets the design off too easily. There *was* a group who needed this
urgently — regulated industries, finance, anything with a real impersonation risk. We
built the right thing and then offered it to everyone equally, as a line item on a
pricing page, instead of aiming it at the people whose problem it solved.

Being early to a need is a defensible strategy. It only pays off if you find the
minority who have that need today. We designed the feature well and never did that
second part.
