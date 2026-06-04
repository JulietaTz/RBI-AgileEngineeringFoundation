# G111 – Prompt Results

## Thema

Social Login mit Google im ToolShop-System

---

## 1. Zero-Shot Prompting

### Prompt

```text
Erstelle Testfälle für den Social Login mit Google im ToolShop-System.
```

### Ergebnis

| Testfall-ID | Testfallname | Beschreibung |
|---|---|---|
| TC-SL-001 | Google Login erfolgreich | Prüfen, ob ein Benutzer sich mit einem gültigen Google-Konto anmelden kann. |
| TC-SL-002 | Google Konto nicht registriert | Prüfen, ob ein Benutzer mit einem Google-Konto korrekt weitergeleitet oder registriert wird, wenn noch kein Konto im ToolShop existiert. |
| TC-SL-003 | Google Login abgebrochen | Prüfen, ob eine verständliche Fehlermeldung angezeigt wird, wenn der Benutzer den Google-Login abbricht. |
| TC-SL-004 | Ungültige Google-Anmeldedaten | Prüfen, ob der Login fehlschlägt, wenn ungültige Google-Zugangsdaten verwendet werden. |
| TC-SL-005 | Google Zugriff verweigert | Prüfen, ob das System korrekt reagiert, wenn der Benutzer die Freigabe der Google-Daten ablehnt. |
| TC-SL-006 | Bereits bestehende E-Mail-Adresse | Prüfen, ob das System korrekt reagiert, wenn die Google-E-Mail-Adresse bereits für ein normales ToolShop-Konto verwendet wird. |
| TC-SL-007 | Login nach erfolgreicher Registrierung | Prüfen, ob der Benutzer nach erfolgreicher Google-Registrierung automatisch eingeloggt wird. |
| TC-SL-008 | Session nach Login | Prüfen, ob nach dem Google-Login eine gültige Benutzersession erstellt wird. |
| TC-SL-009 | Logout nach Google Login | Prüfen, ob der Benutzer sich nach einem Google-Login erfolgreich ausloggen kann. |
| TC-SL-010 | Netzwerkfehler während Google Login | Prüfen, ob bei Verbindungsproblemen eine verständliche Fehlermeldung angezeigt wird. |
| TC-SL-011 | Google Dienst nicht erreichbar | Prüfen, ob das System korrekt reagiert, wenn der Google-Login-Dienst nicht verfügbar ist. |
| TC-SL-012 | Zurück-Button im Browser | Prüfen, ob der Benutzer nach Nutzung des Zurück-Buttons nicht in einen fehlerhaften Zustand gelangt. |

---

## 2. Few-Shot Prompting

### Prompt

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

### Ergebnis

#### TC-SL-003 – Google Konto nicht registriert

**Voraussetzung:**  
Der Benutzer besitzt ein gültiges Google-Konto, das noch nicht mit einem ToolShop-Konto verknüpft ist.

**Beschreibung:**  
Der Benutzer wählt „Mit Google anmelden“ und authentifiziert sich erfolgreich bei Google.

**Erwartetes Ergebnis:**  
Das System erstellt ein neues Benutzerkonto oder zeigt eine entsprechende Information gemäß den Anforderungen an.

#### TC-SL-004 – Google Authentifizierung fehlgeschlagen

**Beschreibung:**  
Während des Google-Authentifizierungsprozesses tritt ein technischer Fehler auf.

**Erwartetes Ergebnis:**  
Eine verständliche Fehlermeldung wird angezeigt und der Benutzer bleibt auf der Login-Seite.

#### TC-SL-005 – Google Konto gesperrt

**Voraussetzung:**  
Das verwendete Google-Konto ist gesperrt oder deaktiviert.

**Beschreibung:**  
Der Benutzer versucht, sich mit dem gesperrten Google-Konto anzumelden.

**Erwartetes Ergebnis:**  
Der Login wird nicht durchgeführt und eine verständliche Fehlermeldung wird angezeigt.

#### TC-SL-006 – Keine Internetverbindung während Google Login

**Beschreibung:**  
Der Benutzer startet den Google-Login-Prozess, während keine Internetverbindung besteht.

**Erwartetes Ergebnis:**  
Der Login wird nicht durchgeführt und eine passende Fehlermeldung wird angezeigt.

#### TC-SL-007 – Benutzer verweigert Google-Berechtigungen

**Beschreibung:**  
Der Benutzer startet den Google-Login-Prozess, verweigert jedoch die angeforderten Berechtigungen.

**Erwartetes Ergebnis:**  
Der Login wird abgebrochen und eine verständliche Fehlermeldung wird angezeigt.

#### TC-SL-008 – Google Login nach Session-Ablauf

**Beschreibung:**  
Der Benutzer startet den Google-Login-Prozess, lässt die Sitzung ablaufen und versucht anschließend fortzufahren.

**Erwartetes Ergebnis:**  
Der Benutzer wird aufgefordert, den Anmeldeprozess erneut zu starten oder eine entsprechende Meldung wird angezeigt.

#### TC-SL-009 – Google Login mit bereits angemeldetem Benutzer

**Voraussetzung:**  
Der Benutzer ist bereits im ToolShop-System angemeldet.

**Beschreibung:**  
Der Benutzer klickt erneut auf „Mit Google anmelden“.

**Erwartetes Ergebnis:**  
Der Benutzer bleibt angemeldet und das System verhält sich fehlerfrei.

#### TC-SL-010 – Google Login mit mehreren Google-Konten

**Voraussetzung:**  
Im Browser sind mehrere Google-Konten angemeldet.

**Beschreibung:**  
Der Benutzer wählt beim Google-Login eines der verfügbaren Konten aus.

**Erwartetes Ergebnis:**  
Der Benutzer wird mit dem ausgewählten Google-Konto erfolgreich angemeldet.

#### TC-SL-011 – Google Login mit ungültiger Rückmeldung von Google

**Beschreibung:**  
Das ToolShop-System erhält eine ungültige oder unvollständige Antwort vom Google-Authentifizierungsdienst.

**Erwartetes Ergebnis:**  
Der Login wird nicht durchgeführt und eine verständliche Fehlermeldung wird angezeigt.

#### TC-SL-012 – Erfolgreicher Google Login nach vorherigem Abbruch

**Voraussetzung:**  
Der Benutzer hat einen vorherigen Google-Login-Versuch abgebrochen.

**Beschreibung:**  
Der Benutzer startet den Google-Login erneut und authentifiziert sich erfolgreich.

**Erwartetes Ergebnis:**  
Der Benutzer wird erfolgreich eingeloggt und auf die Startseite weitergeleitet.

---

## 3. Stepwise Reasoning / Schrittweise Anweisungen

### Prompt

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

### Ergebnis

| Testfall-ID | Testfallname | Beschreibung |
|---|---|---|
| TC-SL-001 | Google Login erfolgreich | Benutzer meldet sich mit einem gültigen Google-Konto an und wird erfolgreich im ToolShop-System eingeloggt. |
| TC-SL-002 | Google Login abgebrochen | Benutzer bricht den Google-Authentifizierungsprozess ab. Eine verständliche Fehlermeldung wird angezeigt. |
| TC-SL-003 | Google Login mit bereits registrierter E-Mail | Benutzer meldet sich mit einem Google-Konto an, dessen E-Mail-Adresse bereits im ToolShop-System existiert. Das Konto wird korrekt erkannt und der Benutzer wird eingeloggt. |
| TC-SL-004 | Google Login mit neuem Benutzer | Benutzer meldet sich erstmals mit einem gültigen Google-Konto an. Ein neues Benutzerkonto wird erstellt und der Benutzer wird eingeloggt. |
| TC-SL-005 | Google Login ohne E-Mail-Freigabe | Benutzer verweigert die Freigabe der E-Mail-Adresse. Die Anmeldung wird abgelehnt und eine passende Fehlermeldung wird angezeigt. |
| TC-SL-006 | Google Login bei Verbindungsproblem | Während der Google-Authentifizierung tritt ein Netzwerkfehler auf. Der Benutzer bleibt ausgeloggt und erhält eine verständliche Fehlermeldung. |
| TC-SL-007 | Google Login mit ungültigem Token | Das System erhält ein ungültiges oder abgelaufenes Google-Token. Der Login wird abgelehnt. |
| TC-SL-008 | Google Login mehrfach hintereinander | Benutzer klickt mehrmals schnell auf den Google-Login-Button. Es wird nur ein Login-Prozess gestartet und kein doppeltes Konto erstellt. |
| TC-SL-009 | Google Login nach Logout | Benutzer loggt sich mit Google ein, meldet sich wieder ab und loggt sich erneut mit demselben Google-Konto ein. Der Login funktioniert korrekt. |
| TC-SL-010 | Google Login mit deaktiviertem Google-Konto | Benutzer versucht sich mit einem deaktivierten oder gesperrten Google-Konto anzumelden. Der Login wird nicht durchgeführt. |
| TC-SL-011 | Google Login Weiterleitung korrekt | Nach erfolgreichem Google Login wird der Benutzer auf die richtige Zielseite im ToolShop-System weitergeleitet. |
| TC-SL-012 | Google Login Session wird erstellt | Nach erfolgreichem Login wird eine gültige Benutzersession erstellt, sodass der Benutzer geschützte Bereiche nutzen kann. |
| TC-SL-013 | Google Login Fehlerseite vermeiden | Bei einem Fehler im Google Login wird keine technische Fehlerseite angezeigt, sondern eine benutzerfreundliche Meldung. |
| TC-SL-014 | Google Login Datenschutzprüfung | Das System speichert nur die notwendigen Benutzerdaten aus dem Google-Konto. |
| TC-SL-015 | Google Login Button sichtbar | Der Google-Login-Button ist auf der Login- oder Registrierungsseite sichtbar und eindeutig beschriftet. |

