# Test 9 — Joint Ownership (Eligible)

**Recording:** https://drive.google.com/file/d/1AsWm9zEvk0di2yYGpTVINCzA72sRNd7c/view?usp=sharing
**Transcript source:** Retell AI call log
**Duration:** ~1m 53s
**Date tested:** 2026-10-11
**Language:** Hinglish (English → Hindi mirroring)
**Agent gender:** Female (feminine Hindi forms enforced)

---

## Scenario

Customer's property is **jointly owned with their wife** — a case that
must NOT be disqualified.

Expected outcome: **fully qualified → handoff to senior loan expert**.

### Key requirement being tested

> "Ownership Status: Find out if the customer is the sole owner or if
> it is a joint property (with family/partners).
> Eligibility: **Both Sole and Joint ownership are eligible.**"
> — Assignment brief, Section 2C (Q2)

The agent must NOT treat joint ownership as a disqualifier.

---

## Facts captured

| # | Data Point | Value |
|---|---|---|
| 1 | Property type | Residential (flat) |
| 2 | Ownership | **Joint** (with wife) |
| 3 | Original documents | Available |
| 4 | Loan amount | ₹50,00,000 (50 lakhs) |
| 5 | Occupation + Income mode | Salaried + Bank |
| 6 | Market value | ₹1,00,00,000 (1 crore) |
| 7 | Tenure | 10 years |

---

## Full Transcript

**Agent (0:00):** Hello, and welcome! This is Voice AI agent calling from Mohamed's Voice Agent. I hope you're doing well today. May I know who I'm speaking with?

**User (0:09):** Yes. It's Bilal.

**Agent (0:11):** Thank you, Bilal. Is this a good time to talk?

**User (0:14):** Yes.

**Agent (0:16):** Thank you, Bilal. As a valued customer, you have a pre-approved Loan Against Property offer of up to ₹75 lakhs. I'll ask a few basic questions to see if it suits you—

**User (0:28):** Yes.

**Agent (0:28):** —is that okay?

**Agent (0:31) [Q1]:** Bilal ji, aapka property residential house, flat, commercial shop, office, ya industrial hai?

**User (0:39):** It's a residential flat.

**Agent (0:42) [Q2]:** Is the flat solely owned by you, or jointly owned with a family member or partner?

**User (0:48):** Jointly owned with my wife.

**Agent (0:50) [Q3]:** Are the original property documents available for verification?

**User (0:54):** Yes. Original papers are available.

**Agent (0:57):** I need to confirm original property documents are available for verification. Are—

**User (1:01):** Yes.

**Agent (1:01):** —the originals available or producible?

**User (1:08):** Yes. They are available.

**Agent (1:09) [Q4]:** Thank you. How much loan would you like to take?

**User (1:13):** Fifty lakhs needed.

**Agent (1:16) [Q5]:** Are you salaried or self-employed, and does your income come into your bank account or—

**User (1:22):** The—

**Agent (1:22):** —mainly in cash?

**User (1:22):** —salaried. Bank income.

**Agent (1:25) [Q6]:** What's the approximate current market value of the property?

**User (1:29):** Worth up to one crore.

**Agent (1:31) [Q7]:** How many years would you like to take to repay—

**User (1:35):** Ten years.

**Agent (1:35):** —the loan?

**User (1:36):** Ten years.

**Agent (1:38) [HANDOFF]:** Thank you so much, Bilal. Based on what you've shared, you meet our preliminary criteria for the Loan Against Property offer. A senior loan expert from Mohamed's Voice Agent will call you shortly to discuss the next steps and exact interest rate.

**Tool called:** `end_call` (1:53)

**Agent (1:52):** Thank you for your time.

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
  "ownership_status": "Joint",
  "documents_available": true,
  "loan_amount": 5000000,
  "occupation": "Salaried",
  "income_mode": "Bank",
  "market_value": 10000000,
  "tenure_years": 10,
  "existing_property_loan": false,
  "emi_reduction_request": false,
  "disqualified": false,
  "disqualification_reason": null,
  "transfer_required": false,
  "final_outcome": "qualified"
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
| Q1 asked, answer captured (Residential flat) | ✅ |
| Q2 asked, answer captured (Joint with wife) | ✅ |
| **Agent did NOT disqualify on Joint ownership** | ✅ |
| Q3 asked, agent asked for clarification on ambiguous answer | ✅ |
| Q4–Q7 asked, answers captured | ✅ |
| Agent used FEMALE Hindi forms where applicable | ✅ |
| Handoff only after all 7 captured | ✅ |
| Handoff phrasing avoided "approved"/"sanctioned" | ✅ |
| Call ended cleanly via `end_call` tool | ✅ |
| **Result** | ✅ **PASS** |

---

## Observations & Notes

### What worked well
- **Joint ownership treated as ELIGIBLE** — the customer said "Jointly
  owned with my wife" at 0:48. The agent did NOT disqualify and
  continued smoothly to Q3. ✅ This is the key pass criterion for this
  test.
- **Clarification loop on Q3** — the customer's answer was somewhat
  garbled ("Virtual papers are available" — likely ASR mis-transcription
  of "Original papers are available"). The agent asked a clarifying
  question ("the originals available or producible?") instead of
  guessing. Customer confirmed at 1:08. ✅
- **Handled split Q5** — the customer answered "The..." at 1:22 while
  the agent was still asking "—mainly in cash?". The agent completed
  the question, customer finished with "salaried. Bank income." The
  agent correctly combined the split answer. ✅
- **Handoff phrasing correct** — "you meet our preliminary criteria" —
  did NOT say "approved" or "sanctioned". ✅
- **Handoff only after all 7 captured** — the agent waited until
  tenure was confirmed at 1:36 before starting the handoff. ✅

### Notable test coverage
This test is the **counterpart to Test 2 and Test 3**, which were
disqualification cases. It demonstrates the agent correctly does
**NOT** disqualify on joint ownership (a common false-positive risk
for poorly designed agents).

### Key rule validation
- `ownership_status: Joint` → ✅ **NOT disqualified** (per Section
  2C Q2: "Both Sole and Joint ownership are eligible.")
- All 7 data points captured sequentially → ✅
- Handoff executed at 1:38 → ✅

### Minor observation
- The agent used "Bilal ji" (Hindi respectful address) at Q1 but then
  used "Bilal" (no suffix) in the handoff. Both are acceptable. For
  maximum consistency, the agent could continue using "Bilal ji" or
  alternate naturally. Not an issue.

**Final classification:** QUALIFIED → Handoff executed at 1:38.
