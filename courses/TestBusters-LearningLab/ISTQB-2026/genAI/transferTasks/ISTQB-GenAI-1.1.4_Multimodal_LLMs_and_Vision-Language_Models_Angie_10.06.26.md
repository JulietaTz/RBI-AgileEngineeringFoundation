# 1. Title

Use Multimodal AI for UI Test Support

---

# 2. Syllabus Reference

ISTQB GenAI – 1.1.4 Multimodal LLMs and Vision-Language Models

---

# 3. Learning Objective

Apply multimodal prompting to support UI-focused testing while controlling for misinterpretation risks.

---

# 4. Context / Scenario

The team wants to test whether screenshot + text prompts can support UI anomaly detection for the Toolshop webshop (https://practicesoftwaretesting.com).

Two pages were selected:
- **Page A:** Product Listing Page
- **Page B:** Shopping Cart Page

---

# 5. Multimodal Prompt Used

The following prompt was used for both pages (screenshot + text):

> "You are a UI tester reviewing a webshop screenshot. Please identify any potential UI issues and accessibility concerns visible in this image. For each finding, specify: (1) which element is affected, (2) where on the screen it is located, (3) why it is a potential issue. Be specific and avoid assumptions about functionality not visible in the screenshot."

---

# 6. Findings Table

## Page A – Product Listing Page

| # | AI Finding | Element | Location | Classification | Manual Validation Notes |
|---|---|---|---|---|---|
| 1 | Low color contrast on filter labels | Filter sidebar text | Left side | ✅ Valid issue | Confirmed – light grey text on white background fails WCAG AA |
| 2 | Missing alt text on product images | Product image thumbnails | Center grid | ✅ Valid issue | Confirmed – images have no descriptive alt attributes |
| 3 | Inconsistent button sizes across product cards | "Add to Cart" buttons | Product card grid | ⚠️ Unclear | Slight visual variation detected – needs responsive design check |
| 4 | Pagination controls too small for touch | Pagination bar | Bottom of page | ✅ Valid issue | Confirmed – tap targets below 44px recommended minimum |
| 5 | Sort dropdown has no visible focus indicator | Sort dropdown | Top right | ❌ False positive | Focus indicator present but only visible on keyboard navigation |

## Page B – Shopping Cart Page

| # | AI Finding | Element | Location | Classification | Manual Validation Notes |
|---|---|---|---|---|---|
| 6 | Quantity input field has no min/max label | Quantity field | Cart item row | ✅ Valid issue | Confirmed – no hint for valid range shown to user |
| 7 | Remove item button lacks accessible label | "X" remove button | Cart item row | ✅ Valid issue | Confirmed – icon-only button with no aria-label |
| 8 | Total price not updated dynamically on quantity change | Price summary | Right sidebar | ❌ False positive | Price does update – AI misread static screenshot as missing behavior |
| 9 | Checkout button color insufficient contrast | "Proceed to Checkout" button | Bottom right | ⚠️ Unclear | Color ratio borderline – requires contrast checker tool to confirm |
| 10 | Empty cart state message missing | Cart area | Center | ❌ False positive | AI assumed cart was empty based on layout – cart was not empty |

---

# 7. Finding Summary

| Classification | Count |
|---|---|
| ✅ Valid issue | 5 |
| ❌ False positive | 3 |
| ⚠️ Unclear | 2 |
| **Total** | **10** |

---

# 8. Prompt Refinements to Reduce False Positives

## Refinement 1 – Request evidence-based findings only

**Original prompt element:** "identify any potential UI issues"

**Refined:** "Only report issues that are directly visible in the screenshot. Do not infer or assume behavior, interactions, or states not shown in the image."

**Reason:** Eliminates false positives caused by the AI assuming dynamic behavior (e.g., finding #8 and #10) from a static screenshot.

---

## Refinement 2 – Scope the output format strictly

**Original prompt element:** Open-ended findings list

**Refined:** "For each finding, answer these three questions: (1) What exact element is affected? (2) Where exactly on screen is it (top/bottom/left/right/center)? (3) What specific visual evidence supports this as an issue? If you cannot answer all three, do not report the finding."

**Reason:** Forces the AI to provide verifiable, specific evidence per finding – reduces vague or assumed findings (e.g., finding #5).

---

## Refinement 3 – Add accessibility standard reference

**Original prompt element:** "accessibility concerns"

**Refined:** "Check for accessibility issues based on WCAG 2.1 Level AA criteria only. For each accessibility finding, state which specific WCAG criterion is violated (e.g., 1.4.3 Contrast Minimum)."

**Reason:** Anchors findings to a concrete standard, making it easier to validate or reject them during manual review, and reduces over-reporting of minor stylistic issues.

---

# 9. Key Learnings

- Multimodal AI is effective for detecting **visual and static accessibility issues** (contrast, missing labels, icon-only buttons).
- AI **cannot reliably assess dynamic behavior** from a static screenshot – manual validation is always required.
- Prompt quality directly impacts finding quality: specific, structured prompts yield fewer false positives.
- Classification of findings (valid / false positive / unclear) is a critical step before any findings enter the defect backlog.

---

# 10. Expected Outcome / Deliverable

A markdown report with findings table and prompt refinement notes – see Sections 6–8 above.

---

# 11. Timebox

50 minutes
