# Test 4 — No Original Documents (Disqualification)

**Recording:** https://drive.google.com/file/d/1t5CNvTGex73o6wq6vq1ysN6nVlbpUEqB/view?usp=sharing
**Transcript source:** Retell AI call log
**Duration:** ~1m 08s
**Date tested:** 2026-10-11
**Language:** English-dominant (agent mirrored customer's English)
**Agent gender:** Female (feminine Hindi forms available but not
triggered — customer stayed in English)

---

## Scenario

Customer has only photocopies of the property documents — no originals
available. Expected outcome: **immediate disqualification at Q3** — no
further eligibility questions asked, polite close, call ends.

### Key requirement being tested

> "The customer must have Original Documents available to proceed."
> — Assignment brief, Section 2C (Q3)

The agent must treat "photocopies only" as **unavailable** and disqualify
immediately — NOT continue to Q4–Q7.

---

## Facts captured before disqualification

| # | Data Point | Value |
|---|---|---|
| 1 | Property type | Residential |
| 2 | Ownership | Sole (implied) |
| 3 | Original documents | **Photocopies only** ❌ |
| 4 | Loan amount | (not asked — DQ'd) |
| 5 | Occupation + Income mode | (not asked — DQ'd) |
| 6 | Market value | (not asked — DQ'd) |
| 7 | Tenure | (not asked — DQ'd) |

---

## Full Transcript

**Agent (0:00):** Hello, and welcome! This is Voice AI agent calling from Mohamed's Voice Agent. I hope you're doing well today. May I know who I'm speaking with?

**User (0:09):** Yes. This is Willard.

**Agent (0:11):** Thank you, Willard. Is this a good time to talk?

**User (0:15):** Yes.

**Agent (0:16):** Thank you, Willard. As a valued customer, you have a pre-approved—

**User (0:21):** (unintelligible audio)

**Agent (0:23):** —I'm calling about a pre-approved Loan Against Property offer of up to ₹75 lakhs. I'd just ask a few basic questions to see if it—

**User (0:32):** Yes.

**Agent (0:32):** —suits you — does that work?

**Agent (0:35) [Q1]:** What type of property do you own — residential, commercial, or industrial?

**User (0:40):** Residential.

**Agent (0:42) [Q2]:** Is the property solely owned by you, or jointly owned—

**User (0:46):** Yes. Sole.

**Agent (0:46):** —with someone else?

**User (0:47):** (unintelligible)

**Agent (0:50) [Q3]:** Are your original property documents available for verification?

**User (0:55):** Yes. I have photocopies of the property documents.

**Agent (1:00) [DISQUALIFY]:** Thank you. Original property documents are required for verification, so we can't proceed with this offer. Thank you for your—

**Tool called:** `end_call` (1:07)

**Agent (1:08):** —time.

*(Call ends)*

---

## Post-Call Extraction (Retell)

```json
{
  "customer_name": "Willard",
  "customer_verified": true,
  "customer_available": true,
  "callback_time": null,
  "property_type": "Residential",
  "ownership_status": "Sole",
  "documents_available": false,
  "loan_amount": null,
  "occupation": null,
  "income_mode": null,
  "market_value": null,
  "tenure_years": null,
  "existing_property_loan": false,
  "emi_reduction_request": false,
  "disqualified": true,
  "disqualification_reason": "original_documents_unavailable",
  "transfer_required": false,
  "final_outcome": "disqualified"
}
```

---

## Verification Checklist

| Check | Result |
|---|---|
| Agent greeted warmly without assuming name | ✅ |
| Agent captured name from voice | ✅ (Willard) |
| Agent confirmed availability | ✅ |
| Offer presented briefly | ✅ |
| Q1 asked cleanly, answer captured | ✅ |
| Q2 asked cleanly, answer captured | ✅ |
| Q3 asked cleanly | ✅ |
| Agent correctly interpreted "photocopies only" as **NOT available** | ✅ |
| Agent disqualified IMMEDIATELY — did NOT ask Q4–Q7 | ✅ |
| Disqualification message cited correct reason (original docs required) | ✅ |
| Agent was polite, not pushy | ✅ |
| Call ended cleanly via `end_call` tool | ✅ |
| **Result** | ✅ **PASS** |

---

## Observations & Notes

### What worked well
- **Correct semantic parsing of "photocopies"** — the customer said "Yes.
  I have photocopies" — a confusing answer because they said "Yes" (which
  could be misread as "I have originals") but then clarified it was
  photocopies only. The agent correctly disqualified. This is a strong
  demonstration of the agent not being fooled by a surface-level "Yes." ✅
- **Immediate disqualification** — from hearing "photocopies" at 0:55,
  the agent disqualified by 1:00 (5 seconds) and did NOT ask any further
  questions. ✅
- **Rule clearly explained** — "Original property documents are required
  for verification, so we can't proceed." Clear, non-negotiable, polite. ✅
- **Clean exit** via `end_call` tool. ✅

### Minor observations
1. **Customer name "Willard"** — note that this is an ASR artifact. The
   customer likely said a different name (perhaps "Bilal" or a Hindi
   name), and the ASR mis-transcribed it as "Willard." This is a known
   limitation of English-optimized ASR on Hindi-accented English. If
   this becomes common in your testing, consider switching the ASR
   provider to Deepgram or Soniox (as noted in your Hindi accuracy fix).
   In this transcript, it does NOT affect the outcome since the
   disqualification is on documents, not name.

2. **Interruption at 0:21 and 0:42** — the customer's barge-ins split
   the agent's sentences mid-flow. The agent recovered gracefully each
   time by continuing the thought. This demonstrates good interruption
   handling. ✅

3. **Ambiguous Q2 answer** — at 0:46, the customer said "Yes. Solo" and
   then "wanna" (likely "want to say something"). The agent correctly
   interpreted "Solo" as "Sole owner" and moved on. ✅

### Key rule validation
- `documents_available: false` (photocopies only) → ✅ **Disqualified
  immediately** per Section 6 → Q3.
- No Q4–Q7 asked → ✅ per assignment Section 3 "Immediate Disqualification."
- Reason explained → ✅ per assignment requirement.

**Final classification:** DISQUALIFIED → Call ended at 1:08.
