### 1. Title
Explain How LLM Output Is Produced in a Testing Context

---

### 2. Syllabus Reference
ISTQB GenAI – 1.1.2 Foundations of Generative AI and LLMs

---

### 3. Learning Objective
Describe core LLM mechanics and connect them to practical implications for test analysis and design.

---

### 4. Context / Scenario
Your team uses AI outputs but often treats them as facts. You need a short learning artifact that explains why outputs can be plausible but still wrong (Keyword: Haluzination).

---

### 5. Task Instructions
1. Use this sample prompt: “Generate boundary value test ideas for cart quantity update.”
2. Describe in simple terms:
   - Tokenization
   - Context window usage
   - Next-token prediction
   - Why non-determinism can occur
   - Haluzination
3. List 3 consequences for testers when validating AI-generated test content.
4. Add 2 quality controls the team should always apply.

---

### 6. Expected Outcome / Deliverable
A markdown note with one short process description and a practical control checklist.

Process Description

Das Large Language Model (LLM) verarbeitet den Prompt schrittweise.
Zuerst wird der Text in Tokens zerlegt (Tokenization). Diese Tokens werden innerhalb eines begrenzten Kontextfensters verarbeitet, das bestimmt, wie viele Informationen gleichzeitig berücksichtigt werden können.

Die Ausgabe entsteht durch Next-token Prediction, wobei das Modell jeweils das wahrscheinlichste nächste Wort basierend auf gelernten Mustern auswählt.

Da mehrere mögliche Token-Sequenzen ähnliche Wahrscheinlichkeiten haben, ist der Prozess nicht deterministisch und kann unterschiedliche Ergebnisse erzeugen.

Das Modell kann außerdem sogenannte Halluzinationen erzeugen, bei denen Inhalte plausibel wirken, aber faktisch falsch oder nicht überprüft sind.

Consequences for Testers
KI-generierte Testfälle können logisch wirken, aber Fehler enthalten.
Wichtige Randfälle können fehlen oder falsch abgedeckt sein.
Ergebnisse dürfen nicht ungeprüft übernommen werden und müssen validiert werden.

Quality Control Checklist
KI-generierte Testfälle immer gegen Anforderungen und Business Rules prüfen
Kritische Szenarien (Boundary, Negative Cases) manuell validieren


### 7. Timebox
60 minutes

---

### 8. Hints (Optional)
- Keep the explanation test-team friendly (no deep math required).
- Focus on “what this means for verification.”
