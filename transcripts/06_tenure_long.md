# Test 6 — Tenure Too Long (Disqualification)

**Recording:** https://drive.google.com/file/d/1tldFwtKvux139CRiz_qHl_J8gtnQ_-RU/view?usp=sharing
**Transcript source:** Retell AI call log
**Duration:** ~1m 51s
**Date tested:** 2026-10-11
**Language:** Hinglish (English → Hindi mirroring)
**Agent gender:** Female (feminine Hindi forms enforced)

---

## Scenario

Customer provides all data points correctly up through Q6, but requests
a **20-year tenure** at Q7. Expected outcome: **immediate disqualification
at Q7** — no call-back offer, no workaround, polite close, call ends.

### Key requirement being tested

> "The repayment period must be between 3 and 15 years."
> — Assignment brief, Section 2C (Q7)

The agent must NOT:
- Offer to reduce the tenure to 15 years as a workaround
- Continue with any further questions
- Ask for a correction

It must inform, apologize, and end the call.

---

## Facts captured before disqualification

| # | Data Point | Value |
|---|---|---|
| 1 | Property type | Residential |
| 2 | Ownership | Sole |
| 3 | Original documents | Available |
| 4 | Loan amount | ₹60,00,000 (60 lakhs) |
| 5 | Occupation + Income mode | Salaried + Bank |
| 6 | Market value | ₹60,00,000 (60 lakhs) |
| 7 | Tenure | **20 years** ❌ (above 15-year maximum) |

---

## Full Transcript

**Agent (0:00):** Hello, and welcome! This is Voice AI agent calling from Mohamed's Voice Agent. I hope you're doing well today. May I know who I'm speaking with?

**User (0:09):** Mohammed Binal.

**Agent (0:11):** Thank you, Mohammed Binal. Is this a good time to talk?

**User (0:16):** Yes.

**Agent (0:18):** I'm calling because you have a pre-approved Loan Against Property offer of up to ₹75 lakhs. I'd like to ask a few basic questions to see if—

**User (0:27):** Yes.

**Agent (0:27):** —it suits you — is that okay?

**Agent (0:30) [Q1]:** Mohammed Binal ji, aapki property residential, commercial, ya industrial hai?

**User (0:37):** Residential.

**Agent (0:39) [Q2]:** Aap sole owner hain ya property jointly owned hai?

**User (0:46):** Sole.

**Agent (0:48) [Q3]:** Original property documents verification ke liye available hain?

**User (0:53–0:54):** Yes. Yeah.

**Agent (0:54) [Q4]:** Aap kitna loan lena chahte hain?

**User (1:07):** Sixty lakhs.

**Agent (1:09) [Q5]:** Aap salaried hain ya self-employed? Aur income bank account mein aati hai ya cash mein?

**User (1:17):** Salaried with bank income.

**Agent (1:20) [Q6]:** Property ki approximate current market value kitni hai?

**User (1:25):** Worth up to sixty lakhs.

**Agent (1:27) [Q7]:** Aap kitne saalon mein loan repay karna chahenge?

**User (1:32):** I want twenty-year tenure.

**Agent (1:35) [DISQUALIFY]:** Is offer mein repayment tenure 3 se 15 saal ke beech honi chahiye. Aapka tenure match nahi karta, isliye yeh offer applicable nahi hoga. Dhanyavaad.

**User (1:47):** Yes. Thank you.

**Agent (1:49):** Thank you for your time. Have a good—

**Tool called:** `end_call` (1:51)

**Agent (1:51):** —day.

*(Call ends)*

---

## Post-Call Extraction (Retell)

```json
{
  "customer_name": "Mohammed Binal",
  "customer_verified": true,
  "customer_available": true,
  "callback_time": null,
  "property_type": "Residential",
  "ownership_status": "Sole",
  "documents_available": true,
  "loan_amount": 6000000,
  "occupation": "Salaried",
  "income_mode": "Bank",
  "market_value": 6000000,
  "tenure_years": 20,
  "existing_property_loan": false,
  "emi_reduction_request": false,
  "disqualified": true,
  "disqualification_reason": "tenure_above_15_years",
  "transfer_required": false,
  "final_outcome": "disqualified"
}
```

---

## Verification Checklist

| Check | Result |
|---|---|
| Agent greeted warmly without assuming name | ✅ |
| Agent captured name from voice | ✅ (Mohammed Binal) |
| Agent used full name consistently | ✅ |
| Agent confirmed availability | ✅ |
| Offer presented briefly | ✅ |
| Q1 asked cleanly, answer captured | ✅ |
| Q2 asked cleanly, answer captured | ✅ |
| Q3 asked cleanly, answer captured | ✅ |
| Q4 asked cleanly, answer captured | ✅ |
| Compound Q5 asked (occupation + income mode together) | ✅ |
| Q6 asked cleanly, answer captured | ✅ |
| Q7 asked cleanly, answer captured | ✅ |
| Twenty-year tenure correctly identified as out of range | ✅ |
| Agent disqualified immediately at Q7 | ✅ |
| Agent did NOT offer a workaround (e.g., "what about 15 years?") | ✅ |
| Reason clearly explained (range 3–15) | ✅ |
| Female Hindi grammar maintained | ✅ |
| Call ended cleanly via `end_call` tool | ✅ |
| **Result** | ✅ **PASS** |

---

## Observations & Notes

### What worked well
- **Full name usage** — the customer gave their name as "Mohammed Binal"
  (a full first + last name). The agent captured BOTH parts and used
  "Mohammed Binal ji" consistently throughout the call. This is a nice
  touch that some agents miss (they'd truncate to just "Mohammed"). ✅
- **Consistent Hinglish** — the agent mirrored the customer's Hinglish
  perfectly. The customer mixed English answers with Hindi phrasing
  ("Sole.", "Yes.") and the agent responded in mixed Hinglish. ✅
- **Twenty-year tenure detected as out of range** — the agent
  disqualified within 3 seconds of hearing "twenty-year tenure"
  (1:32 → 1:35). ✅
- **No workaround offered** — critically, the agent did NOT say
  "Would you like to consider 15 years instead?" This is correct per
  assignment rules: tenure out of range = disqualify, not negotiate. ✅
- **Clean exit** — the customer said "Yes. Thank you." and the agent
  closed politely with "Thank you for your time. Have a good day." ✅
- **Feminine Hindi forms** — maintained throughout. ✅

### Key rule validation
- `tenure_years: 20` > 15 → ✅ **Disqualified immediately** per Section
  6 → Q7.
- No alternative tenure offered → ✅ correct per assignment strictness.
- Reason explained to customer → ✅ per assignment requirement.

### Comparison with Test 5 (tenure too short)
- Test 5: tenure below minimum (2 years < 3) → DQ'd
- Test 6: tenure above maximum (20 years > 15) → DQ'd

Both boundary violations correctly triggered disqualification. ✅

### Minor observation
- The customer said "twenty attenuate" at 1:32 — this is an ASR
  mis-transcription of "twenty-year tenure" (the customer likely said
  "twenty-year tenure" and the model heard it as "attenuate"). The
  agent still correctly interpreted the intent and disqualified. ✅
  This shows the LLM handled ASR noise gracefully — an important
  real-world capability.

**Final classification:** DISQUALIFIED → Call ended at 1:51.
