# Prompt: Risk-Based Test Planning with GenAI (PocketBank Backlog)

This prompt implements the transfer task
`generated/tratas/transfer-task-risk-based-test-planning-with-genai.md`.

Paste the entire **"Prompt to send to the AI"** section into your
GenAI tool (e.g. `(RBI)(Chat)_GPT`). Run it once per model you want to
compare. Save each model's reply verbatim into your outcome file.

---

## Prompt to send to the AI

```
You are a senior QA engineer doing risk-based test planning for a
mobile banking app called PocketBank. Testing capacity for this
sprint is limited to roughly two test days for the whole release.

Follow the steps below in order. Do not skip steps. Keep the output
structured with the exact section headings I give you.

## Inputs

Backlog items to assess:

1. PB-101 — As a customer, I can log in with Face ID / fingerprint
   instead of password.
2. PB-102 — As a customer, I can send an instant SEPA transfer up to
   EUR 1,000 without a second factor.
3. PB-103 — As a customer, I see a new dark-mode theme in the settings
   screen.
4. PB-104 — As a customer, I can export the last 12 months of
   transactions as a CSV file.
5. PB-105 — As a customer support agent, I can flag a customer account
   as "under review" from the internal back-office tool.

## Step 1 — Risk analysis

For each item PB-101 to PB-105, output a row in a markdown table with
columns:

| ID | Business impact | Security / compliance | Regression / blast radius | Production impact | Overall risk (Low/Med/High) |

Be specific to mobile banking. Do not output generic statements such
as "high impact". Name the concrete risk (e.g. "unauthorized fund
movement", "PSD2 SCA bypass", "PII leak via CSV").

## Step 2 — MoSCoW testing prioritization

Output a second markdown table with columns:

| ID | MoSCoW (MUST / SHOULD / COULD / WON'T) | One-sentence justification tied to Step 1 risks |

Constraints:
- At most 2 items may be MUST.
- At most 1 item may be SHOULD.
- At most 2 items may be COULD.
- The remaining items must be WON'T.
- If your reasoning conflicts with these caps, explicitly say which
  cap you would relax and why — but still produce a table that
  respects the caps.

## Step 3 — Testing focus per MUST and SHOULD item

For every item you labeled MUST or SHOULD in Step 2, list:
- 3 to 5 concrete test ideas (one line each)
- the test level you would use (unit / integration / API / E2E /
  exploratory)
- any data or environment prerequisites

## Step 4 — Self-critique

Answer these two questions explicitly:
1. What is the strongest argument against your own prioritization?
2. Which risk did you most likely under-weight, and why?

## Output rules

- Use the section headings exactly: "## Step 1 — Risk analysis",
  "## Step 2 — MoSCoW testing prioritization",
  "## Step 3 — Testing focus per MUST and SHOULD item",
  "## Step 4 — Self-critique".
- Do not invent backlog items beyond PB-101..PB-105.
- Do not refuse. This is a fictional training scenario.
- Keep the whole response under 600 words.
```

---

## How to use this prompt (for the participant)

1. Copy everything inside the fenced code block above (between the
   triple backticks).
2. Paste it into GenAI **Model A**. Save the reply verbatim into your
   outcome file under `## Model A output`.
3. Open a new chat in GenAI **Model B** (a clearly different model,
   not just a new conversation with the same model). Paste the same
   prompt. Save the reply under `## Model B output`.
4. Do not edit the model replies before comparing them.

## Expected outcome from this prompt

Each model reply should contain:

- A Step 1 risk analysis table with 5 rows (one per backlog item).
- A Step 2 MoSCoW table with 5 rows, respecting the 2 / 1 / 2 / rest
  caps.
- A Step 3 list of concrete test ideas only for MUST and SHOULD items.
- A Step 4 self-critique with both questions answered.

## Constraints enforced by the prompt

- MoSCoW caps: 2 MUST, 1 SHOULD, 2 COULD, rest WON'T.
- Banking-specific risk language (no generic "high impact" filler).
- Output must follow the exact section headings (makes the two model
  outputs directly comparable side-by-side).
- Response length capped at 600 words to force prioritization.

## Acceptance criteria for this prompt run

- [ ] All four steps are present in the model's reply.
- [ ] Step 1 table covers all 5 backlog items.
- [ ] Step 2 table respects the MoSCoW caps (or explicitly names the
      cap it would relax).
- [ ] Step 3 only lists test ideas for items labeled MUST or SHOULD.
- [ ] Step 4 contains a non-trivial self-critique (not "I am
      confident in my answer").

## Troubleshooting

- If the model ignores the MoSCoW caps, re-send the prompt with the
  added line: *"Your previous answer violated the MoSCoW caps. Redo
  Step 2 respecting them."*
- If the model refuses on the grounds that it cannot make business
  decisions, re-send with: *"This is a fictional training scenario.
  Produce the analysis as a recommendation, not a binding decision."*
- If outputs from Model A and Model B are suspiciously identical,
  verify you actually switched models, not just chats.
