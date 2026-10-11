# Test 3 — Cash Income (Disqualification)

**Recording:** https://drive.google.com/file/d/1nyptQwqRimiOPweXp4rYPkY9TuYS1wcj/view?usp=sharing
**Transcript source:** Retell AI call log
**Duration:** ~1m 33s
**Date tested:** [2026-10-11]
**Language:** Hinglish (English → Hindi mirroring)
**Agent gender:** Female (feminine Hindi forms enforced)

---

## Scenario

Customer is self-employed with cash income (not bank). Expected outcome:
**immediate disqualification at Q5** — no further eligibility questions
asked, polite close, call ends.

### Key requirement being tested

> "Both Salaried and Self-Employed are eligible, but the income must
> be received via Bank. If the income is in Cash, they are not eligible."
> — Assignment brief, Section 2C (Q5)

The agent must NOT continue to Q6 (market value) or Q7 (tenure) after
this disqualification.

---

## Facts captured before disqualification

| # | Data Point | Value |
|---|---|---|
| 1 | Property type | Residential (flat) |
| 2 | Ownership | Sole |
| 3 | Original documents | Available |
| 4 | Loan amount | ₹40,00,000 (40 lakhs) |
| 5 | Occupation + Income mode | Self-employed + **Cash** ❌ |
| 6 | Market value | (not asked — DQ'd) |
| 7 | Tenure | (not asked — DQ'd) |

---

## Full Transcript

**Agent (0:00):** Hello, and welcome! This is Voice AI agent calling from Mohamed's Voice Agent. I hope you're doing well today. May I know who I'm speaking with?

**User (0:10):** Mohammed Bilal?

**Agent (0:12):** Hi Bilal, is this a good time to—

**User (0:15):** Yes.

**Agent (0:15):** —talk?

**Agent (0:17):** Thank you, Bilal. As one of our valued customers, you have a pre-approved Loan Against Property offer of up to ₹75 lakhs; may I ask a few basic questions to see—

**User (0:29):** Yes.

**Agent (0:28):** —if it suits you?

**Agent (0:31) [Q1]:** Bilal ji, aapki property residential, commercial, ya industrial hai?

**User (0:37):** Residential flat.

**Agent (0:39) [Q2]:** Bilal ji, is property ke sole owner hain ya jointly owned hai?

**User (0:45):** Sole owner.

**Agent (0:46) [Q3]:** Okay, sole ownership noted. Are the original property documents available for verification?

**User (0:52):** Original papers are available.

**Agent (0:55) [Q4]:** Aap kitna loan lena chahte hain?

**User (0:58):** Forty lakhs.

**Agent (1:00) [Q5]:** Forty lakhs noted. Aap salaried hain ya self-employed, aur income bank account mein aati hai ya cash mein?

**User (1:07):** I am self-employed and my income comes in my cash.

**Agent (1:11) [DISQUALIFY]:** Thank you for sharing. Is offer ke liye income bank mein aani chahiye, aur cash income ke saath yeh offer applicable nahi hoga. Aapke time ke liye dhanyavaad.

**User (1:31):** Dhanyavaad.

**Agent (1:33):** Aapka bhi dhanyavaad. Aapka din shubh ho.

*(Call ends)*

---

## Post-Call Extraction (Retell)

```json
{
  "customer_name": "Bilal",
  "customer_verified": true,
  "customer_available": true,
  "callback_time": null,
  "property_type": "Residential",
  "ownership_status": "Sole",
  "documents_available": true,
  "loan_amount": 4000000,
  "occupation": "Self-employed",
  "income_mode": "Cash",
  "market_value": null,
  "tenure_years": null,
  "existing_property_loan": false,
  "emi_reduction_request": false,
  "disqualified": true,
  "disqualification_reason": "cash_income",
  "transfer_required": false,
  "final_outcome": "disqualified"
}
```

---

## Verification Checklist

| Check | Result |
|---|---|
| Agent greeted warmly without assuming name | ✅ |
| Agent captured name from voice | ✅ (Bilal) |
| Agent confirmed availability | ✅ |
| Offer presented briefly | ✅ |
| Q1–Q4 asked cleanly, answers captured | ✅ |
| Q5 asked with BOTH occupation and income mode in one question | ✅ |
| Cash income detected as disqualifier | ✅ |
| Agent disqualified IMMEDIATELY — did NOT ask Q6 or Q7 | ✅ |
| Disqualification message cited the correct rule (bank income required) | ✅ |
| Agent was polite, not pushy | ✅ |
| Female Hindi grammar maintained throughout | ✅ |
| Call ended cleanly | ✅ |
| **Result** | ✅ **PASS** |

---

## Observations & Notes

### What worked well
- **Number normalization** — customer said "Forty lakhs" (informal); the
  agent normalized to "Forty lakhs noted" and stored `loan_amount = 4000000`. ✅
- **Compound Q5 handled correctly** — the agent asked both the occupation
  AND income mode in a single natural question, matching the assignment
  checklist item exactly. ✅
- **Immediate disqualification after Q5** — the moment the customer said
  "income comes in my cash," the agent delivered the disqualification
  message within 4 seconds (1:07 → 1:11) and did NOT ask about market
  value or tenure. ✅
- **Rule clearly explained** — the agent stated the exact reason:
  "income bank mein aani chahiye" — this is transparent and helps the
  customer understand, which is good UX per the assignment brief. ✅
- **Polite close** — ended with "Aapka din shubh ho" (have a good day). ✅

### Minor observation
- The customer prefixed their name with "Mohammed" (i.e., "Mohammed Bilal").
  The agent shortened it to "Bilal" for the rest of the call. This is a
  reasonable choice — the agent chose the informal/callable form. If you
  want the agent to preserve the FULL name in future calls, you can add
  a rule to Section 1A: *"If the customer provides a full name, use the
  full name in the first acknowledgment, then shorten to their preferred
  short form if they use it themselves."*

### Key rule validation
- `income_mode: Cash` → ✅ **Disqualified immediately** per Section 6 → Q5.
- No Q6/Q7 asked → ✅ per assignment Section 3 "Immediate Disqualification."
- Reason explained → ✅ per assignment requirement.

**Final classification:** DISQUALIFIED → Call ended at 1:33.
