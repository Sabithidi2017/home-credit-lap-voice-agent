
---

## 📄 FILE 5: `test_scenarios.md`

```markdown
# Test Scenarios

15 scenarios tested — 12 required + 3 adversarial.

| # | Scenario | Input | Expected |
|---|---|---|---|
| 1 | Perfect eligible | All 7 valid | Handoff |
| 2 | Agricultural | "kheti ki zameen" | DQ at Q1 |
| 3 | Cash income | Self-employed + cash | DQ at Q5 |
| 4 | No originals | Photocopies only | DQ at Q3 |
| 5 | Tenure short | 2 years | DQ at Q7 |
| 6 | Tenure long | 20 years | DQ at Q7 |
| 7 | >75L accepts | 90L → accepts 75L | Qualified |
| 8 | >75L declines | 90L → declines | Polite close |
| 9 | Joint ownership | Joint with wife | Qualified |
| 10 | Existing loan | Has loan → transfer | Transfer |
| 11 | EMI reduction | Wants EMI reduce | Transfer |
| 12 | Busy customer | "In a meeting" | Ask callback |
| B1 | Out-of-order | Multi facts in one line | Skip known Qs |
| B2 | Correction | "10 yrs…actually 12" | Latest wins |
| B3 | Diversion | "What's interest rate?" | Deflect, resume |
