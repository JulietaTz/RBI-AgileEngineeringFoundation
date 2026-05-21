# TRATAS – GenAI Testfall-Generator: Warenkorb & Checkout

## Ziel
Verwenden Sie generative KI, um wertvolle und wiederverwendbare Testfälle für die Warenkorb- und Checkout-Funktionalität des ToolShop-Systems zu erstellen und die Ergebnisse kritisch zu bewerten.

---

## Kontext (System Under Test)
- ToolShop E-Commerce Plattform: https://practicesoftwaretesting.com/
- Fokusbereiche:
  - Warenkorb (Hinzufügen, Aktualisieren, Entfernen von Artikeln)
  - Checkout (Benutzerfluss, Zahlung, Validierung)

---

## Aufgabe (≤ 30 Minuten)

### Teil 1 – Verständnis der Testbasis (5 Min)
1. Öffnen Sie die ToolShop-Anwendung
2. Führen Sie die folgenden Aktionen aus:
   - Fügen Sie ein Produkt zum Warenkorb hinzu
   - Navigieren Sie zum Checkout

**Dokumentieren Sie:**
- 2–3 zentrale Schritte des Benutzerflusses

1. Ein Produkt wird zum Warenkorb hinzugefügt und der Warenkorb geöffnet.
2. Über die Schaltfläche „Zur Kasse“ erfolgt die Navigation zum Checkout bzw. zur Login-Seite.
3. Der Checkout-Prozess wird mit der Zahlungsmethode „Kaufe jetzt, zahle später“ mit 3 monatlichen Raten erfolgreich abgeschlossen.

---

### Teil 2 – Erstellen eines Prompts (5 Min)

Verwenden Sie den folgenden strukturierten Prompt:

<Definieren Sie den Prompt – der Testfall-Titel sollte die Syntax T1T5 haben>

* ### ERSTE PROMPT-VERSION: 
Du bist Software-Tester.

Die Testfälle sollen auf der ToolShop-Anwendung basieren:
https://practicesoftwaretesting.com/

Erstelle realistische und detaillierte Testfälle für den Warenkorb- und Checkout-Flow.

Verwende für die Testfall-Titel die T1T5-Syntax:

- T1 = Standard Case (Happy Path)
- T2 = Alternative Case
- T3 = Exception Case
- T4 = Negative Case
- T5 = Misuse Case

Verwende folgende Namenssyntax:

#### `<Tx>_<Feature>_<BusinessRule>_<ExpectedResult>`

Beispiel:
T1_Checkout_PaymentWithCreditCard_OrderCompletedSuccessfully

Für jeden Testfall sollen folgende Informationen enthalten sein:
- Vorbedingungen
- Testschritte
- Erwartetes Ergebnis

Die Testfälle sollen realistisch für die ToolShop-Anwendung sein und typische Benutzer- sowie Fehlerszenarien abdecken.

#### ERSTE ERGEBNISSE:
Die erste Prompt-Version erzeugte bereits realistische Testfälle für den Warenkorb- und Checkout-Prozess. 
* ### Beispiel:
Warenkorb & Checkout Testfälle
T1_Cart_AddAvailableProduct_ProductDisplayedInCart

Vorbedingungen: Benutzer ist auf der Produktübersicht. Ein Produkt ist verfügbar.
Testschritte: Produkt öffnen → „Add to cart“ klicken → Warenkorb öffnen.
Erwartetes Ergebnis: Produkt wird mit Name, Preis, Menge 1 und Zwischensumme im Warenkorb angezeigt.

T1_Cart_UpdateQuantity_TotalPriceRecalculatedCorrectly

Vorbedingungen: Ein Produkt befindet sich im Warenkorb.
Testschritte: Menge von 1 auf 2 erhöhen.
Erwartetes Ergebnis: Menge wird aktualisiert, Zwischensumme und Gesamtsumme werden korrekt neu berechnet.

* ### VERBESSERTE PROMT-VERSION:
Zusätzlich ergänzt:
Beispiel:
TC_101 – T1_Checkout_PaymentWithCreditCard_OrderCompletedSuccessfully
Für jeden Testfall sollen folgende Informationen enthalten sein:
- Test-ID
- Vorbedingungen
- Testschritte (nummeriert)
- Erwartetes Ergebnis
Die Testschritte sollen klar, strukturiert und realistisch für die ToolShop-Anwendung formuliert sein.
Die erwarteten Ergebnisse sollen präzise und überprüfbar formuliert werden.

#### ERGEBNISSE:
Durch die zusätzliche Strukturierung mit Test-ID, nummerierten Testschritten und präziseren Anforderungen wurden die Ergebnisse übersichtlicher und konsistenter.

* ### Beispiel:
TC_101 – T1_Cart_AddProductToCart_ProductAddedSuccessfully

Test-ID: TC_101

Vorbedingungen:
Benutzer befindet sich auf der Produktübersicht. Mindestens ein Produkt ist verfügbar.

Testschritte:

Ein verfügbares Produkt auswählen.
Auf der Produktdetailseite auf Add to cart klicken.
Den Warenkorb öffnen.

Erwartetes Ergebnis:
Das ausgewählte Produkt wird im Warenkorb mit korrektem Produktnamen, Preis, Menge 1 und Zwischensumme angezeigt.

---

### Teil 3 – Generieren von Testfällen (10 Min)
1. Führen Sie den Prompt mit einem generativen KI-Tool aus (z. B. ChatGPT)
2. Überprüfen Sie die Ausgabe
3. Wählen Sie mindestens 5 nützliche Testfälle aus

# Shopping Cart Test Cases

---

## TC_102 – T1_Cart_UpdateProductQuantity_TotalUpdatedCorrectly

**Test-ID:** TC_102

### Vorbedingungen
- Ein Produkt befindet sich im Warenkorb.

### Testschritte
1. Warenkorb öffnen.
2. Die Produktmenge von 1 auf 2 erhöhen.
3. Aktualisierung prüfen.

### Erwartetes Ergebnis
- Die Menge wird auf 2 gesetzt.
- Die Zwischensumme und Gesamtsumme werden korrekt neu berechnet.

---

## TC_112 – T4_Cart_SetQuantityToZero_ValidationErrorDisplayed

**Test-ID:** TC_112

### Vorbedingungen
- Ein Produkt befindet sich im Warenkorb.

### Testschritte
1. Warenkorb öffnen.
2. Produktmenge auf 0 setzen.
3. Änderung speichern oder Eingabefeld verlassen.

### Erwartetes Ergebnis
- Menge 0 wird nicht als gültige Bestellmenge akzeptiert.
- Eine Validierungsmeldung wird angezeigt oder das Produkt wird bewusst aus dem Warenkorb entfernt.

---

# Checkout Test Cases

---

## TC_103 – T1_Checkout_ValidCustomerData_OrderCompletedSuccessfully

**Test-ID:** TC_103

### Vorbedingungen
- Benutzer ist eingeloggt.
- Warenkorb enthält mindestens ein Produkt.

### Testschritte
1. Warenkorb öffnen.
2. Checkout starten.
3. Adressdaten prüfen oder gültig ausfüllen.
4. Versandart auswählen.
5. Zahlungsart auswählen.
6. Bestellung bestätigen.

### Erwartetes Ergebnis
- Die Bestellung wird erfolgreich abgeschlossen.
- Eine Bestellbestätigung wird angezeigt.
- Der Warenkorb ist leer.

---

## TC_110 – T3_Checkout_ProductUnavailableBeforeOrder_OrderBlockedWithErrorMessage

**Test-ID:** TC_110

### Vorbedingungen
- Ein Produkt befindet sich im Warenkorb.

### Testschritte
1. Produkt in den Warenkorb legen.
2. Produkt wird vor Abschluss der Bestellung nicht mehr verfügbar.
3. Checkout starten oder Bestellung bestätigen.

### Erwartetes Ergebnis
- Die Bestellung wird blockiert.
- Eine verständliche Fehlermeldung zeigt, dass das Produkt nicht verfügbar ist.

---

## TC_115 – T4_Checkout_InvalidCreditCardData_PaymentRejected

**Test-ID:** TC_115

### Vorbedingungen
- Benutzer ist eingeloggt.
- Warenkorb enthält ein Produkt.

### Testschritte
1. Checkout starten.
2. Zahlungsart "Credit Card" auswählen.
3. Ungültige Kartennummer oder abgelaufenes Ablaufdatum eingeben.
4. Bestellung bestätigen.

### Erwartetes Ergebnis
- Zahlung wird abgelehnt.
- Bestellung wird nicht erstellt.
- Eine Fehlermeldung zur ungültigen Zahlung wird angezeigt.

---

## TC_118 – T5_Checkout_ManipulateCartTotalInBrowser_ServerPriceUsedForOrder

**Test-ID:** TC_118

### Vorbedingungen
- Warenkorb enthält ein Produkt.
- Benutzer hat Zugriff auf Browser Developer Tools.

### Testschritte
1. Warenkorb öffnen.
2. Angezeigten Preis oder Gesamtbetrag im Browser manipulieren.
3. Checkout starten.
4. Bestellung bestätigen.

### Erwartetes Ergebnis
- Die Bestellung verwendet den serverseitig berechneten Originalpreis.
- Manipulierte Frontend-Werte werden ignoriert.

---
### **Hinweis: Die vollständigen KI-generierten Testfälle wurden zusätzlich in der separaten Datei `ToolShop_GenAI_Testcases(G101- ISTQB-TRATAS)_Guelbin` dokumentiert.**
---

Bei der Auswahl der Testfälle habe ich mich auf businesskritische und funktional wichtige Szenarien konzentriert, die für den Bestell- und Checkout-Prozess besonders relevant sind.
Dabei wollte ich sowohl positive als auch negative Testfälle sowie sicherheitsrelevante Aspekte abdecken.
Zusätzlich habe ich darauf geachtet, möglichst alle Testdesign-Kategorien (T1–T5) mit mindestens einem Testfall zu repräsentieren.

---

### Teil 4 – Bewertung der Qualität (10 Min)

Bewerten Sie die generierten Testfälle:

- Sind sie realistisch für ToolShop?
- Fehlen wichtige Szenarien?
- Sind einige Testfälle zu allgemein?


Die generierten Testfälle sind insgesamt realistisch und gut auf den ToolShop-Anwendungsfall übertragbar. Besonders positiv ist, dass die Testfälle zu den vorgegebenen Testdesign-Kategorien passen und sowohl normale Abläufe als auch Fehler- und Grenzfälle berücksichtigen.

Trotzdem wurden nicht alle möglichen Szenarien abgedeckt. Es könnten noch weitere relevante Testfälle ergänzt werden, zum Beispiel:
- Checkout als Gastbenutzer
- Versuch, ein nicht verfügbares Produkt in den Warenkorb zu legen
- Tests für verschiedene Zahlungsarten
- Eingabe einer sehr hohen Produktmenge im Warenkorb
- Validierung bei Änderungen der Produktmenge

Einige Testfälle könnten außerdem noch konkreter formuliert werden. Zum Beispiel könnten genaue Testdaten, konkrete Fehlermeldungen oder erwartete Systemzustände nach dem Test ergänzt werden. Insgesamt ist die Qualität der generierten Testfälle aber gut, da sie praxisnah, verständlich und für den ToolShop relevant sind.

Die Testfälle eignen sich grundsätzlich auch als Grundlage für Regressionstests und spätere Testautomatisierung, da die Testschritte und erwarteten Ergebnisse klar beschrieben sind.

---

Verbessern Sie das Ergebnis:
- Fügen Sie mindestens 2 fehlende Testfälle hinzu, die von der KI nicht generiert wurden

## TC_201 – T4_Checkout_InvalidCouponCode_ErrorMessageDisplayed

**Test-ID:** TC_201

### Vorbedingungen
- Benutzer ist eingeloggt.
- Warenkorb enthält mindestens ein Produkt.

### Testschritte
1. Checkout starten.
2. Einen ungültigen Gutscheincode eingeben.
3. Gutschein anwenden.
4. Bestellung fortsetzen.

### Erwartetes Ergebnis
- Der Gutscheincode wird nicht akzeptiert.
- Eine verständliche Fehlermeldung wird angezeigt.
- Der Gesamtpreis bleibt unverändert.

---

## TC_202 – T4_Cart_HighProductQuantity_ErrorMessageDisplayed

**Test-ID:** TC_202

### Vorbedingungen
- Ein Produkt befindet sich im Warenkorb.

### Testschritte
1. Warenkorb öffnen.
2. Eine sehr hohe Produktmenge eingeben (z. B. 9999).
3. Enter drücken oder Eingabefeld verlassen.

### Erwartetes Ergebnis
- Die eingegebene Menge wird nicht akzeptiert.
- Eine Validierungsmeldung oder Fehlermeldung wird angezeigt.
- Das System akzeptiert keine unrealistisch hohe Bestellmenge.

---

## Erwartetes Ergebnis / Team-Mehrwert

- Erste Sammlung von KI-generierten Testfällen für Warenkorb & Checkout
- Identifikation von:
  - Lücken in den KI-generierten Tests
  - Typischen Schwächen generativer KI
- Grundlage für:
  - Regressionstests
  - Testautomatisierung

---

## Akzeptanzkriterien

- Der Warenkorb- und Checkout-Flow wurde manuell untersucht
- Ein strukturierter Prompt wurde erstellt und ausgeführt
- Mindestens 5 generierte Testfälle wurden ausgewählt
- Die Testfälle enthalten:
  - Vorbedingungen
  - Testschritte
  - Erwartete Ergebnisse
- Eine kritische Bewertung der KI-Ausgabe wurde durchgeführt
- Mindestens 2 zusätzliche Testfälle wurden manuell ergänzt
- Die Ergebnisse sind dokumentiert und wurden mit dem Team geteilt (z. B. Testmanagement-Tool, Confluence)