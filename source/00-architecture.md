# Portfolio Architecture — Ling-Lian Wang

## Positioning

**Product Designer / UX Designer** (not UI Designer — per your decision)

The single most important thing this portfolio must communicate in the first five seconds:

> **You are not a typical co-op applicant.** You have three years shipping a real B2B SaaS product with enterprise clients. Most of the pool has coursework.

Everything else is supporting evidence.

## Site structure

```
/                     Home
/work/editor-role     Case 1 — The Editor Role (flagship)
/work/mobile-id       Case 2 — Mobile ID verification
/work/synastry        Case 3 — Synastry dating app (in progress)
/about                About
resume.pdf            Download
```

## Home page

**Hero**

> **Ling-Lian Wang**
> Product Designer
>
> I spent five years in Taiwan building software for enterprises — two as a system
> analyst on HR and payroll systems, three designing DottedSign, a B2B e-signature
> platform. My work is consistently in products where being wrong has real
> consequences: payroll that has to reconcile, signatures that have to hold up
> legally, integrations that can't fail quietly.
>
> Now I'm studying Interactive Media Design at Algonquin College in Ottawa, looking
> for a product design co-op starting Winter 2027.
>
> Ottawa, ON · Study permit + co-op work permit · Available Winter 2027

**Then:** three case cards → short about strip → contact.

Card headlines (these are what gets read, so they carry the thesis):

| Case | Headline | One-liner |
|---|---|---|
| 1 | The Editor Role | Adding flexibility to a system built on immutability |
| 2 | Mobile ID | Designing identity verification inside someone else's API constraints |
| 3 | Synastry | What if a dating app told you *how* to relate to a match, not just that you matched? |

## Case 1 — The Editor Role  ✅ drafted

Flagship. Longest, most detailed. See `01-editor-role.md`.

**Screens to recreate (4):**
1. Task creation — assigning an Editor stage in the signing flow
2. Editor's document management dialog — signed docs greyed out and locked
3. Editor's field placement view
4. Audit trail showing editor modifications

## Case 2 — Mobile ID  ✅ drafted (see `02-mobile-id.md`)

**Thesis:** every meaningful decision here was forced by an external API's constraints,
and the design work was in absorbing those constraints so the user never felt them.

**Spine:**
- Why advanced identity verification matters in e-signature (impersonation risk)
- The TWCA constraint that dictated everything: verification stamps a digital
  certificate, which locks the document — so verification *cannot* happen before signing
- Optional phone pre-fill: designed around initiators not always knowing signer details
- QR code as a device bridge, skipped entirely when already on mobile
- Keeping the user inside DottedSign — you call the API, so you own the whole UI
  instead of dumping users into a third-party page
- The full failure chain: 5 attempts → 24h lock → task goes pending → initiator notified
- Deliberate scope call: "signer has no Mobile ID" is handled by human conversation,
  not by the system

**Reflection (agreed framing):** low adoption wasn't a user problem. The feature was
correct and genuinely ahead of the market on security — but it was positioned as a
general paid add-on rather than aimed at the regulated industries that actually needed
it. Building ahead of the market is a real strategy; we just didn't target it.

**Screens to recreate (6):**
1. Task creation — setting Mobile ID verification + optional phone field
2. Post-submit QR code modal (desktop)
3. Mobile verification input (number / carrier / national ID + SIM-network warning)
4. Verifying → success
5. Failure with attempts remaining → 24h lockout
6. Initiator's "task on hold" notification

## Case 3 — Synastry dating app  ⏳ not started

**Do not write this as "I believe in astrology."** Write it as:

> Mainstream dating apps give you a black-box match. They tell you *that* someone is
> compatible, never *why*, and never how to actually get along with them. Synastry —
> chart comparison — is an interpretive framework a large number of people already
> use and trust. What happens if you build the matching product around an explanation
> the user can read, argue with, and act on?

That argument holds whether or not the reader believes in astrology, because the
subject is **explainability and relationship guidance**, not prediction accuracy.

**Personal insight to use:** Ottawa's population is dispersed; ordinary social overlap
is rare. This is first-hand, and first-hand beats desk research in a portfolio.

**Real precedent to cover in competitive analysis:** NUiT (astrology dating),
Co-Star, The Pattern, plus Hinge/Bumble for the black-box-matching contrast.

**Design ethics question to include deliberately:** the 流年 / transit feature
predicts difficulties in a *third party's* life and discloses them to someone else.
That is a genuine privacy and consent problem. Working through it openly — what you'd
show, what you'd withhold, who consents to what — will do more for you than any
polished screen.

**Scope:** design work primarily, plus informal interviews with friends who use dating
apps, plus competitive analysis if time allows.

## Voice rules for all writing

- Plain English. Short sentences. No "leveraged," "seamless," "robust," "journey."
- Never invent a metric. Qualitative evidence, clearly attributed, is honest and fine.
- Every design decision needs a *because*. Decisions without reasoning read as taste.
- Reflections stay specific. "I'd validate more" is empty; "we never showed it to the
  client who asked for it, and here's why" is not.

## Build plan

1. ✅ Case 1 content
2. ✅ Case 2 content
2b. ✅ Resume rewritten — resume/resume.html + Ling-Lian-Wang-Resume.pdf (one page)
3. Case 3 research + content
4. Design system (dark, technical) + home page
5. Recreate all 10 screens as SVG/HTML — no fake screenshots, built assets
6. Build site → GitHub Pages
7. Export PDF from the same content
