# Tutorial: MCP Artifact Validator für TTT426

## Ziel

Du baust einen **einfachen MCP-Server mit genau einem Zweck**: TTT426-Artefakte validieren.

Der Server prüft Markdown-Artefakte gegen den Metadatenstandard (`schemas/artifact-metadata.schema.yaml`). Er schreibt keine Artefakte, ändert keinen Workflow-State und führt keine Quality Gates aus.

Voraussetzung: Der dateibasierte Workflow (Agents, Skills, Templates, Schemas) existiert bereits.

---

## Was dieser MCP-Server macht

### Aufgabe

Ein Agent übergibt den Pfad zu einem Artefakt (z. B. `projects/toolshop/working/test-cases.md`). Der Server:

1. liest die Datei,
2. extrahiert das YAML-Frontmatter,
3. prüft Pflichtfelder und erlaubte Statuswerte,
4. liefert ein strukturiertes Ergebnis: `passed` oder `failed` plus Fehlerliste.

### Was er nicht macht

- keine fachliche Bewertung (Inhalt, Risiken, Review-Urteil)
- keine Workflow-Transitions
- keine Quality-Gate-Entscheidung
- keine Human Approval
- kein Schreiben in Projektdateien

### Warum MCP statt nur Script?

| Variante | Nutzen |
|---|---|
| `scripts/validate_artifact.py` | CLI / CI |
| MCP-Tool `validate_artifact` | Agent kann denselben Check **als Tool** aufrufen |

Beide nutzen dieselbe Prüfregel. MCP macht die Validierung für Cursor-Agenten aufrufbar und deterministisch.

```text
Agent schreibt Artefakt
        │
        ▼
MCP: validate_artifact(path)
        │
        ├── passed  → weiter (Review / QG / Handover)
        └── failed  → Fehler beheben, erneut schreiben
```

---

## Teil A: MCP-Server erstellen

### Schritt 1: Ordner anlegen

Im Workflow-Paket:

```text
TTT426-agentic-workflow/
  mcp/
    artifact-validator/
      README.md
      requirements.txt
      server.py
      validator.py
```

`validator.py` enthält die reine Prüflogik (ohne MCP).  
`server.py` exponiert diese Logik als MCP-Tool.

### Schritt 2: Abhängigkeiten

`requirements.txt`:

```text
mcp[cli]>=1.2.0
PyYAML>=6.0
```

Installation (Beispiel mit venv):

```bash
cd TTT426-agentic-workflow/mcp/artifact-validator
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### Schritt 3: Prüflogik (`validator.py`)

Diese Logik entspricht dem Tutorial-Schritt zum Artifact Validator und dem Schema `artifact-metadata.schema.yaml`.

```python
from __future__ import annotations

from pathlib import Path

import yaml

REQUIRED_FIELDS = {
    "artifact_id",
    "title",
    "artifact_type",
    "project",
    "version",
    "status",
    "created_by",
    "created_at",
    "source_inputs",
    "assumptions",
    "open_questions",
}

ALLOWED_STATUS = {
    "draft",
    "in-review",
    "changes-requested",
    "approved",
    "rejected",
    "obsolete",
}


def extract_frontmatter(content: str) -> dict:
    if not content.startswith("---\n"):
        raise ValueError("Artifact has no YAML frontmatter.")

    parts = content.split("---", 2)
    if len(parts) < 3:
        raise ValueError("Artifact frontmatter is not closed.")

    metadata = yaml.safe_load(parts[1])
    if not isinstance(metadata, dict):
        raise ValueError("Artifact frontmatter must contain a YAML object.")

    return metadata


def validate_artifact(path: str | Path) -> dict:
    """Validate a TTT426 markdown artifact. Returns a structured result."""
    artifact_path = Path(path)
    errors: list[str] = []

    if not artifact_path.is_file():
        return {
            "passed": False,
            "path": str(artifact_path),
            "errors": [f"File not found: {artifact_path}"],
        }

    try:
        content = artifact_path.read_text(encoding="utf-8")
        metadata = extract_frontmatter(content)
    except (OSError, ValueError, yaml.YAMLError) as exc:
        return {
            "passed": False,
            "path": str(artifact_path),
            "errors": [str(exc)],
        }

    missing = REQUIRED_FIELDS.difference(metadata.keys())
    if missing:
        errors.append(f"Missing metadata fields: {sorted(missing)}")

    status = metadata.get("status")
    if status not in ALLOWED_STATUS:
        errors.append(f"Invalid artifact status: {status!r}")

    return {
        "passed": len(errors) == 0,
        "path": str(artifact_path),
        "artifact_id": metadata.get("artifact_id"),
        "status": status,
        "errors": errors,
    }
```

### Schritt 4: MCP-Server (`server.py`)

Ein Tool reicht: `validate_artifact`.

```python
from __future__ import annotations

from mcp.server.fastmcp import FastMCP

from validator import validate_artifact as run_validation

mcp = FastMCP("ttt426-artifact-validator")


@mcp.tool()
def validate_artifact(path: str) -> dict:
    """
    Validate a TTT426 markdown artifact against the artifact metadata schema.

    Checks YAML frontmatter for required fields and allowed status values.
    Does not judge content quality, risk, or review findings.
    """
    return run_validation(path)


if __name__ == "__main__":
    mcp.run(transport="stdio")
```

Hinweise:

- Stdio-Transport: Cursor startet den Server als Subprozess.
- **Kein `print()` auf stdout** — das stört das MCP-Protokoll. Logging nur nach stderr.
- Die Docstring des Tools ist die Anweisung für den Agenten: kurz und klar halten.

### Schritt 5: Lokal ohne Cursor testen

```bash
source .venv/bin/activate
python -c "
from validator import validate_artifact
print(validate_artifact('../../projects/toolshop/working/test-cases.md'))
"
```

Erwartetes Ergebnis bei korrektem Frontmatter:

```text
{'passed': True, 'path': '...', 'artifact_id': 'TCA-TOOLSHOP-001', 'status': 'draft', 'errors': []}
```

Optional: MCP Inspector (`npx @modelcontextprotocol/inspector`) gegen `server.py` starten und das Tool manuell aufrufen.

---

## Teil B: In Cursor einbinden

### Schritt 6: MCP in Cursor registrieren

Projektspezifisch (empfohlen): Datei im **Repository-Root** (nicht im Kursunterordner):

`.cursor/mcp.json`

```json
{
  "mcpServers": {
    "ttt426-artifact-validator": {
      "command": "/ABSOLUTE/PATH/TO/TTT426-agentic-workflow/mcp/artifact-validator/.venv/bin/python",
      "args": [
        "/ABSOLUTE/PATH/TO/TTT426-agentic-workflow/mcp/artifact-validator/server.py"
      ]
    }
  }
}
```

Alternative: globale Config unter `~/.cursor/mcp.json`.

Danach:

1. Cursor MCP-Einstellungen öffnen
2. Server `ttt426-artifact-validator` prüfen (grüner Status)
3. Bei Fehler: absolute Pfade und venv-Python kontrollieren

Pfad- und Config-Details können sich je Cursor-Version ändern — bei Problemen die aktuelle [Cursor-Dokumentation](https://cursor.com/docs) prüfen.

### Schritt 7: Smoke-Test im Chat

Prompt:

```text
Validate the artifact projects/toolshop/working/test-cases.md
using the ttt426-artifact-validator MCP tool.
```

Erwartung: Der Agent ruft `validate_artifact` auf und gibt `passed` / `errors` wieder, ohne den Dateiinhalt neu zu „bewerten“.

---

## Teil C: In den TTT426-Workflow einbinden

MCP allein ändert den Workflow nicht. Du verdrahtest ihn in Agenten, Workflows und dem Response-Verhalten.

### Schritt 8: Orchestrator — Tool bekannt machen

In `agents/test-orchestrator.agent.md` die Tool-Liste ergänzen:

```yaml
tools:
  - read
  - search
  - edit
  - terminal
  - mcp:ttt426-artifact-validator
```

(Exacte Tool-Benennung hängt von der Cursor-Version ab. Wichtig: Der Orchestrator darf das Validator-Tool nutzen.)

Ergänze unter Responsibilities:

```markdown
- After an artifact is created or updated, request validation via
  the artifact validator MCP tool before advancing the workflow.
- Treat validation failure as a blocker. Do not mark a quality gate
  as passed when artifact validation failed.
```

### Schritt 9: Specialist Agents — Validierung vor Handover

In `test-analyst.agent.md`, `test-designer.agent.md` (und später weiteren Produzenten):

```markdown
## Completion Criteria

1. Artifact written to the defined project path.
2. Artifact validated with MCP tool `validate_artifact`.
3. Validation result `passed: true`.
4. Only then request independent review or create handover.
```

### Schritt 10: Workflow-Schritte ergänzen

In den Workflows, die Artefakte erzeugen (z. B. `01`, `03`, `04`), nach „Create artifact“ einen festen Schritt einfügen:

```markdown
## Step: Validate artifact

1. Call MCP tool `validate_artifact` with the artifact path.
2. If `passed` is false:
   - list all errors,
   - fix the artifact,
   - re-run validation.
3. If `passed` is true:
   - proceed to review or quality gate.
```

Beispiel-Reihenfolge in Workflow 04:

```text
Load inputs
  → Design test cases (Skill)
  → Write test-cases.md
  → Validate artifact (MCP)     ← neu
  → Independent review (Skill)
  → QG-03 evidence
  → Handover
```

### Schritt 11: AGENTS.md Response Behaviour

In `AGENTS.md` den Pflichtablauf schärfen:

```markdown
## Required Response Behaviour

For every substantial task:

1. Identify the requested workflow step.
2. Identify required inputs.
3. Check whether the inputs exist.
4. Report blocking gaps.
5. Execute the relevant skill.
6. Create or update the defined artifact.
7. Validate the artifact with the artifact validator MCP tool.
8. Run the required quality gate.
9. Record the handover and next step.
```

Punkt 7 ist der MCP-Schritt. Er ersetzt weder Review noch Quality Gate.

### Schritt 12: Abgrenzung klar halten

| Baustein | Prüft |
|---|---|
| MCP `validate_artifact` | Frontmatter, Pflichtfelder, Statuswerte |
| Skill `review-test-artifact` | fachliche Qualität, Traceability-Urteil, Findings |
| Quality Gate `qg-0N-*.md` | Checkliste inkl. Evidence |
| Human Approval | Freigabe / Restrisiko |

Regel: **Validierung ≠ Review ≠ Freigabe.**

---

## Teil D: Abnahme

### Checkliste „Server fertig“

- [ ] `validator.py` prüft ein gültiges Toolshop-Artefakt mit `passed: true`
- [ ] Ein Artefakt ohne Frontmatter liefert `passed: false` und klare Fehler
- [ ] Ein Artefakt mit ungültigem `status` schlägt fehl
- [ ] Cursor zeigt den MCP-Server als verbunden
- [ ] Agent kann `validate_artifact` im Chat aufrufen

### Checkliste „Workflow eingebunden“

- [ ] Orchestrator kennt den Validator
- [ ] Produzierende Agents fordern Validierung vor Review/Handover
- [ ] Mindestens ein Workflow enthält den Schritt „Validate artifact“
- [ ] `AGENTS.md` listet Validierung im Response Behaviour
- [ ] Fehlgeschlagene Validierung blockiert den Fortschritt (kein stilles Weiterlaufen)

### Manueller Durchlauf

1. Öffne ein gültiges Artefakt → Validierung PASS
2. Entferne temporär `artifact_id` aus dem Frontmatter → Validierung FAIL
3. Stelle das Feld wieder her → PASS
4. Starte Workflow-Schritt Design/Analysis und prüfe, ob der Agent vor dem Handover validiert

---

## Spätere Erweiterungen (bewusst später)

Erst wenn der einfache Validator stabil läuft:

- Existenz der `source_inputs`-Pfade prüfen
- eindeutige `artifact_id` im Projektverzeichnis
- Pflichtabschnitte im Markdown-Body
- CLI und MCP dieselbe Funktion teilen lassen (`scripts/validate_artifact.py` importiert `validator.py`)
- CI: GitHub Action ruft dieselbe Prüflogik auf

Nicht in diesen MVP aufnehmen: State-Machine, QG-Ausführung, ToolShop-API, Testiny.

---

## Kurzfassung

1. **Eine** Prüflogik für Artefakt-Metadaten  
2. **Ein** MCP-Tool: `validate_artifact`  
3. In Cursor registrieren  
4. In Orchestrator, Specialist Agents, Workflows und `AGENTS.md` als Pflichtschritt vor Review/QG verdrahten  

Damit bleibt der MCP klein und deterministisch — und der Workflow bekommt eine harte, wiederholbare Artefaktprüfung, ohne fachliche Urteilskraft an das Tool abzugeben.
