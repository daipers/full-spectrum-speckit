---
description: Scaffold a new GSD milestone by running `/gsd:new-project` through the CLI.
agent: build
model: gpt-5.1-codex-mini
---

1. Ensure the CLI is installed (`gsd-install` helps); if not, run `!npx get-shit-done-cc@latest --opencode --local` before proceeding.
2. From the repository root run `!npx get-shit-done-cc --opencode --local` and select `OpenCode` as the runtime, `local` as the location, and accept the default prompts. This triggers the `/gsd:new-project` flow.
3. Let the installer generate the planning scaffolding: `PROJECT.md`, `REQUIREMENTS.md`, `ROADMAP.md`, `STATE.md`, and `.planning/research/`. Capture any resolved requirements or constraints.
4. Import the latest Speckit spec (if present) as phase context by merging the spec index + section files in order:
   ```bash
   FEATURE_DIR=$(ls -td .specify/specs/*/ 2>/dev/null | head -n 1)
   if [ -n "$FEATURE_DIR" ]; then
     FEATURE=$(basename "$FEATURE_DIR")
     PHASE_DIR=".planning/phases/$FEATURE"
     mkdir -p "$PHASE_DIR"
     python - <<'PY'
import os
import re
from pathlib import Path

feature = os.environ.get("FEATURE")
phase_dir = os.environ.get("PHASE_DIR")

spec = Path(f".specify/specs/{feature}/spec.md")
sections_dir = spec.parent / "sections"
output = Path(phase_dir) / f"{feature}-CONTEXT.md"

text = spec.read_text(encoding="utf-8")
links = re.findall(r"\((sections/[^)]+\.md)\)", text)
section_paths = [spec.parent / p for p in links]
if not section_paths:
    section_paths = sorted(sections_dir.glob("*.md"))

with output.open("w", encoding="utf-8") as f:
    f.write(text.strip() + "\n")
    for path in section_paths:
        if path.exists():
            f.write("\n---\n\n")
            f.write(path.read_text(encoding="utf-8").strip() + "\n")

print(f"Imported spec into {output}")
PY
   else
     echo "No .specify/specs/*/spec.md found; skipping spec import"
   fi
   ```
5. After the flow completes, list the new files (`!ls PROJECT.md REQUIREMENTS.md ROADMAP.md STATE.md .planning`) and summarize their contents so the next phase has context.
6. (Optional) If you are in a git repo, run `!git status` to stage the new planning documents or note what to commit before the next phase.
