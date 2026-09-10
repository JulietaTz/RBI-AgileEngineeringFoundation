# Selecting Prompting Techniques — Short Exercise

**Syllabus Reference**
ISTQB GenAI — 2.2.5 Selecting Context-Appropriate Prompting Techniques for Software Testing

## Link to the Transfer Task File
https://github.com/rgroetz2/TBLL-AgileEngineeringFoundation/blob/main/courses/TestBusters-LearningLab/ISTQB-2026/genAI/transferTasks/Chapter2/ISTQB-GenAI-2.2.5_Selecting_Prompting_Techniques_Short_20260807.md

---
# Outcome

### Task 1

Primary need: Precision; technique: Few-shot; prompt: “Follow these examples: subtotal €100, discount 10%, tax 20% → expected total €108; subtotal €50, discount 0%, tax 20% → expected total €60. Generate 5 concise checkout test inputs in the same format, applying the discount before tax and rounding totals to two decimal places.”; setting: temperature 0.0

---

### Task 2

Primary need: Precision; technique: Constrained output; prompt: “Return exactly 3 concise WCAG checklist items for a ToolShop product details page as bullet points; each item must state what to check and the expected accessible behavior.”; setting: temperature 0.0

---

### Task 3

Primary need: Summarization; technique: Chunking; prompt: “Split the following 50 recent product reviews into manageable chunks, identify recurring positive, negative, and mixed sentiment, and combine the findings into exactly 3 concise bullet points without inventing information: [reviews].”; setting: temperature 0.0

