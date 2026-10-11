# Home Credit — LAP Voice Agent

AI Voice Agent built for the **Home Credit Loan Against Property (LAP)**
qualification use case, submitted for the **SalesAgents AI — AI Intern
(Prompt Engineer)** assignment.

## 🎯 Objective

Design and deploy an AI Voice Agent that:
- Warmly greets and verifies the intended customer
- Presents a pre-approved LAP offer (up to ₹75 Lakhs) as a loyalty reward
- Collects **7 eligibility data points sequentially**
- Handles out-of-order answers, interruptions, and diversions
- **Immediately disqualifies** ineligible customers
- Detects **existing loan / EMI reduction** cases → transfers to specialist
- Hands off qualified leads to a **senior loan expert**

## 🏗️ Architecture

**Single-Prompt State Machine** on Retell AI with priority-based turn logic:

    Greeting → Identity Capture → Offer → Eligibility Loop → Decision
                                                             ├── Handoff
                                                             ├── Disqualify
                                                             └── Transfer

**Per-turn priority:**
1. Check for disqualifying answer
2. Check for transfer intent
3. Continue with earliest missing question
4. Handoff (only when all 7 captured)

## 🧠 Key Design Decisions

| Decision | Reason |
|---|---|
| Voice-based identity capture | No hardcoded name; agent asks & stores |
| State-first extraction | Handles out-of-order & multi-answer turns |
| RAG-gated product info | No fabricated rates/fees |
| Feminine Hindi grammar | `agent_gender = Female` strict adherence |
| Hinglish mirroring | Matches customer's natural code-switching |
| No minimum market value | Not specified in brief — do not invent |
| Name used sparingly | Natural conversational tone |

## 🔗 Submission Links

- **Live Agent (Retell):** [PASTE RETell PUBLIC LINK HERE]
- **Screen Recording:** [PASTE DRIVE / LOOM LINK HERE]
- **System Prompt:** [system_prompt.md](./system_prompt.md)
- **Knowledge Base:** [knowledge_base.md](./knowledge_base.md)
- **Test Scenarios:** [test_scenarios.md](./test_scenarios.md)
- **Test Results:** [test_results.md](./test_results.md)

## 📂 Repo Structure

See individual files. All transcripts in `transcripts/`.

## 🛠️ Stack

- **Platform:** Retell AI
- **LLM:** GPT-4.1
- **Voice:** [your voice name]
- **Language:** Hindi (hi-IN) + English
- **Knowledge Base:** Custom LAP product KB (see `knowledge_base.md`)

## 📄 License

MIT
