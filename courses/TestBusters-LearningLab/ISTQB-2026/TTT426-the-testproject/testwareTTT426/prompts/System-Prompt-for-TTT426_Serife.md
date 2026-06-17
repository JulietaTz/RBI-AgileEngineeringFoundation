# 2.1.3 – Design a System Prompt for GitHub Copilot with Testing Guardrails

## Referenz zum ISTQB-Lehrplan

**ISTQB GenAI – 2.1.3 System-Prompt und Benutzer-Prompt**

## Link zur Transfer Task

*https://github.com/rgroetz2/TBLL-AgileEngineeringFoundation/blob/main/courses/TestBusters-LearningLab/ISTQB-2026/genAI/transferTasks/Chapter2/2.1.3-System-Prompt-for-TTT426.md*

## Outcome

### Learning Summary

Im Rahmen dieser Transfer Task wurde ein strukturierter System-Prompt für GitHub Copilot entwickelt. Ziel war es, die Qualität, Konsistenz und Sicherheit von KI-generierten Testartefakten zu verbessern und gleichzeitig typische Probleme wie vermischte Teststufen, fehlende Testentwurfsgrundlagen oder unsichere Testdaten zu vermeiden.

Der entwickelte System-Prompt definiert GitHub Copilot als Test-Engineering-Assistenten und enthält verbindliche Guardrails basierend auf den TTT426-Konzepten Testtyp, Testtechnik, Teststufe, Testdaten, Testumgebung und Testautomatisierung.

Besonderer Wert wurde auf die Vermeidung unbegründeter Annahmen, die transparente Behandlung von Unsicherheiten sowie auf standardisierte Ausgabeformate gelegt. Darüber hinaus stellt der Prompt sicher, dass Testdesign vor Testautomatisierung erfolgt und dass sensible oder produktive Daten nicht verwendet werden.

| Konzept | Kernaussage |
|----------|-------------|
| Rollenbeschreibung | Copilot agiert als Test-Engineering-Assistent |
| Testtyp | Jeder Test muss einem klar definierten Testtyp zugeordnet werden |
| Teststufe | Unit-, Integrations-, System- und Abnahmetests werden eindeutig getrennt |
| Testtechnik | Testfälle basieren auf einer explizit genannten Testentwurfstechnik |
| Testdaten | Nur realistische und nicht sensible Testdaten dürfen verwendet werden |
| Testumgebung | Fehlende Umgebungsinformationen müssen kenntlich gemacht werden |
| Testautomatisierung | Testdesign muss vor der Generierung von Automatisierungscode erfolgen |
| Unsicherheiten | Fehlende Informationen werden transparent gemacht und Rückfragen angeregt |
| Strukturierte Ausgaben | Standardisierte Formate fördern Konsistenz und Nachvollziehbarkeit |

### Validierung des Prompts

Der entwickelte System-Prompt erfüllt die Anforderungen der Transfer Task durch folgende Eigenschaften:

- Verbesserung der Konsistenz durch verbindliche Ausgabeformate
- Reduzierung von Mehrdeutigkeiten durch explizite Teststufen und Testtypen
- Unterstützung von Junior-Testern durch klare Strukturen und Vorgaben
- Vermeidung von Halluzinationen durch Kennzeichnung fehlender Informationen
- Förderung sicherer Testpraktiken durch den Ausschluss sensibler Daten
- Nachvollziehbare Testautomatisierung durch die Vorgabe „Testdesign vor Automatisierung“

### Fazit

Die Erstellung eines strukturierten System-Prompts zeigt, wie das Verhalten eines KI-gestützten Entwicklungswerkzeugs gezielt gesteuert werden kann. Durch klar definierte Guardrails wird die Qualität der erzeugten Testartefakte verbessert und das Risiko fehlerhafter oder unsicherer Ergebnisse reduziert.

> **Merksatz:**
>
> Ein System-Prompt wirkt wie ein Vertrag zwischen Anwender und KI.
>
> Er definiert Rollen, Regeln, Qualitätsanforderungen und Grenzen, innerhalb derer die KI arbeiten soll.
>
> Je präziser diese Vorgaben formuliert sind, desto konsistenter und zuverlässiger sind die erzeugten Ergebnisse.