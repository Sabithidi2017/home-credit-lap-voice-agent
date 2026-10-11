# Test 8 — Loan Amount > ₹75L, Declines (Polite Close)

**Recording:** https://drive.google.com/file/d/1MQSnWLlfr9vsdyf6wutJChT4QzCXGHjW/view?usp=sharing
**Transcript source:** Retell AI call log
**Duration:** ~0m 47s
**Date tested:** 2026-10-11
**Language:** English-dominant (agent mirrored customer's English)
**Agent gender:** Female (feminine Hindi forms available but not
triggered — customer stayed in English)

---

## Scenario

Customer requests ₹90 lakhs — above the ₹75 lakh maximum. The agent
offers the ₹75 lakh maximum alternative. The customer **declines**.

Expected outcome: **polite close** — no further eligibility questions,
no disqualification language (this isn't a "not eligible" case, it's a
"customer declined the alternative" case).

### Key requirement being tested

> "If a customer requests more than 75 Lakhs, explain the limit and ask
> if they would like to proceed with the maximum allowed amount."
> — Assignment brief, Section 3 (Loan Amount Limits)

The agent must:
- NOT disqualify
- Explain the limit
- Offer the ₹75L alternative
- If declined → politely close the call

---

## Facts captured

| # | Data Point | Value |
|---|---|---|
| 1 | Property type | (not asked — customer pre-empted) |
| 2 | Ownership | (not asked) |
| 3 | Original documents | (not asked) |
| 4 | Loan amount | ₹90,00,000 requested → **declined ₹75L** |
| 5 | Occupation + Income mode | (not asked) |
| 6 | Market value | (not asked) |
| 7 | Tenure | (not asked) |

---

## Full Transcript

**Agent (0:00):** Hello, and welcome! This is Voice AI agent calling from Mohamed's Voice Agent. I hope you're doing well today. May I know who I'm speaking with?

**User (0:09):** I'm Muhman Bilang.

**Agent (0:12):** Thank you, Muhman. Is this a good time to talk?

**User (0:15):** Yes.

**Agent (0:17):** Thank you, Muhman. As a valued customer, you have a pre-approved Loan Against Property offer of up to ₹75 lakhs. I'd like to ask a few basic questions to see—

**User (0:28):** Yes. But I need ninety—

**Agent (0:28):** —if it suits you — is that okay?

**User (0:30):** —lakhs.

**Agent (0:32):** The offer goes up to ₹75 lakhs. Would you like to proceed with ₹75 lakhs?

**User (0:38):** No. I want ninety lakhs.

**Agent (0:40) [POLITE CLOSE]:** That's completely fine. Thank you so much for your time today. Have a wonderful day.

**User (0:46):** Thank you.

**Agent (0:47):** You're welcome. Have a wonderful day.

*(Call ends)*

---

## Post-Call Extraction (Retell)

```json
{
  "customer_name": "Muhman",
  "customer_verified": true,
  "customer_available": true,
  "callback_time": null,
  "property_type": null,
  "ownership_status": null,
  "documents_available": null,
  "loan_amount": 9000000,
  "occupation": null,
  "income_mode": null,
  "market_value": null,
  "tenure_years": null,
  "existing_property_loan": false,
  "emi_reduction_request": false,
  "disqualified": false,
  "disqualification_reason": null,
  "transfer_required": false,
  "final_outcome": "declined_max_amount"
}
```

---

## Verification Checklist

| Check | Result |
|---|---|
| Agent greeted warmly without assuming name | ✅ |
| Agent captured name from voice | ✅ (Muhman) |
| Agent confirmed availability | ✅ |
| Offer presented briefly | ✅ |
| Customer's "I need ninety lakhs" correctly parsed | ✅ |
| Agent did NOT disqualify | ✅ |
| Agent explained the ₹75L limit clearly | ✅ |
| Agent offered the ₹75L alternative | ✅ |
| Customer declined → agent politely closed | ✅ |
| Agent did NOT continue with Q1–Q7 | ✅ |
| Agent did NOT try to persuade or push | ✅ |
| Agent did NOT say "you're not eligible" | ✅ |
| Call ended politely (no `end_call` tool call shown — ended naturally) | ✅ |
| **Result** | ✅ **PASS** |

---

## Observations & Notes

### What worked well
- **Immediate recognition of >75L limit** — the customer interrupted
  the agent's offer presentation at 0:28 ("Yes. But I need ninety
  lakhs") and the agent did NOT continue with the presentation.
  It pivoted immediately to the limit explanation and offer. ✅
- **Correct handling of the >75L case** — the agent said "The offer
  goes up to ₹75 lakhs. Would you like to proceed with ₹75 lakhs?"
  This matches the assignment requirement exactly. ✅
- **Polite close on decline** — the customer's refusal ("No. I want
  ninety lakhs") was met with "That's completely fine. Thank you so
  much for your time today. Have a wonderful day." No pushiness. ✅
- **Did NOT disqualify** — critically, the agent did NOT use
  disqualification language like "You don't meet the criteria." It
  correctly treated this as a "customer declined the alternative"
  case, not a "customer failed eligibility" case. This distinction
  is important and the agent got it right. ✅
- **Did NOT continue with Q1–Q7** — the agent recognized that there
  was no point in asking eligibility questions after the customer
  refused the offer. ✅
- **Warm closing tone** — "Have a wonderful day" repeated twice,
  keeping the tone courteous. ✅

### Minor observations
1. **Name ASR issue** — "Muhman Bilang" is likely an ASR
   mis-transcription of the customer's actual name. Given the customer
   introduced themselves as "Muhman Bilang," the agent used "Muhman"
   consistently. If the ASR had been more accurate (e.g., with
   boosted keywords including Indian names), it might have captured
   the name correctly. This is a minor ASR limitation, not a prompt
   issue.

2. **No explicit `end_call` tool called** — unlike previous tests, the
   transcript does not show a tool call for ending the call. The call
   ended naturally after the agent's final "Have a wonderful day."
   This is fine for a declined case — the agent kept the call open
   for a moment to allow the customer a final word, then the call
   ended. If you want to be strict about tool-call endings, you can
   add a rule in Section 7A of the prompt:
   *"After the polite close message for declined cases, call the
   end_call tool."*

3. **Customer pre-empted the offer** — the customer volunteered the
   loan amount before the agent even asked Q4. The agent correctly
   captured this out-of-order information and used it immediately
   rather than asking Q4 later. ✅

### Key rule validation
- `loan_amount: 9000000` > ₹75L → ✅ Agent explained the limit and
  offered ₹75L alternative (per assignment Section 3).
- Customer declined → ✅ Polite close, no further questions.
- Did NOT disqualify → ✅ Correct — this is a "declined alternative"
  case, not a "not eligible" case.

### Comparison with disqualification tests
- Test 2 (agricultural): Customer **fails** a criterion → DISQUALIFY.
- Test 3 (cash income): Customer **fails** a criterion → DISQUALIFY.
- Test 7 (this test): Customer requests > limit and **declines**
  alternative → POLITE CLOSE (not DQ).

The distinction is important — the agent correctly treated these
as different outcomes. ✅

**Final classification:** DECLINED ALTERNATIVE → Call ended at 0:47.
