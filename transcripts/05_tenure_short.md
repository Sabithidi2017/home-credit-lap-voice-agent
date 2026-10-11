# Test 5 — Tenure Too Short (Disqualification)

**Recording:** https://drive.google.com/file/d/1YxafwcRYn3WdjfCjC3aFDIXx_9e_SssL/view?usp=sharing
**Transcript source:** Retell AI call log
**Duration:** ~1m 53s
**Date tested:** 2026-10-11
**Language:** Hinglish (English → Hindi mirroring)
**Agent gender:** Female (feminine Hindi forms enforced)

---

## Scenario

Customer provides all data points correctly up through Q6, but requests
a **2-year tenure** at Q7. Expected outcome: **immediate disqualification
at Q7** — no call-back offer, polite close, call ends.

### Key requirement being tested

> "The repayment period must be between 3 and 15 years."
> — Assignment brief, Section 2C (Q7)

The agent must NOT:
- Offer an alternative tenure (like 3 years) as a workaround
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
| 4 | Loan amount | ₹30,00,000 (30 lakhs) |
| 5 | Occupation + Income mode | Salaried + Bank |
| 6 | Market value | ₹60,00,000 (60 lakhs) |
| 7 | Tenure | **2 years** ❌ (below 3-year minimum) |

---

## Full Transcript

**Agent (0:00):** Hello, and welcome! This is Voice AI agent calling from Mohamed's Voice Agent. I hope you're doing well today. May I know who I'm speaking with?

**User (0:09):** Yes. This is Mohammed.

**Agent (0:12):** Thank you, Mohammed. Is this a good time to talk?

**User (0:15):** Yes.

**Agent (0:17):** Thank you, Mohammed. As a valued customer, you have a pre-approved Loan Against Property offer of up to ₹75 lakhs. I'll ask a few basic questions to see if it—

**User (0:28):** Yes.

**Agent (0:28):** —suits you — is that okay?

**Agent (0:30) [Q1]:** Mohammed ji, aapka property kaunsa hai — residential house, flat, commercial shop, office, ya industrial?

**User (0:38):** (unintelligible audio)

**Agent (0:40):** Can you repeat that, please? Is it a residential, commercial, or industrial property?

**User (0:44):** Residential.

**Agent (0:46) [Q2]:** Okay. Aap sole owner hain ya property jointly owned hai, family member ya partner ke saath?

**User (0:55):** Sole.

**Agent (0:57) [Q3]:** Are the original property documents available for verification?

**User (1:01–1:02):** Yes. Documents are available.

**Agent (1:04) [Q4]:** Aap kitna loan lena chahte hain?

**User (1:07):** Thirty lakhs.

**Agent (1:09) [Q5]:** Are you salaried or self-employed, and does your income come into your bank account—

**User (1:14):** Yes. Salaried with—

**Agent (1:14):** —or in cash?

**User (1:15):** —bank income.

**Agent (1:17) [Q6]:** Property ki approximate current market value kitni hai?

**User (1:21):** Worth up to sixty lakhs.

**Agent (1:23) [Q7]:** Aap kitne saalon mein loan repay karna chahenge?

**User (1:29):** I want to repay in two years.

**Agent (1:32) [DISQUALIFY]:** Is offer mein repayment tenure 3 se 15 saal ke beech honi chahiye. Aapka tenure match nahi karta, isliye yeh offer applicable nahi hoga. Dhanyavaad.

**User (1:45):** Then here what?

**Agent (1:47):** For a two-year tenure, this offer isn't applicable because the minimum is three years. Thank you for your—

**Tool called:** `end_call` (1:53)

**Agent (1:53):** —time, Mohammed.

*(Call ends)*

---

## Post-Call Extraction (Retell)

```json
{
  "customer_name": "Mohammed",
  "customer_verified": true,
  "customer_available": true,
  "callback_time": null,
  "property_type": "Residential",
  "ownership_status": "Sole",
  "documents_available": true,
  "loan_amount": 3000000,
  "occupation": "Salaried",
  "income_mode": "Bank",
  "market_value": 6000000,
  "tenure_years": 2,
  "existing_property_loan": false,
  "emi_reduction_request": false,
  "disqualified": true,
  "disqualification_reason": "tenure_below_3_years",
  "transfer_required": false,
  "final_outcome": "disqualified"
}
```

---

## Verification Checklist

| Check | Result |
|---|---|
| Agent greeted warmly without assuming name | ✅ |
| Agent captured name from voice | ✅ (Mohammed) |
| Agent confirmed availability | ✅ |
| Offer presented briefly | ✅ |
| Q1 asked cleanly | ✅ |
| Agent handled unintelligible audio by re-asking (not guessing) | ✅ |
| Q2–Q6 captured correctly | ✅ |
| Compound Q5 correctly asked (occupation + income mode together) | ✅ |
| Q7 asked, answer captured | ✅ |
| Two-year tenure correctly identified as out of range | ✅ |
| Agent disqualified immediately at Q7 | ✅ |
| Agent did NOT offer a workaround (e.g., "what about 3 years?") | ✅ |
| Reason clearly explained (min 3, max 15) | ✅ |
| Agent handled customer's follow-up "Then here what?" politely | ✅ |
| Female Hindi grammar maintained | ✅ |
| Call ended cleanly via `end_call` tool | ✅ |
| **Result** | ✅ **PASS** |

---

## Observations & Notes

### What worked well
- **Handled unintelligible audio correctly at Q1** — when the customer's
  first answer was garbled, the agent did NOT guess. It said "Can you
  repeat that, please?" and re-asked with the three valid options
  (residential, commercial, industrial). This is exactly the correct
  behavior for voice agents. ✅
- **Compound Q5 split across interruptions** — the customer started
  answering at 1:14 ("Yes. Salaried with—") before the agent finished
  asking ("—or in cash?"). The agent smoothly completed the question,
  and the customer finished with "bank income." The agent correctly
  combined the split answers into `occupation: Salaried`,
  `income_mode: Bank`. ✅ This is excellent conversational state
  management.
- **Two-year tenure detected as out of range** — the agent immediately
  disqualified within 3 seconds of hearing "two years" (1:29 → 1:32). ✅
- **No workaround offered** — critically, the agent did NOT say "Would
  you like to consider 3 years instead?" This is correct per assignment
  rules: tenure out of range = disqualify, not negotiate. ✅
- **Feminine Hindi forms maintained** — the agent stayed in feminine
  first-person throughout (though Q5's "Are you salaried..." was in
  English because the customer had switched to English earlier). ✅
- **Handled follow-up question from customer** — when the customer
  asked "Then here what?" at 1:45, the agent didn't panic or go silent;
  it politely clarified the reason again ("minimum is three years")
  before ending the call. ✅ This shows composure under real-world
  customer pushback.

### Minor observation
- The agent delivered the disqualification in Hindi, but the customer
  had primarily been speaking English during Q4–Q7. The customer
  still understood (they responded with a question in English). For
  maximum alignment, the agent could have mirrored the customer's
  English at the disqualification message too. This is a
  Hinglish-mirroring nuance — worth noting in the approach doc as a
  potential refinement.

### Key rule validation
- `tenure_years: 2` < 3 → ✅ **Disqualified immediately** per Section
  6 → Q7.
- No alternative tenure offered → ✅ correct per assignment strictness.
- Reason explained to customer → ✅ per assignment requirement.
- Handled customer's objection politely → ✅ good UX.

**Final classification:** DISQUALIFIED → Call ended at 1:53.
