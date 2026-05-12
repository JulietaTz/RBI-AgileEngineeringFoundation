# ToolShop_GenAI_Testcases

## PROMPT

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

```text
<Tx>_<Feature>_<BusinessRule>_<ExpectedResult>
```

Beispiel:

```text
T1_Checkout_PaymentWithCreditCard_OrderCompletedSuccessfully
```

Für jeden Testfall sollen folgende Informationen enthalten sein:

- Vorbedingungen
- Testschritte
- Erwartetes Ergebnis

Die Testfälle sollen realistisch für die ToolShop-Anwendung sein und typische Benutzer- sowie Fehlerszenarien abdecken.

---

# KI-generierte Testfälle – ChatGPT

Ich prüfe kurz die ToolShop-Anwendung, damit die Testfälle zu den tatsächlichen Warenkorb- und Checkout-Feldern passen. Danach formuliere ich sie direkt in der gewünschten T1–T5-Namenssyntax.

*Nachgedacht für 10s*

## Warenkorb & Checkout Testfälle

### T1_Cart_AddAvailableProduct_ProductDisplayedInCart

**Vorbedingungen:**  
Benutzer ist auf der Produktübersicht. Ein Produkt ist verfügbar.

**Testschritte:**  
Produkt öffnen → „Add to cart“ klicken → Warenkorb öffnen.

**Erwartetes Ergebnis:**  
Produkt wird mit Name, Preis, Menge 1 und Zwischensumme im Warenkorb angezeigt.

---

### T1_Cart_UpdateQuantity_TotalPriceRecalculatedCorrectly

**Vorbedingungen:**  
Ein Produkt befindet sich im Warenkorb.

**Testschritte:**  
Menge von 1 auf 2 erhöhen.

**Erwartetes Ergebnis:**  
Menge wird aktualisiert, Zwischensumme und Gesamtsumme werden korrekt neu berechnet.

---

### T1_Checkout_RegisteredUserWithValidData_OrderCompletedSuccessfully

**Vorbedingungen:**  
Benutzer ist registriert und eingeloggt. Warenkorb enthält mindestens ein Produkt.

**Testschritte:**  
Warenkorb öffnen → Checkout starten → Liefer-/Rechnungsdaten prüfen → Zahlungsart auswählen → Bestellung abschließen.

**Erwartetes Ergebnis:**  
Bestellung wird erfolgreich erstellt und eine Bestellbestätigung wird angezeigt.

---

### T1_Checkout_PaymentWithCreditCard_OrderCompletedSuccessfully

**Vorbedingungen:**  
Warenkorb enthält Produkt. Benutzer ist eingeloggt. Gültige Kreditkartendaten liegen vor.

**Testschritte:**  
Checkout starten → Kreditkarte wählen → gültige Kartendaten eingeben → Bestellung bestätigen.

**Erwartetes Ergebnis:**  
Zahlung wird akzeptiert, Bestellung wird abgeschlossen, Warenkorb ist leer.

---

### T2_Cart_RemoveOneOfMultipleProducts_RemainingProductsStayInCart

**Vorbedingungen:**  
Warenkorb enthält mindestens zwei verschiedene Produkte.

**Testschritte:**  
Ein Produkt aus dem Warenkorb entfernen.

**Erwartetes Ergebnis:**  
Nur das ausgewählte Produkt wird entfernt. Die übrigen Produkte bleiben unverändert im Warenkorb.

---

### T2_Cart_ContinueShoppingAfterAddingProduct_CartKeepsProduct

**Vorbedingungen:**  
Produkt wurde in den Warenkorb gelegt.

**Testschritte:**  
„Continue shopping“/Zurück zur Produktübersicht wählen → weiteres Produkt ansehen → Warenkorb erneut öffnen.

**Erwartetes Ergebnis:**  
Bereits hinzugefügtes Produkt bleibt im Warenkorb erhalten.

---

### T2_Checkout_EditBillingAddress_NewAddressUsedForOrder

**Vorbedingungen:**  
Benutzer ist eingeloggt und hat gespeicherte Profildaten. Warenkorb enthält Produkt.

**Testschritte:**  
Checkout starten → Adressdaten ändern → Bestellung fortsetzen.

**Erwartetes Ergebnis:**  
Geänderte Adresse wird im Checkout übernommen und für die Bestellung verwendet.

---

### T2_Checkout_AlternativePaymentMethod_OrderCompletedSuccessfully

**Vorbedingungen:**  
Warenkorb enthält Produkt. Benutzer ist eingeloggt.

**Testschritte:**  
Checkout starten → alternative Zahlungsart wie Bank Transfer oder Cash on Delivery auswählen → Bestellung bestätigen.

**Erwartetes Ergebnis:**  
Bestellung wird mit der gewählten Zahlungsart erfolgreich abgeschlossen.

---

### T3_Cart_ProductBecomesUnavailableBeforeCheckout_CheckoutBlockedWithMessage

**Vorbedingungen:**  
Produkt liegt im Warenkorb, wird aber vor Checkout nicht mehr verfügbar.

**Testschritte:**  
Checkout starten.

**Erwartetes Ergebnis:**  
Checkout wird blockiert oder Produkt wird als nicht verfügbar markiert. Benutzer erhält eine verständliche Fehlermeldung.

---

### T3_Checkout_SessionExpiresDuringCheckout_UserRedirectedToLogin

**Vorbedingungen:**  
Benutzer ist eingeloggt und befindet sich im Checkout.

**Testschritte:**  
Session ablaufen lassen → Bestellung bestätigen.

**Erwartetes Ergebnis:**  
Benutzer wird zum Login weitergeleitet. Bestellung wird nicht ohne Authentifizierung erstellt.

---

### T3_Checkout_PaymentProviderTimeout_OrderNotDuplicated

**Vorbedingungen:**  
Warenkorb enthält Produkt. Zahlung wird über Kreditkarte durchgeführt.

**Testschritte:**  
Zahlungsbestätigung auslösen → Timeout/Netzwerkunterbrechung simulieren.

**Erwartetes Ergebnis:**  
Benutzer erhält Fehlermeldung. Es wird keine doppelte Bestellung oder doppelte Zahlung erzeugt.

---

### T4_Cart_SetQuantityToZero_ValidationErrorDisplayed

**Vorbedingungen:**  
Ein Produkt befindet sich im Warenkorb.

**Testschritte:**  
Menge auf 0 setzen oder Menge löschen.

**Erwartetes Ergebnis:**  
System akzeptiert die ungültige Menge nicht. Fehlermeldung wird angezeigt oder Produkt wird bewusst entfernt.

---

### T4_Checkout_EmptyCart_ProceedToCheckoutNotAllowed

**Vorbedingungen:**  
Warenkorb ist leer.

**Testschritte:**  
Warenkorb öffnen → versuchen, Checkout zu starten.

**Erwartetes Ergebnis:**  
Checkout ist nicht möglich. Benutzer sieht Hinweis, dass der Warenkorb leer ist.

---

### T4_Checkout_InvalidCreditCardData_PaymentRejected

**Vorbedingungen:**  
Warenkorb enthält Produkt. Benutzer ist eingeloggt.

**Testschritte:**  
Checkout starten → Kreditkarte wählen → ungültige Kartennummer, abgelaufenes Datum oder falschen CVV eingeben → Zahlung bestätigen.

**Erwartetes Ergebnis:**  
Zahlung wird abgelehnt. Bestellung wird nicht erstellt. Validierungsfehler werden angezeigt.

---

### T5_Checkout_ManipulateCartTotalInBrowser_OrderUsesServerCalculatedAmount

**Vorbedingungen:**  
Warenkorb enthält Produkt. Benutzer nutzt Browser DevTools.

**Testschritte:**  
Angezeigten Preis oder Total im Frontend manipulieren → Checkout abschließen.

**Erwartetes Ergebnis:**  
Server ignoriert manipulierte Frontend-Werte. Bestellung verwendet den korrekten serverseitigen Preis.

---

### T5_Checkout_SQLInjectionInAddressFields_InputRejectedOrEscaped

**Vorbedingungen:**  
Benutzer ist im Checkout-Adressformular.

**Testschritte:**  
In Adressfelder Werte wie `' OR '1'='1` eingeben → Checkout fortsetzen.

**Erwartetes Ergebnis:**  
Eingabe wird sicher behandelt. Keine Fehlfunktion, kein Datenbankfehler, keine unautorisierte Aktion.

---

### T5_Checkout_ScriptInjectionInNameField_ScriptNotExecuted

**Vorbedingungen:**  
Benutzer ist im Checkout-Formular.

**Testschritte:**  
In Namens- oder Adressfeld `<script>alert(1)</script>` eingeben → Bestellung fortsetzen oder Bestätigung anzeigen.

**Erwartetes Ergebnis:**  
Script wird nicht ausgeführt. Eingabe wird escaped, validiert oder abgelehnt.

---

# VERBESSERTE PROMPT-VERSION

Prompt wurde zusätzlich ergänzt:

Beispiel:

```text
TC_101 – T1_Checkout_PaymentWithCreditCard_OrderCompletedSuccessfully
```

Für jeden Testfall sollen folgende Informationen enthalten sein:

- Test-ID
- Vorbedingungen
- Testschritte (nummeriert)
- Erwartetes Ergebnis

Die Testschritte sollen klar, strukturiert und realistisch für die ToolShop-Anwendung formuliert sein.  
Die erwarteten Ergebnisse sollen präzise und überprüfbar formuliert werden.

---

# ERGEBNISSE

Hier sind detaillierte Testfälle für den Warenkorb- und Checkout-Flow der ToolShop-Anwendung. Grundlage: ToolShop v5.0.

## TC_101 – T1_Cart_AddProductToCart_ProductAddedSuccessfully

**Test-ID:** TC_101

### Vorbedingungen
- Benutzer befindet sich auf der Produktübersicht.
- Mindestens ein Produkt ist verfügbar.

### Testschritte
1. Ein verfügbares Produkt auswählen.
2. Auf der Produktdetailseite auf Add to cart klicken.
3. Den Warenkorb öffnen.

### Erwartetes Ergebnis
- Das ausgewählte Produkt wird im Warenkorb mit korrektem Produktnamen, Preis, Menge 1 und Zwischensumme angezeigt.

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

## TC_104 – T1_Checkout_PaymentWithCreditCard_OrderCompletedSuccessfully

**Test-ID:** TC_104

### Vorbedingungen
- Benutzer ist eingeloggt.
- Warenkorb enthält ein Produkt.
- Gültige Kreditkartendaten liegen vor.

### Testschritte
1. Checkout starten.
2. Zahlungsart Credit Card auswählen.
3. Gültige Kartennummer, Ablaufdatum und CVV eingeben.
4. Bestellung abschließen.

### Erwartetes Ergebnis
- Die Zahlung wird akzeptiert.
- Die Bestellung wird erfolgreich erstellt und eine Bestätigung wird angezeigt.

---

## TC_105 – T2_Cart_RemoveProductFromCart_CartUpdatedSuccessfully

**Test-ID:** TC_105

### Vorbedingungen
- Mindestens ein Produkt befindet sich im Warenkorb.

### Testschritte
1. Warenkorb öffnen.
2. Beim Produkt auf Remove/Delete klicken.
3. Warenkorb erneut prüfen.

### Erwartetes Ergebnis
- Das Produkt wird aus dem Warenkorb entfernt.
- Die Gesamtsumme wird aktualisiert.
- Bei keinem weiteren Produkt wird der Warenkorb als leer angezeigt.

---

## TC_106 – T2_Cart_AddMultipleProducts_AllProductsDisplayedCorrectly

**Test-ID:** TC_106

### Vorbedingungen
- Benutzer befindet sich auf der Produktübersicht.
- Mehrere Produkte sind verfügbar.

### Testschritte
1. Erstes Produkt in den Warenkorb legen.
2. Zur Produktübersicht zurückkehren.
3. Zweites Produkt in den Warenkorb legen.
4. Warenkorb öffnen.

### Erwartetes Ergebnis
- Beide Produkte werden separat mit korrektem Namen, Preis, Menge und Zwischensumme angezeigt.

---

## TC_107 – T2_Checkout_EditAddressDuringCheckout_NewAddressSavedForOrder

**Test-ID:** TC_107

### Vorbedingungen
- Benutzer ist eingeloggt.
- Warenkorb enthält ein Produkt.

### Testschritte
1. Checkout starten.
2. Adressdaten im Checkout ändern.
3. Checkout fortsetzen.
4. Bestellung bestätigen.

### Erwartetes Ergebnis
- Die geänderte Adresse wird im Checkout angezeigt und für die Bestellung verwendet.

---

## TC_108 – T2_Checkout_AlternativePaymentMethod_OrderCompletedSuccessfully

**Test-ID:** TC_108

### Vorbedingungen
- Benutzer ist eingeloggt.
- Warenkorb enthält ein Produkt.

### Testschritte
1. Checkout starten.
2. Eine alternative Zahlungsart, z. B. Bank Transfer oder Cash on Delivery, auswählen.
3. Bestellung bestätigen.

### Erwartetes Ergebnis
- Die Bestellung wird mit der ausgewählten Zahlungsart erfolgreich abgeschlossen.

---

## TC_109 – T3_Checkout_SessionExpiredDuringCheckout_UserRedirectedToLogin

**Test-ID:** TC_109

### Vorbedingungen
- Benutzer ist eingeloggt und befindet sich im Checkout.

### Testschritte
1. Checkout-Prozess starten.
2. Session ablaufen lassen oder Benutzer in einem zweiten Tab ausloggen.
3. Bestellung bestätigen.

### Erwartetes Ergebnis
- Die Bestellung wird nicht erstellt.
- Benutzer wird zur Login-Seite weitergeleitet oder erhält eine Session-Fehlermeldung.

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

## TC_111 – T3_Checkout_PaymentTimeout_OrderNotCreatedTwice

**Test-ID:** TC_111

### Vorbedingungen
- Benutzer ist eingeloggt.
- Warenkorb enthält ein Produkt.
- Kreditkartenzahlung ist ausgewählt.

### Testschritte
1. Gültige Zahlungsdaten eingeben.
2. Bestellung bestätigen.
3. Timeout oder Verbindungsunterbrechung während der Zahlung simulieren.
4. Seite neu laden oder erneut bestätigen.

### Erwartetes Ergebnis
- Es wird keine doppelte Bestellung erzeugt.
- Benutzer erhält eine eindeutige Meldung zum Zahlungsstatus.

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

## TC_113 – T4_Checkout_EmptyCart_CheckoutNotAllowed

**Test-ID:** TC_113

### Vorbedingungen
- Warenkorb ist leer.

### Testschritte
1. Warenkorb öffnen.
2. Versuchen, den Checkout zu starten.

### Erwartetes Ergebnis
- Checkout ist nicht möglich.
- Benutzer erhält einen Hinweis, dass der Warenkorb leer ist.

---

## TC_114 – T4_Checkout_MissingRequiredAddressFields_ValidationErrorsDisplayed

**Test-ID:** TC_114

### Vorbedingungen
- Benutzer ist eingeloggt.
- Warenkorb enthält ein Produkt.

### Testschritte
1. Checkout starten.
2. Pflichtfelder wie Straße, Stadt oder Postleitzahl leer lassen.
3. Checkout fortsetzen.

### Erwartetes Ergebnis
- Checkout wird nicht fortgesetzt.
- Für jedes fehlende Pflichtfeld wird eine Validierungsmeldung angezeigt.

---

## TC_115 – T4_Checkout_InvalidCreditCardData_PaymentRejected

**Test-ID:** TC_115

### Vorbedingungen
- Benutzer ist eingeloggt.
- Warenkorb enthält ein Produkt.

### Testschritte
1. Checkout starten.
2. Zahlungsart Credit Card auswählen.
3. Ungültige Kartennummer oder abgelaufenes Ablaufdatum eingeben.
4. Bestellung bestätigen.

### Erwartetes Ergebnis
- Zahlung wird abgelehnt.
- Bestellung wird nicht erstellt.
- Eine Fehlermeldung zur ungültigen Zahlung wird angezeigt.

---

## TC_116 – T5_Checkout_SQLInjectionInAddressField_InputHandledSafely

**Test-ID:** TC_116

### Vorbedingungen
- Benutzer befindet sich im Checkout-Adressformular.

### Testschritte
1. In ein Adressfeld den Wert `' OR '1'='1` eingeben.
2. Checkout fortsetzen.
3. Systemverhalten prüfen.

### Erwartetes Ergebnis
- Die Eingabe wird sicher verarbeitet, validiert oder abgelehnt.
- Es tritt kein Datenbankfehler auf und keine unautorisierte Aktion wird ausgeführt.

---

## TC_117 – T5_Checkout_ScriptInjectionInNameField_ScriptNotExecuted

**Test-ID:** TC_117

### Vorbedingungen
- Benutzer befindet sich im Checkout-Formular.

### Testschritte
1. In ein Namens- oder Adressfeld `<script>alert(1)</script>` eingeben.
2. Checkout fortsetzen.
3. Bestellübersicht oder Bestätigung prüfen.

### Erwartetes Ergebnis
- Das Script wird nicht ausgeführt.
- Die Eingabe wird escaped, validiert oder abgelehnt.

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