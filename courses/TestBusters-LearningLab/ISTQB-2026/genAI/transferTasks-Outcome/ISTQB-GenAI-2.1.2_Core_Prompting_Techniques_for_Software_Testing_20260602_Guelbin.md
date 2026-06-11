# Vergleich von Core Prompting Techniques

## Reference to ISTQB Syllabus Chapter

ISTQB GenAI – 2.1.2 Core Prompting Techniques for Software Testing

## Link to the Transfer Task File

[Transfer Task](https://github.com/guelbin/RBI-AgileEngineeringFoundation/blob/main/courses/TestBusters-LearningLab/ISTQB-2026/genAI/transferTasks/ISTQB-GenAI-2.1.2_Core_Prompting_Techniques_for_Software_Testing_20260405.md)


## Outcome

**Szenario:** Social Login mit Google im ToolShop-System.

Ziel ist die Erstellung und Bewertung von Testfällen für die Anmeldung über einen externen Identitätsanbieter.

## Verwendete Prompting-Techniken

| Technik | Prompt |
|---|---|
| Zero-Shot | Erstelle Testfälle für den Social Login mit Google im ToolShop-System. |
| Few-Shot | Siehe Prompt unten |
| Stepwise Reasoning / schrittweise Anweisungen | Siehe Prompt unten |

### Zero-Shot Prompt

```text
Erstelle Testfälle für den Social Login mit Google im ToolShop-System.
```

### Few-Shot Prompt

```text
Beispiel 1:
TC-SL-001

Google Login erfolgreich

Voraussetzung:
Der Benutzer besitzt ein gültiges Google-Konto.

Beschreibung:
Der Benutzer wählt „Mit Google anmelden“, authentifiziert sich erfolgreich bei Google und wird im ToolShop-System angemeldet.

Erwartetes Ergebnis:
Der Benutzer wird erfolgreich eingeloggt und auf die Startseite weitergeleitet.

Beispiel 2:
TC-SL-002

Benutzer bricht Google Login ab

Beschreibung:
Der Benutzer startet den Google-Login-Prozess, bricht die Authentifizierung jedoch ab.

Erwartetes Ergebnis:
Eine verständliche Fehlermeldung wird angezeigt und der Benutzer bleibt auf der Login-Seite.

Erstelle weitere Testfälle für den Social Login mit Google im ToolShop-System.
Verwende dieselbe Struktur und Namenskonvention.
```

### Stepwise Reasoning Prompt

```text
Denke Schritt für Schritt.

1. Analysiere die Anforderungen.
2. Identifiziere mögliche Risiken.
3. Leite Testbedingungen ab.
4. Erstelle Testfälle.

Gib die Ergebnisse als Tabelle mit den Spalten
Testfall-ID, Testfallname und Beschreibung aus.

Thema: Social Login mit Google im ToolShop-System.
```



## Prompt Execution Results

Die vollständigen Ergebnisse aller Prompt-Ausführungen wurden in einer separaten Datei dokumentiert:

[Prompt Execution Results](https://github.com/guelbin/RBI-AgileEngineeringFoundation/blob/feature/g111-core-prompting-techniques/courses/TestBusters-LearningLab/ISTQB-2026/genAI/transferTasks-Outcome/ISTQB-GenAI-2.1.2_Prompting_Techniques%20_Results_20260602_Guelbin.md)


## Bewertung der Ergebnisse

| Kriterium | Zero-Shot | Few-Shot | Stepwise Reasoning |
|---|---|---|---|
| Korrektheit | Gut | Sehr gut | Sehr gut |
| Vollständigkeit | Mittel | Gut | Sehr gut |
| Ausführungsreife / Execution Readiness | Mittel | Gut | Sehr gut |

## Empfehlung

Zero-Shot eignet sich für schnelle erste Ideen und einfache Aufgaben.

Few-Shot eignet sich, wenn eine bestimmte Struktur oder Namenskonvention eingehalten werden soll.

Stepwise Reasoning lieferte die vollständigsten und am besten nutzbaren Ergebnisse. Für die Erstellung hochwertiger Testfälle im Softwaretest ist diese Technik am besten geeignet.

## Kurze Richtlinie

| Technik | Empfohlene Verwendung |
|---|---|
| Zero-Shot | Schnelle Ideenfindung und einfache Aufgaben |
| Few-Shot | Vorgegebene Struktur oder Format einhalten |
| Stepwise Reasoning | Komplexe Testanalyse und vollständige Testfallerstellung |
