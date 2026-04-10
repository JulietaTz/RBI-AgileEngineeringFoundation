### 1. Title

AI Paradigm Fit Check for Toolshop Cart Testing

---

### 2. Syllabus Reference

ISTQB GenAI – 1.1.1 AI Spectrum: Symbolic AI, Classical Machine Learning, Deep Learning and Generative AI

---

### 3. Learning Objective

Apply the AI spectrum to one concrete Toolshop testing context and decide which paradigm gives the best starting point for sprint test design.

---

### 4. Context / Scenario

Serife joins sprint preparation for the Toolshop webshop. The team needs a quick, practical decision on how different AI paradigms could support testing of the cart and checkout area in the upcoming sprint.

---

### 5. Task Instructions

1. Open https://practicesoftwaretesting.com/ and execute a short cart flow: add one product to cart, open cart, start checkout.
2. Open Toolshop documentation and review one relevant behavior statement for cart/checkout:
   - https://github.com/testsmith-io/practice-software-testing/tree/main/docs
   - https://github.com/testsmith-io/practice-software-testing/tree/main/sprint5
3. Fill in the matrix below for all four paradigms, using short phrases (max. 12 words per cell).
4. Add one final recommendation sentence for this sprint: “Start with **_ because _**.”

| Paradigm | Best use in cart/checkout testing | Why useful here | Key caution |

| Symbolic AI | | | |
| Classical ML | | | |
| Deep Learning | | | |
| Generative AI | | | |

### Decision Matrix

| Paradigma | Toolshop Testing Use Case | Hauptvorteil | Risiko |

| Symbolic AI | Validierung der Warenkorb-Gesamtsumme und Löschfunktion | Klare regelbasierte Logik | Wenig flexibel bei unerwarteten Fällen |
| Classical ML | Analyse von Abbrüchen im Checkout | Lernt aus Nutzerdaten | Benötigt ausreichend Daten |
| Deep Learning | Erkennung ungewöhnlicher Checkout-Verhalten | Erkennt komplexe Muster | Schwer zu interpretieren |
| Generative AI | Generierung von Testfällen für Cart und Checkout | Schnelle Testfallerstellung | Kann fehlerhafte Tests erzeugen |

### Empfehlung

Start mit Generative AI, da schnell Testfälle aus Anforderungen generiert werden können.
---

### 6. Expected Outcome / Deliverable

One markdown artifact with a completed 4-row matrix and one sprint recommendation sentence.

---

### 7. Timebox

15 minutes

---

### 8. Hints (Optional)

- Keep each cell short and practical, not theoretical.
- If uncertain, compare Symbolic AI and Generative AI first, then complete the other two.
