### 1. Title
Transform Weak Prompts into High-Quality Test Prompts

---

### 2. Syllabus Reference
ISTQB GenAI – 2.1.1 Structure of Prompts for Generative AI in Testing

---

### 3. Learning Objective
Apply a structured prompt pattern to increase quality, consistency, and actionability of AI-generated test outputs.

---

### 4. Context / Scenario
Your team receives generic and inconsistent AI results because prompts vary by tester. You need a standard prompt structure.

---

### 5. Task Instructions
1. Review "APP: Unstructured Prompts for Test Activities" in this task.
2. Rewrite each using: role, context, task, constraints, output format, acceptance criteria.
3. Run old and new prompts.
4. Compare results on clarity, completeness, and testability.
5. Define one reusable prompt template for the team.

---

### 6. Expected Outcome / Deliverable
Before/after prompt set and one shared template as md-file.

---

### 7. Timebox
50 minutes

---

### 8. Hints (Optional)
- Use explicit output schema (table fields, numbering, priority).
- Keep constraints realistic and verifiable.

# APP: Unstructured Prompts for Test Activities

## Prompt 1: Exploratory Testing
You are testing the registration workflow of a webshop that now
supports social media login (Google, GitHub, LinkedIn).

Explore the feature freely. Try different entry points, switch between
registration methods, interrupt flows, and combine actions in unusual
ways.

Focus on discovering unexpected behavior, inconsistencies, or usability
issues. Document what you observe and what surprises you.

---

## Prompt 2: Test Design
You are responsible for designing test cases for the extended
registration feature with social media login.

Without using a predefined structure, think about what should be tested.
Consider normal usage, variations, edge cases, and failures.

Write down the test scenarios that you believe are most important to
ensure quality.

---

## Prompt 3: Risk-Based Testing
Assume the social media registration feature will go live tomorrow.

You do not have time to test everything.

Based on your understanding, identify the biggest risks related to this
feature. Focus your testing on areas that could cause the highest impact
for users or the business.

Explain what you would test first and why.

# Prompt Engineering für QA-Aufgaben
**ISTQB CT-GenAI – 2.1.1 | Team: CT-GenAI / fAVENGERS | Juni 2026**

---

## Prompt-Struktur nach ISTQB 2.1.1

| Komponente | Beschreibung |
|---|---|
| **Rolle** | Perspektive / Persona, die das LLM einnehmen soll |
| **Kontext** | Hintergrundinformationen zum Testobjekt und zur Situation |
| **Instruktion** | Klare, imperativ formulierte Aufgabenbeschreibung |
| **Eingabedaten** | User Stories, Akzeptanzkriterien, Schemas, Beispiele |
| **Einschränkungen** | Limits, Datenschutzregeln, Scope-Grenzen |
| **Ausgabeformat** | Gewünschtes Format, Felder, Nummerierung, Priorität |

---

## Before / After Vergleich

---

### Prompt 1 – Exploratives Testen

#### ❌ VORHER (unstrukturiert)

```
You are testing the registration workflow of a webshop that now supports 
social media login (Google, GitHub, LinkedIn).
Explore the feature freely. Try different entry points, switch between 
registration methods, interrupt flows, and combine actions in unusual ways.
Focus on discovering unexpected behavior, inconsistencies, or usability issues. 
Document what you observe and what surprises you.
```

**Probleme:** Keine klare Aufgabe, kein Output-Format, keine Einschränkungen,
Ergebnisse nicht reproduzierbar, stark testerabhängig.

---

#### ✅ NACHHER (strukturiert)

```
ROLLE:
Du bist ein erfahrener QA-Engineer mit Schwerpunkt auf explorativem Testen 
von Authentifizierungssystemen.

KONTEXT:
Ein Webshop hat seine Registrierung um Social-Media-Login erweitert 
(Google, GitHub, LinkedIn). Die bestehende E-Mail-Registrierung bleibt erhalten. 
Das Feature befindet sich im Staging-Environment vor dem Go-Live.

INSTRUKTION:
Erstelle eine strukturierte Explorations-Charter für das Social-Login-Feature. 
Identifiziere kritische Testbereiche, ungewöhnliche Nutzungspfade und mögliche 
Inkonsistenzen zwischen den Login-Methoden.

EINGABEDATEN:
- Feature: Social-Media-Login (Google, GitHub, LinkedIn)
- Bestehendes Feature: klassische E-Mail/Passwort-Registrierung
- Testumgebung: Staging, keine echten OAuth-Tokens verwenden

EINSCHRÄNKUNGEN:
- Keine echten Nutzerdaten oder OAuth-Zugangsdaten im Prompt nennen
- Maximal 5 Explorations-Bereiche
- Fokus auf funktionale und UX-Risiken, kein Sicherheits-Penetrationstest

AUSGABEFORMAT:
Markdown-Tabelle mit den Spalten:
| # | Explorationsbereich | Testziel | Ungewöhnliche Szenarien | Erwartetes Risiko (Hoch/Mittel/Niedrig) |
```

**Verbesserungen:** Klare Scope-Begrenzung, reproduzierbar, Output direkt ins
Testtool übernehmbar, Datenschutz explizit adressiert.

---

### Prompt 2 – Test Design

#### ❌ VORHER (unstrukturiert)

```
You are responsible for designing test cases for the extended registration 
feature with social media login.
Without using a predefined structure, think about what should be tested. 
Consider normal usage, variations, edge cases, and failures.
Write down the test scenarios that you believe are most important to ensure quality.
```

**Probleme:** „Without using a predefined structure" konterkariert gutes Test Design,
keine Akzeptanzkriterien, Output-Format völlig offen.

---

#### ✅ NACHHER (strukturiert)

```
ROLLE:
Du bist ein ISTQB-zertifizierter Test Designer mit Erfahrung in 
Äquivalenzklassenanalyse und Grenzwertanalyse.

KONTEXT:
User Story: „Als Nutzer möchte ich mich mit meinem Google-, GitHub- oder 
LinkedIn-Account registrieren, damit ich kein neues Passwort benötige."
Akzeptanzkriterien:
- Erfolgreiche OAuth-Weiterleitung für alle drei Provider
- Korrekte Übernahme von Name und E-Mail aus dem Social Profile
- Fehlermeldung bei bereits registrierter E-Mail-Adresse
- Abbruch des OAuth-Flows führt zur Registrierungsseite zurück

INSTRUKTION:
Generiere Testfälle für das Social-Login-Feature nach der 
Äquivalenzklassenanalyse. Decke Happy Path, negative Fälle und Grenzwerte ab.

EINGABEDATEN:
Akzeptanzkriterien wie oben angegeben.
Provider: Google, GitHub, LinkedIn.

EINSCHRÄNKUNGEN:
- Maximal 12 Testfälle
- Keine echten OAuth-Credentials als Testdaten verwenden (Platzhalter nutzen)
- Jeder Testfall muss einem Akzeptanzkriterium zugeordnet sein

AUSGABEFORMAT:
Nummerierte Tabelle:
| TC-ID | Titel | Vorbedingung | Testschritte | Erwartetes Ergebnis | AC-Referenz | Priorität (H/M/N) |
```

**Verbesserungen:** Direkte Anbindung an Akzeptanzkriterien, klare Testfall-Limite,
Traceability zu AC sichergestellt.

---

### Prompt 3 – Risikobasiertes Testen

#### ❌ VORHER (unstrukturiert)

```
Assume the social media registration feature will go live tomorrow.
You do not have time to test everything.
Based on your understanding, identify the biggest risks related to this feature. 
Focus your testing on areas that could cause the highest impact for users or 
the business. Explain what you would test first and why.
```

**Probleme:** Kein Kontext zur Anwendung, keine Risikokategorien vorgegeben,
Output stark LLM-abhängig, nicht reproduzierbar.

---

#### ✅ NACHHER (strukturiert)

```
ROLLE:
Du bist ein QA-Lead mit Erfahrung in risikobasierter Teststrategie 
(ISTQB-Standard) für B2C-E-Commerce-Anwendungen.

KONTEXT:
Go-Live ist in 24 Stunden. Das neue Feature (Social-Media-Login via Google, 
GitHub, LinkedIn) wurde im Staging getestet, aber nicht vollständig abgenommen. 
Es handelt sich um ein öffentliches Webshop-System mit ca. 5.000 aktiven Nutzern. 
Bekannte Voraussetzung: OAuth-Redirect-URLs sind konfiguriert, aber noch nicht 
in der Produktion verifiziert.

INSTRUKTION:
Erstelle eine priorisierte Risikoliste für das Social-Login-Feature und 
definiere die Testreihenfolge für das verbleibende Zeitfenster.

EINGABEDATEN:
- Feature: Social-Media-Login (Google, GitHub, LinkedIn)
- Bekannte Einschränkung: OAuth-Redirect-URLs in Produktion ungetestet
- Nutzerbasis: 5.000 aktive Accounts, vorwiegend E-Mail-registriert

EINSCHRÄNKUNGEN:
- Maximal 8 Risiken
- Nur Risiken mit direktem User- oder Business-Impact
- Keine theoretischen Sicherheitsrisiken ohne konkreten Angriffsvektor

AUSGABEFORMAT:
| Priorität | Risiko | Auswirkung | Wahrscheinlichkeit | Empfohlener Test | Zeitaufwand (min) |
Abschließend: Empfohlene Testreihenfolge als nummerierte Liste.
```

**Verbesserungen:** Konkreter Zeitdruck und Systemkontext geben dem LLM echte
Entscheidungsgrundlage, Output direkt als Teststrategie-Dokument verwendbar.

---

## Vergleich: Qualitätskriterien

| Kriterium | Unstrukturierte Prompts | Strukturierte Prompts |
|---|---|---|
| **Klarheit** | Vage, interpretationsabhängig | Eindeutig, imperativ formuliert |
| **Vollständigkeit** | Fehlende Komponenten (kein Format, kein Scope) | Alle 6 ISTQB-Komponenten vorhanden |
| **Testbarkeit** | Output nicht direkt nutzbar | Output direkt ins Testtool übernehmbar |
| **Reproduzierbarkeit** | Stark testerabhängig | Konsistente Ergebnisse bei Wiederholung |
| **Datenschutz** | Nicht adressiert | Explizit als Einschränkung definiert |
| **Human-Review-Aufwand** | Hoch (viel Nacharbeit nötig) | Niedrig (strukturiertes Format) |

---

## Wiederverwendbares Team-Prompt-Template

```markdown
## [AUFGABENTYP] – Prompt Template v1.0
**Datum:** [YYYY-MM-DD] | **Erstellt von:** [Name] | **Use Case:** [z.B. Testfall-Generierung]

---

ROLLE:
Du bist ein [erfahrener QA-Engineer / Test Designer / QA-Lead] mit Schwerpunkt 
auf [Bereich, z.B. Authentifizierungssystemen / REST-APIs / UI-Testing].

KONTEXT:
[Beschreibung des Testobjekts – Anwendung, Feature, aktueller Status]
[Relevante technische Details (kein Prod-System, Staging-Environment)]
[Bekannte Einschränkungen oder Voraussetzungen]

INSTRUKTION:
[Klare, imperativ formulierte Aufgabe – was soll das LLM TUN?]
[Verwendete Technik, z.B.: Äquivalenzklassenanalyse / Risikoanalyse / Charter]

EINGABEDATEN:
- User Story / Akzeptanzkriterien: [Text oder "siehe Anhang"]
- Bekannte Fehler / Voraussetzungen: [Liste oder "keine"]
- Beispiel-Output (optional): [Falls Few-Shot gewünscht]

EINSCHRÄNKUNGEN:
- Keine echten Produktionsdaten, Passwörter oder personenbezogene Daten
- Maximale Anzahl Outputs: [z.B. 10 Testfälle]
- Scope: [z.B. nur funktionale Tests, kein Sicherheits-Pentest]
- [Weitere teamspezifische Regeln]

AUSGABEFORMAT:
[Markdown-Tabelle / nummerierte Liste / JSON / Freitext]
Pflichtfelder: [z.B. ID, Titel, Schritte, Erwartetes Ergebnis, Priorität]

AKZEPTANZKRITERIEN FÜR DEN OUTPUT:
- [ ] Jeder Punkt ist einem Anforderungselement zugeordnet
- [ ] Kein Output enthält echte Nutzerdaten
- [ ] Format entspricht der Vorgabe
- [ ] Human-Review vor Übernahme ins Testtool durchgeführt
```

---

**Hinweis:** Dieses Template basiert auf ISTQB CT-GenAI Syllabus 2.1.1.
Alle Prompts müssen vor der Nutzung von einem Teammitglied auf Datenschutz
geprüft werden. Output niemals direkt ohne Human-Review übernehmen.

*CT-GenAI / fAVENGERS – Version 1.0 – Juni 2026*
