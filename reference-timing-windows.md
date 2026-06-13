# Reference: Timing Windows & SLA

## Follow-up SLA by Path

| Path | Trigger Signal | Triage Window | Message Sent By | Confidence |
|------|---|---|---|---|
| **Path 1: Hot Lead** | Demo completed + booking requested | 0–10 min | Immediate | 95% |
| **Path 2: Abandoned** | Demo departed midway | 0–10 min | Immediate + clarification | 65% |
| **Path 3: Lukewarm** | Demo completed, no booking | 0–30 min, trigger nurture sequence | Day 1 within 2h of triage | 65% |
| **Path 4: Repeat** | Prospect viewed 2+ times | 0–10 min | Immediate (human handoff) | 95% |
| **Path 5: Competitor** | Email domain match or research signal | 0–10 min | Polite close (no follow-up) | 95% |

## Why 10 Minutes Matters

- **0–10 min:** Prospect is still in "exploring" mindset. Message arrives while they're still thinking about the product.
- **10–30 min:** Lead is cooling. Message still relevant, but nurture sequence is more appropriate.
- **30–60 min:** Lead is lukewarm. Escalate to human or begin 3-day drip.
- **60+ min:** Lead is cold. Treat as archived unless repeat visitor signal appears.

## Message Tone by Time

- **0–3 min:** "That was great! Let's keep momentum."
- **3–10 min:** "You just saw [feature]. Let's talk about [your case]."
- **10–30 min:** "You've had time to think. What questions came up?"
- **30+ min:** "Let's schedule when you have the team available." (human handoff)

## Renewal Trigger (Repeat Visitor)

A prospect who returns after 3+ days resets the clock:

| Visit # | Time Gap | Action |
|---------|----------|--------|
| 1st view | — | Demo Desk triage normal path |
| 2nd view | 1–3 days | Increase confidence +15%; note return |
| 3rd view | 4+ days from 1st | Human handoff; high-intent signal |
| 4+ view | Any gap | Direct assignment to account rep |

## Segment-Specific Timing Adjustments

**Sales-led:**
- High urgency: Send immediately (deals require rapid response and sales engagement)
- Soft close on Path 3: "Sales team moves fast; let's schedule while momentum is high."

**Product-led (PLG):**
- Moderate urgency: 5–10 min window acceptable
- Soft close: "Self-serve exploration at your pace; support is one click away."

**Enterprise:**
- Moderate urgency: 10–15 min window acceptable
- Soft close: "Security and procurement aligned; let's connect you with our implementation team."

## Escalation-to-Human Triggers

Escalate immediately (skip Demo Desk, go to account rep):

1. **Repeat visitor (3+ views)**
2. **Express booking request with questions** (wants to talk, not just schedule)
3. **Segment-specific question** (asks about the buyer's workflow, stack, SOC 2 / security review for Enterprise, etc.)
4. **Team engagement** (prospect mentions bringing decision-makers, legal/compliance review, stakeholder alignment, etc.)

## Unsubscribe & Blacklist Rules

- **Explicit "not interested":** Honor immediately. No follow-up.
- **Hard bounce (invalid email):** Note in CRM, do not retry.
- **Spam complaint:** Flag prospect domain (if corporate), alert compliance.
- **Competitor employee repeated requests:** After 2 visits, add competitor domain to "research signal" filter.

---

## Messaging Window Enforcement

**Automatic archival:**
- If triage request arrives >60 minutes after demo end, do not send Path 1/2 message
- Instead, check: is this a repeat visitor? If yes, human handoff. If no, archive and trigger nurture-if-lukewarm only.

**Clock reset on revisit:**
- Each new visit restarts the timing window
- Visit 2 occurs 3 days later? Treat as new lead, new 10-min window, but note repeat in CRM.

---

## TCPA Compliance Notes

**US-specific (not stored in this operator, noted for outbound implementation):**

All SMS-based follow-up requires prior express written consent. In the demo:
- ✓ Collect email (always covered under CAN-SPAM)
- ⚠️ SMS only with explicit opt-in
- ✓ Email messaging after click "Tell Me More" or booking = implied consent

Never SMS Path 5 (competitor) or non-opted-in prospects.

Email follow-up (all paths) is compliant with CAN-SPAM unsubscribe footer.
