# The Editor Role
### Adding flexibility to a system built on immutability

---

**Product** — DottedSign, a B2B e-signature platform by Kdan Mobile
**My role** — Product Designer. Problem definition, user flows, wireframes, interaction
rules, functional spec. Visual execution was handled by our UI designer; I owned the
behaviour and the rules.
**Team** — PM, product designer (me), UI designer, front-end, back-end, QA
**Timeline** — About one month from design through release
**Status** — Shipped

---

## The system I was designing inside

E-signature products rest on one promise: once a document is signed, nothing about it can change. That promise is what gives the signature legal weight.

DottedSign enforced it strictly. Once a signing task was created, its documents were frozen — no replacements, no additions, no edits to signature fields.

That constraint was correct. It was also blocking a sale.

## The trigger

A large Japanese real estate client told us they would not buy the product without the ability to change documents after a task had been created.

It would have been easy to treat this as one client's custom requirement. When I looked at how they actually worked, it wasn't.

## What was actually happening

In their offices, the person who creates a signing task is an administrative assistant. Their job is to start the process. But they are not the person who knows which contracts a given deal requires — that's the case handler, further down the chain.

The client told us this pattern covered around 70% of their signing volume.

DottedSign's model assumed the initiator was also the person who understood the deal: you create a task, you attach the right documents, you send it. For this client — and for any organization where administrative work and domain knowledge sit with different people — that assumption was wrong.

So the problem wasn't "let people edit documents." It was:

> **How do you let a second person correct a task already in flight, without breaking the immutability the product's legal standing depends on?**

## Constraints I was working within

1. **Legal** — anything already signed must remain untouched, and every change must be accountable
2. **Technical** — the existing document-upload framework couldn't support inserting an editing step at arbitrary points in a flow
3. **Commercial** — feature access in DottedSign is tied to account plan tier
4. **Time** — the deal had a deadline

## Design decisions

### 1. Model the Editor as a stage, not a permission layer

DottedSign tasks were already a sequence of stages: signer, then signer, then CC recipient. Users understood that model.

Rather than introduce a new concept — an "edit mode," a permissions panel — I made Editor a stage type, assigned the same way you assign a signer.

Nothing new to learn. If you knew how to build a signing flow, you already knew how to put an editor in one.

### 2. Inherit the initiator's permissions, not the editor's own

This is the decision I spent the most time on.

DottedSign gates features by account plan. So: if the initiator is on a plan that includes advanced field types and the editor is on a lower plan, what can the editor do?

If the editor works under their own plan, they can't fully correct the task — they might be unable to recreate a field the initiator placed. The task becomes unfixable, which defeats the entire feature.

So the editor operates with **the initiator's permissions**, for that task only. The editor is acting on the initiator's behalf, so they get the initiator's capabilities.

This also made the next decision possible.

### 3. Don't gate who can be an editor

I chose not to restrict editor assignment to internal, account-holding users. Any email address can be assigned, the same as an external signer.

This is a deliberate transfer of risk. The initiator is already trusting this person with their documents. The product's job is to make that choice explicit and traceable — not to make it for them.

### 4. Forward-only authority

An editor can only modify stages that come after their own. They cannot touch anything that has already happened.

This is what keeps the legal promise intact while still allowing correction. The past is settled; the future is still editable.

### 5. Show the lock, don't just enforce it

Already-signed documents stay visible to the editor — they need that context to judge what's missing — but in the document management dialog they render greyed out and are not selectable for any add, edit, or delete action.

The boundary is visible before it's hit, rather than surfacing as an error after.

### 6. Every change goes in the audit trail

The editor receives an email telling them a task is waiting for their review. Every modification they make — documents added, signers changed, stages inserted — is written to the task's audit trail, attributed to them.

The result: a task can now be corrected mid-flight, and anyone reading the record afterwards can see exactly who changed what, and when.

## The trade-off I made

I wanted editors to be assignable at any stage — first, middle, or last.

The document-upload framework couldn't support that within the deadline. I laid out the options and their costs, and the PM made the call: ship first-stage-only, which covered the client's actual scenario, and revisit later.

I still think that was the right decision for the timeline. It's also the part of the design I'd most want to finish.

## Outcome

The feature shipped and ran without incident. The client's workflow was unblocked and the deal closed.

It was also used almost exclusively by that one client.

## What I'd do differently

**We never showed the design to the client who asked for it.**

The team's framing was that this was our product decision, not contract work — and validating with them would have undercut that framing. It protected how we saw ourselves, and it cost us the only real feedback loop we had available.

**We solved a structural problem without checking whether the structure was common.**

The gap I found — that the person who starts a signing workflow often isn't the person who understands it — was, I believe, real well beyond this one company. But I never tested that. I designed for it and moved on.

Recognizing an insight and demonstrating it has a market are two different pieces of work. I did the first one.
