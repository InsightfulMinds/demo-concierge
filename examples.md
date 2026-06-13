# Demo Desk — Worked Examples

Each example demonstrates a decision path with explicit rule citations and outcome. Personas are illustrative B2B SaaS buyers; details are sample content, not live data.

---

## Example 1: Demo Completed — Hot Lead (Path 1 — Booking Already Scheduled)

**Input:**
```
Prospect: Maya Chen, Northwind Analytics (Series B)
Email: maya@northwindanalytics.com
Role: Head of Growth
Segment: Sales-led
Language: English
Demo completion: 12:47 PM, full walkthrough (12 min 34 sec)
Demo outcome: Viewed pricing page, clicked "Schedule Demo Call"
Call booked by prospect: 12:52 PM (5 minutes later)
Time-to-triage: 3 minutes (demo end to signal arrival)
```

**Demo Desk Analysis:**

1. **Rule 0 check:** ✓ Complete event data (timestamp, name, outcome, booking)
2. **Path classification:** Demo Completed — Hot Lead (Path 1)
   - Completed to end ✓
   - Booking already made ✓
   - Time-to-message: < 10 min ✓
3. **Message template:** Booking Confirmation (Growth persona, English)
4. **Next step:** null (booking is confirmed; no handoff needed)
5. **Confidence:** High (95%)

**Action Taken:**

Send immediately:
```
To: maya@northwindanalytics.com
Subject: Your Demo Desk call is locked in

Hi Maya,

Nice work getting through the full walkthrough. Your call is on the calendar for [DATE/TIME].

We'll dig into how Demo Desk fits Northwind's follow-up motion — so the minutes after a prospect shows interest never go quiet.

See you then.

— Demo Desk
```

**Outcome:** Booking confirmed, clean handoff to the account team. The call is already scheduled; the routing job is complete.

---

## Example 2: Demo Abandoned Midway (Path 2 — Clarity Needed)

**Input:**
```
Prospect: Diego Santos, Lumen Logistics
Email: diego@lumenlogistics.com
Role: RevOps Lead
Segment: Sales-led
Language: Spanish
Demo start: 2:15 PM
Demo abandonment: 2:19 PM (4.5 minutes in)
Abandonment signal: Exited at the integrations step, no error, no scroll past first section
Last screen viewed: Integrations overview
Time-to-triage: 8 minutes (demo end to signal arrival)
```

**Demo Desk Analysis:**

1. **Rule 0 check:** ✓ Event data present (timestamp, partial completion, session analytics)
2. **Path classification:** Demo Abandoned Midway (Path 2)
   - Departed before completion ✓
   - No completion signal ✓
   - No explicit objection, just exit ✓
3. **Message template:** Clarification + Short Offer (RevOps persona, Spanish)
4. **Next step:** human handoff (if no response in 60 min, hand off to a human)
5. **Confidence:** Medium (65%)

**Action Taken:**

Send within the window (Spanish template, native tone — not a translation):
```
To: diego@lumenlogistics.com
Subject: Una pregunta rápida sobre tu demo

Hola Diego,

Vi que saliste del demo en el paso de integraciones. Pasa seguido — esa parte puede verse densa al principio.

¿Fue la forma en que se conectan las integraciones lo que no quedó claro, o no se vio cómo encaja en el stack de Lumen?

Si quieres, te muestro la versión de 5 minutos en una llamada. Sin venta.

Link: [SHORT_CALL_LINK]

— Demo Desk
```

**Outcome:** Clarification offered. If Diego responds, the pathway branches:
- Responds with a question → send a short clip or a brief call
- No response in 1h → human handoff triggers (a human sends the follow-up)
- Books a call → graduates to Path 1 (booking confirmed)

---

## Example 3: Demo Completed — Lukewarm (Path 3 — Nurture Sequence)

**Input:**
```
Prospect: Priya Nair, Forge DevTools
Email: priya@forgedevtools.com
Role: VP Engineering
Segment: Product-led (PLG → sales-assist)
Language: English
Demo completion: 4:33 PM, 11 min 12 sec
Demo outcome: Completed all sections, read docs + pricing, did NOT book
Action taken: Clicked "Tell me more" (nurture consent signal)
Time-to-triage: 12 minutes (demo end to signal arrival)
Notes: "Wants to align with the platform team before committing"
```

**Demo Desk Analysis:**

1. **Rule 0 check:** ✓ Event data present (timestamp, completion, nurture consent)
2. **Path classification:** Demo Completed — Lukewarm (Path 3)
   - Completed to end ✓
   - No immediate booking ✓
   - Showed interest (clicked "Tell me more") ✓
3. **Message template:** Nurture sequence trigger (technical / DevTools tone, 3-day cadence)
4. **Next step:** nurture sequence (system-owned; a human reviews on unsubscribe)
5. **Confidence:** Medium (65%)

**Action Taken:**

Trigger the automated sequence:
```
Prospect: Priya Nair, priya@forgedevtools.com
Segment: Product-led (PLG)
Message sequence: nurture-plg-3day
Start time: [timestamp + 2 minutes]
Contact schedule: [Day 1 = 2 PM today, Day 2 = 10 AM tomorrow, Day 3 = 3 PM day after]
Language: English
Stop trigger: Unsubscribe click or "Schedule call" click

Day 1 email subject: "What you just saw in the Demo Desk walkthrough"
Day 2 email subject: "How a DevTools team books more demos without more reps"
Day 3 email subject: "Bring the platform team — 15-min walkthrough"
```

**Outcome:** Priya receives 3 nurture emails over 3 days. If she books a call on Day 3, the pathway graduates to Path 1 (booking confirmed). If she unsubscribes, the sequence stops and the event is recorded.

---

## Example 4: Repeat Visitor (Path 4 — Escalation to Human)

**Input:**
```
Prospect: Sarah Kim, Brightloop (Seed)
Email: sarah@brightloop.io
Role: Founder / CEO
Segment: Founder-led eval
Language: English
Visit history:
  - Visit 1: May 15, 10:34 AM (5 min 18 sec in demo)
  - Visit 2: May 18, 2:47 PM (9 min 44 sec, viewed pricing)
  - Visit 3: May 22, 11:12 AM (11 min 56 sec, completed, viewed scheduling)
Time between Visit 1 → 2: 3 days (returned)
Time between Visit 2 → 3: 4 days (returned again)
Time-to-triage: 6 minutes (from Visit 3 completion)
```

**Demo Desk Analysis:**

1. **Rule 0 check:** ✓ Event data complete (three visits, timestamps, engagement progression)
2. **Path classification:** Repeat Visitor (Path 4)
   - Same prospect, 3 views ✓
   - Increasing engagement (5 min → 9.5 min → 12 min) ✓
   - 3–4 days between views (sweet spot: not forgotten, not impatient) ✓
3. **Message template:** Escalation to Account Executive (founder-eval tone)
4. **Next step:** human handoff (assign to an AE)
5. **Confidence:** High (95%)

**Action Taken:**

Send immediately + escalate to a human:
```
To: sarah@brightloop.io
Subject: Want to take a closer look at Demo Desk together?

Hi Sarah,

You've stopped by the demo a few times this past week — always good to see. Sounds like you're weighing whether it's the right fit before bringing the team in, which makes total sense.

If it'd help, I can introduce you to the AE who works with founder-led teams day to day — no pressure, just a friendly walkthrough.

[SPECIALIST_CALL_LINK]

— Demo Desk

---

Internal escalation (not sent to prospect):
Prospect: Sarah Kim, sarah@brightloop.io
Segment: Founder-led eval
Confidence: High (repeat visitor, 3 views, 8-day engagement window)
Assignment: [AE Name]
Action: Schedule a 1:1 with the founder-led playbook
CRM note: "Strong intent — 3 visits, completed demo, viewed pricing. Ready for a tailored call."
```

**Outcome:** Sarah receives the AE's calendar link. The AE gets a CRM alert plus full visit history and calls within 4 hours. This is a real opportunity.

---

## Anti-Example 1: What NOT to Do — Competitor Research (Path 5)

**Input:**
```
Prospect: alex.johnson@rival-saas.com
Email analysis: rival-saas.com domain (competitor employee)
Demo completion: 8:42 PM, 12 min 18 sec
Demo outcome: Visited every section, viewed the API/changelog docs
Time-to-triage: 2 minutes
Language: English
Request note: "Generic demo request, no personalization"
```

**Bad action (spray-and-pray approach):**
```
Send to alex.johnson@rival-saas.com:

Subject: You loved the Demo Desk demo — book your call

Hi Alex,

Great to see you exploring Demo Desk! You made it through the full demo, so you know we mean business.

Let's schedule a call to talk features, pricing, and integrations.

[BOOKING_LINK]

— Demo Desk
```

**Why this fails:**
- ❌ Alex works for a competitor
- ❌ Treats competitive research as a genuine lead (wastes the team's time)
- ❌ Ignores the implicit "no pitch to competitors" boundary
- ❌ Invites future spam complaints when the competitor flags the sender

**Correct action (Demo Desk discipline):**
```
Send to alex.johnson@rival-saas.com:

Subject: Thanks for checking out Demo Desk

Hi Alex,

Thanks for taking the time to explore the demo. We're flattered you're evaluating the space.

If you're ever building on our side or thinking about a partnership, let's talk.

— Demo Desk
```

**Why this works:**
- ✓ Acknowledges the competitor context gracefully
- ✓ Sets a boundary (no spam follow-up)
- ✓ Leaves the partnership door open
- ✓ Signals confidence ("we know you're evaluating tools")
- ✓ Prevents CRM pollution and future unsubscribe complaints

---

## Anti-Example 2: What NOT to Do — No Rule 0 Discipline

**Bad input (rule violation):**
```
Prospect name: [unknown]
Email: [unknown]
Demo outcome: [unknown, signal arrived with no context]
Time-to-triage: [unknown]
Segment: [unknown]
Message request: "Send a follow-up"
```

**Bad action (spray-and-pray):**
```
Send email to [unknown recipient]:

"Hi there,

Thanks for checking out the demo! Here's your personal booking link.

Looking forward to working with you.

— Demo Desk"
```

**Why this fails:**
- ❌ Violates Rule 0 (no event data)
- ❌ Generic template (no segment personalization)
- ❌ Reads as automated and low-effort
- ❌ High bounce rate (unknown recipient)

**Correct action (Rule 0 enforcement):**
```
Return error:

"Rule 0 violation: insufficient event data.

Cannot triage a demo signal without:
- Prospect name + company
- Demo completion timestamp
- Demo outcome (completed / abandoned)
- Segment classification
- Language preference

Please provide complete event data before resubmitting."
```

**Why this works:**
- ✓ Enforces discipline (prevents low-quality outreach)
- ✓ Signals professional standards ("we don't email ghosts")
- ✓ Protects sender reputation (deliverability)
- ✓ Moat: Demo Desk never sends generic follow-ups
