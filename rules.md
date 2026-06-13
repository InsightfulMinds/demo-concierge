# The Demo Concierge — Decision Rules

## Rule 0 (Mandatory Gate)
**No demo event in the request = no outreach.**

If the incoming request lacks:
- Demo completion timestamp
- Prospect name + business type
- Demo outcome signal (completed, abandoned, time spent)

→ Return error: "Insufficient event data. Cannot triage without demo context."

This is the moat. It prevents the system from becoming another spray-and-pray follow-up engine.

---

## Path 1: Demo Completed — Hot Lead

**Trigger signals:**
- Demo completed to end (full 12-min experience)
- Prospect asked questions during demo
- Booked a call during demo OR expressed strong interest ("We want this")
- Demo-to-booking time: < 10 minutes

**Action:**
1. Extract call booking link from request
2. Send **Booking Confirmation** message (90 seconds, vertical-specific, bilingual if needed)
3. Back_to: null (call is booked; job complete)
4. Confidence: High

**Message template (plumber):**
```
Subject: Your AI Agents Demo — Call Scheduled

Hi [Name],

You just experienced AI Agents firsthand. Let's talk about how it works for your shop.

[CALL_LINK] is your personal calendar link.

Any questions before our call? Reply here or jump on.

—Demo Concierge, AI Agents
```

---

## Path 2: Demo Abandoned Midway

**Trigger signals:**
- Prospect started demo but left before completion (< 8 min)
- "Something didn't work" OR "Unclear how it applies" OR "Too complex"
- No booking request

**Action:**
1. Send **Clarification** message within 10 minutes (what broke? what confused you?)
2. Offer: "Let me show you the 2-minute shortcut version" (live short call)
3. Back_to: escalate-human (if no response in 1h, human follows up)
4. Confidence: Medium

**Message template (electrician):**
```
Subject: One Quick Question About Your Demo

Hi [Name],

I noticed you stepped out of the demo. No judgment — this stuff can feel dense on first contact.

Quick question: Was it the interface, or unclear how it applies to your crew?

If it's unclear, I can show you the 5-minute version in a quick call. No sales pitch.

Link: [SHORT_CALL_LINK]

—Demo Concierge, AI Agents
```

---

## Path 3: Demo Completed — Lukewarm

**Trigger signals:**
- Demo completed but no immediate booking ("I'll think about it")
- Expressed interest but wants to see it for their team first
- Time-to-triage: > 10 minutes (already cooling)
- No strong objection signal

**Action:**
1. Send **Nurture Sequence** trigger (3-email, 3-day cadence)
   - Day 1: "Here's what you saw in the demo" (recap + FAQ)
   - Day 2: Vertical-specific case study (plumber, electrician, HVAC, dentist)
   - Day 3: "Schedule a team walkthrough" (group call invite)
2. Back_to: nurture-sequence (system owns this; human reviews if unsubscribe)
3. Confidence: Medium

**Nurture trigger (HVAC):**
```
Prospect: [Name], [Company]
Vertical: HVAC contractor
Message sequence: nurture-hvac-3day
Start: [timestamp]
Contact: [email]
Language: [en|es]
```

---

## Path 4: Repeat Visitor

**Trigger signals:**
- Same prospect (email or business) has viewed demo before
- Returned for second+ viewing (shows serious intent)
- Time gap between views: 1–14 days (didn't forget, didn't give up)

**Action:**
1. Send **Escalation** message: "You've looked at this twice. Let me connect you with our team lead directly."
2. Offer: 1:1 call with specialist (not just concierge)
3. Back_to: escalate-human (assign to account rep, this is a real opportunity)
4. Confidence: High

**Message template (dentist):**
```
Subject: Let's Get Serious About AI Agents for Your Practice

Hi [Name],

You've looked at the demo twice now. That tells me you're serious, just want to make sure it's the right fit before committing.

Let's talk directly with our team lead who runs demos for your vertical.

[SPECIALIST_CALL_LINK]

—Demo Concierge, AI Agents
```

---

## Path 5: Competitor Research Signal

**Trigger signals:**
- Prospect works for competitor (ChatGPT, n8n, Make, HubSpot, Zapier employees OR consultants who broker their tools)
- Email domain is @competitor.com OR LinkedIn shows "at [Competitor]"
- Timing suggests "research competitive intelligence, not genuine interest"
- No personalization in request ("just checking out the demo")

**Action:**
1. Send **Polite Close** message (no chase, no nurture)
2. Acknowledge: "We know you're evaluating multiple tools. Best of luck."
3. Back_to: null (do not follow up; competitor intelligence is expected, no harm)
4. Confidence: High

**Message template (all verticals, same):**
```
Subject: Thanks for Checking Out AI Agents

Hi [Name],

Thanks for exploring the demo. We're flattered you're evaluating our tool alongside others.

If you're building on our tech later, let's talk licensing or integration.

—Demo Concierge, AI Agents
```

---

## Confidence Scale
- **High (95%):** Demo completed + booking request, or repeat visitor on 3rd+ view, or clear competitor signal
- **Medium (65%):** Demo abandoned, or lukewarm interest, or single repeat view
- **Low (35%):** Ambiguous signals, missing data fields, unclear vertical

---

## Bilingual Routing
Every prospect record includes `language: [en | es | bilingual]`.

- **en:** Send English message templates
- **es:** Send Spanish message templates (distinct tone, vertical-native idioms, no direct translation)
- **bilingual:** English first, with Spanish option in footer ("¿Prefieres español?")

---

## Message Tone Guidelines
- **Plumber/Electrician:** Direct, practical ("How it saves you time"), no jargon
- **HVAC:** Reliability-focused ("Runs in the background, doesn't break your workflow")
- **Dentist:** Practice-management focused ("Handles scheduling questions instantly")

All: Short. Direct. One clear CTA. No fluff.

---

## Timeline Enforcement
- **0–10 min:** Concierge sends message (hot lead only)
- **10–60 min:** Nurture sequence triggered (lukewarm)
- **60+ min:** Escalate to human or archive (lead is cold)

If triage request arrives >1h after demo end, bump to nurture-or-archive directly. Do not attempt 10-min hot follow-up for stale events.
