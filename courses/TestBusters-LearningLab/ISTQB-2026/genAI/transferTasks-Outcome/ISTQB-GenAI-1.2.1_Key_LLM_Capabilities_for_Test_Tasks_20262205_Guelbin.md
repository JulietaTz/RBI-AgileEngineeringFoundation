### 1. Titel
LLM-Fähigkeiten den QA-Lifecycle-Aktivitäten zuordnen

---

### 2. Lehrplan-Referenz
ISTQB GenAI – 1.2.1 Zentrale LLM-Fähigkeiten für Testaufgaben

---

### 3. Lernziel
Erkennen, welche LLM-Fähigkeiten in bestimmten Testaufgaben praktischen Mehrwert schaffen und wo weiterhin Risiken bestehen.

---

### 4. Kontext / Szenario
Dein Testleiter möchte den Einsatz von KI nach ihrem Nutzen priorisieren. Für die Sprint-Planung benötigst du eine klare Zuordnung von Fähigkeiten zu Aufgaben.

---

### 5. Aufgabenstellung
1. Liste mindestens 6 LLM-Fähigkeiten auf (z. B. Zusammenfassung, Extraktion, Generierung, Klassifikation, Transformation, Erklärung).
2. Ordne jede Fähigkeit einer QA-Aktivität zu (Analyse, Design, Ausführungsunterstützung, Triage, Reporting, Monitoring).
3. Definiere für jede Zuordnung den erwarteten Nutzen sowie das wichtigste Risiko.
4. Wähle die 2 wichtigsten „Quick Wins“ für die sofortige Einführung im Team aus.

---

### 6. Erwartetes Ergebnis / Deliverable
Eine Zuordnungstabelle der Fähigkeiten mit Nutzen, Risiken und einer Quick-Win-Empfehlung.

# LLM-Fähigkeiten-Mapping – QA-Lifecycle-Aktivitäten

## 1. Fähigkeiten-Mapping (Übersicht)

Die folgende Matrix zeigt mögliche Einsatzbereiche zentraler LLM-Fähigkeiten entlang des QA-Lifecycles. Für die weitere Bewertung wurden die wichtigsten Zuordnungen hinsichtlich Nutzen und Risiken ausgewählt.

| QA-Aktivitäten | Zusammenfassung | Extraktion | Generierung | Klassifikation | Transformation | Erklärung |
|---------------|----------------|------------|------------|---------------|---------------|-----------|
| **Analyse** | Lange Anforderungen oder User Stories schnell zusammenfassen | Wichtige Informationen aus Anforderungen extrahieren | Fragen zur Klärung von Anforderungen generieren | Anforderungen nach Priorität und Kritikalität klassifizieren | Anforderungen in strukturierte Testbedingungen umwandeln | Komplexe Anforderungen verständlich erklären |
| **Testdesign** | Testbedingungen aus umfangreicher Dokumentation extrahieren und zusammenfassen | Relevante Regeln aus Anforderungen für Testfälle und Testbedingungen extrahieren | Testfälle auf Basis von Anforderungen generieren | Testfälle nach Funktionen und Priorität klassifizieren | Anforderungen oder User Stories in Testfälle umwandeln | Komplexe Testfälle verständlich erklären |
| **Unterstützung der Testausführung** | Lange Testlogs oder Fehlermeldungen kompakt zusammenfassen | Wichtige Informationen aus Testlogs extrahieren | Testskripte generieren oder verbessern und geeignete Testverfahren vorschlagen | Testergebnisse nach Fehlertyp klassifizieren | Testergebnisse in strukturierte Form umwandeln | Komplexe oder unklare Testergebnisse erklären |
| **Triage** | Relevante Informationen aus ähnlichen Bug-Reports kompakt zusammenfassen | Relevante Informationen aus ähnlichen Bug-Reports kompakt zusammenfassen	Wichtige Fehlerinformationen aus Tickets extrahieren | Wichtige Fehlerinformationen aus Tickets extrahieren	Vorschläge zur Priorisierung und Bewertung von Fehlern erstellen | Fehler nach Typ, Auswirkung und Dringlichkeit klassifizieren | Bug-Reports in strukturierte Entscheidungsgrundlagen umwandeln | Auswirkungen kritischer Fehler verständlich erläutern |
| **Reporting** | Testergebnisse für Stakeholder übersichtlich zusammenfassen | Relevante Informationen für Testberichte extrahieren | Testergebnisse verständlich zusammenfassen und dokumentieren | Testergebnisse nach Priorität für Berichte klassifizieren | Rohdaten in strukturierte Testberichte umwandeln | Komplexe Testergebnisse verständlich interpretieren |
| **Monitoring** | Monitoring-Daten oder Alerts kompakt zusammenfassen | Relevante Informationen aus Monitoring-Daten extrahieren | Monitoring-Berichte automatisch generieren | Anomalien in Monitoring-Daten klassifizieren | Monitoring-Daten in strukturierte Berichte umwandeln | Warnmeldungen und Anomalien verständlich erklären |

---

## 2. Nutzen und Risiken

| LLM-Fähigkeit | QA-Aktivität | Erwarteter Nutzen | Wichtigstes Risiko |
|--------------|-------------|-------------------|-------------------|
| Zusammenfassung | Analyse | Lange Anforderungen schnell erfassen und Zeit sparen | Wichtige Details oder Randbedingungen können verloren gehen |
| Generierung | Testdesign | Schnelle Erstellung oder Verbesserung von Testfällen und geeigneten Testideen | Unvollständige oder fachlich falsche Testfälle |
| Extraktion | Testausführungsunterstützung | Relevante Informationen aus Logs und Fehlermeldungen schneller finden | Falsche Interpretation von Log-Einträgen |
| Klassifikation | Triage | Fehler nach Typ, Auswirkung und Dringlichkeit klassifizieren, um die Entscheidungsfindung zu unterstützen | Fehlklassifizierung kritischer Defekte |
| Transformation | Reporting | Rohdaten automatisch in verständliche Berichte umwandeln | Ergebnisse können verfälscht oder unvollständig dargestellt werden |
| Erklärung | Monitoring | Anomalien und Warnmeldungen verständlich erläutern | Halluzinationen oder falsche Ursachenanalysen |

---

## 3. Quick-Win-Empfehlungen

| Quick Win | Nutzen |
|-----------|---------|
| **Zusammenfassung + Analyse** | - Schnelle Erfassung langer Anforderungen<br>- Hohe Zeitersparnis bei geringem Risiko<br>- Beispiel: Eine 15-seitige Spezifikation wird auf die wichtigsten Anforderungen und Akzeptanzkriterien zusammengefasst. |
| **Generierung + Testdesign** | - Schnelle Erstellung oder Verbesserung von Testfällen und geeigneten Testideen<br>- Unterstützung bei der Verbesserung bestehender Testskripte<br>- Beispiel: Aus einer User Story werden positive, negative und Grenzwert-Testfälle vorgeschlagen. |

---

## Fazit 

LLMs können zahlreiche QA-Aktivitäten entlang des gesamten Testprozesses unterstützen. Besonders bei der Anforderungsanalyse und im Testdesign bieten sie ein hohes Effizienzpotenzial. Die Zusammenfassung von Anforderungen sowie die Generierung von Testfällen ermöglichen schnelle Produktivitätsgewinne bei vergleichsweise geringem Einführungsaufwand. Um Risiken wie Halluzinationen oder Fehlklassifikationen zu minimieren, sollten die Ergebnisse jedoch stets fachlich überprüft werden.

---

### 7. Zeitvorgabe
20 Minuten

---

### 8. Hinweise (Optional)
- Priorisiere Aktivitäten mit wiederholtem kognitivem Aufwand.
- Halte die Risikobeschreibungen konkret und bezogen auf den Testprozess.