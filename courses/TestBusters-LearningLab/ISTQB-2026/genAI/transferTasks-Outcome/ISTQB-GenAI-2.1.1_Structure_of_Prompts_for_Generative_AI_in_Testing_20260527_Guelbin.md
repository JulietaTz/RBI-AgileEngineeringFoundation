# Title: 2.1.1 - Transform Weak Prompts into High-Quality Test Prompts

## Reference to ISTQB Syllabus Chapter

**ISTQB GenAI – 2.1.1 Structure of Prompts for Generative AI in Testing**

## Link to the Transfer Task File

`courses/TestBusters-LearningLab/ISTQB-2026/genAI/transferTasks/ISTQB-GenAI-2.1.1_Structure_of_Prompts_for_Generative_AI_in_Testing_20260405.md`

---

# Outcome

## Prompt 1 – Exploratory Testing

* ### Original Prompt

```text
Du testest den Registrierungs-Workflow eines Webshops, der nun Social-Media-Login unterstützt (Google, GitHub, LinkedIn).

Erkunde die Funktion frei. Probiere unterschiedliche Einstiegspunkte aus, wechsle zwischen Registrierungsmethoden, unterbreche Abläufe und kombiniere Aktionen auf ungewöhnliche Weise.

Konzentriere dich darauf, unerwartetes Verhalten, Inkonsistenzen oder Usability-Probleme zu entdecken. Dokumentiere deine Beobachtungen und alles, was dich überrascht.
```

* ### Improved Prompt

#### Rolle

Du bist ein Senior Software Tester mit Erfahrung im Testen von E-Commerce-Anwendungen.

#### Kontext

Der Webshop ToolShop unterstützt seit Kurzem die Registrierung über Social-Media-Logins (Google, GitHub und LinkedIn). Die neue Funktion soll vor dem Release auf mögliche Fehler, Inkonsistenzen und Usability-Probleme untersucht werden.

#### Instruktion

Führe ein exploratives Testing des Registrierungs-Workflows durch. Teste verschiedene Einstiegspunkte, wechsle zwischen den Registrierungsarten, unterbreche Abläufe bewusst und kombiniere Aktionen auf ungewöhnliche Weise. Identifiziere unerwartetes Verhalten, Inkonsistenzen und potenzielle Usability-Probleme.

#### Eingabedaten

**User Story**

Als Kunde möchte ich mich über Google, GitHub oder LinkedIn registrieren können, damit ich schnell und einfach ein Konto erstellen kann.

**Akzeptanzkriterien**

* Die Registrierung über Google funktioniert erfolgreich.
* Die Registrierung über GitHub funktioniert erfolgreich.
* Die Registrierung über LinkedIn funktioniert erfolgreich.
* Nach erfolgreicher Registrierung wird der Benutzer angemeldet.
* Bei Fehlern oder abgebrochenen Anmeldungen wird eine verständliche Fehlermeldung angezeigt.

#### Einschränkungen

* Konzentriere dich auf den Registrierungsprozess.
* Berücksichtige sowohl positive als auch negative Szenarien.
* Dokumentiere nur beobachtbare Ergebnisse.
* Triff keine Annahmen über die interne Implementierung.

#### Ausgabeformat

Dokumentiere die Ergebnisse in einer Tabelle mit den folgenden Spalten:

* Testidee
* Testschritte
* Erwartetes Ergebnis
* Beobachtung
* Priorität

---

## Prompt 2 – Test Design

* ### Original Prompt

```text
Du bist verantwortlich für die Erstellung von Testfällen für die erweiterte Registrierungsfunktion mit Social-Media-Login.

Überlege ohne vorgegebene Struktur, was getestet werden sollte. Berücksichtige normale Nutzung, Variationen, Randfälle und Fehlersituationen.

Schreibe die Testszenarien auf, die deiner Meinung nach am wichtigsten sind, um die Qualität sicherzustellen.
```

* ### Improved Prompt

#### Rolle

Senior QA Tester

#### Kontext

ToolShop Social Login

#### Eingabedaten

User Story + AC

#### Instruktion

Erstelle Testfälle für normale Nutzung, Alternativszenarien, Randfälle und Fehlersituationen.

#### Einschränkungen

* Fokus auf die Registrierungsfunktion über Google, GitHub und LinkedIn
* Berücksichtige nur die Web-Anwendung

#### Ausgabeformat

Tabelle mit:

* Testfall-ID
* Test Cases mit T1-T5 Test Ansatz
* Priorität

Erstelle Testfälle für:

* T1 (Happy Path)
* T2 (Alternative Fälle)
* T3 (Exception Cases)
* T4 (Negative Fälle)
* T5 (Misuse Cases)

Verwende folgende Syntax:

`<Tx>_<Feature>_<BusinessRule>_<Expected_Result>`

---

## Prompt 3 – Risk-Based Testing

* ### Original Prompt

```text
Angenommen, die Social-Media-Registrierungsfunktion geht morgen live.

Du hast nicht genügend Zeit, alles zu testen.

Identifiziere basierend auf deinem Verständnis die größten Risiken dieser Funktion. Konzentriere deine Tests auf Bereiche, die den größten Einfluss auf Benutzer oder das Unternehmen haben könnten.

Erkläre, was du zuerst testen würdest und warum.
```

* ### Improved Prompt

#### Rolle

Du bist ein Senior QA Tester mit Erfahrung im risikobasierten Testen.

#### Kontext

Der Webshop ToolShop unterstützt die Registrierung über Social-Media-Logins (Google, GitHub und LinkedIn). Die Funktion wird morgen produktiv gesetzt. Aufgrund begrenzter Zeit können nicht alle Bereiche getestet werden.

#### Eingabedaten

* User Story zur Registrierung mit Social-Media-Login
* Akzeptanzkriterien
* Geplanter Go-Live am nächsten Tag

#### Instruktion

Identifiziere die größten Risiken der Social-Media-Registrierungsfunktion. Bewerte die Risiken nach ihrer möglichen Auswirkung auf Benutzer und Unternehmen und priorisiere sie. Erläutere, welche Bereiche zuerst getestet werden sollten und warum.

#### Einschränkungen

* Konzentriere dich auf Risiken mit hoher geschäftlicher oder benutzerbezogener Auswirkung
* Berücksichtige nur die Web-Anwendung
* Fokus auf die Registrierung über Google, GitHub und LinkedIn

#### Ausgabeformat

| Nr. | Risiko | Warum ist das Risiko kritisch? | Priorität | Empfohlene Testaktivität |
| --- | ------ | ------------------------------ | --------- | ------------------------ |

---

## Prompt Execution Results

Die vollständigen Ergebnisse der Prompt-Ausführungen sind in folgendem Dokument verfügbar:

* [Prompt-Ausführungen](https://github.com/guelbin/RBI-AgileEngineeringFoundation/blob/feature/g110-prompt-engineering/courses/TestBusters-LearningLab/ISTQB-2026/genAI/transferTasks-Outcome/ISTQB-GenAI-2.1.1_Prompt_Execution_Results_GuelbinDeniz.md)

---

# Vergleich der Ergebnisse

| Kriterium       | Original Prompt                                                                      | Improved Prompt                                                                      |
| --------------- | ------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------ |
| Klarheit        | Allgemeine Beobachtungen und Antworten                                               | Präzisere und strukturierte Antworten                                                |
| Vollständigkeit | Wichtige Szenarien werden erkannt, aber nicht vollständig und systematisch abgedeckt | Systematische Abdeckung von Happy Path, Alternativ-, Ausnahme- und Fehlerszenarien   |
| Testbarkeit     | Schwer reproduzierbar, da Testschritte fehlen                                        | Nachvollziehbar und reproduzierbar durch klare Testschritte und erwartete Ergebnisse |   

---


# Reusable Team Prompt Template

## Rolle

[Definiere die Rolle, z. B. Software Tester, Test Manager oder Test Automation Engineer.]

## Kontext

[Beschreibe das Testobjekt, die Funktionalität und relevante Hintergrundinformationen.]

## Instruktion

[Beschreibe die auszuführende Aufgabe klar und präzise.]

## Eingabedaten

### User Story

[Beschreibe die User Story.]

### Akzeptanzkriterien

[Liste die relevanten Akzeptanzkriterien auf.]

### Weitere relevante Informationen

[Zusätzliche Informationen, Anforderungen oder Randbedingungen.]

## Einschränkungen

[Definiere relevante Grenzen oder Rahmenbedingungen.]

## Ausgabeformat

[Beschreibe die gewünschte Struktur der Antwort, z. B. Tabelle, Liste oder Testfallformat.]

---

# Learning Summary

Durch diese Aufgabe habe ich verstanden, wie wichtig eine strukturierte Prompt-Gestaltung für die Qualität von KI-generierten Ergebnissen ist.

Die Verwendung von Rolle, Kontext, Eingabedaten, Einschränkungen und einem klar definierten Ausgabeformat führte zu deutlich präziseren und besser nutzbaren Ergebnissen.

Der Vergleich mit den ursprünglichen Prompts zeigte, dass unstrukturierte Prompts oft unvollständige oder uneinheitliche Antworten liefern.

Strukturierte Prompts helfen dabei, reproduzierbare und nachvollziehbare Ergebnisse zu erzeugen und fördern eine einheitliche Nutzung von GenAI innerhalb eines Testteams.

Durch die Bereitstellung von User Story und Akzeptanzkriterien als Eingabedaten erhielt das LLM deutlich präzisere und besser nachvollziehbare Ergebnisse. Dies zeigt, wie wichtig vollständige und strukturierte Eingabedaten für die Qualität der generierten Antworten sind.