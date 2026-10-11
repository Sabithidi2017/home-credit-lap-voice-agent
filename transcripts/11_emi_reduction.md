# Test 11 — EMI Reduction → Transfer Branch

**Recording:** https://drive.google.com/file/d/1yCBxjpsSmiar-E_brGBGI2B4Ng846A1D/view?usp=sharing
**Transcript source:** Retell AI call log
**Duration:** ~0m 21s
**Date tested:** 2026-10-11
**Language:** English-dominant (agent mirrored customer's English)
**Agent gender:** Female

---

## Scenario

Customer mentions they want to **reduce their current EMI** — a second
type of trigger for the **transfer branch** (separate from
existing-loan but routed the same way).

Expected outcome: **immediate transfer message → end call**. The agent
must NOT ask any of the 7 eligibility questions.

### Key requirement being tested

> "The 'Transfer' Logic Switch: Our standard flow is for a 'Fresh Loan.'
> However, if a customer mentions they already have an existing loan on
> the property **or want to reduce their current EMI**, the bot must
> inform the customer that a specialist for loan transfer will contact
> them shortly and then end the call."
> — Assignment brief, Section 3 (emphasis added)

The agent must recognize **EMI reduction intent** as a transfer trigger,
not just existing-loan declarations.

---

## Facts captured

| # | Data Point | Value |
|---|---|---|
| 1–7 | (not asked — transfer branch taken) | — |
| EMI reduction request | Yes → transfer | ✅ |

---

## Full Transcript

**Agent (0:00):** Hello, and welcome! This is Voice AI agent calling from Mohamed's Voice Agent. I hope you're doing well today. May I know who I'm speaking with?

**User (0:09):** Yes. It's Bilal. Want to reduce my current EMI.

**Agent (0:14) [TRANSFER]:** Since you'd like to reduce your current EMI, a loan-transfer specialist will handle your case and contact you—

**Tool called:** `end_call` (0:21)

**Agent (0:21):** —shortly. Thank you for your time, Bilal.

*(Call ends)*

---

## Post-Call Extraction (Retell)

```json
{
  "customer_name": "Bilal",
  "customer_verified": true,
  "customer_available": true,
  "callback_time": null,
  "property_type": null,
  "ownership_status": null,
  "documents_available": null,
  "loan_amount": null,
  "occupation": null,
  "income_mode": null,
  "market_value": null,
  "tenure_years": null,
  "existing_property_loan": false,
  "emi_reduction_request": true,
  "disqualified": false,
  "disqualification_reason": null,
  "transfer_required": true,
  "final_outcome": "transfer"
}
```

---

## Verification Checklist

| Check | Result |
|---|---|
| Agent greeted warmly without assuming name | ✅ |
| Agent captured name from voice | ✅ (Bilal) |
| Agent did NOT proceed to offer presentation | ✅ |
| Agent immediately recognized EMI-reduction intent | ✅ |
| Agent did NOT ask Q1 (property type) or any other eligibility question | ✅ |
| Agent informed customer about loan-transfer specialist | ✅ |
| Agent did NOT confuse EMI reduction with existing-loan (both map to transfer) | ✅ |
| Agent did NOT use disqualification language | ✅ |
| Agent was polite, brief, and warm | ✅ |
| Call ended cleanly via `end_call` tool | ✅ |
| **Result** | ✅ **PASS** |

---

## Observations & Notes

### What worked well
- **Blazing-fast recognition** — the customer said "Want to reduce my
  current EMI" at 0:09. The agent pivoted to the transfer branch at
  0:14 — a 5-second response. ✅
- **Skipped everything else** — no offer presentation, no Q1–Q7. The
  agent recognized the intent on the very first customer turn. ✅
- **Correctly mapped EMI → transfer** — the assignment treats EMI
  reduction as a separate trigger from existing-loan, but both route
  to the same specialist. The agent correctly routed EMI reduction
  cases to the transfer specialist. ✅
- **Brief and warm** — total call duration 21 seconds. Efficient
  without being curt. ✅
- **Used customer's name once** — "Bilal" appears once at the end.
  Not overused. ✅
- **Clean tool exit** — `end_call` invoked properly. ✅

### Key rule validation
- `emi_reduction_request: true` → ✅ **Transfer branch triggered
  immediately** per Section 5B.
- No eligibility questions asked → ✅ per assignment Section 3.
- Customer informed about specialist → ✅ per assignment.
- Call ended → ✅ per assignment.

### Comparison with Test 10 (existing loan)
Both Test 10 and Test 11 route to the same **transfer** outcome, but
via different triggers:
- **Test 10** — existing-loan declaration → transfer
- **Test 11** — EMI reduction request → transfer

The agent correctly handled both triggers. This is strong evidence
that the prompt's Section 5B covers **all** transfer-eligible intents,
not just the primary one. ✅

### Minor observation
- **End-call timing** — the `end_call` tool was invoked at 0:21 while
  the agent was mid-sentence ("...contact you—" → tool call → "—shortly.").
  The audio continued playing after the tool fired. This is Retell's
  "finish speaking before ending" behavior, which is correct — it
  ensures the customer hears the complete message. ✅

**Final classification:** TRANSFER → Call ended at 0:21.
