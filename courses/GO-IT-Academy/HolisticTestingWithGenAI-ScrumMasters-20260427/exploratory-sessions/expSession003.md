# Example Session Sheet

## CHARTER
Analyze AI tool recommendation quality and response behavior under varied user prompts.

## TESTER
Jonas Leitner

## TEST NOTES
I varied user intent, budget, and skill descriptions to observe recommendation relevance and latency.

### View:
- AI advisor chat interface
- Recommendation cards panel
- Save recommendation action

### Use Cases:
- Request recommendations for home woodworking project with low budget
- Request recommendations for professional workshop upgrade
- Provide vague prompt and evaluate AI clarification questions

## RISKS
- Irrelevant suggestions reduce trust in AI assistant
- Missing rationale makes recommendations non-actionable
- High response time causes session abandonment

## BUGS### 

BUG TS-2301
AI returns 6 tools in some responses although UI contract is 3-5 recommendations.

BUG TS-2302
First response after idle period exceeds 8 seconds; spinner stays active and no timeout message is shown.

## Summary
Recommendation baseline is promising, but contract and performance deviations are significant for user satisfaction.