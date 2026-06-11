# Erstelle einen wiederverwendbaren System-Prompt für die QA-Arbeit

## Reference to ISTQB Syllabus Chapter

ISTQB GenAI – 2.1.3 System Prompts and User Prompts

## Link to the Transfer Task File

https://github.com/rgroetz2/TBLL-AgileEngineeringFoundation/blob/main/courses/TestBusters-LearningLab/ISTQB-2026/genAI/transferTasks/ISTQB-GenAI-2.1.3_System_Prompts_and_User_Prompts_20260405.md

# Outcome

## Objective

Ziel dieser Transfer Task war es, die unterschiedlichen Aufgaben von System Prompts und User Prompts zu verstehen sowie zu bewerten, wie ein wiederverwendbarer System Prompt die Konsistenz der Ergebnisse über verschiedene QA-Aktivitäten hinweg verbessern kann.

## System Prompt v1

```text
Du bist ein Junior Softwaretester.
Antworte immer klar und in einer formellen Sprache.
Vermeide Annahmen und gib kurze und klare Antworten.
```

## User Prompt 1 – Test Analysis

```text
Erstelle eine Testanalyse für die Login-Funktion des ToolShop-Systems:
https://practicesoftwaretesting.com/

Analysiere die verfügbaren Anforderungen und leite daraus folgende Punkte ab:
- Testbedingungen
- Risiken
- Testobjekte
```

## User Prompt 2 – Test Design

```text
Erstelle Testfälle für die Login-Funktion des ToolShop-Systems:
https://practicesoftwaretesting.com/

Berücksichtige die verfügbaren Anforderungen und die erwartete Funktionalität.
```

## User Prompt 3 – Defect Triage

```text
Führe ein Defect Triage für die Login-Funktion des ToolShop-Systems durch.

Berücksichtige die verfügbaren Informationen und identifiziere mögliche Defekte sowie deren Priorität und Schweregrad.
```

## Evaluation of System Prompt v1

### Stärken

- Die KI hat die definierte Rolle als Junior Softwaretester konsequent eingehalten.
- Die Antworten waren klar, kurz und in einer formellen Sprache formuliert.
- Die Ergebnisse waren gut strukturiert und verständlich.
- Es wurden keine unbegründeten Annahmen getroffen.
- Die Ausgaben waren für die drei unterschiedlichen Aufgaben weitgehend konsistent.

## System Prompt v2

Die zweite Version ergänzt zusätzliche Regeln zur Ausgabestruktur sowie eine Orientierung am ISTQB® CTFL und – sofern relevant – am ISTQB® Generative AI for Software Testing Syllabus. Ziel ist eine noch höhere Konsistenz und Nachvollziehbarkeit der Antworten.


```text
Du bist ein Junior Softwaretester.
Antworte immer klar und in einer formellen Sprache. Orientiere dich an der Terminologie und den Prinzipien des ISTQB® Certified Tester Foundation Level (CTFL) sowie – sofern relevant – des ISTQB® Generative AI for Software Testing Syllabus.
Vermeide unbegründete Annahmen. Wenn Informationen fehlen oder unklar sind, stelle Rückfragen.
Verwende eine einheitliche und strukturierte Ausgabestruktur mit klaren Überschriften und Aufzählungen.
```

## Prompt Execution Results

Die detaillierten Ergebnisse der Prompt-Ausführung wurden aus Gründen der Übersichtlichkeit in einer separaten Markdown-Datei dokumentiert.

- [Prompt Execution Results](https://github.com/guelbin/RBI-AgileEngineeringFoundation/blob/feature/G112-system-user-prompts/courses/TestBusters-LearningLab/ISTQB-2026/genAI/transferTasks-Outcome/ISTQB-GenAI-2.1.3_System_Prompt_Execution_Results_20260605_Guelbin.md)

## Comparison: System Prompt v1 vs. System Prompt v2

Der erste System Prompt lieferte bereits klare, strukturierte und gut verständliche Ergebnisse. Die Antworten waren konsistent, formal formuliert und für die jeweiligen QA-Aufgaben gut geeignet.

Die überarbeitete Version führte zu einer noch einheitlicheren Struktur und einer besseren Kennzeichnung fehlender oder nicht spezifizierter Informationen. Zudem wurden offene Fragen explizit hervorgehoben und die Antworten stärker an der gewünschten ISTQB®-Terminologie orientiert.

Insgesamt waren die Unterschiede nicht grundlegend, aber die zweite Version verbesserte die Konsistenz, Transparenz und Wiederverwendbarkeit der generierten Ergebnisse. Die Aufgabe hat gezeigt, dass bereits kleine Anpassungen am System Prompt die Qualität und Struktur der Ausgaben positiv beeinflussen können.

## Learning Summary

- System Prompts definieren das allgemeine Verhalten und die Qualitätsanforderungen der KI, während User Prompts die konkrete Aufgabe beschreiben.
- Bereits kleine Anpassungen am System Prompt können die Konsistenz und Struktur der Ausgaben verbessern.
- Eine präzise Formulierung des System Prompts hilft dabei, fehlende Informationen transparent zu kennzeichnen und unbegründete Annahmen zu vermeiden.
- Nicht explizit definierte Aspekte werden vom LLM eigenständig interpretiert und können zu unterschiedlichen Ergebnissen führen.
- Die Orientierung an ISTQB®-Terminologie kann durch den System Prompt unterstützt werden, die fachliche Korrektheit der Ergebnisse muss jedoch weiterhin durch eine menschliche Überprüfung sichergestellt werden.