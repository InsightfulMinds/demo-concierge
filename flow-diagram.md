# The Demo Desk — Decision Flow (Annotated)

## ASCII Flow Diagram

```
                         DEMO EVENT ARRIVES
                              |
                              v
                    ┌─────────────────┐
                    │  Rule 0 Check   │
                    │ (Data Complete?)│
                    └────────┬────────┘
                             |
                   ┌─────────┴─────────┐
                   |                   |
              FAIL │                   │ PASS
                   v                   v
            ┌────────────┐      ┌──────────────────┐
            │   ERROR    │      │ SIGNAL ANALYSIS  │
            │ Return:    │      │ (5-way classify) │
            │ "Insuff.   │      └─────────┬────────┘
            │  Data"     │                |
            └────────────┘          ┌─────┴──────────────────────────────┐
                                    |                                    |
                         ┌──────────┴─────────┐              ┌──────────┴──────────┐
                         |                    |              |                     |
                    ┌────v────┐  ┌───────v───┐  ┌──────v──────┐  ┌────────v──┐  ┌─v──────┐
                    │ Path 1  │  │  Path 2   │  │  Path 3    │  │  Path 4  │  │Path 5 │
                    │  HOT    │  │ ABANDONED │  │ LUKEWARM   │  │ REPEAT   │  │COMPET │
                    │ Booked  │  │ Midway    │  │ No Booking │  │ Visitor  │  │Research
                    └────┬────┘  └────┬──────┘  └─────┬──────┘  └────┬─────┘  └─┬──────┘
                         |            |               |              |          |
                 ┌───────v─────┐ ┌───v──────┐  ┌─────v──────┐ ┌────v────┐ ┌──v──────┐
                 │ BOOKING     │ │ CLARIFY  │  │ NURTURE    │ │ESCALATE │ │POLITE   │
                 │CONFIRMATION │ │ MESSAGE  │  │ SEQUENCE   │ │  TO     │ │CLOSE    │
                 │ (Immediate) │ │ (0–10m)  │  │ (3-day)    │ │ HUMAN   │ │NO FOLLOW│
                 └───────┬─────┘ └────┬─────┘  └─────┬──────┘ └────┬────┘ └──┬──────┘
                         |            |              |             |         |
                    ┌────v────┐  ┌────v──────────┐  ┌─v────────┐ ┌──v──────┐ └──v──────┐
                    │next step: │  │next step:       │  │next step:  │ │next step: │  │next step: │
                    │  null   │  │human handoff │  │nurture-  │ │escalate │  │  null   │
                    │         │  │ (if no resp   │  │sequence  │ │-human   │  │         │
                    │  DONE   │  │  in 60 min)   │  │          │ │ (assign │  │  DONE   │
                    └─────────┘  └────┬─────────┘  │          │ │  rep)   │  └─────────┘
                                      |            └─────┬────┘ └────┬────┘
                                      |                  |            |
                              ┌───────v──────────┐       |      ┌─────v──────┐
                              │ HUMAN FOLLOWS    │       |      │ REP OWNS   │
                              │ UP (via email     │       |      │ RELATIONSHIP
                              │  or call)         │       |      │ (assignment)
                              └──────────────────┘  ┌────v──┐   └────────────┘
                                                    │ EMAIL │
                                                    │ SENT  │
                                                    │(Day 1)│
                                                    └───┬───┘
                                                        |
                                                   ┌────v──────┐
                                                   │ DAY 2     │
                                                   │ CASE      │
                                                   │ STUDY     │
                                                   └────┬──────┘
                                                        |
                                                   ┌────v──────┐
                                                   │ DAY 3     │
                                                   │ TEAM      │
                                                   │ WALKTHRU  │
                                                   └────┬──────┘
                                                        |
                                               ┌────────v────────┐
                                               │ BOOKING OR       │
                                               │ ARCHIVE          │
                                               └──────────────────┘
```

---

## Step-by-Step Walkthrough

### 1. **DEMO EVENT ARRIVES**
Event payload: `{prospect_name, email, segment, demo_completion_timestamp, outcome_signal, language}`

### 2. **RULE 0 CHECK**
Gate: Are ALL required fields present?

- ✓ YES → Proceed to Signal Analysis
- ✗ NO → Return error, reject request

### 3. **SIGNAL ANALYSIS** (5-Way Classification)

#### Path 1: HOT LEAD
- Demo completed to end (12+ min)
- Prospect booked a call during demo
- Time-to-triage: 0–10 min
- Confidence: 95%

#### Path 2: ABANDONED MIDWAY
- Demo started but exited before completion (< 10 min)
- Explicit "something broke" or session end signal
- Time-to-triage: 0–10 min
- Confidence: 65%

#### Path 3: LUKEWARM
- Demo completed but no booking
- Showed interest (clicked "Tell Me More" or visited pricing)
- Time-to-triage: 0–30 min (trigger nurture sequence)
- Confidence: 65%

#### Path 4: REPEAT VISITOR
- Same prospect viewed 2+ times
- Time between views: 1–14 days
- Increasing engagement signal (longer session time each visit)
- Time-to-triage: 0–10 min (human handoff)
- Confidence: 95%

#### Path 5: COMPETITOR RESEARCH
- Email domain match (@rival-saas.com, @competitor.io, etc.)
- OR research-only behavior (visited source code, API docs)
- OR no personalization signal
- Time-to-triage: 0–10 min (polite close, no follow-up)
- Confidence: 95%

### 4. **MESSAGE SELECTION & TEMPLATE MATCH**

Each path routes to:
1. **Template:** By segment (Sales-led, Product-led, Founder-led, Enterprise)
2. **Language:** By language field (EN, ES, bilingual)
3. **Tone:** By segment norms (see Segment-Specific Tone Adjustments)

### 5. **MESSAGE SENT & ROUTING DECISION**

Each message includes:
- **next step:** Where this prospect goes if next action is needed
  - `null` = complete (no follow-up)
  - `human handoff` = human takes over after timeout
  - `nurture sequence` = automated 3-day sequence
  - `human handoff` = assign to account rep (repeat visitor)

### 6. **TIMING GATES (SLA Enforcement)**

| Path | Send By | Action If No Response |
|------|---------|------------------------|
| Hot | 10 min | null (booking confirmed) |
| Abandoned | 10 min | Human handoff at 60 min |
| Lukewarm | 30 min | Nurture sequence (auto) |
| Repeat | 10 min | Human handoff (assign rep) |
| Competitor | 10 min | null (polite close only) |

### 7. **OUTCOME RECORDED**

Log: `{prospect, path, confidence, message_sent, next step, timestamp, cta_response_if_any}`

---

## Decision Tree (Logic View)

```
IF event.prospect.name IS NULL → RULE 0 VIOLATION → ERROR

IF event.demo.completion == "full" AND event.booked_call == true
  → PATH 1 (HOT)
  → ACTION: Booking Confirmation
  → next step: null

IF event.demo.completion == "partial" AND event.session_seconds < 600
  → PATH 2 (ABANDONED)
  → ACTION: Clarification
  → next step: human handoff (60-min timeout)

IF event.demo.completion == "full" AND event.booked_call == false AND event.cta_click == "tell_me_more"
  → PATH 3 (LUKEWARM)
  → ACTION: Nurture trigger
  → next step: nurture sequence

IF event.visit_history.count >= 2 AND event.visit_history.time_range_days >= 1
  AND event.visit_history.latest_engagement > event.visit_history.first_engagement
  → PATH 4 (REPEAT)
  → ACTION: Escalate to account rep
  → next step: human handoff (immediate assign)

IF event.prospect.email.domain IN [competitor_domains]
  OR event.demo.pages_viewed INCLUDES [source_code, api_docs]
  OR event.demo.outcome_signal == "research_only"
  → PATH 5 (COMPETITOR)
  → ACTION: Polite close
  → next step: null

FOR ALL PATHS:
  IF message_sent == true
    → LOG: {path, confidence, timestamp, next step, language}
  IF next step == human handoff AND 60_minutes_elapsed AND no_response
    → TRIGGER: Human handoff
  IF next step == nurture sequence
    → TRIGGER: Day 1, Day 2, Day 3 emails over 3 days
```

---

## Bilingual Routing Loop

```
IF event.language == "es"
  → Use Spanish templates (NOT translated English)
  
IF event.language == "en"
  → Use English templates

IF event.language == "bilingual"
  → Send English
  → Include footer: "¿Prefieres español?"
```

---

## Confidence Scoring

**Path 1 (Hot):** 95%
- Hard signals: booked call, full demo completion

**Path 2 (Abandoned):** 65%
- Medium confidence: partial completion, no explicit reason

**Path 3 (Lukewarm):** 65%
- Medium confidence: completed but no booking

**Path 4 (Repeat):** 95%
- High confidence: pattern of return visits + engagement increase

**Path 5 (Competitor):** 95%
- High confidence: domain match or explicit research behavior

---

## Time-Critical Gates

**Demo Desk must process within these windows:**

- **0–10 min:** Paths 1, 2, 4, 5 (hot/abandoned/repeat/competitor)
- **0–30 min:** Path 3 (lukewarm, nurture sequence trigger)
- **60 min timeout:** Path 2 human handoff gate (if no response)
- **3 days:** Path 3 nurture sequence (Day 1, Day 2, Day 3)

Missing these windows costs the entire business case (leads go cold).

---

## Segment-Specific Tone Adjustments

**Sales-led:** Speed and relevance
- "Quick turnaround on next steps"
- "Answers objections immediately"
- Emphasize: responsiveness, deal momentum

**Product-led (PLG):** Technical, self-serve, stack fit
- "Integrates with your existing tools"
- "Your team discovers value in minutes"
- Emphasize: technical depth, no hand-holding required

**Founder-led:** Direct, outcome-focused
- "Moves your business forward"
- "One clear next action"
- Emphasize: efficiency, short feedback loops

**Enterprise:** Cross-team routing, context-rich handoff
- "Connects the right stakeholders"
- "Full context for your evaluation"
- Emphasize: thoroughness, compliance-aware, multi-team enablement

All segments: Short, one clear CTA, no jargon.

---

## Error Handling

```
Rule 0 Violation (missing data)
  → Status: ERROR
  → Response: Return error message, request resubmission
  → Message sent: NO
  → CRM action: NONE (not logged as qualified lead)

Classification ambiguity (e.g., "repeat visit but also competitor domain")
  → Apply highest-confidence rule first
  → Repeat visitor (95% confidence) > Competitor (95% confidence)
  → Escalate to human for disambiguation
```

---

## Measurement Points

The Demo Desk logs these metrics for every event:

1. **Path classification accuracy** (did we categorize correctly?)
2. **Message send time** (within SLA window?)
3. **Response rate** (prospect replied or clicked?)
4. **Booking rate** (did this lead convert?)
5. **Language routing accuracy** (correct template sent?)
6. **Human handoff quality** (did human rep get full context?)

These drive future iteration and tuning.
