
**Objective**: 

This file lists the ambiguities, missing information, and incorrect/contradictory statements that were intentionally injected into  for the Chapter 3 (Static Testing) transfer task. It is intended for facilitators/trainers to check learner findings against, and to support debrief discussion after the review exercise.

**Not for learner use before completing the exercise:** Do not open, share, or reference this file until you have performed your own independent review and documented your findings. Using it beforehand defeats the purpose of the exercise — it trains you to find defects yourself, not to recognize a prepared list. If you're a learner and you're reading this before finishing your review, stop here and go back to the source document.

---

## Review Notes — Injected Defects (Facilitator Reference)

*Do not distribute this section to learners before the exercise.*

### Ambiguities

| # | Location | Ambiguity |
|---|----------|-----------|
| A1 | User story | "Tools" is undefined — could mean physical/DIY tools (matches Toolshop's product vision) or software/dev tools (matches the generic wording "AI assistant recommends tools for my project"). Domain is genuinely unclear from the story alone. |
| A2 | AC: clarifying questions | "Skill level" and "budget" have no defined scale, units, or currency (Toolshop supports multiple currencies elsewhere in the backlog). |
| A3 | User story | "Project" is not defined — scope could range from a single DIY task to a multi-month professional job, which affects how many/what type of tools are relevant. |
| A4 | AC: response time | "Response time under 5 seconds" doesn't state the trigger point (per message vs. whole conversation) or conditions (load, concurrent users). |
| A5 | AC: save for later | "Save for later" doesn't specify whether this requires an account, how long saved data is retained, or how it's retrieved. |
| A6 | AC: "3-5 relevant tools" | No criteria defined for what makes a tool "relevant" — relevance ranking is left entirely to the AI with no acceptance threshold. |

### Missing Information

| # | Area | What's missing |
|---|------|-----------------|
| M1 | Error/empty states | No behavior defined for when the AI cannot produce any suitable recommendation. |
| M2 | Accessibility | No accessibility requirements, despite the product vision explicitly promising "barrier-free accessibility." |
| M3 | Data privacy | No mention of how free-text project descriptions (potentially containing personal data) are handled, stored, or protected. |
| M4 | Localization | No mention of language support, despite a related existing story (US-F2.4 – AI Use In My Language) suggesting this is a real concern. |
| M5 | Security | No mention of prompt-injection risk or input validation for the chat interface. |
| M6 | Catalog linkage | No requirement stating recommended tools must exist/be purchasable in the Toolshop catalog. |
| M7 | Testability | No measurable, objective criteria QA could use to verify recommendation quality/"reasoning" beyond "3-5 tools shown." |

### Incorrect / Contradictory Information

| # | Location | Issue |
|---|----------|-------|
| W1 | Implementation notes: caching | Caching AI responses for 24 hours contradicts the personalized, conversational nature of the feature — a customer refining their project description would receive a stale, previously cached answer. |
| W2 | Implementation notes: performance metric | Measuring the 5-second target "server-side only, excluding... the third-party AI API call" contradicts the AC's plain-language, user-facing expectation — the customer experiences the full round trip, not just server processing. |
| W3 | Implementation notes: pricing source | Sourcing prices from "the AI provider's general knowledge of the market" instead of the live Toolshop catalog risks showing incorrect/outdated prices, undermining the product vision's "trusted marketplace" goal. |
| W4 | Implementation notes: save feature | "No authentication... save link is emailed to whatever address the customer types" is a security/spam risk (arbitrary third-party email injection) and contradicts typical expectations for a personalized, saved feature. |
| W5 | Title/heading typo | "Accepatnce Criteria" is misspelled in the source artifact — a minor but genuine finding for a formal review. |
