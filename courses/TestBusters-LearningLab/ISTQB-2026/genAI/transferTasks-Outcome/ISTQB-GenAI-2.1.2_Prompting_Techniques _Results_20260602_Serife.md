# 2.1.1 - Vergleich und Bewertung von Prompting-Techniken für Softwaretest

## Referenz zum ISTQB Syllabus Kapitel

ISTQB GenAI –  2.1.1 Prompt Engineering und LLM-basierte Testanwendungen

## Link zur Transfer Task Datei

*https://github.com/rgroetz2/TBLL-AgileEngineeringFoundation/blob/main/courses/TestBusters-LearningLab/ISTQB-2026/genAI/transferTasks/ISTQB-GenAI-2.1.2_Core_Prompting_Techniques_for_Software_Testing_20260405.md*

---

## Outcome

### Vergleich der Prompting-Techniken

#### Bewertung

| Technik            | Korrektheit | Vollständigkeit | Ausführungsreife |
| ------------------ | ----------- | --------------- | ---------------- |
| Prompt-Verkettung  | Hoch        | Hoch            | Hoch             |
| Few-Shot Prompting | Hoch        | Mittel          | Hoch             |
| Meta-Prompting     | Mittel      | Hoch            | Mittel           |

---

### Erkenntnisse

#### Prompt-Verkettung
* Liefert die vollständigsten Ergebnisse
* Ermöglicht eine strukturierte Vorgehensweise
* Besonders geeignet für komplexe Testaufgaben

#### Few-Shot Prompting
* Schnell und einfach einsetzbar
* Liefert konsistente Ergebnisse
* Geeignet, wenn bereits Beispiele vorhanden sind

#### Meta-Prompting
* Hilft bei der Optimierung von Prompts
* Unterstützt die Wiederverwendung von Prompts
* Weniger geeignet für die direkte Testausführung

---

### Empfehlung für praktische Anwendung

| Situation                                    | Empfohlene Technik |
| -------------------------------------------- | ------------------ |
| Schnelle Erstellung mit vorgegebenem Format  | Few-Shot Prompting |
| Komplexe Analyse und Testfallerstellung      | Prompt-Verkettung  |
| Optimierung und Wiederverwendung von Prompts | Meta-Prompting     |

---

### Learning Summary

| Concept            | One-line Takeaway                                                           |
| ------------------ | --------------------------------------------------------------------------- |
| Prompt-Verkettung  | Strukturierte Mehrstufen-Prompts liefern vollständige und nachvollziehbare Testergebnisse |
| Few-Shot Prompting | Mit aussagekräftigen Beispielen lassen sich schnell konsistente Test-Artefakte erzeugen |
| Meta-Prompting     | Durch Reflexion auf die Prompts selbst entstehen wiederverwendbare Qualitäts-Templates |

---

### Fazit

Für Softwaretest-Aufgaben mit hohen Anforderungen an Qualität und Vollständigkeit liefert die **Prompt-Verkettung** die besten Ergebnisse.

**Few-Shot Prompting** eignet sich besonders für standardisierte Ausgaben, während **Meta-Prompting** vor allem bei der Verbesserung und Wiederverwendung von Prompts einen Mehrwert bietet.

> *Jeden KI-generierten Test-Artefakt als unreviewed First Draft eines Junior-Kollegen behandeln, der das eigene System nie gesehen hat.*