### 1. Title
Define Team Guardrails for Chatbot-Based Test Work

---

### 2. Syllabus Reference
ISTQB GenAI – 1.2.2 AI Chatbots and LLM-based Test Applications

---

### 3. Learning Objective
Establish practical usage patterns and controls for chatbot-supported QA tasks.

---

### 4. Context / Scenario
Different testers use AI chatbots differently, producing inconsistent quality and compliance risk. The team needs a shared usage standard.

---

### 5. Task Instructions
1. Define 5 chatbot use cases in your QA workflow.
2. For each use case, specify:
   - Allowed input types
   - Required human review step
   - Expected output format
   - Main risk
3. Add 3 “not allowed” usage rules.
4. Propose one escalation path when chatbot output is unsafe or low quality.

# Team-Richtlinie: Chatbot-Nutzung im QA-Prozess
**CT-GenAI / fAVENGERS · Version 1.0 · Juni 2026 · ISTQB CT-GenAI 1.2.2**

---

## ✅ Erlaubte Use Cases

| # | Use Case | Erlaubte Eingaben | Human-Review-Schritt | Erwartetes Output-Format | Hauptrisiko |
|---|---|---|---|---|---|
| 1 | **Testfall-Generierung** (aus Anforderungen) | Anonymisierte User Stories, Akzeptanzkriterien (keine Prod-Daten) | Tester prüft Vollständigkeit & Abdeckung vor Übernahme ins Testtool | Markdown-Tabelle: ID, Titel, Vorbedingung, Schritte, Erwartetes Ergebnis | 🔴 Halluzination |
| 2 | **Anforderungsanalyse** (Lücken & Ambiguitäten) | Spec-Texte ohne personenbezogene Daten, Ticket-Beschreibungen | QA-Lead validiert identifizierte Lücken mit Product Owner | Liste offener Fragen + priorisierte Risiken | 🟡 Fehlinterpretation |
| 3 | **Defect-Analyse** (Root Cause Hypothesen) | Anonymisierte Fehlermeldungen, Stack Traces (ohne API-Keys/Passwörter) | Entwickler bestätigt Root Cause vor Bugfix-Umsetzung | Nummerierte Hypothesen + empfohlene Debug-Schritte | 🔴 Vertrauliche Daten |
| 4 | **Testdaten-Synthese** (Musterdaten generieren) | Datenstruktur/Schema (JSON/CSV-Template), Formatvorgaben | Tester prüft Datenformat & Plausibilität vor Einsatz im Test | JSON oder CSV, klar strukturiert, kommentiert | 🟡 Ungültige Formate |
| 5 | **Exploratives Testen** (Checklisten & Heuristiken) | Funktionsbeschreibungen, UI-Screenshots (ohne PII) | Tester wählt relevante Punkte aus, ergänzt domänenspezifisches Wissen | Priorisierte Checkliste mit Testtechniken-Hinweisen | 🟢 Oberflächlichkeit |

---

## 🚫 Nicht erlaubt (3 Regeln)

1. Keine echten Produktionsdaten, Passwörter, API-Keys oder personenbezogene Daten eingeben (DSGVO)
2. Kein direktes Übernehmen von Chatbot-Output ohne menschliche Prüfung („kein Copy & Deploy")
3. Keine Nutzung für Sicherheits- oder Compliance-Entscheidungen ohne Freigabe durch QA-Lead und Security-Team

---

## ⬆️ Eskalationspfad

Bei unsicherem oder minderwertigem Chatbot-Output:

1. Output als unzuverlässig markieren, nicht weiternutzen
2. QA-Lead informieren (Discord `#qa-ai-issues`) mit Prompt-Kopie & Output-Screenshot
3. Falls Datenleck möglich: Security-Team sofort einschalten
4. Lessons Learned im Weekly dokumentieren, Prompt-Bibliothek aktualisieren

---

> Diese Richtlinie gilt für alle Chatbot-Tools (Claude, ChatGPT, Copilot etc.) · Überarbeitung quartalsweise · Verantwortlich: QA-Lead

---

### 6. Expected Outcome / Deliverable
A one-page team guideline for chatbot usage in testing.

---

### 7. Timebox
60 minutes

---

### 8. Hints (Optional)
- Include privacy-safe input examples.
- Keep governance lightweight but explicit.
