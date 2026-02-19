
# Repository Agent Guide

This repo is the AI Workspace Template (Python). Follow the local
rules in `.antigravity/rules.md`, and `CONTEXT.md`.

## Must-Read Files (Before Any Work)
- `src/mission.md` (required by `.antigravity/rules.md`)
- `.antigravity/rules.md` (core agent behavior + coding standards)
- `CONTEXT.md` (project conventions and architecture)
- `.context/system_prompt.md` (agent persona + core directives)

## Artifact-First Workflow
- For non-trivial tasks, create a plan file in `artifacts/plan_[workflow_id].md`.
- Store test/log output in `artifacts/logs/`.
- If UI is modified, include a screenshot artifact.
- Keep artifacts lightweight and deterministic.

## Build / Lint / Test Commands
### Setup
- `python -m venv venv && source venv/bin/activate`
- `pip install -r requirements.txt`

### Run Agent
- `python src/agent.py "Your task here"`

### Docker
- `docker-compose up --build`

### Tests
- Run all tests: `pytest`
- Run test folder: `pytest tests/`


### Lint / Format
- No repo-level linter/formatter configs were found.
- Follow existing style and PEP 8; avoid reformatting unrelated code.
- Optional sanity check for tools: `python -m py_compile src/tools/*.py`

### CI
- GitHub Actions runs `pytest tests/` on pushes/PRs (`.github/workflows/test.yml`).

## Code Style (Python)
- **Type hints are required** for all function signatures.
- **Docstrings are required** for functions/classes; use Google-style format.
  Include `Args:`, `Returns:`, and `Raises:` where applicable.
- **Use Pydantic** for data models and settings (`pydantic`, `pydantic-settings`).
- **External API calls** must be wrapped in tools under `src/tools/`.
- **Use `<thought>` blocks** for non-trivial logic .
- **Imports**: stdlib → third-party → local (`src.`) with blank lines between.
- **Formatting**: 4-space indent, one statement per line, keep lines readable.
- **Strings**: prefer f-strings for interpolation; keep quote style consistent.

## Architecture & Patterns
- **Tool discovery**: public functions in `tools/*.py` are auto-loaded.
  Keep tool functions small, pure when possible, and well-documented.
- **MCP integration**: optional via `mcp_servers.json` and `.env`.
- **Context injection**: `.context/` markdown files are auto-loaded.
- **Memory**: JSON-based memory via `MemoryManager` (`src/memory.py`).

## Testing & Reliability
- Use `pytest` fixtures and `assert` statements (see `tests/`).
- Keep tests deterministic; avoid external network calls.
- Store test logs in `artifacts/logs/` per Antigravity rules.

## Notes for Agents
- The template expects a “Think → Act → Reflect” workflow.
- Avoid destructive commands (`rm -rf`, etc.).
