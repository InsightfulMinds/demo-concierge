# Demo Desk — Intent-Response Operator

**Version:** 1.1
**Status:** Production-ready  
**Last updated:** 2026-06-12

## What This Is

Demo Desk is an intent-driven follow-up operator that owns the critical minutes after a prospect shows interest.

**Problem it solves:** Most companies obsess over getting prospects into a demo. Demo Desk owns what happens immediately after intent is shown.

**What it does:** Demo Desk monitors intent signals, evaluates signal quality, classifies intent, and routes the appropriate follow-up action within minutes.

**Core message:** Every intent signal gets the right follow-up: booked call confirmation, abandoned-demo recovery, nurture sequence, human escalation, or graceful close.

---

## Quick Start (30 seconds)

### What You Need
- An intent event: `{prospect_name, email, business_context, source, event_timestamp, outcome_signal, language}`
- Decision rules in `rules.md` (5 paths + Rule 0)
- Message templates in `reference-message-templates.md`

### How It Works

1. **Intent is shown** → prospect's interaction triggers an event
2. **Demo Desk receives event** within 1–2 minutes
3. **Rule 0 check:** Is event data complete? If no → error return. If yes → proceed.
4. **Classify path:** Which of 5 outcomes matches this signal?
   - Hot (booked call) → Path 1 (send confirmation)
   - Abandoned (exited midway) → Path 2 (send clarification)
   - High engagement (completed, clicked deeper, form submission, no booking) → Path 3 (start nurture)
   - Repeat visitor or multiple visits → Path 4 (escalate human)
   - Competitor research (domain match) → Path 5 (polite close)
5. **Send message** matching path, segment, and language
6. **Record outcome** in CRM with confidence score

### Examples
- See `examples.md` for 4 worked examples (one per main path) + 2 anti-examples

---

## File Structure

```
demo-concierge/
├── identity.md                      # Who the Demo Desk is
├── rules.md                         # 5 decision paths + Rule 0 (the moat)
├── examples.md                      # 4 worked examples + anti-patterns
├── reference-timing-windows.md      # SLA by path, TCPA notes
├── reference-message-templates.md   # Templates by segment (EN+ES)
├── README.md                        # This file
├── PROOF_LOG.md                     # 10 simulated events + decisions
├── ANTI_EXAMPLES.md                 # 3 ways naive follow-up fails
├── index.html                       # Operator overview page with interactive triage preview
├── social.html                      # Social proof page (IG / YouTube / LinkedIn / TikTok)
├── flow-diagram.md                  # Annotated decision flow (ASCII/Mermaid)
├── WRITEUP.md                       # 3-paragraph narrative
├── VIDEO_SCRIPT.md                  # 60–90 second script for Sean
├── sources.md                       # Methodology citations

```

---

## Core Rules (TL;DR)

### Rule 0 — The Moat
**No complete intent event data = no outreach.** Period.

Missing: prospect name, business context, source, timestamp, or outcome signal → return error. Do not guess. Do not send generic mail.

### 5 Decision Paths

| Path | Signal | Action | Back-to |
|------|--------|--------|---------|
| **1: Hot** | Completed + booked | Send booking confirmation | null |
| **2: Abandoned** | Exited midway | Send clarification + offer short call | escalate-human (if no response in 1h) |
| **3: High engagement** | Completed, form submit, clicked deeper, no booking | Trigger short nurture sequence | nurture-sequence (system-owned) |
| **4: Repeat** | Multiple visits in a short period | Escalate to account rep | escalate-human (high intent) |
| **5: Competitor** | Competitor domain or "research only" | Polite close, no follow-up | null |

---

## Bilingual Routing

Every prospect record includes `language: [en | es | bilingual]`.

- **English:** Use EN templates
- **Spanish:** Use ES templates (not a translation — native tone, segment-native idioms)
- **Bilingual:** English first, with "Prefieres español?" footer option

All examples + templates provided in both languages.

---

## Confidence Scale

- **High (95%):** Booked call, repeat visitor 3+, clear competitor signal
- **Medium (65%):** Abandoned demo, lukewarm completion, single repeat view
- **Low (35%):** Ambiguous signals, missing fields

Confidence scores inform CRM priority and human handoff urgency.

---

## Testing (5 Adversarial Cases Embedded)

See `PROOF_LOG.md` for:

1. **Test case 1:** Prospect abandons after 3 minutes — do we send Path 2 correctly?
2. **Test case 2:** Repeat visitor (3 views in 7 days) — does Path 4 escalate to human?
3. **Test case 3:** Competitor research signal (competitor domain, e.g. @rival-saas.com) — does Path 5 send polite close, NOT sales pitch?
4. **Test case 4:** No demo event data — does Rule 0 catch it and return error?
5. **Test case 5:** Bilingual prospect (Spanish speaker, Sales-led — e.g. Mercado Cloud) — does ES template send instead of EN?

All test cases have expected outcomes. Proof log shows actual results.

---

## Implementation

**With Claude:**
1. Copy `identity.md` into a new conversation
2. Paste the demo event (JSON format recommended)
3. Say: "Triage this demo event"
4. Demo Desk outputs: path classification + message + back_to routing

**With a DIY automation platform:**
1. Webhook receives demo event from the demo experience
2. Call Claude API with `identity.md` + `rules.md` + event data
3. Return message to send + routing decision
4. Send message via email (Path 1–4) or skip (Path 5)
5. Log outcome in CRM

---

## FAQ

### "What if the prospect didn't complete the demo, but came back later?"
See Path 4 (Repeat Visitor). If they viewed 2+ times, escalate to human on the latest view. The system gets smarter with engagement history.

### "Can we use this for other demo experiences?"
Yes. The rules (path classification, Rule 0 discipline, bilingual routing) are segment-agnostic. Update `reference-message-templates.md` for your segments, keep the logic.

### "What about SMS follow-up?"
Email is always compliant (CAN-SPAM). SMS requires prior express written consent. Never SMS Path 5 or non-opted-in prospects. See `reference-timing-windows.md` for TCPA notes.

### "How do we prevent the Demo Desk from over-messaging?"
- Rule 0 blocks low-quality events
- Path 3 (Lukewarm) triggers a 3-day nurture *sequence*, not daily emails
- Unsubscribe is honored immediately
- Repeat visitors bump to Path 4 (human takes over, no Demo Desk cycling)

---

## Metrics to Track

If deployed, measure:

- **Path 1 → Call booking rate:** % of hot leads who show up to call
- **Path 2 → Clarification response rate:** % who reply to "what broke?" message
- **Path 3 → Nurture-to-booking rate:** % of lukewarm leads who book after 3-day sequence
- **Path 4 → Human escalation → deal rate:** % of repeat visitors assigned to rep who close
- **Path 5 → Spam complaint rate:** Should be ~0 (polite close prevents complaints)
- **Overall:** Demo → booking conversion rate before Demo Desk vs. after

---

## Design Notes

### Why Path 0 (Rule 0)?
Discipline is the moat. AI systems that say yes to everything are replaceable. The Demo Desk that says "no incomplete data" is defensible and respects prospect signal quality.

### Why segment-specific templates?
"We save you time" means different things to a Sales-led buyer (speed to pipeline) vs. a Product-led buyer (fit with their stack). Generic templates feel low-effort. Specific ones signal respect for the buyer's go-to-market.

### Why minutes?
The strongest moment is immediately after interest is shown. Demo Desk's job is to act while the prospect still remembers what they saw, what they wanted, and where they got stuck.

### Why bilingual?
SaaS companies targeting LatAm and US-Hispanic markets know a meaningful share of their buyers prefer Spanish-language business communication. Offering Spanish-native follow-up signals respect and closes more deals.

---

## Contact & Support

Built for Demo Desk (demo experience).

Questions about this operator? Triage the demo event and the Demo Desk will tell you what it needs.

---

## License & Attribution

This operator is provided as a production-ready prompt system. Deploy, customize, measure.

Methodology includes citations from **Cialdini, Voss, Whitmore (GROW), Gallwey** — see `sources.md`.
