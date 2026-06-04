### 1. Titel

Team-Guidelines für den Einsatz von Chatbot-basierten Testaufgaben definieren

---

### 2. Lehrplan-Referenz

ISTQB GenAI – 1.2.2 KI-Chatbots und LLM-basierte Testanwendungen

---

### 3. Lernziel

Praktische Nutzungsmuster und Kontrollmechanismen für chatbot-gestützte QA-Aufgaben festlegen.

---

### 4. Kontext / Szenario

Verschiedene Testerinnen und Tester nutzen KI-Chatbots auf unterschiedliche Weise. Dadurch entstehen inkonsistente Ergebnisse sowie potenzielle Qualitäts- und Compliance-Risiken. Das Team benötigt daher einen gemeinsamen Standard für die Nutzung von Chatbots.

---

### 5. Aufgabenstellung

1. Definiere 5 Anwendungsfälle für Chatbots in deinem QA-Workflow.
2. Lege für jeden Anwendungsfall fest:
   - Zulässige Eingabetypen
   - Erforderlichen menschlichen Review-Schritt
   - Erwartetes Ausgabeformat
   - Hauptrisiko
3. Formuliere 3 Regeln für nicht erlaubte Nutzungen.
4. Schlage einen Eskalationsprozess vor, wenn die Ausgabe des Chatbots unsicher oder von geringer Qualität ist.

---

### 6. Erwartetes Ergebnis / Deliverable

Eine einseitige Team-Richtlinie für die Nutzung von Chatbots im Software Testing.

## Team-Richtlinie für die Nutzung von KI-Chatbots im Software Testing

Ziel dieser Richtlinie ist die einheitliche und verantwortungsvolle Nutzung von KI-Chatbots im Software-Testprozess. Die folgenden Regeln definieren zulässige Anwendungsfälle, notwendige Kontrollmaßnahmen sowie den Umgang mit unsicheren KI-Ausgaben.

## Zulässige Anwendungsfälle

| Nr. | Anwendungsfall | Zulässige Eingabetypen | Erforderlicher menschlicher Review-Schritt | Erwartetes Ausgabeformat | Hauptrisiko |
|------|----------------|------------------------|-------------------------------------------|--------------------------|-------------|
| 1 | Erstellung von Testfällen | - User Story<br>- Acceptance Criteria | - Tester prüft Vollständigkeit und Relevanz der Testfälle<br>- Senior Tester validiert kritische Testfälle | Testfall-Tabelle | Unvollständige Testabdeckung |
| 2 | Analyse von Anforderungen | - Anforderungsdokumente<br>- User Stories<br>- Acceptance Criteria | - Tester prüft identifizierte Unklarheiten<br>- PO bestätigt fachliche Interpretation | Liste der Unklarheiten und Risiken | Falsche Interpretation von Anforderungen |
| 3 | Generierung von Testdaten | - Acceptance Criteria<br>- Anonymisierte Beispieldaten<br>- Feldbeschreibungen | - Tester validiert Korrektheit, Vollständigkeit und Datenschutzkonformität | Tabelle | Datenschutzrisiken |
| 4 | Verbesserung von Bug Reports | - Testergebnisse<br>- Defect-/Bug-Reports<br>- Fehlermeldungen | - Tester überprüft technische Korrektheit und Vollständigkeit | Überarbeiteter Bug Report (strukturierter Text) | Verlust wichtiger technischer Details |
| 5 | Erstellung von Test-Checklisten | - Anforderungsdokumente<br>- User Stories<br>- Prozessbeschreibungen | - Tester überprüft Vollständigkeit und Relevanz der Prüfpunkte | Liste | Fehlende wichtige Prüfpunkte |

## Regeln für nicht erlaubte Nutzungen

1. Keine Eingabe sensibler oder personenbezogener Daten.
2. KI-Ergebnisse dürfen nicht ohne menschliche Prüfung übernommen werden.
3. Der Chatbot darf keine fachlichen oder qualitätsrelevanten Entscheidungen treffen.

## Eskalationsprozess

1. Tester überprüft die Ausgabe auf Korrektheit und Vollständigkeit.
2. Die Ergebnisse werden mit den Anforderungen verglichen.
3. Bei Unsicherheiten erfolgt ein Review durch einen anderen Tester.
4. Bei weiterhin bestehenden Unsicherheiten erfolgt eine Abstimmung mit dem Product Owner.
5. Falls erforderlich, werden die Prompts angepasst und die Anfrage erneut ausgeführt.

## Hinweis

KI-Chatbots dienen ausschließlich als Unterstützung und ersetzen keine fachliche Prüfung durch das Testteam.

---

### 7. Zeitvorgabe

60 Minuten

---

### 8. Hinweise (Optional)

- Verwende datenschutzkonforme Beispiele für Eingaben.
- Halte die Governance möglichst schlank, aber eindeutig und nachvollziehbar.