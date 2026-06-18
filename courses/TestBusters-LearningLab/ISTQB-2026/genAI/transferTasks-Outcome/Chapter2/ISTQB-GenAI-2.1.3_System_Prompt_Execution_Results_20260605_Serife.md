# 2.1.3 – System-Prompt und Benutzer-Prompt unterscheiden und anwenden

## Referenz zum ISTQB-Lehrplan

**ISTQB GenAI – 2.1.3 System-Prompt und Benutzer-Prompt**

## Link zur Transfer Task

*https://github.com/guelbin/RBI-AgileEngineeringFoundation/blob/feature/G112-system-user-prompts/courses/TestBusters-LearningLab/ISTQB-2026/genAI/transferTasks/ISTQB-GenAI-2.1.3_System_Prompts_and_User_Prompts_20260405.md*

## Outcome

### Learning Summary

Im Rahmen dieser Transfer Task wurde der Unterschied zwischen System-Prompts und Benutzer-Prompts untersucht. Beide Prompt-Arten erfüllen unterschiedliche Aufgaben bei der Interaktion mit einem Large Language Model (LLM).

Der System-Prompt definiert den dauerhaften Rahmen einer Konversation. Er legt die Rolle des Modells, Verhaltensregeln, Einschränkungen und Qualitätsanforderungen fest. Der Benutzer-Prompt beschreibt dagegen die konkrete Aufgabe oder Fragestellung, die während der jeweiligen Interaktion bearbeitet werden soll.

Es wurde erkannt, dass ein klar formulierter System-Prompt wesentlich zur Konsistenz und Qualität der erzeugten Ergebnisse beiträgt. Insbesondere im Testkontext können durch geeignete System-Prompts Qualitätsrichtlinien, Teststandards und fachliche Vorgaben dauerhaft verankert werden. Benutzer-Prompts liefern anschließend die konkreten Arbeitsaufträge innerhalb dieses Rahmens.

| Konzept | Kernaussage |
|----------|-------------|
| System-Prompt | Definiert Rolle, Verhalten und Rahmenbedingungen des LLM |
| Benutzer-Prompt | Enthält die konkrete Aufgabe oder Frage des Nutzers |
| Priorität | System-Prompts haben Vorrang vor Benutzer-Prompts |
| Konsistenz | System-Prompts sorgen für einheitliche und vorhersehbare Antworten |
| Flexibilität | Benutzer-Prompts können für jede Interaktion angepasst werden |
| Qualitätssteuerung | Guardrails im System-Prompt reduzieren Fehlinterpretationen und unerwünschte Ausgaben |
| Transparenz | Unsicherheiten und fehlende Informationen sollten explizit benannt werden |

### Fazit

Die Aufgabe hat verdeutlicht, dass System-Prompts und Benutzer-Prompts unterschiedliche, aber komplementäre Funktionen erfüllen. Während der System-Prompt bestimmt, **wie** das Modell arbeiten soll, definiert der Benutzer-Prompt, **was** das Modell bearbeiten soll.

Für den professionellen Einsatz von Generativer KI im Softwaretest sollten System-Prompts gezielt genutzt werden, um Rollen, Qualitätsanforderungen und Einschränkungen festzulegen. Dadurch werden konsistentere, nachvollziehbarere und qualitativ hochwertigere Ergebnisse erzeugt.

> **Merksatz:**
>
> System-Prompt = „Wie soll das Modell arbeiten?“
>
> Benutzer-Prompt = „Welche Aufgabe soll das Modell bearbeiten?“
>
> Ein guter System-Prompt schafft den Rahmen für qualitativ hochwertige Ergebnisse, während der Benutzer-Prompt die konkrete Arbeitsanweisung liefert.