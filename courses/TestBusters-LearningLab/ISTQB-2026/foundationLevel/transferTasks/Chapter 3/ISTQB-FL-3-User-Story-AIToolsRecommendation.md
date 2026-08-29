# US-F2.1-AIToolsRecommendation

> **Purpose of this document:** Work product for a static testing / review exercise (ISTQB FL Chapter 3 — Static Testing). It intentionally contains ambiguities, missing information, and incorrect statements for learners to detect using a review technique.

## User Story

As a customer,
I want an AI assistant to recommend tools for my project
so I find the optimal solution.

## Business Context

Part of the **F2 – AI Tool Advisor** feature for the Toolshop marketplace (Roy's). Product vision positions Toolshop as an "intelligent, trusted, and globally accessible marketplace" combining AI-powered recommendations with community reviews and international commerce.

## Actors

- **Customer** – guest or registered user browsing Toolshop who wants tool recommendations for a project.
- **AI Assistant (Advisor)** – chat-based system component that interprets the project description and returns tool recommendations.

## Functional Requirements

1. Provide a chat-style interface where the customer describes their project in free text.
2. The AI asks clarifying questions if needed (examples given: skill level, budget).
3. The AI returns 3–5 relevant tool recommendations with reasoning for each.
4. Each recommendation displays: tool name, price, key features, and rationale for suitability.
5. The customer can save a set of recommendations for later reference.
6. The system must respond within 5 seconds.

## Non-Functional Requirements (as drafted)

- Performance: response time under 5 seconds.
- (No other non-functional requirements specified — see Review Notes.)

## Implementation Details / Technical Notes

> Notes captured from the engineering design discussion.

- The chat interface calls an external LLM API; the customer's free-text project description and any clarifying answers are forwarded as the prompt.
- To hit the performance target, AI responses are **cached for 24 hours per user session** and reused if the customer sends a similar message again.
- The 5-second target is **measured server-side only**, from receipt of the request to generation of the response payload; it excludes network latency and the third-party AI API call itself.
- Tool prices shown in recommendations are **pulled from the AI provider's general knowledge of the market**, not queried live from the Toolshop product catalog.
- The "save for later" feature requires **no authentication** — a save link is emailed to whatever address the customer types into the chat.
- Recommended tools are not required to map to actual purchasable Toolshop catalog items.

## Recommendations (from tech lead / BA)

- Reuse the existing chat widget component from the customer support feature for the conversational UI.
- Log all AI conversations for quality monitoring (retention period not yet defined).
- Consider reusing the ranking logic from the product search feature for scoring "relevance," pending confirmation this is compatible with AI-generated candidates.

## Acceptance Criteria (verbatim from source)

- Chat-style interface for user to describe project
- AI asks clarifying questions if needed (skill level, budget)
- Recommendations include 3-5 relevant tools with reasoning
- Each recommendation shows: tool name, price, key features, why suitable
- Option to save recommendations for later
- Response time under 5 seconds

## Out of Scope

- Not specified.

## Dependencies / Assumptions

- Assumes an external AI/LLM provider is already integrated with the platform.
- Assumes "tools" refers to the same domain as the Toolshop catalog (see Review Notes — this is disputed).

