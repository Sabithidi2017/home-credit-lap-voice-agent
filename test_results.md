# Test Results — Home Credit LAP Voice Agent

**Agent:** Voice AI agent (Female)
**Company:** Mohamed's Voice Agent
**Platform:** Retell AI
**Total Scenarios Tested:** 13 (12 required + 1 adversarial)
**Date of Testing:** 2026-10-11

---

## 📊 Summary

| Metric | Value |
|---|---|
| Total tests run | 13 |
| Passed | 13 / 13 |
| Failed | 0 |
| Qualified handoffs | 2 (Tests 1, 9) |
| Disqualifications | 7 (Tests 2, 3, 4, 5, 6, 7, 13) |
| Transfers | 2 (Tests 10, 11) |
| Polite closes / no action | 2 (Tests 8, 12) |
| Adversarial checks passed | 1 / 1 (Test 13) |

> **Note:** All recording links are stored at the top of each
> transcript file inside [`transcripts/`](./transcripts/). Click the
> transcript link in the table below to access the recording.

---

## ✅ Required Scenarios

---

### Test 1 — Perfect Eligible Customer

| Field | Value |
|---|---|
| **Scenario** | All 7 data points valid |
| **Customer input** | "Yes, this is Bilal. Residential flat, sole owner, original papers available, need 50 lakhs, salaried with bank income, worth 1 crore, 15-year tenure." |
| **Expected** | All 7 captured → Handoff |
| **Actual** | All 7 captured → Handoff |
| **Transcript (incl. recording link)** | [./transcripts/01_eligible.md](./transcripts/01_eligible.md) |
| **Post-call extraction** | `final_outcome: qualified` |
| **Result** | ✅ PASS |

---

### Test 2 — Agricultural Property (Immediate Disqualification)

| Field | Value |
|---|---|
| **Scenario** | Property is agricultural land |
| **Customer input** | "Yes, this is Bilal. It's agricultural land — kheti ki zameen." |
| **Expected** | Disqualify at Q1, polite exit, no further questions |
| **Actual** | Disqualified at Q1, call ended politely |
| **Transcript (incl. recording link)** | [./transcripts/02_agricultural.md](./transcripts/02_agricultural.md) |
| **Post-call extraction** | `disqualified: true`, `reason: agricultural_property` |
| **Result** | ✅ PASS |

---

### Test 3 — Cash Income (Disqualification)

| Field | Value |
|---|---|
| **Scenario** | Self-employed with cash income |
| **Customer input** | "Residential flat, sole owner, original papers available, 40 lakhs. Self-employed, income comes in cash." |
| **Expected** | Disqualify at Q5, polite exit |
| **Actual** | Disqualified at Q5 |
| **Transcript (incl. recording link)** | [./transcripts/03_cash_income.md](./transcripts/03_cash_income.md) |
| **Post-call extraction** | `disqualified: true`, `reason: cash_income` |
| **Result** | ✅ PASS |

---

### Test 4 — No Original Documents (Disqualification)

| Field | Value |
|---|---|
| **Scenario** | Customer has only photocopies |
| **Customer input** | "Residential, sole owner, I have photocopies of the property documents." |
| **Expected** | Disqualify at Q3 |
| **Actual** | Disqualified at Q3 |
| **Transcript (incl. recording link)** | [./transcripts/04_no_documents.md](./transcripts/04_no_documents.md) |
| **Post-call extraction** | `disqualified: true`, `reason: original_documents_unavailable` |
| **Result** | ✅ PASS |

---

### Test 5 — Tenure Too Short (Disqualification)

| Field | Value |
|---|---|
| **Scenario** | Customer requests 2-year tenure |
| **Customer input** | "Residential, sole, originals available, 30 lakhs, salaried with bank income, worth 60 lakhs. I want to repay in 2 years." |
| **Expected** | Disqualify at Q7 |
| **Actual** | Disqualified at Q7 |
| **Transcript (incl. recording link)** | [./transcripts/05_tenure_short.md](./transcripts/05_tenure_short.md) |
| **Post-call extraction** | `disqualified: true`, `reason: tenure_below_3_years` |
| **Result** | ✅ PASS |

---

### Test 6 — Tenure Too Long (Disqualification)

| Field | Value |
|---|---|
| **Scenario** | Customer requests 20-year tenure |
| **Customer input** | "Residential, sole, originals available, 60 lakhs, salaried with bank income, worth 60 lakhs. Twenty-year tenure." |
| **Expected** | Disqualify at Q7 |
| **Actual** | Disqualified at Q7 |
| **Transcript (incl. recording link)** | [./transcripts/06_tenure_long.md](./transcripts/06_tenure_long.md) |
| **Post-call extraction** | `disqualified: true`, `reason: tenure_above_15_years` |
| **Result** | ✅ PASS |

---

### Test 7 — Loan > ₹75L Accepts Alternative, Then Tenure Disqualification

| Field | Value |
|---|---|
| **Scenario** | Customer asks ₹90L → accepts ₹75L → revises to ₹30L → fails 2-year tenure |
| **Customer input** | "I need ninety lakhs." → "Seventy-five lakhs is fine." → "I choose thirty lakhs." → "In two years." |
| **Expected** | Handle ₹75L cap, track correction to ₹30L, DQ on 2-year tenure |
| **Actual** | All three behaviors executed correctly |
| **Transcript (incl. recording link)** | [./transcripts/07_loan_above_75_accepts.md](./transcripts/07_loan_above_75_accepts.md) |
| **Post-call extraction** | `loan_amount: 3000000`, `tenure_years: 2`, `disqualified: true`, `final_outcome: disqualified` |
| **Result** | ✅ PASS |

---

### Test 8 — Loan Amount > ₹75L, Declines (Polite Close)

| Field | Value |
|---|---|
| **Scenario** | Customer requests ₹90L, declines ₹75L alternative |
| **Customer input** | "I need ninety lakhs." → (offered ₹75L) "No. I want ninety lakhs." |
| **Expected** | Explain limit → offer ₹75L → customer declines → polite close |
| **Actual** | Polite close delivered, no further questions asked |
| **Transcript (incl. recording link)** | [./transcripts/08_loan_above_75_declines.md](./transcripts/08_loan_above_75_declines.md) |
| **Post-call extraction** | `final_outcome: declined_max_amount` |
| **Result** | ✅ PASS |

---

### Test 9 — Joint Ownership (Eligible)

| Field | Value |
|---|---|
| **Scenario** | Property jointly owned with wife |
| **Customer input** | "Residential flat, jointly owned with my wife. Original papers available. 50 lakhs needed. Salaried, bank income. Worth 1 crore. 10-year tenure." |
| **Expected** | Continue through all — joint ownership is eligible |
| **Actual** | Continued through all → Handoff |
| **Transcript (incl. recording link)** | [./transcripts/09_joint_ownership.md](./transcripts/09_joint_ownership.md) |
| **Post-call extraction** | `ownership_status: Joint`, `final_outcome: qualified` |
| **Result** | ✅ PASS |

---

### Test 10 — Existing Loan → Transfer Branch

| Field | Value |
|---|---|
| **Scenario** | Customer already has a loan on the property |
| **Customer input** | "Yes, Bilal here. Actually, I already have a loan on this property." |
| **Expected** | Immediate transfer message → end call |
| **Actual** | Transfer message delivered, no eligibility questions asked |
| **Transcript (incl. recording link)** | [./transcripts/10_existing_loan.md](./transcripts/10_existing_loan.md) |
| **Post-call extraction** | `existing_property_loan: true`, `transfer_required: true`, `final_outcome: transfer` |
| **Result** | ✅ PASS |

---

### Test 11 — EMI Reduction → Transfer Branch

| Field | Value |
|---|---|
| **Scenario** | Customer wants to reduce EMI |
| **Customer input** | "Yes, Bilal. I want to reduce my current EMI." |
| **Expected** | Transfer message → end call |
| **Actual** | Transfer message delivered |
| **Transcript (incl. recording link)** | [./transcripts/11_emi_reduction.md](./transcripts/11_emi_reduction.md) |
| **Post-call extraction** | `emi_reduction_request: true`, `transfer_required: true`, `final_outcome: transfer` |
| **Result** | ✅ PASS |

---

### Test 12 — Busy Customer

| Field | Value |
|---|---|
| **Scenario** | Customer is busy, wants callback |
| **Customer input** | "Yes. But I am in a meeting right now." → "Call me tomorrow five PM." |
| **Expected** | Ask callback time → confirm → end call |
| **Actual** | Callback time captured and confirmed |
| **Transcript (incl. recording link)** | [./transcripts/12_busy_customer.md](./transcripts/12_busy_customer.md) |
| **Post-call extraction** | `callback_time: "Tomorrow 5 PM"`, `final_outcome: busy` |
| **Result** | ✅ PASS |

---

## 🧪 Adversarial Scenarios

---

### Test 13 — Out-of-Order Multi-Answer + Cash Income Disqualification

| Field | Value |
|---|---|
| **Scenario** | Customer volunteers 4 facts in one sentence (out of order), then fails on cash income |
| **Customer input** | "It's a residential flat, jointly owned with my wife, up to eighty lakhs, and I have the original papers available." → later "mainly in cash" |
| **Expected** | Extract all 4 facts → skip re-asking → offer ₹75L → DQ on cash |
| **Actual** | 4 facts extracted, no re-asks, ₹75L alternative offered and accepted, DQ'd on cash |
| **Transcript (incl. recording link)** | [./transcripts/13_out_of_order.md](./transcripts/13_out_of_order.md) |
| **Post-call extraction** | `property_type: Residential`, `ownership: Joint`, `documents: true`, `loan_amount: 7500000`, `income_mode: Cash`, `disqualified: true`, `final_outcome: disqualified` |
| **Result** | ✅ PASS |

---

## 📈 Consolidated Outcomes

| # | Scenario | Outcome | Transcript |
|---|---|---|---|
| 1 | Perfect Eligible | qualified | [01](./transcripts/01_eligible.md) |
| 2 | Agricultural | disqualified | [02](./transcripts/02_agricultural.md) |
| 3 | Cash Income | disqualified | [03](./transcripts/03_cash_income.md) |
| 4 | No Original Docs | disqualified | [04](./transcripts/04_no_documents.md) |
| 5 | Tenure Short | disqualified | [05](./transcripts/05_tenure_short.md) |
| 6 | Tenure Long | disqualified | [06](./transcripts/06_tenure_long.md) |
| 7 | >75L Accepts → Tenure DQ | disqualified | [07](./transcripts/07_loan_above_75_accepts.md) |
| 8 | >75L Declines | declined_max_amount | [08](./transcripts/08_loan_above_75_declines.md) |
| 9 | Joint Ownership | qualified | [09](./transcripts/09_joint_ownership.md) |
| 10 | Existing Loan | transfer | [10](./transcripts/10_existing_loan.md) |
| 11 | EMI Reduction | transfer | [11](./transcripts/11_emi_reduction.md) |
| 12 | Busy Customer | busy | [12](./transcripts/12_busy_customer.md) |
| 13 | Out-of-Order Multi-Answer | disqualified | [13](./transcripts/13_out_of_order.md) |

---

## 🎯 Notes

- All 13 scenarios passed as expected.
- Female Hindi grammar was maintained in every Hinglish turn
  (`kar rahi hoon`, `samajh gayi`, `bata sakti hoon`, `aani chahiye`).
- No eligibility rule was violated.
- No fabricated interest rate, fee, or market value threshold was
  stated at any point.
- Customer name was captured from voice, never assumed.
- Handoff was only triggered after all 7 data points were captured.
- Multi-fact extraction validated in Test 13 (4 facts from 1 sentence).
- Correction handling validated in Test 7 (₹75L → ₹30L update).

---

## 🔗 Accessing Recording Links

Each transcript file begins with a **`**Recording:**`** line at the
top. Click any transcript link in the tables above to open the
transcript, then use the recording URL at the top to view the audio.

**Consolidated screen recording:** [PASTE DRIVE / LOOM LINK]
