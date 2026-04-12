### 1. Title
Erklären, wie LLM-Ausgaben im Testkontext entstehen

---

### 2. Syllabus Reference
ISTQB GenAI – 1.1.2 Grundlagen von Generativer KI und LLMs

---

### 3. Lernziel
Beschreiben Sie die grundlegenden Mechanismen von LLMs und stellen Sie einen Bezug zu praktischen Auswirkungen auf Testanalyse und Testdesign her.

---

### 4. Kontext / Szenario
Ihr Team verwendet KI-Ausgaben, behandelt diese jedoch häufig als Fakten. Sie benötigen ein kurzes Lernartefakt, das erklärt, warum Ausgaben plausibel erscheinen können, aber dennoch falsch sein können (Stichwort: Halluzination).
---

### 5. Aufgabenstellung
1. Verwenden Sie folgenden Beispiel-Prompt: “Generate boundary value test ideas for cart quantity update.”
   * Ich habe den gleichen Prompt zweimal eingegeben und unterschiedliche Ergebnisse bekommen. Das zeigt, dass ein LLM nicht deterministisch ist.
   
2. Beschreiben Sie in einfachen Worten:

   - **Tokenisierung:** Ein Text wird in kleinere Einheiten, sogenannte Tokens, zerlegt, damit das Model ihn verarbeiten kann.

   - **Nutzung des Kontextfensters:** Das Kontextfenster umfasst alle Informationen, die das Modell gleichzeitig berücksichtigt, wie Eingaben, Systemanweisungen und Ausgaben.  
      - Wenn die Menge an Informationen zu groß ist, kann das Modell frühere Informationen vergessen.
      - Die Größe wird vom Modell bestimmt und ist begrenzt, d. h. sie hängt von der Modellarchitektur ab.

   - **Next-Token-Vorhersage:** Ein LLM erzeugt Text, indem es das wahrscheinlichste nächste Wort basierend auf dem Kontext vorhersagt.  

   - **Warum Nicht-Determinismus auftreten kann:** Ein LLM basiert auf Wahrscheinlichkeiten und kann daher bei der gleichen Eingabe unterschiedliche Ausgaben erzeugen.

   - **Halluzination:** Ein LLM kann plausible, aber falsche Antworten erzeugen, da es auf Wahrscheinlichkeiten basiert und kein echtes Wissen hat, sondern nur Muster aus Trainingsdaten nutzt. Deshalb dürfen diese Ergebnisse nicht ungeprüft übernommen werden.


3. Listen Sie 3 Konsequenzen für Tester auf, wenn sie KI-generierte Testinhalte validieren.
   
   * Der Tester muss überprüfen, ob die Ergebnisse wirklich korrekt sind, da ein LLM nicht immer zuverlässige Ergebnisse liefert (Halluzination).

   * Ein LLM ist nicht deterministisch, daher muss der Tester die Ergebnisse sorgfältig überprüfen (Non-determinism).

   * Ein LLM kann falsche Ergebnisse erzeugen, wenn der Text zu lang ist und das Modell aufgrund des begrenzten Kontextfensters frühere Informationen vergisst (Kontextfenster).


4. Fügen Sie 2 Qualitätskontrollen hinzu, die das Team immer anwenden sollte.

Das Team sollte immer:
- überprüfen, ob die Ergebnisse korrekt sind.  
- überprüfen, ob die Ergebnisse mit den Anforderungen übereinstimmen.

---

### 6. Erwartetes Ergebnis / Deliverable

Eine Markdown-Notiz mit einer kurzen Prozessbeschreibung und einer praktischen Kontroll-Checkliste.

---

### 7. Timebox
60 minutes

---

### 8. Hinweise (Optional)
Halten Sie die Erklärung verständlich für das Testteam (keine tiefgehende Mathematik erforderlich).
Konzentrieren Sie sich darauf, „was das für die Verifikation bedeutet“.
