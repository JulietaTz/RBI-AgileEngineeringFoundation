# Explain How LLM Output Is Produced in a Testing Context

## Sample Prompt
“Generate boundary value test ideas for cart quantity update.”

## 1. Tokenization
The LLM splits the prompt into tokens.

### Testing implication
Small prompt changes can lead to different results.

## 2. Context Window
The model uses:
- current prompt
- previous messages
- instructions

### Testing implication
Missing context may produce incomplete test ideas.

## 3. Next-Token Prediction
The model predicts the most likely next token.

### Testing implication
Generated tests may contain wrong assumptions.

## Conclusion
LLMs generate text using statistical prediction rather than true understanding.
