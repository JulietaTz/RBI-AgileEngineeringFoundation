# 1. Title

Quick AI Spectrum Mapping for Toolshop Testing

---

# 2. Syllabus Reference

ISTQB GenAI – 1.1.1 AI Spectrum: Symbolic AI, Classical Machine Learning, Deep Learning and Generative AI

---

# 3. Learning Objective

Identify, in a simple and practical way, when each AI paradigm is useful for testing in Toolshop. Produce a lightweight comparison your Scrum team can use immediately.

---

# 4. Content / Scenario

The Toolshop application (https://practicesoftwaretesting.com) is an e-commerce platform. For this task, the **Login Flow** was chosen as the feature under test.

## Task: AI Spectrum Comparison Table

| Paradigm | Toolshop testing use case | Main strength | Caution/risk |
|---|---|---|---|
| Symbolic AI | Validierung von Login-Regeln (z.B. Pflichtfelder, E-Mail-Format-Checks) | Klar nachvollziehbare Regeln, deterministisch und vollständig erklärbar | Unflexibel bei unvorhergesehenen oder komplexen Eingaben |
| Classical ML | Erkennung von ungewöhnlichen Login-Mustern (z.B. Brute-Force-Angriffe) | Lernt aus historischen Daten und erkennt Anomalien automatisch | Benötigt viele Trainingsdaten, schwer zu debuggen |
| Deep Learning | Visuelles Regressionstesting der Login-UI mittels CNN (Convolutional Neural Network) | Erkennt subtile visuelle Änderungen in der UI automatisch | Schwer erklärbar (Black Box), hoher Rechenaufwand |
| Generative AI | Automatische Generierung von Testfällen aus den Login-Anforderungen | Spart erheblich Zeit bei der Testfallerstellung im Sprint | Kann ungenaue oder halluzinierte Testfälle erzeugen |

## Final Decision

For requirement-to-test-case generation in this sprint, I would use **Generative AI** first, because it can quickly draft a broad set of test cases directly from the login requirements, saving the team significant time during sprint planning.

---

# 5. Task Instructions

1. Open https://practicesoftwaretesting.com/ and choose one feature flow (e.g., search, login, or checkout).
2. Create a table with exactly four rows: Symbolic AI, Classical ML, Deep Learning, Generative AI.
3. For each row, fill only these three fields:
   - One testing use case for your chosen Toolshop flow
   - One main strength
   - One caution/risk
4. Add one sentence: Which paradigm would you use first for requirement-to-test-case generation in this sprint, and why?

---

# 6. Expected Outcome / Deliverable

One markdown note containing the completed 4-row table and one final decision sentence.

---

# 7. Timebox

30 minutes

---

# 8. Hints (Optional)

- Stay concrete: write examples for your chosen Toolshop flow only.
- If unsure, start with Generative AI for draft test ideas and note one review risk.
