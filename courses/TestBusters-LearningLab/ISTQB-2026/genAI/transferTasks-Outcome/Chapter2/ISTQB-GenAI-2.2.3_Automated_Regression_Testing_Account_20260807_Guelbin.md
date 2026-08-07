
# Create a set of regression tests with GenAI for the profile/account feature.


---

## 2. Syllabus Reference
ISTQB GenAI – 2.2.3 Automated Regression Testing with Generative AI

---

## Link to the Transfer Task File
https://github.com/rgroetz2/TBLL-AgileEngineeringFoundation/blob/main/courses/TestBusters-LearningLab/ISTQB-2026/genAI/transferTasks/Chapter2/ISTQB-GenAI-2.2.3_Automated_Regression_Testing_Account.md


---
# Outcome


## 1. Change Notes and Feature Context

### Prompt 1

Für V6 wurden Änderungen an der Profile/Account-Funktion durchgeführt.

Folgende Änderungen wurden gemacht:

- Für die Profilfelder ist jetzt eine Feldvalidierung verfügbar.
- Die Passwortspeicherung wurde von MD5 auf SHA-256 geändert.

Auf der Profilseite gibt es folgende Felder:

- Vorname
- Nachname
- E-Mail-Adresse (nicht änderbar)
- Telefonnummer
- Straße
- Postleitzahl
- Stadt
- Bundesland
- Land

Die Profildaten können über „Profil ändern“ geändert werden.

Außerdem kann der Benutzer sein Passwort über „Passwort ändern“ ändern. Es gibt auch Passwortanforderungen und eine Anzeige der Passwortstärke.

Analysiere bitte:

1. Welche Funktionen oder Komponenten könnten durch diese Änderungen betroffen sein?
2. Welche möglichen Seiteneffekte oder Regressionsrisiken könnten entstehen?

Bitte triff keine Annahmen über Anforderungen, die nicht angegeben sind.

Erstelle noch keine Testfälle.

Zusätzlich habe ich einen Screenshot der aktuellen Profilseite beigefügt. Nutze ihn als zusätzlichen Kontext für die Analyse.

---

## 2. Impact- und Regressionsanalyse

### 2.1 Möglicherweise betroffene Funktionen oder Komponenten

| Bereich | Mögliche Auswirkung |
|---|---|
| Profilfelder | Vorname, Nachname, Telefonnummer, Straße, Postleitzahl, Stadt, Bundesland und Land sind von der neuen Feldvalidierung betroffen. |
| E-Mail-Adresse | Die E-Mail-Adresse ist nicht änderbar. Es sollte berücksichtigt werden, ob sie durch die Änderungen an der Profilfunktion unbeabsichtigt beeinflusst wird. |
| Profil ändern | Das Speichern bzw. Aktualisieren der Profildaten kann durch die neue Validierungslogik beeinflusst werden. |
| Anzeige der Profildaten | Nach einer Änderung müssen die gespeicherten Profildaten weiterhin korrekt auf der Profilseite angezeigt werden. |
| Passwort ändern | Die Änderung der Passwortspeicherung von MD5 auf SHA-256 kann den Prozess zum Ändern und Speichern des Passworts betreffen. |
| Passwortanforderungen | Die vorhandenen Regeln für das neue Passwort könnten im Zusammenhang mit der Passwortänderung betroffen sein. |
| Passwortstärke-Anzeige | Die Anzeige der Passwortstärke sollte weiterhin korrekt funktionieren. |
| Authentifizierung / Login | Da das gespeicherte Passwort für die Authentifizierung verwendet wird, kann die Änderung des Hash-Verfahrens Auswirkungen auf den Login haben. |
| Bestehende Benutzerkonten | Bei bereits vorhandenen Passwörtern besteht ein möglicher Integrations- bzw. Kompatibilitätsbereich. |

### 2.2 Mögliche Seiteneffekte und Regressionsrisiken

Bei der Feldvalidierung besteht insbesondere das Risiko, dass bisher akzeptierte Profildaten nach der Änderung anders behandelt werden. Außerdem könnte die Validierung unbeabsichtigt auch andere Felder beeinflussen.

Weitere mögliche Risiken sind:

- Gültige Änderungen werden nicht gespeichert.
- Ungültige Eingaben werden trotz Validierung gespeichert.
- Nur ein Teil der geänderten Profildaten wird übernommen.
- Die nicht änderbare E-Mail-Adresse wird unbeabsichtigt editierbar oder verändert.

Bei der Umstellung von MD5 auf SHA-256 liegen die wichtigsten Regressionsrisiken bei der Passwortänderung und Authentifizierung.

Ein neu gesetztes Passwort könnte beispielsweise nicht korrekt gespeichert werden oder ein Benutzer könnte sich nach einer erfolgreichen Passwortänderung nicht mehr mit dem neuen Passwort anmelden. Auch die weitere Nutzung bestehender Benutzerkonten könnte betroffen sein.

Da keine konkreten Validierungsregeln angegeben wurden, werden keine Annahmen über erlaubte Zeichen, Feldlängen oder Formate getroffen.

---

## 3. Erstellung möglicher Regressionstests mit GenAI

### Prompt 2

Nutze die zuvor erstellte Impact- und Regressionsanalyse als Grundlage.

Erstelle mögliche Regressionstests für die Profile/Account-Funktion in den Kategorien T1–T5.

- **T1:** Happy Path
- **T2:** Alternative Path
- **T3:** Exception Path
- **T4:** Negative Test
- **T5:** Misuse Test

Berücksichtige dabei besonders die in der Analyse identifizierten Regression Areas und die Änderungen in V6.

Identifiziere außerdem, welche Tests als Smoke-Tests und welche aufgrund der V6-Änderungen als High-Risk-Tests relevant sind.

Triff keine Annahmen über Anforderungen oder konkrete Validierungsregeln, die nicht angegeben wurden.

### 3.1 Von GenAI vorgeschlagene Regressionstests

| ID | Kat. | Möglicher Regressionstest | Erwartung auf hoher Ebene | Smoke | High Risk |
|---|---|---|---|:---:|:---:|
| RT-01 | T1 | Gültige Profildaten ändern und über „Profil ändern“ speichern | Profildaten werden erfolgreich gespeichert und korrekt angezeigt. | ✓ | ✓ |
| RT-02 | T1 | Passwort mit Eingaben ändern, die die angezeigten Passwortanforderungen erfüllen | Passwortänderung ist erfolgreich. | ✓ | ✓ |
| RT-03 | T1 | Nach erfolgreicher Passwortänderung mit dem neuen Passwort anmelden | Anmeldung mit dem neuen Passwort funktioniert. | ✓ | ✓ |
| RT-04 | T1 | Bestehender Benutzer meldet sich mit seinem bestehenden Passwort an | Bestehende Authentifizierung funktioniert weiterhin. | ✓ | ✓ |
| RT-05 | T2 | Mehrere Profilfelder gleichzeitig ändern | Alle vorgenommenen Änderungen werden korrekt verarbeitet und gespeichert. | – | ✓ |
| RT-06 | T2 | Nur ein änderbares Profilfeld ändern | Die einzelne Änderung wird korrekt gespeichert; andere Profildaten bleiben erhalten. | – | ✓ |
| RT-07 | T2 | Neues Passwort eingeben und Passwortstärke-Anzeige beobachten | Passwortstärke-Anzeige reagiert weiterhin auf die Eingabe. | – | – |
| RT-08 | T3 | Profiländerung mit einer Eingabe durchführen, die von der neuen Feldvalidierung abgelehnt wird | Die Validierung verhindert eine nicht akzeptierte Eingabe bzw. deren Speicherung. | – | ✓ |
| RT-09 | T3 | Passwort ändern, wobei neues Passwort und Passwortbestätigung nicht übereinstimmen | Passwortänderung wird nicht erfolgreich durchgeführt. | – | ✓ |
| RT-10 | T3 | Passwort ändern, ohne die angezeigten Passwortanforderungen zu erfüllen | Passwortänderung wird nicht erfolgreich durchgeführt. | – | ✓ |
| RT-11 | T4 | Profiländerung mit ungültiger Eingabe versuchen | Ungültige Eingabe wird entsprechend der implementierten Validierung behandelt. | – | ✓ |
| RT-12 | T4 | Passwortänderung mit falschem aktuellen Passwort versuchen | Passwort wird nicht erfolgreich geändert. | – | ✓ |
| RT-13 | T4 | Nach Passwortänderung Anmeldung mit dem bisherigen Passwort versuchen | Das bisherige Passwort ermöglicht keine Anmeldung mehr. | – | ✓ |
| RT-14 | T5 | Versuchen, die nicht änderbare E-Mail-Adresse zu verändern | E-Mail-Adresse kann über die Profilfunktion nicht geändert werden. | – | – |
| RT-15 | T5 | Wiederholt ungültige Eingaben in validierte Profilfelder eingeben und speichern | Die Feldvalidierung wird nicht umgangen. | – | ✓ |
| RT-16 | T5 | Wiederholt versuchen, ein Passwort zu setzen, das die angezeigten Anforderungen nicht erfüllt | Passwortanforderungen lassen sich über die normale Benutzeroberfläche nicht umgehen. | – | ✓ |

### Smoke-Tests

Für einen kurzen Smoke-Test nach V6 wurden RT-01 bis RT-04 vorgeschlagen.

Damit werden die zentralen Funktionen geprüft:

- Profil ändern
- Passwort ändern
- Login mit dem neuen Passwort
- Login eines bestehenden Benutzers

### High-Risk-Tests

Die High-Risk-Tests konzentrieren sich insbesondere auf die beiden V6-Änderungen:

**Neue Feldvalidierung:**
RT-01, RT-05, RT-06, RT-08, RT-11 und RT-15.

**MD5 → SHA-256:**
RT-02, RT-03, RT-04, RT-09, RT-10, RT-12, RT-13 und RT-16.

---

## 4. Selected Regression Subset

Nach der Überprüfung der von GenAI vorgeschlagenen Tests wurde folgendes Regressionstest-Subset ausgewählt.

Berücksichtigt wurden insbesondere die zentralen Funktionen sowie die durch die V6-Änderungen betroffenen High-Risk-Bereiche.

| ID | Kat. | Ausgewählter Regressionstest | Smoke | High Risk | Begründung für die Auswahl |
|---|---|---|:---:|:---:|---|
| RT-01 | T1 | Gültige Profildaten ändern und über „Profil ändern“ speichern | ✓ | ✓ | Zentrale Profilfunktion und direkt von der neuen Feldvalidierung betroffen. |
| RT-02 | T1 | Passwort mit Eingaben ändern, die die angezeigten Passwortanforderungen erfüllen | ✓ | ✓ | Die Passwortspeicherung wurde von MD5 auf SHA-256 geändert. Die Passwortänderung muss weiterhin funktionieren. |
| RT-03 | T1 | Nach erfolgreicher Passwortänderung mit dem neuen Passwort anmelden | ✓ | ✓ | Prüft, ob das neu gespeicherte Passwort nach der Umstellung auf SHA-256 für den Login funktioniert. |
| RT-04 | T1 | Bestehender Benutzer meldet sich mit seinem bestehenden Passwort an | ✓ | ✓ | Prüft, ob bestehende Benutzerkonten nach der Änderung der Passwortspeicherung weiterhin funktionieren. |
| RT-05 | T2 | Mehrere Profilfelder gleichzeitig ändern | – | ✓ | Prüft, ob mehrere Änderungen trotz der neuen Feldvalidierung vollständig gespeichert werden. |
| RT-06 | T2 | Nur ein änderbares Profilfeld ändern | – | ✓ | Prüft, ob eine einzelne Profiländerung korrekt gespeichert wird und andere Profildaten unverändert bleiben. |
| RT-09 | T3 | Passwort ändern, wobei neues Passwort und Passwortbestätigung nicht übereinstimmen | – | ✓ | Prüft, ob eine fehlerhafte Passwortänderung weiterhin korrekt verhindert wird. |
| RT-10 | T3 | Passwort ändern, ohne die angezeigten Passwortanforderungen zu erfüllen | – | ✓ | Prüft, ob die vorhandenen Passwortanforderungen weiterhin berücksichtigt werden. |
| RT-11 | T4 | Profiländerung mit ungültiger Eingabe versuchen | – | ✓ | Prüft direkt die in V6 neu eingeführte Feldvalidierung bei ungültigen Eingaben. |
| RT-12 | T4 | Passwortänderung mit falschem aktuellen Passwort versuchen | – | ✓ | Prüft, ob eine Passwortänderung mit einem falschen aktuellen Passwort weiterhin verhindert wird. |
| RT-13 | T4 | Nach Passwortänderung Anmeldung mit dem bisherigen Passwort versuchen | – | ✓ | Ergänzt RT-03 und prüft, ob das alte Passwort nach einer erfolgreichen Passwortänderung nicht mehr verwendet werden kann. |
| RT-14 | T5 | Versuchen, die nicht änderbare E-Mail-Adresse zu verändern | – | – | Prüft, ob die E-Mail-Adresse trotz Änderungen an der Profilfunktion weiterhin nicht geändert werden kann. |
| RT-15 | T5 | Wiederholt ungültige Eingaben in validierte Profilfelder eingeben und speichern | – | ✓ | Prüft, ob die neue Feldvalidierung durch wiederholte ungültige Eingaben nicht umgangen werden kann. |
| RT-16 | T5 | Wiederholt versuchen, ein Passwort zu setzen, das die angezeigten Anforderungen nicht erfüllt | – | ✓ | Prüft, ob die vorhandenen Passwortanforderungen auch bei wiederholten Versuchen nicht umgangen werden können. |

---

## 5. Bewusst ausgeschlossene Tests

Nach der Überprüfung der GenAI-Vorschläge wurden folgende Tests bewusst nicht in das Regressionstest-Subset aufgenommen.

| ID | Kat. | Ausgeschlossener Test | Begründung für den Ausschluss |
|---|---|---|---|
| RT-07 | T2 | Neues Passwort eingeben und Passwortstärke-Anzeige beobachten | Die Passwortstärke-Anzeige ist im Vergleich zu den ausgewählten Tests weniger kritisch und nicht direkt von der Änderung der Passwortspeicherung von MD5 auf SHA-256 betroffen. |
| RT-08 | T3 | Profiländerung mit einer Eingabe durchführen, die von der neuen Feldvalidierung abgelehnt wird | Der Test überschneidet sich stark mit RT-11. Die Behandlung ungültiger Eingaben durch die neue Feldvalidierung wird bereits durch RT-11 abgedeckt. |

---

## 6. T1 Automation

Für die Automatisierung der T1-Regressionstests wird Playwright verwendet.

Die ausgewählten T1-Tests sind:

- RT-01 – Gültige Profildaten ändern und speichern
- RT-02 – Passwort erfolgreich ändern
- RT-03 – Login mit neuem Passwort nach Passwortänderung
- RT-04 – Bestehender Benutzer kann sich weiterhin anmelden

### Automatisierter Test – RT-04

Als erster T1-Test wurde RT-04 für die Automatisierung vorbereitet.

```ts
import { test, expect } from '@playwright/test';

test('RT-04 - Bestehender Benutzer kann sich anmelden', async ({ page }) => {

  // Login-Seite öffnen
  await page.goto('HIER_DIE_LOGIN_URL_EINTRAGEN');

  // E-Mail eingeben
  await page.getByLabel(/E-Mail|Email/i)
    .fill('HIER_TEST_EMAIL_EINTRAGEN');

  // Passwort eingeben
  await page.getByLabel(/Passwort|Password/i)
    .fill('HIER_TEST_PASSWORT_EINTRAGEN');

  // Login
  await page.getByRole('button', { name: /Anmelden|Login/i })
    .click();

  // Erfolgreichen Login prüfen
  await expect(page).not.toHaveURL(/login/i);
});

```

---

### Hinweis

Die konkrete Login-URL und die Testdaten werden vor der Testausführung durch die tatsächliche URL der Testumgebung und die Daten eines bestehenden Testbenutzers ersetzt.

Es werden keine nicht angegebenen Anforderungen oder Testdaten angenommen.
