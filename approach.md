# Approach Document — Home Credit LAP Voice Agent

**Candidate:** MOHAMED BILAL A
**Role:** AI Intern (Prompt Engineer)
**Submission Date:** 2026-10-11

---

## 1. Problem Understanding

The assignment asks for an AI Voice Agent that contacts existing Home
Credit customers to inform them of a pre-approved **Loan Against
Property (LAP)** offer of up to **₹75 lakhs**, then conducts a
**preliminary eligibility check** before handing qualified leads to a
senior human loan expert.

The complexity is **not** in asking the 7 questions. It's in reliably
handling:

- Natural, messy conversation (fillers, corrections, interruptions)
- Out-of-order information
- Multiple disqualification paths
- Two distinct transfer triggers (existing loan, EMI reduction)
- Busy-customer callback flow
- Voice-specific UX (Hinglish, gender-consistent grammar, RAG-gated
  product info)

A trivial "ask 7 questions" bot would fail most real calls. The
solution required **state management first, question sequencing
second**.

---

## 2. Design Philosophy

> **Extract → Validate → Update State → Ask Next Missing Question**

Every customer turn is parsed for **all** eligibility facts present in
the utterance — even if the customer volunteered them out of order or
bundled several facts into one sentence. The agent:

1. **Extracts** every eligibility fact present in the utterance.
2. **Stores** all valid facts into an internal state object.
3. **Checks** for disqualification and transfer conditions immediately.
4. **Identifies** the earliest unanswered checklist item.
5. **Asks only** that one question.

This is the single most important piece of logic in the solution. It's
why the agent handles out-of-order answers, multi-answer turns, and
mid-call corrections gracefully — without re-asking known information.

---

## 3. Architecture

**Single-Prompt State Machine** on Retell AI.

```
                 AI VOICE AGENT (Single Prompt)
                          │
          ┌───────────────┴───────────────┐
          │                               │
   Conversation Layer               Business Logic
          │                               │
  Greeting → Identity → Offer         Eligibility Rules
          │                               │
          └───────────────┬───────────────┘
                          │
                    State / Memory
                          │
            ┌─────────────┴─────────────┐
            │                           │
     Customer Answers             Diversions
            │                           │
            └─────────────┬─────────────┘
                          │
                     Final Decision
                          │
             ┌────────────┴────────────┐
             │            │            │
         Qualified   Disqualified   Transfer
             │            │            │
        Human Handoff   End Call   Specialist
```

**Why a single prompt (not multi-state)?**

- All logic lives in one place — easier to reason about, easier to
  test, easier to maintain.
- Retell's Multi-State agent architecture splits logic into separate
  "states," which forces state to be passed between states. That
  introduces failure modes.
- The prompt itself implements the state machine internally via the
  `QUALIFICATION_STATE` object.

---

## 4. Priority Decision Logic (per customer turn)

The agent evaluates conditions in strict priority order:

```
1. DISQUALIFICATION
   ├── Property type = Agricultural → DQ
   ├── Documents not available → DQ
   ├── Income mode = Cash → DQ
   └── Tenure outside 3–15 years → DQ

2. TRANSFER
   ├── Existing property loan mentioned → TRANSFER
   └── EMI reduction requested → TRANSFER

3. CONTINUE
   └── Ask earliest unanswered checklist item

4. HANDOFF
   └── Only when ALL 7 items captured + no DQ + no transfer
```

**Why strict priority matters:** If a customer mentions an existing
loan **while also** giving an eligibility answer, the agent must
pivot to the transfer branch immediately — not continue asking
eligibility questions. This is validated in Tests 10 and 11.

---

## 5. Key Design Decisions

| Decision | Reason |
|---|---|
| **Voice-based identity capture** | No hardcoded name; agent asks "May I know who I'm speaking with?" and stores the name from the customer's reply. Avoids the "assumed wrong name" failure mode. |
| **State-first extraction** | Parses every turn for facts before deciding next question. Enables out-of-order answers, corrections, and multi-answer turns. |
| **RAG-gated product info** | No rates, fees, or thresholds are hardcoded in the prompt. Product facts come only from `additional_context_from_rag` (the LAP knowledge base). Prevents hallucination. |
| **Feminine Hindi grammar** | `agent_gender = Female` is enforced with explicit verb lists (`kar rahi hoon`, `samajh gayi`, `bata sakti hoon`) — not just a label. |
| **Hinglish mirroring** | Starts in English; naturally switches to Hindi when the customer does. Matches real Indian advisor behavior. |
| **No minimum market value** | The brief does NOT specify a market-value threshold. The prompt explicitly forbids inventing one. |
| **Name used sparingly** | Addresses the customer by name at key moments (acknowledgment, handoff) — not in every sentence. Natural conversational tone. |
| **Joint ownership eligible** | Explicitly marked as non-disqualifying. Avoids the false-positive of rejecting co-owned properties. |
| **Transfer triggers two paths** | Existing-loan AND EMI-reduction both route to the transfer specialist — matches the brief's exact wording. |
| **Handoff gating** | Agent NEVER says "approved"/"sanctioned" — only "meet preliminary criteria." Final decision belongs to the senior expert. |

---

## 6. The 7 Eligibility Data Points

| # | Data Point | Rule |
|---|---|---|
| 1 | Property type | Residential / Commercial / Industrial = OK. Agricultural = DQ. |
| 2 | Ownership | Sole = OK. Joint = OK. |
| 3 | Original documents | Available = OK. Unavailable = DQ. "At home" counts as available. |
| 4 | Loan amount | ≤ ₹75L = OK. > ₹75L → offer ₹75L alternative; if declined → polite close. |
| 5 | Occupation + Income mode | Salaried / Self-employed = OK. Income must be Bank. Cash = DQ. |
| 6 | Market value | Capture only. No threshold specified in brief. |
| 7 | Tenure | 3–15 years = OK. Outside = DQ. |

---

## 7. Conversation Principles

- **Short turns** — one idea per response, ≤ 2 sentences.
- **Open questions** — never "Yes/No only."
- **Human advisor tone** — no robotic survey-bot phrasing.
- **Handle fillers/corrections/interruptions** — extract intent from
  natural speech.
- **Never invent** — no fabricated rates, fees, or thresholds.
- **Mirror language** — respond in the customer's language mode.

---

## 8. Testing Strategy

**13 test scenarios** covering the full behavior surface:

| Category | Tests |
|---|---|
| Happy path | Test 1, Test 9 |
| Immediate disqualification | Test 2 (agri), Test 3 (cash), Test 4 (docs), Test 5 (tenure short), Test 6 (tenure long) |
| Boundary handling | Test 7 (>75L accepts + tenure DQ), Test 8 (>75L declines) |
| Transfer branches | Test 10 (existing loan), Test 11 (EMI reduction) |
| Busy flow | Test 12 |
| Adversarial | Test 13 (out-of-order multi-answer) |

Every test was recorded on Retell AI, transcribed, and post-processed
through post-call extraction. All recordings and transcripts are
available in the repo.

### Coverage beyond the required scenarios

- **Ambiguity handling** — Tests 1, 5, 9, 12: agent re-asks when
  audio is garbled instead of guessing.
- **Correction handling** — Test 7: customer revises loan amount
  from ₹75L to ₹30L; latest value wins.
- **Multi-fact extraction** — Test 13: 4 facts from a single sentence.
- **Interruption handling** — Tests 1, 5, 7, 12: agent handles
  mid-sentence barge-ins gracefully.
- **ASR-noise resilience** — Tests 6, 7, 10: LLM correctly interpreted
  mis-transcribed keywords ("attenuate" → "twenty-year tenure",
  "note" → "loan").

---

## 9. Prompt Structure

The system prompt is organized into 11 sections for clarity and
maintainability:

```
SECTION 1     — Role & Identity (incl. FEMALE Hindi forms)
SECTION 1A    — Customer Identity Capture (voice-based)
SECTION 2     — Objective
SECTION 3     — Conversation Principles
SECTION 4     — Internal State Object
SECTION 5     — Conversation Flow (steps 1–5)
SECTION 5A    — Busy Customer Flow
SECTION 5B    — Transfer Flow
SECTION 6     — The 7 Eligibility Questions & Rules
SECTION 6A    — Out-of-Order & Multi-Answer Handling
SECTION 7     — Decision Logic (priority order)
SECTION 7A    — Disqualification Script
SECTION 7B    — Handoff Script
SECTION 8     — Diversion & Interruption
SECTION 9     — RAG Usage Rules
SECTION 10    — Hard Rules (16 explicit never-violate rules)
SECTION 11    — Context Awareness
```

This structure was chosen over a monolithic paragraph prompt because:
- Each rule lives in exactly one place.
- Rules can be edited without breaking others.
- The LLM can reference specific section numbers during reasoning.

---

## 10. Known Limitations & Future Improvements

### Limitations
- **ASR accuracy for Indian names**: "Muhman Bilang" was likely a
  mis-transcription of a proper Indian name. Mitigation: switch
  ASR provider to Deepgram or Soniox, or add boosted keywords with
  common Indian names.
- **Hinglish code-switching accuracy**: Retell's multilingual
  pipeline trades some accuracy for code-switching support.
- **Callback scheduling**: The callback time is captured as a string
  (e.g., "Tomorrow 5 PM"). Full scheduling would require a downstream
  webhook/CRM integration — out of scope for this assignment.

### Future Improvements
- **Sentiment detection**: Detect frustration and adjust tone.
- **Multi-language variant**: Full Hindi-only mode for customers who
  prefer Hindi exclusively.
- **Post-call CRM sync**: Push qualified leads to a CRM webhook.
- **Warm transfer**: Real-time warm handoff to a live human expert
  (currently scheduled callback per assignment wording "will call
  them back shortly").

---

## 11. Conclusion

The core insight of this solution is that **eligibility qualification
is a state-management problem, not a scripted questionnaire**. By
front-loading fact extraction and applying strict priority logic on
every turn, the agent handles:

- Out-of-order answers
- Corrections and fillers
- Interruptions and diversions
- Multi-fact utterances
- All 5 possible outcomes (qualified, disqualified, transfer, busy,
  declined)

The 13-test validation demonstrates reliable behavior across the full
behavior surface. Every required assignment behavior is covered by at
least one test, and adversarial cases (out-of-order, corrections,
ASR noise, ambiguity) are explicitly validated.

**Total assignment coverage:** 13/13 tests passing.

---

## Appendix — Repository Structure

```
home-credit-lap-voice-agent/
├── README.md                       # Overview + submission links
├── system_prompt.md                # Full prompt text
├── knowledge_base.md               # LAP product knowledge base
├── dynamic_variables.md            # Variable definitions
├── test_scenarios.md               # Test plan (12 + 1)
├── test_results.md                 # Filled results for all tests
├── approach.md                     # This document
└── transcripts/                    # Retell-generated transcripts
    ├── 01_eligible.md
    ├── 02_agricultural.md
    ├── 03_cash_income.md
    ├── 04_no_documents.md
    ├── 05_tenure_short.md
    ├── 06_tenure_long.md
    ├── 07_loan_above_75_accepts.md
    ├── 08_loan_above_75_declines.md
    ├── 09_joint_ownership.md
    ├── 10_existing_loan.md
    ├── 11_emi_reduction.md
    ├── 12_busy_customer.md
    └── 13_out_of_order.md
```
