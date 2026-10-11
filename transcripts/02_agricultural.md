# Test 2 — Agricultural Property (Immediate Disqualification)

**Recording:** https://drive.google.com/file/d/1blhR1rI30dmvlzLLzV3_sAARZalpbjl1/view?usp=sharing
**Transcript source:** Retell AI call log
**Duration:** ~1m 12s
**Date tested:** 2026-10-11
**Language:** Hinglish (English → Hindi mirroring)
**Agent gender:** Female (feminine Hindi forms enforced)

---

## Scenario

Customer confirms the property is agricultural land.
Expected outcome: **immediate disqualification at Q1** — no further
eligibility questions asked, polite close, call ends.

### Key requirement being tested

> "If the property is Agricultural, the customer is not eligible."
> — Assignment brief, Section 2C

The agent must NOT continue asking Q2–Q7 after this disqualification.

---

## Full Transcript

**Agent (0:00):** Hello, and welcome! This is Voice AI agent calling from Mohamed's Voice Agent. I hope you're doing well today. May I know who I'm speaking with?

**User (0:08):** Yes. This is Viral.

**Agent (0:11):** Thank you, Viral. Is this a good time to talk?

**User (0:15):** No. This is Bilal.

**Agent (0:18):** Sorry, Bilal. Is this a good time to talk?

**User (0:21):** Yes.

**Agent (0:23):** Thank you, Bilal. As a valued customer, you have a pre-approved Loan Against Property offer of up to ₹75 lakhs. I'd like to ask a few basic questions to see if it—

**User (0:35):** Yes.

**Agent (0:35):** —suits you, is that okay?

**Agent (0:37) [Q1]:** Bilal ji, aapka property kaunsa hai — residential house, flat, commercial shop, office, ya industrial?

**User (0:45):** It's agricultural land. Kheti ki zameen.

**Agent (0:49):** Main samajh gayi. Unfortunately, agricultural property is not covered under this offer. Aapke time ke liye dhanyavaad.

**User (1:06):** Dhanyavaad.

**Agent (1:09):** Aapka bhi dhanyavaad, Bilal. Have a good—

**Tool called:** `end_call` (1:12)

**Agent (1:12):** —day.

*(Call ends)*

---

## Post-Call Extraction (Retell)

```json
{
  "customer_name": "Bilal",
  "customer_verified": true,
  "customer_available": true,
  "callback_time": null,
  "property_type": "Agricultural",
  "ownership_status": null,
  "documents_available": null,
  "loan_amount": null,
  "occupation": null,
  "income_mode": null,
  "market_value": null,
  "tenure_years": null,
  "existing_property_loan": false,
  "emi_reduction_request": false,
  "disqualified": true,
  "disqualification_reason": "agricultural_property",
  "transfer_required": false,
  "final_outcome": "disqualified"
}
```

---

## Verification Checklist

| Check | Result |
|---|---|
| Agent greeted warmly without assuming name | ✅ |
| Agent handled mid-identification correction (Viral → Bilal) gracefully | ✅ |
| Agent apologized and used corrected name | ✅ |
| Offer presented briefly | ✅ |
| Q1 asked cleanly | ✅ |
| Customer's "agricultural land" answer correctly parsed | ✅ |
| Agent disqualified IMMEDIATELY — did NOT ask Q2–Q7 | ✅ |
| Disqualification message was polite, not pushy | ✅ |
| Agent used FEMALE Hindi form (`samajh gayi`) | ✅ |
| No attempt to persuade or override rule | ✅ |
| Call ended cleanly via `end_call` tool | ✅ |
| **Result** | ✅ **PASS** |

---

## Observations & Notes

### What worked well
- **Handled identity correction gracefully** — the user first said "Viral"
  and then corrected to "Bilal." The agent apologized and used the
  corrected name for the rest of the call. ✅
- **Immediate disqualification** — upon hearing "agricultural land"
  at 0:45, the agent disqualified within 4 seconds (0:49) and did NOT
  ask any further eligibility questions. ✅
- **Empathetic tone** — "Main samajh gayi" (I understand) prefixed the
  disqualification, softening the rejection. ✅
- **Feminine grammar maintained** — "Main samajh gayi" is the correct
  FEMALE past-tense form. ✅
- **Clean exit** — agent used `end_call` tool properly and wished the
  customer a good day. ✅

### Edge case handled
- **Identity correction (Viral → Bilal):** This is exactly the kind of
  natural conversational turn the assignment brief asked the agent to
  handle ("Do not design the bot to only handle Yes/No answers").
  The agent updated its internal `customer_name` state from "Viral" to
  "Bilal" without re-verifying or breaking flow. ✅

### Key rule validation
- `property_type: Agricultural` → ✅ **Disqualified immediately** per
  Section 6 → Q1 rule.
- No further questions asked → ✅ per assignment Section 3
  "Immediate Disqualification."
- Politely informed → ✅ per assignment requirement.
- Call ended → ✅ per assignment requirement.

**Final classification:** DISQUALIFIED → Call ended at 1:12.
