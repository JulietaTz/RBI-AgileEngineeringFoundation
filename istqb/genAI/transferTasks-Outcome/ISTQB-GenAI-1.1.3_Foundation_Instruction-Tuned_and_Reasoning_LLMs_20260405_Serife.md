### 1. Title
Select the Right LLM Type for Different Test Tasks

---

### 2. Syllabus Reference
ISTQB GenAI – 1.1.3 Foundation, Instruction-Tuned and Reasoning LLMs

---

### 3. Learning Objective
Differentiate model families and choose an appropriate model type for common QA activities.

---

### 4. Context / Scenario
Your organization offers multiple LLM options. Testers need a practical rule set for choosing a model type based on task complexity and expected output quality.

---

### 5. Task Instructions
1. Define three representative QA tasks:
   - Requirement summarization
   - Test-case generation
   - Defect root-cause reasoning
2. For each task, compare foundation, instruction-tuned, and reasoning models.
3. Rate each model type for: precision, consistency, explainability, and effort.
4. Create one decision rule per task (“if/then”).
5. Add one governance recommendation for model usage.

### 6. Expected Outcome / Deliverable
A decision matrix and 3 clear model-selection rules.

6.1 Anforderungszusammenfassung

| Modelltyp         | Präzision | Konsistenz | Erklärbarkeit | Aufwand |
| ----------------- | --------- | ---------- | ------------- | ------- |
| Foundation        | Mittel    | Niedrig    | Niedrig       | Niedrig |
| Instruction-Tuned | **Hoch**  | Mittel     | Mittel        | Mittel  |
| Reasoning         | Hoch      | Mittel     | Hoch          | Hoch    |

6.2 Testfallgenerierung

| Modelltyp         | Präzision | Konsistenz | Erklärbarkeit | Aufwand |
| ----------------- | --------- | ---------- | ------------- | ------- |
| Foundation        | Niedrig   | Niedrig    | Niedrig       | Niedrig |
| Instruction-Tuned | **Hoch**  | **Hoch**   | Mittel        | Mittel  |
| Reasoning         | Hoch      | Mittel     | Hoch          | Hoch    |

6.3 Ursachenanalyse von Fehlern

| Modelltyp         | Präzision | Konsistenz | Erklärbarkeit | Aufwand  |
| ----------------- | --------- | ---------- | ------------- | -------- |
| Foundation        | Niedrig   | Niedrig    | Niedrig       | Niedrig  |
| Instruction-Tuned | Mittel    | Mittel     | Mittel        | Mittel   |
| Reasoning         | **Hoch**  | Mittel     | **Hoch**      | **Hoch** |


## LLM-Auswahl für QA-Aufgaben

| Aufgabe                         | Regel / Entscheidung |
|--------------------------------|----------------------|
| Anforderungszusammenfassung    | Wenn Anforderungen strukturiert zusammengefasst werden sollen → **Instruction-Tuned LLM verwenden** |
| Testfallgenerierung (Standard) | Wenn Testfälle aus Anforderungen erstellt werden → **Instruction-Tuned LLM verwenden** |
| Testfallgenerierung (komplex)  | Wenn Szenario komplex ist → **Reasoning LLM verwenden** |
| Ursachenanalyse von Fehlern    | Wenn Ursache eines Fehlers analysiert werden soll → **Reasoning LLM verpflichtend verwenden** |

## Governance

- Verwende **immer das einfachste geeignete Modell**
- Verwende **Reasoning-Modelle nur bei Bedarf** (höherer Aufwand / höhere Kosten)
- Setze **Human-in-the-Loop** bei kritischen Ergebnissen ein
- Bewerte Ergebnisse anhand von Qualitätskriterien:
  - **Genauigkeit**
  - **Relevanz**
  - **Verständlichkeit**

---

### 7. Timebox
25 minutes

---

### 8. Hints (Optional)
- Keep ratings relative and evidence-based.
- Avoid “one model fits all” conclusions.
