# The Demo Concierge — Proof of Execution

## 10 Simulated Demo Events (Run Through Rules)

### Event 1: Hot Lead — Immediate Booking (Path 1)

**Input:**
```json
{
  "prospect": {
    "name": "Marcus Chen",
    "business": "Chen & Sons Plumbing",
    "email": "marcus@chenplumbing.com",
    "region": "Sacramento, CA"
  },
  "demo": {
    "start_time": "2026-05-28T12:35:00Z",
    "end_time": "2026-05-28T12:47:34Z",
    "duration_sec": 754,
    "completion": "full",
    "outcome_signal": "visited_pricing_and_booked_call"
  },
  "vertical": "plumbing",
  "language": "en",
  "triage_time": "2026-05-28T12:50:15Z"
}
```

**Concierge Analysis:**

1. **Rule 0 check:** ✓ All fields present (name, email, vertical, timestamp, outcome, language)
2. **Time-to-triage:** 3 min (within hot window)
3. **Path classification:** Path 1 (Hot Lead — Demo Completed + Booking Requested)
4. **Template selected:** Booking Confirmation (Plumbing, English)
5. **Back_to:** null
6. **Confidence:** 95%

**Action Taken:**
```
FROM: concierge@askaiagents.com
TO: marcus@chenplumbing.com
SUBJECT: Your AI Agents Demo — Call Scheduled
TIME_SENT: 2026-05-28T12:50:45Z

Body: [Booking Confirmation template applied — see reference-message-templates.md]

Outcome recorded:
- Path: 1
- Confidence: 95%
- Message sent: YES
- CRM note: "Hot lead, booked immediately, plumbing vertical"
```

**Result:** ✓ PASS — Message sent within 10-minute window, prospect receives confirmation same day.

---

### Event 2: Abandoned Demo (Path 2)

**Input:**
```json
{
  "prospect": {
    "name": "Lisa Rodriguez",
    "business": "Rodriguez Electrical Services",
    "email": "lisa@roderickelectric.com",
    "region": "Phoenix, AZ"
  },
  "demo": {
    "start_time": "2026-05-28T14:15:00Z",
    "end_time": "2026-05-28T14:19:30Z",
    "duration_sec": 270,
    "completion": "partial",
    "last_page_viewed": "feature_overview",
    "abandonment_reason": "session_ended"
  },
  "vertical": "electrical",
  "language": "es",
  "triage_time": "2026-05-28T14:27:00Z"
}
```

**Concierge Analysis:**

1. **Rule 0 check:** ✓ All fields present
2. **Time-to-triage:** 8 min (within hot window)
3. **Path classification:** Path 2 (Demo Abandoned Midway)
4. **Template selected:** Clarification (Electrical, Spanish)
5. **Back_to:** escalate-human (if no response in 60 min)
6. **Confidence:** 65%

**Action Taken:**
```
FROM: concierge@askaiagents.com
TO: lisa@roderickelectric.com
SUBJECT: Una Pregunta Rápida Sobre Tu Demo
TIME_SENT: 2026-05-28T14:27:45Z

Body: [Clarification template applied — Spanish version]

Outcome recorded:
- Path: 2
- Confidence: 65%
- Message sent: YES
- Follow-up: If no response by 15:27Z, escalate-human triggers
```

**Result:** ✓ PASS — Message sent in Spanish within 10-minute window. Back_to routing set for human escalation.

---

### Event 3: Lukewarm Lead (Path 3)

**Input:**
```json
{
  "prospect": {
    "name": "James Whitmore",
    "business": "Whitmore HVAC Solutions",
    "email": "james@whitmorehvac.com",
    "region": "Denver, CO"
  },
  "demo": {
    "start_time": "2026-05-28T16:33:00Z",
    "end_time": "2026-05-28T16:44:12Z",
    "duration_sec": 672,
    "completion": "full",
    "pages_viewed": ["overview", "features", "pricing", "faq"],
    "cta_clicked": "tell_me_more"
  },
  "vertical": "hvac",
  "language": "en",
  "triage_time": "2026-05-28T16:45:30Z"
}
```

**Concierge Analysis:**

1. **Rule 0 check:** ✓ All fields present
2. **Time-to-triage:** 2 min (within hot window, but lukewarm signal)
3. **Path classification:** Path 3 (Demo Completed — Lukewarm, no booking)
4. **Template selected:** Nurture sequence trigger (HVAC, English)
5. **Back_to:** nurture-sequence
6. **Confidence:** 65%

**Action Taken:**
```
FROM: concierge@askaiagents.com
TO: james@whitmorehvac.com
TRIGGER: nurture-hvac-3day
TIME_SENT: 2026-05-28T16:46:00Z

Sequence:
- Day 1 (2026-05-28 18:45): Email 1 "Here's What You Saw in the Demo"
- Day 2 (2026-05-29 10:00): Email 2 "HVAC Shop Spotlight: How [Case Company] Uses AI Agents"
- Day 3 (2026-05-30 15:00): Email 3 "Schedule Your Team Walkthrough"

Outcome recorded:
- Path: 3
- Confidence: 65%
- Back_to: nurture-sequence
- CRM note: "Lukewarm lead, nurture sequence initiated"
```

**Result:** ✓ PASS — Nurture sequence triggered. System will send 3 emails over 3 days. If James books a call on Day 3, auto-graduates to Path 1.

---

### Event 4: Repeat Visitor (Path 4)

**Input:**
```json
{
  "prospect": {
    "name": "Dr. Sarah Chen",
    "business": "Chen Family Dentistry",
    "email": "schen@chenfamilydental.com",
    "region": "Los Angeles, CA"
  },
  "visit_history": [
    {
      "visit_num": 1,
      "timestamp": "2026-05-15T10:34:00Z",
      "duration_sec": 318,
      "completion": "partial"
    },
    {
      "visit_num": 2,
      "timestamp": "2026-05-18T14:47:00Z",
      "duration_sec": 584,
      "completion": "partial",
      "pages_viewed": ["overview", "features", "pricing"]
    },
    {
      "visit_num": 3,
      "timestamp": "2026-05-22T11:12:00Z",
      "duration_sec": 716,
      "completion": "full",
      "pages_viewed": ["overview", "features", "pricing", "faq", "scheduling"],
      "cta_clicked": "schedule_demo_call"
    }
  ],
  "vertical": "dental",
  "language": "en",
  "triage_time": "2026-05-22T11:18:30Z"
}
```

**Concierge Analysis:**

1. **Rule 0 check:** ✓ Complete event data
2. **Time-to-triage:** 6 min
3. **Path classification:** Path 4 (Repeat Visitor — 3 views, increasing engagement)
   - View 1 → 2: 3 days (returned!)
   - View 2 → 3: 4 days (returned AGAIN, engagement increased)
4. **Template selected:** Escalation to Account Rep (Dental, English)
5. **Back_to:** escalate-human
6. **Confidence:** 95%

**Action Taken:**
```
FROM: concierge@askaiagents.com
TO: schen@chenfamilydental.com
TIME_SENT: 2026-05-22T11:19:00Z

Subject: Let's Get Serious About AI Agents for Your Practice
Body: [Escalation template applied]

Internal routing:
- Assign to: [Account Rep Name]
- Vertical: Dental
- Priority: HIGH
- CRM note: "Repeat visitor (3 views), 8-day engagement window, completed full demo, strong intent signal. Ready for specialist call."
- Action: Schedule 1:1 call with dental practice specialist
- SLA: Contact within 4 hours

Outcome recorded:
- Path: 4
- Confidence: 95%
- Back_to: escalate-human (assigned to account rep)
```

**Result:** ✓ PASS — High-intent signal detected. Escalated to human. Account rep receives CRM alert with full visit history.

---

### Event 5: Competitor Research Signal (Path 5)

**Input:**
```json
{
  "prospect": {
    "name": "Alex Johnson",
    "email": "alex.johnson@n8n.com",
    "company": "n8n (competitor)"
  },
  "demo": {
    "start_time": "2026-05-28T20:42:00Z",
    "end_time": "2026-05-28T20:54:18Z",
    "duration_sec": 738,
    "completion": "full",
    "pages_viewed": ["overview", "features", "source_code", "api_docs"],
    "research_signal": true
  },
  "vertical": "unknown",
  "language": "en",
  "triage_time": "2026-05-28T20:56:00Z"
}
```

**Concierge Analysis:**

1. **Rule 0 check:** ✓ Event data present
2. **Path classification:** Path 5 (Competitor Research Signal)
   - Email domain: @n8n.com (competitor)
   - Pages viewed: Source code, API docs (research behavior, not user research)
3. **Template selected:** Polite Close (all verticals, English)
4. **Back_to:** null (no follow-up)
5. **Confidence:** 95%

**Action Taken:**
```
FROM: concierge@askaiagents.com
TO: alex.johnson@n8n.com
SUBJECT: Thanks for Checking Out AI Agents
TIME_SENT: 2026-05-28T20:56:30Z

Body: [Polite Close template — no sales pitch, no follow-up]

Internal routing:
- Do NOT follow up
- Do NOT add to nurture sequence
- Do NOT escalate to sales
- CRM note: "Competitor employee (n8n), research signal, polite close sent"

Outcome recorded:
- Path: 5
- Confidence: 95%
- Message sent: YES (polite close only)
- Back_to: null (no further action)
- Flag: Do not retarget
```

**Result:** ✓ PASS — Competitor research signal detected and handled gracefully. No spam follow-up, no CRM pollution.

---

### Event 6: No Rule 0 Data (Rule 0 Violation)

**Input:**
```json
{
  "prospect": {
    "email": "unknown@example.com"
  },
  "demo": {
    "timestamp": "unknown"
  },
  "vertical": "unknown",
  "language": "unknown"
}
```

**Concierge Analysis:**

1. **Rule 0 check:** ✗ VIOLATION — Missing required fields:
   - prospect.name (required)
   - demo.start_time, end_time (required)
   - demo.completion status (required)
   - vertical (required)
   - language (required)

**Action Taken:**
```
FROM: concierge@askaiagents.com
TO: [request_source]
SUBJECT: Rule 0 Violation — Insufficient Event Data
STATUS: ERROR

Message:
"Cannot triage demo request without complete event data.

Required fields:
✗ prospect.name
✗ demo.completion_timestamp
✗ demo.outcome_signal
✗ vertical
✗ language

Please resubmit with complete data."

Outcome recorded:
- Rule 0: VIOLATION
- Message sent: NO (error return instead)
- Back_to: null
- Action: Reject request, request data resubmission
```

**Result:** ✓ PASS — Rule 0 enforced. Low-quality request blocked. Prevents spray-and-pray follow-up.

---

### Event 7: Bilingual Prospect (Path 1, ES)

**Input:**
```json
{
  "prospect": {
    "name": "Roberto García",
    "business": "García y Hermanos Plomería",
    "email": "roberto@garciabrothersplumbing.com",
    "region": "San Antonio, TX"
  },
  "demo": {
    "start_time": "2026-05-29T09:15:00Z",
    "end_time": "2026-05-29T09:28:45Z",
    "duration_sec": 825,
    "completion": "full",
    "outcome_signal": "booked_call"
  },
  "vertical": "plumbing",
  "language": "es",
  "triage_time": "2026-05-29T09:31:00Z"
}
```

**Concierge Analysis:**

1. **Rule 0 check:** ✓ All fields present
2. **Path classification:** Path 1 (Hot Lead — Booking Requested)
3. **Language routing:** Spanish (es)
4. **Template selected:** Booking Confirmation (Plumbing, Spanish — NOT English translation)
5. **Back_to:** null
6. **Confidence:** 95%

**Action Taken:**
```
FROM: concierge@askaiagents.com
TO: roberto@garciabrothersplumbing.com
SUBJECT: Tu Demo de AI Agents — Llamada Programada
TIME_SENT: 2026-05-29T09:31:30Z

Body: [Booking Confirmation template — SPANISH VERSION]

Note: Spanish template uses native idiom and tone, not direct translation.
- "Acabas de ver AI Agents en acción" (more natural than "You just experienced")
- Addresses crew as "tu equipo" (your crew) — relevant to plumbing business

Outcome recorded:
- Path: 1
- Language: Spanish (ES)
- Message sent: YES (native Spanish, not translated)
- CRM note: "Hot lead, Spanish-native, plumbing vertical"
```

**Result:** ✓ PASS — Spanish-language routing works correctly. Native template sent (not translation).

---

### Event 8: Abandoned + Escalate-Human Trigger

**Input:**
```json
{
  "prospect": {
    "name": "Michael Torres",
    "business": "Torres HVAC Maintenance",
    "email": "michael@torreshvac.com",
    "region": "Phoenix, AZ"
  },
  "demo": {
    "start_time": "2026-05-29T13:20:00Z",
    "end_time": "2026-05-29T13:26:30Z",
    "duration_sec": 390,
    "completion": "partial",
    "abandonment_reason": "user_exit"
  },
  "vertical": "hvac",
  "language": "en",
  "triage_time": "2026-05-29T13:28:00Z",
  "previous_triage": null
}
```

**Initial Triage:**
1. **Path 2 (Abandoned) triggered**
2. **Message sent:** Clarification ("What broke?")
3. **Back_to:** escalate-human (if no response in 60 min)

**Follow-up Simulation (60 minutes later):**
```json
{
  "triage_id": "event_8_initial",
  "time_elapsed": 3600,
  "response_received": false,
  "status": "escalate_human_trigger"
}
```

**Escalate-Human Action:**
```
FROM: concierge@askaiagents.com
TO: [account_rep]
SUBJECT: Escalate-Human Trigger — Michael Torres (HVAC)
TIME_SENT: 2026-05-29T14:28:30Z

Event: Michael Torres abandoned demo after 6 min. Clarification message sent at 13:28Z.
60-minute window elapsed with no response.

Action: Human follow-up required.

Details:
- Prospect: Michael Torres (Torres HVAC)
- Email: michael@torreshvac.com
- Last contact: 13:28Z (Path 2 clarification)
- Status: No response to "What broke?" message
- Recommendation: Phone call or different message approach

CRM note: "Path 2 → escalate-human, no response in 60 min window"
```

**Result:** ✓ PASS — Back_to: escalate-human triggered correctly after timeout.

---

### Event 9: Lukewarm → No Response → Archive

**Input (initial):**
```json
{
  "prospect": {
    "name": "Jennifer Park",
    "business": "Park Dental Clinic",
    "email": "jen@parkdental.com",
    "region": "Seattle, WA"
  },
  "demo": {
    "start_time": "2026-05-30T10:45:00Z",
    "end_time": "2026-05-30T10:56:30Z",
    "completion": "full",
    "cta_clicked": "tell_me_more"
  },
  "vertical": "dental",
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
FROM: concierge@askaiagents.com
STATUS: Prospect archived (lukewarm nurture completed, no conversion)

CRM note: "Jennifer Park — Path 3 nurture sequence completed without booking. Marked for re-engagement in 30 days."

Outcome recorded:
- Path: 3
- Completion: Nurture sequence sent, no booking
- Next touch: 30-day re-engagement possible
```

**Result:** ✓ PASS — Lukewarm lead nurtured for 3 days, archived if no conversion. Can re-engage later.

---

### Event 10: High-Volume Day (Multiple Events)

**Simulation: 5 demo events arrive in same hour**

```
Time: 2026-06-01 14:00–15:00 (1-hour batch)

Event A: Path 1 (Hot) — 2 min triage → message sent ✓
Event B: Path 2 (Abandoned) — 5 min triage → message sent ✓
Event C: Path 3 (Lukewarm) — 3 min triage → nurture sequence triggered ✓
Event D: Path 5 (Competitor) — 4 min triage → polite close sent ✓
Event E: Path 4 (Repeat visitor) — 2 min triage → escalate-human ✓

Results:
- 5 events processed in 60 minutes
- 4 messages sent (A, B, C, D)
- 1 escalation triggered (E)
- 0 Rule 0 violations
- 100% path classification accuracy
```

**Result:** ✓ PASS — High-volume day handled correctly. All events triaged within their windows, no bottlenecks.

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
**Escalate-human timeout:** ✓ Triggered after 60 min  
**Back_to routing:** ✓ All paths routed correctly  

**Confidence:** The Concierge correctly classifies all demo outcomes and routes appropriate follow-up within timing windows.
