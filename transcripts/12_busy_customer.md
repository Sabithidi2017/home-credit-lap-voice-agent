# Test 12 — Busy Customer → Callback Request

**Recording:** https://drive.google.com/file/d/1Cf0-UD5Bct6qqpTDvQtk1xoWs34yu5gz/view?usp=sharing
**Transcript source:** Retell AI call log
**Duration:** ~0m 41s
**Date tested:** 2026-10-11
**Language:** English-dominant (agent mirrored customer's English)
**Agent gender:** Female

---

## Scenario

Customer is busy when the agent calls and requests a callback at a
specific time.

Expected outcome: **agent asks for a preferred callback time →
confirms it → ends call politely**. No offer pitch, no eligibility
questions.

### Key requirement being tested

> "Greeting and Identification: Verify that you are speaking with the
> correct customer. **If the customer is busy, acknowledge this and
> ask for a preferred callback time.**"
> — Assignment brief, Section 2A (emphasis added)

The agent must:
- Recognize the busy state
- Ask for a specific callback time
- Capture the time
- Confirm the time back to the customer
- End the call without pitching or asking eligibility questions

---

## Facts captured

| # | Data Point | Value |
|---|---|---|
| 1–7 | (not asked — busy branch taken) | — |
| Callback time | Tomorrow at 5 PM | ✅ |

---

## Full Transcript

**Agent (0:00):** Hello, and welcome! This is Voice AI agent calling from Mohamed's Voice Agent. I hope you're doing well today. May I know who I'm speaking with?

**User (0:10–0:11):** (unintelligible audio) Mohammed.

**Agent (0:11):** I'm sorry—

**Agent (0:13):** —could you repeat your name, please?

**User (0:16):** Mohammed.

**Agent (0:18):** Thank you, Mohammed. Is this a good time to talk?

**User (0:21):** No. But I am in a meeting right now.

**Agent (0:25):** What would be a convenient time for us—

**User (0:28):** Call me tomorrow—

**Agent (0:28):** —to call you back?

**User (0:29):** —five PM.

**Agent (0:31) [CONFIRM CALLBACK]:** Alright, Mohammed. I'll call you back tomorrow at 5 PM. Thank you.

**User (0:36):** Thank you.

**Agent (0:37):** You're welcome, Mohammed. I'll call you tomorrow at 5 PM.

**Tool called:** `end_call` (0:41)

**Agent (0:40):** Have a good day.

*(Call ends)*

---

## Post-Call Extraction (Retell)

```json
{
  "customer_name": "Mohammed",
  "customer_verified": true,
  "customer_available": false,
  "callback_time": "Tomorrow 5 PM",
  "property_type": null,
  "ownership_status": null,
  "documents_available": null,
  "loan_amount": null,
  "occupation": null,
  "income_mode": null,
  "market_value": null,
  "tenure_years": null,
  "existing_property_loan": false,
  "emi_reduction_request": false,
  "disqualified": false,
  "disqualification_reason": null,
  "transfer_required": false,
  "final_outcome": "busy"
}
```

---

## Verification Checklist

| Check | Result |
|---|---|
| Agent greeted warmly without assuming name | ✅ |
| Agent handled unintelligible audio by re-asking for name | ✅ |
| Agent captured name from voice | ✅ (Mohammed) |
| Agent asked about availability | ✅ |
| Customer said busy ("in a meeting") | ✅ |
| Agent did NOT pitch the offer | ✅ |
| Agent did NOT ask eligibility questions | ✅ |
| Agent asked for preferred callback time | ✅ |
| Agent captured specific time (Tomorrow 5 PM) | ✅ |
| Agent confirmed the time back to customer | ✅ |
| Agent repeated the confirmation once more at close | ✅ |
| Agent was polite, brief, and warm | ✅ |
| Call ended cleanly via `end_call` tool | ✅ |
| **Result** | ✅ **PASS** |

---

## Observations & Notes

### What worked well
- **Handled unintelligible audio at the start** — the customer's
  first response was garbled ("(unintelligible audio)"). The agent
  did NOT guess; it politely asked: "I'm sorry, could you repeat
  your name, please?" ✅ This is exactly the correct behavior for
  voice agents handling ASR noise.
- **Immediate busy-state recognition** — the customer said "No. But
  I am in a meeting right now" at 0:21. The agent recognized the
  busy signal immediately and pivoted to the callback flow at 0:25
  — only 4 seconds. ✅
- **Did NOT pitch the offer** — critically, the agent did NOT
  present the LAP offer at all. It went straight to the callback
  request. ✅ This is the correct behavior per assignment Section 2A.
- **Captured a specific callback time** — the customer said "Call me
  tomorrow five PM." The agent captured both the day ("tomorrow")
  and the time ("5 PM") into `callback_time`. ✅
- **Confirmed the callback time TWICE** — once at 0:31 ("I'll call
  you back tomorrow at 5 PM") and again at 0:37 ("I'll call you
  tomorrow at 5 PM"). Double confirmation is a strong UX pattern —
  it reassures the customer and reduces ambiguity. ✅
- **Handled split turn gracefully** — the customer started answering
  ("Call me tomorrow—") at 0:28 while the agent was still asking
  the question ("—to call you back?"). The agent completed the
  question, and the customer finished with "—five PM." The agent
  merged the split into a single coherent callback time. ✅
- **Warm close** — "Have a good day" after confirming. ✅

### Key rule validation
- Customer busy → ✅ Asked for callback time (per Section 2A)
- Did NOT proceed to offer → ✅ per Section 5A
- Did NOT ask eligibility questions → ✅ per Section 5A
- Callback time captured → ✅ `callback_time: "Tomorrow 5 PM"`
- Call ended → ✅ per Section 5A

### Minor observation
- **The customer said "I am in a meeting right now"** — the
  assignment's example is "I'm in a meeting" — a match. The agent's
  response ("What would be a convenient time for us to call you
  back?") matches the prompt's Section 5A phrasing. ✅
- **Callback time granularity** — the agent captured "Tomorrow 5 PM"
  as a string. If you want to auto-schedule the callback, you'd need
  to convert this to a timestamp. For this assignment, the string
  capture is sufficient.

### Comparison with other test outcomes
- Test 12 is the **only** test where the agent doesn't complete or
  attempt the eligibility flow. This is the correct behavior — the
  busy flow **exits early** before any eligibility questions.

**Final classification:** BUSY → Callback scheduled → Call ended at 0:41.
