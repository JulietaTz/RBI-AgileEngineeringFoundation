### 1. Titel
Verwendung von multimodaler KI zur Unterstützung von UI-Tests

---

### 2. Syllabus-Referenz
ISTQB GenAI – 1.1.4 Multimodale LLMs und Vision-Language-Modelle

---

### 3. Lernziel
Multimodales Prompting anwenden, um UI-bezogene Tests zu unterstützen und gleichzeitig das Risiko von Fehlinterpretationen zu kontrollieren.

---

### 4. Kontext / Szenario
Sie benötigen eine schnellere visuelle Überprüfung von Webshop-Seiten. Das Team möchte testen, ob Screenshot- und Text-Prompts die Erkennung von UI-Anomalien unterstützen können.

---

### 5. Aufgabenstellung
1. Wählen Sie zwei Toolshop-Seiten aus (z. B. Produktliste und Warenkorb).
2. Erstellen Sie pro Seite einen Screenshot.
3. Schreiben Sie einen multimodalen Prompt, der nach potenziellen UI-Problemen und Barrierefreiheitsaspekten fragt.
4. Validieren Sie die Ergebnisse der KI manuell in der UI.
5. Klassifizieren Sie jedes Ergebnis als gültiges Problem, False Positive oder unklar.
6. Schlagen Sie 3 Verbesserungen für den Prompt vor, um False Positives zu reduzieren.

---

## UI- und Accessibility-Analyse des ToolShops

**Prompt-Text:**  
Analysiere die Screenshots dieser Webshop-Seiten (Produktlist und Warenkorb) und identifiziere mögliche UI-Probleme sowie Accessibility-Probleme.

---

## UI-Probleme

| Seite | Problem | Beleg | Klassifikation |
|------|--------|------|---------------|
| Produktliste | Kein Add-to-Cart Button | Auf den Produktkarten ist kein „In den Warenkorb“-Button sichtbar. Dadurch kann der Nutzer den nächsten Schritt nicht direkt ausführen. | valid |
| Produktliste | Kleine Klickflächen | Die Klickflächen in der Produktliste (z. B. Filter oder Buttons) wirken ausreichend groß und gut bedienbar, sodass kein offensichtliches UI-Problem erkennbar ist. | false positive |
| Produktliste | Inkonsistente Bildsprache | Die Bilder auf den Produktkarten wirken unterschiedlich (z. B. in Hintergründen und Perspektiven). Es ist jedoch unklar, ob dies ein Problem darstellt, da die Unterschiede produktbedingt sein können. | unklar |
| Warenkorb | Gesamtsumme nicht hervorgehoben | Die Gesamtsumme ist nicht ausreichend hervorgehoben, obwohl sie eine zentrale Information darstellt. | valid |
| Warenkorb | Call-to-Actions falsche Priorisierung | Die Call-to-Actions sind visuell klar unterschieden: „Zur Kasse“ ist grün und dominant, „Weiter einkaufen“ grau und sekundär. Eine falsche Priorisierung ist nicht erkennbar. | false positive |
| Warenkorb | Fehlende +/- Buttons | Die Mengensteuerung erfolgt nur über ein Eingabefeld, ohne sichtbare +/- Buttons, jedoch ist unklar, ob dies ein Problem darstellt. | unklar |

---

## Accessibility-Probleme

| Seite | Problem | Beleg | Klassifikation |
|------|--------|------|---------------|
| Produktliste | „Nicht auf Lager“ nur farblich hervorgehoben | Der Status „Nicht auf Lager“ ist zwar als Text sichtbar, wird jedoch hauptsächlich durch die rote Farbe hervorgehoben, ohne zusätzliche visuelle Hinweise wie Icons. Dies kann für Nutzer mit Farbsehschwäche schwer erkennbar sein. | valid |
| Produktliste | Unklare semantische Struktur im Filterbereich | Die semantische Struktur kann anhand des Screenshots nicht beurteilt werden, da sie nur im HTML-Code sichtbar ist. | false positive |
| Produktliste | Bewertungssystem unklar | Die Bewertung ist nur per Hover erklärbar, was für Tastatur- oder Screenreader-Nutzer problematisch sein kann. | unklar |
| Warenkorb | Fehlende Screenreader-Beschriftung bei interaktiven Elementen | Beim Test mit einem Screenreader werden nur einzelne Elemente wie das Eingabefeld „Anzahl“ vorgelesen. Andere interaktive Elemente (z. B. Buttons oder das „X“-Symbol) werden nicht oder nicht eindeutig angekündigt. | valid |
| Warenkorb | Call-to-Actions nicht korrekt bewertet | „Zur Kasse“ ist bereits visuell dominant (grün, rechts) → keine falsche Priorisierung. | false positive |
| Warenkorb | Fokusanzeige nur schwach sichtbar | Bei der Tab-Navigation ist ein Fokuszustand vorhanden, jedoch nur schwach sichtbar. Es ist nicht eindeutig erkennbar, ob die visuelle Hervorhebung für Accessibility-Anforderungen ausreichend ist. | unklar |

---

### Accessibility Referenz

Die Analyse basiert auf grundlegenden WCAG 2.1 Prinzipien  
(z. B. Farbkontrast, Beschriftung und Bedienbarkeit):

https://www.ionos.at/digitalguide/websites/web-entwicklung/wcag-richtlinien-fuer-die-barrierefreiheit-im-web/

---

# 🔧 Verbesserungsvorschläge für den Prompt (Reduktion von False Positives)

1. Der Prompt sollte klar vorgeben, dass nur Aspekte bewertet werden, die direkt im Screenshot sichtbar sind, um spekulative Annahmen zu vermeiden.  

2. Die Analyse sollte sich auf visuell überprüfbare UI-Elemente beschränken und keine Aussagen über nicht sichtbare Eigenschaften wie Screenreader-Unterstützung oder Tastaturbedienbarkeit treffen.  

3. Der Prompt sollte verlangen, dass nur eindeutig erkennbare Probleme genannt werden und unsichere Beobachtungen explizit als solche gekennzeichnet werden.

### 6. Erwartetes Ergebnis / Deliverable
Ein Markdown-Bericht mit einer Tabelle der Ergebnisse und Notizen zur Verbesserung der Prompts.

---

### 7. Zeitrahmen
25 Minuten

---

### 8. Hinweise (Optional)
- Fordern Sie explizite Belege an („welches Element, wo auf dem Bildschirm, warum ein Problem“).
- Halten Sie die manuelle Validierung verpflichtend.