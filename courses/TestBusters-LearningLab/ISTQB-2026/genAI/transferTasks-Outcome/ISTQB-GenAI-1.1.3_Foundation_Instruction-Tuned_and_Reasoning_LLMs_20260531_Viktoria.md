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
---

1. Drei repräsentative QA-Aufgaben definieren

a) Anforderungszusammenfassung (Requirement Summarization)
Lange Anforderungen in kurze und verständliche Zusammenfassungen umwandeln.
b) Testfallerstellung (Test-Case Generation)
Aus Anforderungen positive, negative und Grenzwert-Testfälle erstellen.
c) Fehlerursachenanalyse (Defect Root-Cause Reasoning)
Logs, Anforderungen und Testergebnisse analysieren, um die Ursache eines Fehlers zu finden.

2. Vergleich der Modelltypen
| QA-Aufgabe                  | Foundation Model                                         | Instruction-Tuned Model                            | Reasoning Model                                  |
| --------------------------- | -------------------------------------------------------- | -------------------------------------------------- | ------------------------------------------------ |
| Anforderungszusammenfassung | Kann zusammenfassen, aber oft inkonsistent.              | Liefert klare und strukturierte Zusammenfassungen. | Sehr präzise, erkennt auch Zusammenhänge.        |
| Testfallerstellung          | Erstellt einfache Testfälle, übersieht jedoch Randfälle. | Erzeugt konsistente und strukturierte Testfälle.   | Findet zusätzlich Edge Cases und Risiken.        |
| Fehlerursachenanalyse       | Schwach bei komplexen Analysen.                          | Geeignet für einfache Ursachenanalysen.            | Sehr stark bei logischer Analyse und Begründung. |

3. Bewertung der Modelltypen
| Modelltyp         | Präzision | Konsistenz | Erklärbarkeit | Aufwand |
| ----------------- | --------- | ---------- | ------------- | ------- |
| Foundation        | Mittel    | Niedrig    | Niedrig       | Hoch    |
| Instruction-Tuned | Hoch      | Hoch       | Mittel        | Niedrig |
| Reasoning         | Sehr hoch | Hoch       | Sehr hoch     | Mittel  |

4. Entscheidungsregeln
Anforderungszusammenfassung
Wenn Anforderungen kurz und verständlich zusammengefasst werden sollen, dann ein Instruction-Tuned Model verwenden.

Testfallerstellung
Wenn eine hohe Testabdeckung und die Erkennung von Edge Cases erforderlich sind, dann ein Reasoning Model verwenden.

Fehlerursachenanalyse
Wenn Beweise analysiert und Ursachen nachvollziehbar erklärt werden müssen, dann ein Reasoning Model verwenden.

5. Governance-Empfehlung
Empfehlung:
Alle durch KI erzeugten Zusammenfassungen, Testfälle und Fehleranalysen müssen vor der Nutzung von einem Tester überprüft und freigegeben werden.

Begründung:
LLMs können Fehler machen, Informationen auslassen oder falsche Annahmen treffen. Die menschliche Überprüfung stellt Qualität und Zuverlässigkeit sicher.


### 6. Expected Outcome / Deliverable
A decision matrix and 3 clear model-selection rules.

---
Ergebnis (Decision Matrix)
| QA-Aufgabe                  | Foundation | Instruction-Tuned | Reasoning | Empfohlen         |
| --------------------------- | ---------- | ----------------- | --------- | ----------------- |
| Anforderungszusammenfassung | Mittel     | Hoch              | Hoch      | Instruction-Tuned |
| Testfallerstellung          | Mittel     | Hoch              | Sehr hoch | Reasoning         |
| Fehlerursachenanalyse       | Niedrig    | Mittel            | Sehr hoch | Reasoning         |

Modell-Auswahlregeln
1.Wenn Anforderungen zusammengefasst werden sollen, dann ein Instruction-Tuned Model verwenden.
2.Wenn Testfälle mit hoher Abdeckung benötigt werden, dann ein Reasoning Model verwenden.
3.Wenn Fehlerursachen analysiert und begründet werden müssen, dann ein Reasoning Model verwenden.

Governance-Regel
KI-generierte Testergebnisse müssen immer von einem Menschen überprüft werden.

### 7. Timebox
25 minutes

---

### 8. Hints (Optional)
- Keep ratings relative and evidence-based.
- Avoid “one model fits all” conclusions.
