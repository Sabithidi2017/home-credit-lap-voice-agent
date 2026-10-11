# Dynamic Variables

| Variable | Value / Source |
|---|---|
| `agent_name` | Voice AI agent |
| `company_name` | Mohamed's Voice Agent |
| `agent_gender` | Female (hardcoded in prompt) |
| `language_to_speak` | English to Hindi |
| `additional_context_from_rag` | Auto-injected from KB |
| `current_time_Asia/Kolkata` | Auto by Retell |

## API Payload Example

```json
{
  "retell_llm_dynamic_variables": {
    "agent_name": "Voice AI agent",
    "company_name": "Mohamed's Voice Agent",
    "language_to_speak": "English to Hindi",
    "additional_context_from_rag": ""
  }
}
