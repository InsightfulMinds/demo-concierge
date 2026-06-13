# The Demo Desk — Worked Examples

Each example demonstrates a decision path with explicit rule citations and outcome.

---

## Example 1: Demo Completed — Hot Lead (Path 1 — Booking Already Scheduled)

**Input:**
```
Prospect: Marcus Chen, Chen & Sons Plumbing (Sacramento, CA)
Email: marcus@chenplumbing.com
Language: English
Demo completion: 12:47 PM, full demo (12 min 34 sec)
Demo outcome: Visited pricing page, clicked "Schedule Demo Call"
Called booked by prospect: 12:52 PM (5 minutes later, on demo desk calendar)
Vertical: Plumbing
Time-to-triage: 3 minutes (demo end to request arrival)
```

**Demo Desk Analysis:**

1. **Rule 0 check:** ✓ Complete event data (timestamp, name, outcome, booking)
2. **Path classification:** Demo Completed — Hot Lead (Path 1)
   - Completed to end ✓
   - Booking already made ✓
   - Time-to-message: < 10 min ✓
3. **Message template:** Booking Confirmation (plumbing vertical, English)
4. **Back_to field:** null (booking is confirmed; no handoff needed)
5. **Confidence:** High (95%)

**Action Taken:**

Send immediately:
```
To: marcus@chenplumbing.com
Subject: Your Demo Desk Demo — Call Scheduled

Hi Marcus,

Nice work getting through the full demo. Your call is on the calendar for [DATE/TIME].

We'll dig into how Demo Desk works in your workflow — answering customer questions while your crew focuses on the work.

See you then.

—Demo Desk, Demo Desk
```

**Outcome:** Booking confirmed, warm handoff to demo specialist. Call is already scheduled; demo desk job complete.

---

## Example 2: Demo Abandoned Midway (Path 2 — Clarity Needed)

**Input:**
```
Prospect: Lisa Rodriguez, Rodriguez Electrical Services (Phoenix, AZ)
Email: lisa@roderickelectric.com
Language: Spanish
Demo start: 2:15 PM
Demo abandonment: 2:19 PM (4 minutes in)
Abandonment signal: Session end, no error message, prospect did not scroll past first section
Last page viewed: Feature overview (confusing layout noted by similar prospects)
Vertical: Electrical
Time-to-triage: 8 minutes (demo end to request arrival)
```

**Demo Desk Analysis:**

1. **Rule 0 check:** ✓ Event data present (timestamp, partial completion, session analytics)
2. **Path classification:** Demo Abandoned Midway (Path 2)
   - Departed before completion ✓
   - No completion signal ✓
   - No explicit objection, just exit ✓
3. **Message template:** Clarification + Short Offer (electrical vertical, Spanish)
4. **Back_to field:** escalate-human (if no response in 60 min, handoff to human)
5. **Confidence:** Medium (65%)

**Action Taken:**

Send within 10 minutes (Spanish template, native tone):
```
To: lisa@roderickelectric.com
Subject: Una Pregunta Rápida Sobre Tu Demo

Hola Lisa,

Vi que saliste del demo después de pocos minutos. Nada raro — esto puede parecer complicado al principio.

¿Fue la interfaz que te confundió, o no quedó claro cómo funciona en tu negocio?

Si no está claro, te puedo mostrar la versión de 5 minutos en una llamada. Sin vendida.

Link: [SHORT_CALL_LINK]

—Demo Desk, Demo Desk
```

**Outcome:** Clarification offered. If Lisa responds, pathway branches:
- Responds with question → send short video or brief call
- No response in 1h → escalate-human triggers (human team member sends reminder)
- Books call → graduates to Path 4 (repeat visitor attention)

---

## Example 3: Demo Completed — Lukewarm (Path 3 — Nurture Sequence)

**Input:**
```
Prospect: James Whitmore, Whitmore HVAC Solutions (Denver, CO)
Email: james@whitmorehvac.com
Language: English
Demo completion: 4:33 PM, 11 min 12 sec
Demo outcome: Completed all sections, viewed pricing, did NOT book
Action taken: Clicked "Tell Me More" button (nurture consent signal)
Vertical: HVAC
Time-to-triage: 12 minutes (demo end to request arrival)
Notes: "Wants to discuss with operations manager before committing"
```

**Demo Desk Analysis:**

1. **Rule 0 check:** ✓ Event data present (timestamp, completion, nurture consent)
2. **Path classification:** Demo Completed — Lukewarm (Path 3)
   - Completed to end ✓
   - No immediate booking ✓
   - Showed interest (clicked "Tell Me More") ✓
3. **Message template:** Nurture sequence trigger (HVAC-specific, 3-day cadence)
4. **Back_to field:** nurture-sequence (system-owned; human reviews if prospect unsubscribes)
5. **Confidence:** Medium (65%)

**Action Taken:**

Trigger automated sequence:
```
Prospect: James Whitmore, james@whitmorehvac.com
Vertical: HVAC
Message sequence: nurture-hvac-3day
Start time: [timestamp + 2 minutes]
Contact schedule: [Day 1 = 2 PM today, Day 2 = 10 AM tomorrow, Day 3 = 3 PM day after]
Language: English
Stop trigger: Unsubscribe click or "Schedule Call" click

Day 1 email subject: "Here's What You Saw in the Demo Desk Demo"
Day 2 email subject: "HVAC Shop Spotlight: How [Case Company] Uses Demo Desk"
Day 3 email subject: "Schedule Your Team Walkthrough (15 min)"
```

**Outcome:** James receives 3 nurture emails over 3 days. If he books a call on Day 3, pathway graduates to Path 1 (demo scheduled). If he unsubscribes, sequence stops; email recorded in CRM.

---

## Example 4: Repeat Visitor (Path 4 — Escalation to Human)

**Input:**
```
Prospect: Dr. Sarah Chen, Chen Family Dentistry (Los Angeles, CA)
Email: schen@chenfamilydental.com
Language: English
Visit history:
  - Visit 1: May 15, 10:34 AM (5 min 18 sec in demo)
  - Visit 2: May 18, 2:47 PM (9 min 44 sec in demo, viewed pricing)
  - Visit 3: May 22, 11:12 AM (11 min 56 sec, completed, viewed scheduling)
Time between Visit 1 → 2: 3 days (returned!)
Time between Visit 2 → 3: 4 days (returned AGAIN)
Vertical: Dental
Time-to-triage: 6 minutes (from Visit 3 completion)
```

**Demo Desk Analysis:**

1. **Rule 0 check:** ✓ Event data complete (three visits, timestamps, engagement progression)
2. **Path classification:** Repeat Visitor (Path 4)
   - Same prospect, 3 views ✓
   - Increasing engagement (5 min → 9.5 min → 12 min) ✓
   - Time between views: 3–4 days (sweet spot: not forgotten, not impatient) ✓
3. **Message template:** Escalation to Account Rep (dental practice language)
4. **Back_to field:** escalate-human (assign to account representative)
5. **Confidence:** High (95%)

**Action Taken:**

Send immediately + escalate to human:
```
To: schen@chenfamilydental.com
Subject: Let's Get Serious About Demo Desk for Your Practice

Hi Dr. Chen,

You've looked at the demo three times now across the past week. That tells me you're genuinely interested — you just want to make sure it's the right fit for your practice before you commit.

Let's talk directly with our team lead who works with dental practices.

[SPECIALIST_CALL_LINK]

—Demo Desk, Demo Desk

---

Internal escalation:
Prospect: Dr. Sarah Chen, schen@chenfamilydental.com
Vertical: Dental
Confidence: High (repeat visitor, 3 views, 8-day engagement window)
Assignment: [Account Rep Name]
Action: Schedule 1:1 specialist call with dental practice playbook
CRM note: "Strong intent signal — 3 visits, completed demo, viewed pricing. Ready for vertical-specific pitch."
```

**Outcome:** Dr. Chen receives specialist contact link. Account rep receives CRM alert + engagement history. Rep calls within 4 hours. This is a real opportunity.

---

## Anti-Example 1: What NOT to Do — Competitor Research (Path 5)

**Input:**
```
Prospect: alex.johnson@n8n.com
Email analysis: n8n.com domain (competitor employee)
Demo completion: 8:42 PM, 12 min 18 sec
Demo outcome: Visited every section, viewed source code docs
Time-to-triage: 2 minutes
Language: English
Request note: "Generic demo request, no personalization"
```

**Bad action (spray-and-pray approach):**
```
Send to alex.johnson@n8n.com:

Subject: You Loved the Demo Desk Demo — Book Your Call

Hi Alex,

Great to see you exploring Demo Desk! You made it through the full demo, so you know we're serious.

Let's schedule a call to talk features, pricing, and custom integration.

[BOOKING_LINK]

Looking forward to chatting,
—Demo Desk
```

**Why this fails:**
- ❌ Alex works for competitor n8n
- ❌ Treats competitive research as genuine lead (wastes internal resources)
- ❌ Violates implicit "no sales pitch to competitors" moat
- ❌ Creates future spam complaints when Alex's employer bans email list

**Correct action (Demo Desk discipline):**
```
Send to alex.johnson@n8n.com:

Subject: Thanks for Checking Out Demo Desk

Hi Alex,

Thanks for taking the time to explore the demo. We're flattered you're evaluating tools alongside the ecosystem.

If you're building on our tech or considering partnership down the road, let's talk.

—Demo Desk, Demo Desk
```

**Why this works:**
- ✓ Acknowledges competitor context gracefully
- ✓ Sets boundary (no spam follow-up)
- ✓ Leaves partnership door open
- ✓ Signals confidence ("we know you're evaluating tools")
- ✓ Prevents CRM pollution and future unsubscribe complaints

---

## Anti-Example 2: What NOT to Do — No Rule 0 Discipline

**Bad input (rule violation):**
```
Prospect name: [unknown]
Email: [unknown]
Demo outcome: [unknown, request arrived with no context]
Time-to-triage: [unknown]
Vertical: [unknown]
Message request: "Send a follow-up"
```

**Bad action (spray-and-pray):**
```
Send email to [unknown recipient]:

"Hi there,

Thanks for checking out the demo! Here's your personal booking link.

Looking forward to working with you.

—Demo Desk"
```

**Why this fails:**
- ❌ Violates Rule 0 (no event data)
- ❌ Generic template (no vertical personalization)
- ❌ Sender appears automated and low-effort
- ❌ High bounce rate (unknown recipient)

**Correct action (Rule 0 enforcement):**
```
Return error:

"Rule 0 violation: Insufficient event data.

Cannot triage demo request without:
- Prospect name + business
- Demo completion timestamp
- Demo outcome (completed / abandoned)
- Vertical classification
- Language preference

Please provide complete event data before resubmitting."
```

**Why this works:**
- ✓ Enforces discipline (prevents low-quality outreach)
- ✓ Signals professional standards ("we don't email ghosts")
- ✓ Protects sender reputation (email deliverability)
- ✓ Moat: "The Demo Desk never sends generic follow-ups"
