# Demo Desk — Proof of Execution

## 10 Simulated Demo Events (Run Through the Rules)

These ten events mirror the cards on the proof page (`proof.html`). Personas are illustrative B2B SaaS buyers; values are sample content, not live data.

### Event 1: Hot Lead — Immediate Booking (Path 1)

**Input:**
```json
{
  "prospect": {
    "name": "Maya Chen",
    "company": "Northwind Analytics (Series B)",
    "role": "Head of Growth",
    "email": "maya@northwindanalytics.com"
  },
  "demo": {
    "start_time": "2026-05-28T12:35:00Z",
    "end_time": "2026-05-28T12:47:34Z",
    "duration_sec": 754,
    "completion": "full",
    "outcome_signal": "visited_pricing_and_booked_call"
  },
  "segment": "sales-led",
  "language": "en",
  "triage_time": "2026-05-28T12:50:15Z"
}
```

**Demo Desk Analysis:**

1. **Rule 0 check:** ✓ All fields present (name, email, segment, timestamp, outcome, language)
2. **Time-to-triage:** 3 min (within hot window)
3. **Path classification:** Path 1 (Hot Lead — Demo Completed + Booking Requested)
4. **Template selected:** Booking Confirmation (Growth persona, English)
5. **Next step:** none needed — the call is booked
6. **Confidence:** 95%

**Action Taken:**
```
FROM: hello@demodesk.example
TO: maya@northwindanalytics.com
SUBJECT: Your Demo Desk call is locked in
TIME_SENT: 2026-05-28T12:50:45Z

Body: [Booking Confirmation template applied — see reference-message-templates.md]

Outcome recorded:
- Path: 1
- Confidence: 95%
- Message sent: YES
- CRM note: "Hot lead, booked immediately, Sales-led segment"
```

**Result:** ✓ PASS — Confirmation sent within the 10-minute window; the call is locked the same day.

---

### Event 2: Abandoned Demo (Path 2)

**Input:**
```json
{
  "prospect": {
    "name": "Diego Santos",
    "company": "Lumen Logistics",
    "role": "RevOps Lead",
    "email": "diego@lumenlogistics.com"
  },
  "demo": {
    "start_time": "2026-05-28T14:15:00Z",
    "end_time": "2026-05-28T14:19:30Z",
    "duration_sec": 270,
    "completion": "partial",
    "last_step_viewed": "integrations",
    "abandonment_reason": "session_ended"
  },
  "segment": "sales-led",
  "language": "es",
  "triage_time": "2026-05-28T14:27:00Z"
}
```

**Demo Desk Analysis:**

1. **Rule 0 check:** ✓ All fields present
2. **Time-to-triage:** 8 min (within hot window)
3. **Path classification:** Path 2 (Demo Abandoned Midway)
4. **Template selected:** Clarification (RevOps persona, Spanish)
5. **Next step:** hand off to a human if no reply in 60 min
6. **Confidence:** 65%

**Action Taken:**
```
FROM: hello@demodesk.example
TO: diego@lumenlogistics.com
SUBJECT: Una pregunta rápida sobre tu demo
TIME_SENT: 2026-05-28T14:27:45Z

Body: [Clarification template applied — Spanish version, native tone]

Outcome recorded:
- Path: 2
- Confidence: 65%
- Message sent: YES
- Follow-up: If no response by 15:27Z, hand off to a human
```

**Result:** ✓ PASS — Spanish clarification sent within the window; human-handoff routing armed.

---

### Event 3: Lukewarm Lead (Path 3)

**Input:**
```json
{
  "prospect": {
    "name": "Priya Nair",
    "company": "Forge DevTools",
    "role": "VP Engineering",
    "email": "priya@forgedevtools.com"
  },
  "demo": {
    "start_time": "2026-05-28T16:33:00Z",
    "end_time": "2026-05-28T16:44:12Z",
    "duration_sec": 672,
    "completion": "full",
    "pages_viewed": ["overview", "features", "pricing", "docs"],
    "cta_clicked": "tell_me_more"
  },
  "segment": "product-led",
  "language": "en",
  "triage_time": "2026-05-28T16:45:30Z"
}
```

**Demo Desk Analysis:**

1. **Rule 0 check:** ✓ All fields present
2. **Time-to-triage:** 2 min (within window, but lukewarm signal)
3. **Path classification:** Path 3 (Demo Completed — Lukewarm, no booking)
4. **Template selected:** 3-day technical nurture (DevTools tone)
5. **Next step:** start a short nurture
6. **Confidence:** 65%

**Action Taken:**
```
FROM: hello@demodesk.example
TO: priya@forgedevtools.com
TRIGGER: nurture-plg-3day
TIME_SENT: 2026-05-28T16:46:00Z

Sequence:
- Day 1 (2026-05-28 18:45): Email 1 "What you just saw in the Demo Desk walkthrough"
- Day 2 (2026-05-29 10:00): Email 2 "How a DevTools team turns demo signals into pipeline"
- Day 3 (2026-05-30 15:00): Email 3 "Bring the platform team — 15-min walkthrough"

Outcome recorded:
- Path: 3
- Confidence: 65%
- Next step: start a short nurture
- CRM note: "Lukewarm lead, Product-led segment, nurture initiated"
```

**Result:** ✓ PASS — Nurture sequence triggered (3 emails / 3 days); auto-graduates to Path 1 if a call is booked.

---

### Event 4: Repeat Visitor (Path 4)

**Input:**
```json
{
  "prospect": {
    "name": "Sarah Kim",
    "company": "Brightloop (Seed)",
    "role": "Founder / CEO",
    "email": "sarah@brightloop.io"
  },
  "visit_history": [
    { "visit_num": 1, "timestamp": "2026-05-15T10:34:00Z", "duration_sec": 318, "completion": "partial" },
    { "visit_num": 2, "timestamp": "2026-05-18T14:47:00Z", "duration_sec": 584, "completion": "partial", "pages_viewed": ["overview", "features", "pricing"] },
    { "visit_num": 3, "timestamp": "2026-05-22T11:12:00Z", "duration_sec": 716, "completion": "full", "pages_viewed": ["overview", "features", "pricing", "docs", "scheduling"], "cta_clicked": "schedule_demo_call" }
  ],
  "segment": "founder-led",
  "language": "en",
  "triage_time": "2026-05-22T11:18:30Z"
}
```

**Demo Desk Analysis:**

1. **Rule 0 check:** ✓ Complete event data
2. **Time-to-triage:** 6 min
3. **Path classification:** Path 4 (Repeat Visitor — 3 views, increasing engagement)
   - View 1 → 2: 3 days (returned)
   - View 2 → 3: 4 days (returned again, engagement increased)
4. **Template selected:** Escalation to Account Executive (founder-eval tone)
5. **Next step:** hand off to a human (with full visit history)
6. **Confidence:** 95%

**Action Taken:**
```
FROM: hello@demodesk.example
TO: sarah@brightloop.io
TIME_SENT: 2026-05-22T11:19:00Z

Subject: Let's get serious about Demo Desk for Brightloop
Body: [Escalation template applied]

Internal routing:
- Assign to: [AE Name]
- Segment: Founder-led
- Priority: HIGH
- CRM note: "Repeat visitor (3 views), 8-day engagement window, completed full demo, strong intent. Ready for an AE call."
- Action: Schedule a 1:1 with the founder-led playbook
- SLA: Contact within 4 hours
```

**Result:** ✓ PASS — High-intent signal detected; escalated to a human AE with full visit history.

---

### Event 5: Competitor Research Signal (Path 5)

**Input:**
```json
{
  "prospect": {
    "name": "Alex Johnson",
    "company": "Rival SaaS (competitor)",
    "role": "PM",
    "email": "alex.johnson@rival-saas.com"
  },
  "demo": {
    "start_time": "2026-05-28T20:42:00Z",
    "end_time": "2026-05-28T20:54:18Z",
    "duration_sec": 738,
    "completion": "full",
    "pages_viewed": ["overview", "features", "changelog", "api_docs"],
    "research_signal": true
  },
  "segment": "unknown",
  "language": "en",
  "triage_time": "2026-05-28T20:56:00Z"
}
```

**Demo Desk Analysis:**

1. **Rule 0 check:** ✓ Event data present
2. **Path classification:** Path 5 (Competitor Research Signal)
   - Email domain: @rival-saas.com (competitor)
   - Pages viewed: changelog + API docs (research behavior, not buyer intent)
3. **Template selected:** Polite Close (all segments, English)
4. **Next step:** none — do not retarget
5. **Confidence:** 95%

**Action Taken:**
```
FROM: hello@demodesk.example
TO: alex.johnson@rival-saas.com
SUBJECT: Thanks for checking out Demo Desk
TIME_SENT: 2026-05-28T20:56:30Z

Body: [Polite Close template — no pitch, no follow-up]

Internal routing:
- Do NOT follow up
- Do NOT add to nurture
- Do NOT escalate to sales
- CRM note: "Competitor employee, research signal, polite close sent"
```

**Result:** ✓ PASS — Research signal handled gracefully. No spam follow-up, no CRM pollution.

---

### Event 6: No Rule 0 Data (Rule 0 Violation)

**Input:**
```json
{
  "prospect": { "email": "unknown@example.com" },
  "demo": { "timestamp": "unknown" },
  "segment": "unknown",
  "language": "unknown"
}
```

**Demo Desk Analysis:**

1. **Rule 0 check:** ✗ VIOLATION — Missing required fields:
   - prospect.name (required)
   - demo.start_time, end_time (required)
   - demo.completion status (required)
   - segment (required)
   - language (required)

**Action Taken:**
```
STATUS: Rejected — incomplete data, no outreach sent

Message:
"Cannot triage a demo signal without complete event data.

Required fields:
✗ prospect.name
✗ demo.completion_timestamp
✗ demo.outcome_signal
✗ segment
✗ language

Please resubmit with complete data."

Outcome recorded:
- Rule 0: VIOLATION
- Message sent: NO
- Next step: none — reject and request a complete resubmission
```

**Result:** ✓ PASS — Rule 0 enforced. Low-quality request blocked before any outreach. This is the moat.

---

### Event 7: Bilingual Prospect (Path 1, ES)

**Input:**
```json
{
  "prospect": {
    "name": "Roberto García",
    "company": "Mercado Cloud (LatAm SaaS)",
    "role": "COO",
    "email": "roberto@mercadocloud.com"
  },
  "demo": {
    "start_time": "2026-05-29T09:15:00Z",
    "end_time": "2026-05-29T09:28:45Z",
    "duration_sec": 825,
    "completion": "full",
    "outcome_signal": "booked_call"
  },
  "segment": "sales-led",
  "language": "es",
  "triage_time": "2026-05-29T09:31:00Z"
}
```

**Demo Desk Analysis:**

1. **Rule 0 check:** ✓ All fields present
2. **Path classification:** Path 1 (Hot Lead — Booking Requested)
3. **Language routing:** Spanish (es)
4. **Template selected:** Booking Confirmation (Sales-led, Spanish — native, NOT a translation)
5. **Next step:** none needed — the call is booked
6. **Confidence:** 95%

**Action Taken:**
```
FROM: hello@demodesk.example
TO: roberto@mercadocloud.com
SUBJECT: Tu llamada de Demo Desk está confirmada
TIME_SENT: 2026-05-29T09:31:30Z

Body: [Booking Confirmation template — SPANISH VERSION]

Note: Spanish template uses native idiom and tone, not a direct translation.
- "Acabas de ver Demo Desk en acción" (more natural than "You just experienced")
- Speaks to the buyer's team and stack, not a literal English carry-over

Outcome recorded:
- Path: 1
- Language: Spanish (ES)
- Message sent: YES (native Spanish, not translated)
- CRM note: "Hot lead, Spanish-native, Sales-led segment"
```

**Result:** ✓ PASS — Spanish routing correct; native idiom used, not a translation.

---

### Event 8: Abandoned + Human-Handoff Trigger (Path 2 → escalate)

**Input:**
```json
{
  "prospect": {
    "name": "Michael Torres",
    "company": "Vertex Manufacturing",
    "role": "IT Director",
    "email": "michael@vertexmfg.com"
  },
  "demo": {
    "start_time": "2026-05-29T13:20:00Z",
    "end_time": "2026-05-29T13:26:30Z",
    "duration_sec": 390,
    "completion": "partial",
    "abandonment_reason": "user_exit"
  },
  "segment": "enterprise",
  "language": "en",
  "triage_time": "2026-05-29T13:28:00Z",
  "previous_triage": null
}
```

**Initial Triage:**
1. **Path 2 (Abandoned) triggered**
2. **Message sent:** Clarification ("What got in the way?")
3. **Next step:** hand off to a human if no response in 60 min

**Follow-up Simulation (60 minutes later):**
```json
{
  "triage_id": "event_8_initial",
  "time_elapsed": 3600,
  "response_received": false,
  "status": "human_handoff_trigger"
}
```

**Human-Handoff Action:**
```
FROM: hello@demodesk.example
TO: [account_executive]
SUBJECT: Human handoff — Michael Torres (Vertex Manufacturing, Enterprise)
TIME_SENT: 2026-05-29T14:28:30Z

Event: Michael Torres exited the demo after ~6 min. Clarification sent at 13:28Z.
60-minute window elapsed with no response.

Action: Human follow-up required.

Details:
- Prospect: Michael Torres (Vertex Manufacturing)
- Email: michael@vertexmfg.com
- Segment: Enterprise
- Last contact: 13:28Z (Path 2 clarification)
- Status: No response to "What got in the way?"
- Recommendation: A call or a different angle (security/eval framing)

CRM note: "Path 2 → human handoff, no response in 60-min window"
```

**Result:** ✓ PASS — Next step "hand off to a human" triggered correctly after the timeout.

---

### Event 9: Lukewarm → No Response → Archive (Path 3)

**Input (initial):**
```json
{
  "prospect": {
    "name": "Jennifer Park",
    "company": "Cedar Health (healthtech SaaS)",
    "role": "Product Manager",
    "email": "jen@cedarhealth.com"
  },
  "demo": {
    "start_time": "2026-05-30T10:45:00Z",
    "end_time": "2026-05-30T10:56:30Z",
    "completion": "full",
    "cta_clicked": "tell_me_more"
  },
  "segment": "product-led",
  "language": "en",
  "triage_time": "2026-05-30T10:57:30Z"
}
```

**Path 3 (Lukewarm) triggered:**
```
Nurture sequence initiated:
- Day 1 email sent: 2026-05-30 13:00
- Day 2 email sent: 2026-05-31 10:00
- Day 3 email sent: 2026-06-01 15:00

Response tracking:
- Email 1: Opened (no click)
- Email 2: No open
- Email 3: No open
- No booking scheduled
- No reply received
```

**Archive Action (3 days later):**
```
STATUS: Prospect archived (lukewarm nurture completed, no conversion)

CRM note: "Jennifer Park — Path 3 nurture completed without booking. Marked for re-engagement in 30 days."

Outcome recorded:
- Path: 3
- Completion: Nurture sent, no booking
- Next step: re-engage in 30 days
```

**Result:** ✓ PASS — Lukewarm lead nurtured for 3 days, then archived cleanly for later re-engagement.

---

### Event 10: High-Volume Day (Multiple Events)

**Simulation: 5 demo events arrive in the same hour**

```
Time: 2026-06-01 14:00–15:00 (1-hour batch)

Event A: Path 1 (Hot) — 2 min triage → message sent ✓
Event B: Path 2 (Abandoned) — 5 min triage → message sent ✓
Event C: Path 3 (Lukewarm) — 3 min triage → nurture triggered ✓
Event D: Path 5 (Competitor) — 4 min triage → polite close sent ✓
Event E: Path 4 (Repeat visitor) — 2 min triage → human handoff ✓

Results:
- 5 events processed in 60 minutes
- 4 messages sent (A, B, C, D)
- 1 escalation triggered (E)
- 0 Rule 0 violations
- 100% path classification accuracy
```

**Result:** ✓ PASS — High-volume hour handled correctly; every event triaged in-window, no bottlenecks.

---

## Summary

**Total events simulated:** 10
**Paths covered:**
- Path 1 (Hot): 2 events (Event 1, Event 7)
- Path 2 (Abandoned): 2 events (Event 2, Event 8)
- Path 3 (Lukewarm): 2 events (Event 3, Event 9)
- Path 4 (Repeat): 1 event (Event 4)
- Path 5 (Competitor): 1 event (Event 5)
- Rule 0 (Violation): 1 event (Event 6)
- High-volume: 1 event (Event 10, 5 sub-cases)

**Pass rate:** 10/10 (100%)
**Rule 0 enforcement:** ✓ Violation caught correctly
**Bilingual routing:** ✓ Spanish templates sent correctly
**Human-handoff timeout:** ✓ Triggered after 60 min
**Next-step routing:** ✓ All paths routed correctly

**Confidence:** Demo Desk correctly classifies demo outcomes across segments and routes the right follow-up within the timing windows.
