### 1. Title
Use Multimodal AI for UI Test Support
### 2. Syllabus Reference
ISTQB GenAI – 1.1.4 Multimodal LLMs and Vision-Language Models
### 3. Learning Objective
Apply multimodal prompting to support UI-focused testing while controlling for misinterpretation risks.
### 4. Context / Scenario
You need faster visual review of webshop screens. The team wants to test whether screenshot + text prompts can support UI anomaly detection.
### 5. Task Instructions
1. Select two Toolshop pages (e.g., product listing and cart).
2. Capture one screenshot per page.
3. Write one multimodal prompt asking for potential UI issues and accessibility concerns.
4. Validate AI findings manually in the UI.
5. Classify each finding as valid issue, false positive, or unclear.
6. Propose 3 prompt refinements to reduce false positives.
### 6. Expected Outcome / Deliverable
A markdown report with findings table and prompt refinement notes.

🧩 UI-Probleme

| Nr | Seite        | Problem                                           | Beleg                                                                                                                      | Klassifikation |
| -- | ------------ | ------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- | -------------- |
| 1  | Produktliste | Rechtschreibfehler in Navigation + fehlendes Logo | „Sorth“, „Serch“, „Contakt“ sind falsch geschrieben; Logo wird als defektes Bild-Icon angezeigt                            | ✅ valid        |
| 2  | Produktliste | Fehlender Add-to-Cart Button                      | Auf Produktkarten ist kein klarer „In den Warenkorb“-Button sichtbar, wodurch der Kaufprozess nicht direkt fortsetzbar ist | ❓ unklar       |
| 3  | Warenkorb    | Falsche Gesamtsumme                               | Gesamtsumme zeigt $00.00 statt korrekter Berechnung ($28.30), was zu falscher Kaufinformation führt                        | ✅ valid        |
| 4  | Warenkorb    | Inkonsistenter Warenkorb-Zähler                   | Warenkorb-Icon zeigt „2“, obwohl nur 1 Produkt im Warenkorb enthalten ist                                                  | ✅ valid        |


---
♿ Accessibility-Probleme

| Nr | Seite        | Problem                                                    | Beleg                                                                                                                           | Klassifikation   |
| -- | ------------ | ---------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ---------------- |
| 1  | Produktliste | „Nicht auf Lager“ nur farblich dargestellt                 | Status wird hauptsächlich durch rote Farbe vermittelt, ohne zusätzliche visuelle Hinweise (z. B. Icon)                          | ✅ valid          |
| 2  | Produktliste | Bewertungssystem nur eingeschränkt zugänglich              | Bewertung scheint nur über Hover erklärt zu sein, was für Tastatur-/Screenreader-Nutzer problematisch sein kann                 | ❓ unklar         |
| 3  | Warenkorb    | Interaktive Elemente ohne klare Accessibility-Beschriftung | „X“-Button und ähnliche Controls sind visuell vorhanden, aber semantische Beschriftung nicht eindeutig aus Screenshot ableitbar | ❌ false positive |
| 4  | Warenkorb    | Schwache Fokusanzeige                                      | Bei Tab-Navigation ist ein Fokuszustand sichtbar, jedoch visuell sehr schwach und schwer erkennbar                              | ❓ unklar         |


## 4. Wichtigster Eigenbefund

> 🔍 **Warenkorb-Zähler Bug:**
> Der Warenkorb enthält **1 Produkt** (Combination Pliers) in einer Menge von 2.
> Der Zähler im Header zeigt **„2"** – er zählt die Menge, nicht die Produktanzahl.
> Korrekt wäre **„1"**, da sich nur 1 verschiedenes Produkt im Warenkorb befindet.
> Dies ist ein **funktionaler Bug**, der manuell durch Testen entdeckt wurde.

---

## 6. Erwartetes Ergebnis / Deliverable

Ein strukturierter Markdown-Bericht, der folgende Elemente enthält:

- eine klar strukturierte Tabelle der identifizierten UI- und Accessibility-Probleme  
- eine Klassifikation jedes Befunds (valid issue, false positive, unklar)  
- eine kurze Begründung pro Klassifikation (Warum ist es ein Problem oder kein Problem?)  
- eine Trennung zwischen UI- und Accessibility-Analyse  
- eine konsistente Dokumentation der Belege (sichtbare UI-Elemente aus den Screenshots)  
- Notizen zur Verbesserung des Prompts zur Reduktion von False Positives und unklaren Ergebnissen  
### 7. Timebox
50 minutes
### 8. Hints (Optional)
- Ask for explicit evidence (“which element, where on screen, why issue”).
- Keep manual validation mandatory.
