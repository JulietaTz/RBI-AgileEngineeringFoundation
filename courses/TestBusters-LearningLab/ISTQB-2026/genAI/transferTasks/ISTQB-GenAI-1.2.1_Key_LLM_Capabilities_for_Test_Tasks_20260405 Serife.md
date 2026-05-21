### 1. Title
Map LLM Capabilities to QA Lifecycle Activities

---

### 2. Syllabus Reference
ISTQB GenAI – 1.2.1 Key LLM Capabilities for Test Tasks

---

### 3. Learning Objective
Identify which LLM capabilities create practical value in specific test tasks and where risks remain.

---

### 4. Context / Scenario
Your test lead wants to prioritize AI usage by value. You need a clear capability-to-task mapping for sprint planning.

---

### 5. Task Instructions
1. List at least 6 LLM capabilities (e.g., summarization, extraction, generation, classification, transformation, explanation).
2. Map each capability to one QA activity (analysis, design, execution support, triage, reporting, monitoring).
3. For each mapping, define expected benefit and key risk.
4. Select top 2 “quick wins” for immediate team adoption.

---

### 6. Expected Outcome / Deliverable
A capability mapping table with benefits, risks, and quick-win recommendation.

# LLM-Fähigkeiten-Mapping – QA-Lifecycle-Aktivitäten

**Lehrplanreferenz:** ISTQB CT-GenAI – 1.2.1  
**Lernziel:** Identifizieren, welche LLM-Fähigkeiten praktischen Nutzen schaffen.

---

## Fähigkeiten-Mapping-Tabelle

| # | LLM-Fähigkeit | QA-Aktivität | Erwarteter Nutzen | Wichtigstes Risiko |
|---|--------------|--------------|-------------------|-------------------|
| 1 | **Anforderungsanalyse** | Analyse | Identifiziert Unklarheiten und fehlende Informationen | Halluzination — LLM kann nicht vorhandene Anforderungen hinzufügen |
| 2 | **Testfallerstellung** | Design | Spart Zeit durch schnelle Generierung von Testfällen | Kann anwendungsspezifische Szenarien übersehen |
| 3 | **Testdatengenerierung** | Ausführungsunterstützung | Generiert schnell große Datensätze einschließlich Grenzwerte | Generierte Daten spiegeln möglicherweise keine echten Produktionsdaten wider |
| 4 | **Testautomatisierungsunterstützung** | Ausführungsunterstützung | Generiert Testskripte aus Testfallbeschreibungen | Skripte können Fehler enthalten oder Standards nicht entsprechen |
| 5 | **Testergebnisanalyse** | Fehlertriage | Klassifiziert Fehler nach Schweregrad und Priorität | Kann Fehler falsch klassifizieren — Halluzination möglich |
| 6 | **Testdokumentation** | Berichterstattung | Generiert schnell Testberichte und Fehlerberichte | Kann projektspezifischen Kontext nicht berücksichtigen |

---

## Quick-Win-Empfehlungen

| Quick Win | Nutzen | Beispiel |
|-----------|--------|---------|
| ⭐ **Testfallerstellung** | Sofortige Zeitersparnis — kein Setup nötig | T1–T5 Prompt → 10+ Testfälle in Minuten |
| ⭐ **Testergebnisanalyse** | Schnellere Fehlertriage nach jedem Sprint | Testprotokoll einfügen → KI liefert Zusammenfassung |

---

## Wichtigste Erkenntnis

> Generative KI schafft Mehrwert bei textbasierten QA-Aktivitäten.  
> Sie kann jedoch manuelle Erkundung nicht ersetzen.

---

### 7. Timebox
20 minutes

---

### 8. Hints (Optional)
- Prioritize activities with repetitive cognitive effort.
- Keep risk statements concrete and test-process related.
