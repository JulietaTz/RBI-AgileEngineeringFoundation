### 1. Titel
Wählen Sie den richtigen LLM-Typ für verschiedene Testaufgaben

---

### 2. Syllabus-Referenz
ISTQB GenAI – 1.1.3 Foundation-, Instruction-Tuned- und Reasoning-LLMs

---

### 3. Lernziel
Unterscheiden Sie Modellfamilien und wählen Sie einen geeigneten Modelltyp für typische QA-Aktivitäten aus.

---

### 4. Kontext / Szenario
Ihre Organisation bietet mehrere LLM-Optionen an. Tester benötigen ein praktisches Regelwerk, um je nach Aufgabenkomplexität und erwarteter Ausgabequalität den passenden Modelltyp auszuwählen.

---

### 5. Aufgabenstellung
1. Definieren Sie drei repräsentative QA-Aufgaben:

- **Anforderungszusammenfassung**  

   Bei der Anforderungszusammenfassung werden lange oder komplexe Anforderungen in eine kurze und verständliche Form gebracht, um sie besser analysieren zu können. Für diese Aufgabe sind Instruction-Tuned-LLMs gut geeignet, da sie Anweisungen verstehen und strukturierte Zusammenfassungen liefern. Foundation-Modelle können ebenfalls verwendet werden, sind jedoch weniger geeignet, da sie nicht speziell für das Befolgen von Anweisungen optimiert sind. Reasoning-Modelle sind in diesem Fall meist nicht notwendig, da keine komplexe Analyse erforderlich ist.

- **Testfallgenerierung**  
   Bei der Testfallgenerierung werden auf Basis von Anforderungen Testfälle erstellt. Für diese Aufgabe sind Instruction-Tuned-LLMs gut geeignet, da sie Anweisungen verstehen und strukturierte Testfälle generieren können. Reasoning-Modelle werden benötigt, wenn die Aufgabe komplex ist, zum Beispiel bei komplexen Systemen, bei der Analyse von Edge Cases oder wenn tiefes logisches Denken erforderlich ist.

- **Ursachenanalyse von Fehlern**  
   Bei der Ursachenanalyse von Fehlern werden die Ursachen von Problemen im System untersucht, um Fehler zu verstehen und zu beheben. Reasoning-LLMs sind besonders gut geeignet, da sie komplexe Muster erkennen und logische Zusammenhänge analysieren können. Instruction-Tuned-Modelle können ebenfalls unterstützen, sind jedoch weniger geeignet für tiefgehende Analysen. Foundation-Modelle sind für diese Aufgabe meist nicht ausreichend, da sie keine spezialisierten Reasoning-Fähigkeiten besitzen.


---

2. Vergleichen Sie für jede Aufgabe Foundation-, Instruction-Tuned- und Reasoning-Modelle.

   | Aufgabe                         | Foundation | Instruction-Tuned | Reasoning |
   |--------------------------------|------------|------------------|-----------|
   | Anforderungszusammenfassung    | mittel     | hoch             | niedrig   |
   | Testfallgenerierung            | niedrig    | hoch             | mittel    |
   | Ursachenanalyse von Fehlern    | niedrig    | mittel           | hoch      |  


---

3. Bewerten Sie jeden Modelltyp hinsichtlich: Präzision, Konsistenz, Erklärbarkeit und Aufwand.

   | Kriterium       | Foundation LLMs | Instruction-Tuned LLMs | Reasoning-Modelle |
   |-----------------|----------------|------------------------|-------------------|
   | Präzision       | mittel         | hoch                   | hoch              |
   | Konsistenz      | mittel         | hoch                   | mittel            |
   | Erklärbarkeit   | niedrig        | mittel                 | hoch              |
   | Aufwand         | niedrig        | mittel                 | hoch              |

   --> Die Bewertungen sind relativ und hängen von der jeweiligen Aufgabe und Komplexität ab.

---

4. Erstellen Sie pro Aufgabe eine Entscheidungsregel („wenn/dann“).

   - **Regel 1:**  
   Wenn viele oder komplexe Anforderungen vorliegen und eine Zusammenfassung benötigt wird, dann verwende ein Instruction-Tuned-Modell.

   - **Regel 2:**  
   Wenn die Testfälle einfach und strukturiert sind, verwende ein Instruction-Tuned-Modell, während bei komplexen Testfällen mit logischem Denken ein Reasoning-Modell verwendet werden sollte.

   - **Regel 3:**  
   Wenn eine komplexe Ursachenanalyse von Fehlern erforderlich ist und logisches Denken benötigt wird, dann verwende ein Reasoning-Modell.

---

5. Fügen Sie eine Governance-Empfehlung für die Nutzung von Modellen hinzu.

   Die Wahl des richtigen Modells ist entscheidend für die Effizienz, Qualität und Kosten von KI-Anwendungen, da die optimale Auswahl stets von der Komplexität und den Anforderungen der Aufgabe abhängt.


---

### 6. Erwartetes Ergebnis / Deliverable
Eine Entscheidungsmatrix und 3 klare Regeln zur Modellauswahl.

---

### 7. Zeitrahmen
25 Minuten

---

### 8. Hinweise (Optional)
- Halten Sie die Bewertungen relativ und evidenzbasiert.
- Vermeiden Sie Schlussfolgerungen wie „ein Modell passt für alle Fälle“.