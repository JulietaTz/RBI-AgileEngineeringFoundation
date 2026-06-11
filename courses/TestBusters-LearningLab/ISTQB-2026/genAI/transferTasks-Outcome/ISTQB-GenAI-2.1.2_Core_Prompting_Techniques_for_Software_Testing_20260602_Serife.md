# Vergleich von Core Prompting-Techniken anhand einer QA-Aufgabe

## Referenz zum ISTQB Syllabus Kapitel

ISTQB GenAI –  2.1.2 Core Prompting Techniques for Software Testing

## Link zur Transfer Task Datei

*https://github.com/rgroetz2/TBLL-AgileEngineeringFoundation/blob/main/courses/TestBusters-LearningLab/ISTQB-2026/genAI/transferTasks/ISTQB-GenAI-2.1.2_Core_Prompting_Techniques_for_Software_Testing_20260405.md*

---

## Outcome

### Szenario

**Checkout-Validierung**

Anforderung:

Ein Benutzer kann den Checkout nur abschließen, wenn alle Pflichtfelder (Name, E-Mail, Adresse, Zahlungsmethode) korrekt ausgefüllt sind.

---

### Prompt 1 – Prompt-Verkettung

#### Schritt 1
Analysiere die Anforderung und identifiziere alle Validierungsregeln.

#### Schritt 2
Generiere positive und negative Test-Szenarien.

#### Schritt 3
Konvertiere die Szenarien in strukturierte Test-Cases.

---

### Prompt 2 – Few-Shot Prompting

Beispiel:

Anforderung: Der Benutzer muss eine gültige E-Mail-Adresse eingeben.

Test-Case: TC-001 – Validierung einer gültigen E-Mail-Adresse

Generiere Test-Cases für die Checkout-Validierungsanforderung unter Verwendung der gleichen Struktur.

---

### Prompt 3 – Meta-Prompting

Erstelle den bestmöglichen Prompt für die Generierung umfassender Test-Cases zur Checkout-Validierung.

Berücksichtige:

* Positive Test-Cases
* Negative Test-Cases
* Edge Cases
* Ausführungsreife Test-Cases

Nutze den generierten Prompt, um die finalen Test-Cases zu erstellen.

---

### Learning Summary

| Prompting-Technik | Takeaway |
|---|---|
| Prompt-Verkettung | Zerlegung in Schritte verbessert Vollständigkeit und Nachvollziehbarkeit |
| Few-Shot Prompting | Beispiele helfen dem Modell, konsistente und strukturierte Ausgaben zu produzieren |
| Meta-Prompting | Das Modell kann selbst Prompts erstellen und optimieren |
| Output-Qualität | Prompt-Design beeinflusst direkt die Qualität generierter Test-Artefakte |
| Test-Case-Generierung | Verschiedene Techniken eignen sich für unterschiedliche Testsituationen |

> Behandle Prompt Engineering als eine Testing-Skill: Die Qualität des Prompts beeinflusst die Qualität, Vollständigkeit und Nutzerfreundlichkeit von KI-generierten Test-Outputs.